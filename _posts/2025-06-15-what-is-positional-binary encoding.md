---
layout: post
title: "What is Fixed-Width Binary Encoding"
date: 2025-06-15 12:00:00 +0800
tags: [security, cryptography]
description: "An introduction to Fixed-Width Binary Encoding, a compact and efficient method of storing structured data in bytes without identifiers."
status: completed
---

Fixed-Width Binary Encoding is a data structure not often seen in web development.

<!--more-->

### Getting Started

When data is transferred between parties, the convention is a key value pair structure eg.

```
{
    userId: "191B2E92-3E2A-4473-B848-8E8D07046FD7",
    attending: true,
    Date: 1750021782
}
```

If this data example was converted into bytes as is, it would look like:

```go
package main

import (
	"encoding/json"
	"fmt"
)

type UserAttendanceRecord struct {
	UserID    string `json:"userId"`
	Attending bool   `json:"attending"`
	Date      int64  `json:"date"`
}

func main() {
	record := UserAttendanceRecord{
		UserID:    "191B2E92-3E2A-4473-B848-8E8D07046FD7",
		Attending: true,
		Date:      1750021782,
	}

	bytes, err := json.Marshal(record)
	if err != nil {
		panic(err)
	}

	fmt.Printf("Total length: %d bytes\n", len(bytes))
}
```

```
Total length: 84 bytes
```

### Fixed-Width Binary Encoding

Fixed-Width Binary Encoding stores data in an array of bytes without identifiers. Because there are no identifiers, the order and length of the data in the array follows a predefined structure.

Fixed-Width Binary Encoded arrays are used in cryptographic processes where data length is standardized and data transfer efficiency is paramount. An example specification is below.

### User Attendance Data

| Position | Name      | Bytes | Description                            |
| -------- | --------- | ----- | -------------------------------------- |
| 0 - 15   | User Id   | 16    | The UUID of the user                   |
| 16       | Attending | 1     | Represents attendance: 0 = No, 1 = Yes |
| 17 - 20  | Date      | 4     | The UNIX timestamp of the event        |

```
userAttendanceRecord := {
    userId: "191B2E92-3E2A-4473-B848-8E8D07046FD7",
    attending: true,
    Date: 1750021782
}
```

```go
func process() {
	hexStr := "191B2E923E2A4473B8488E8D07046FD7"
	userID, err := hex.DecodeString(hexStr)
	if err != nil {
		panic(err)
	}

	attending := []byte{1}

	date := make([]byte, 4)
	binary.LittleEndian.PutUint32(date, 1750021782)

	pbe := append(userID, attending...)
	pbe = append(pbe, date...)

    return pbe
}
```

```
Total length: 21 bytes
```

The receiver of the data would use the API specs above to decode the provided byte array back into their original value.
