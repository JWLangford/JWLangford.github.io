---
layout: post
title: "Implementing Magic Links With Go"
date: 2025-06-13 12:00:00 +0800
tags: [security, cryptography]
description: "A graphic designer is a professional within the graphic design and graphic arts industry."
status: completed
---

Magic links or Email-Based One-Time Token Authentication are a common login option in todays app ecosystem.

<!--more-->

## Issuing

Below is an example for issuing a token on login.

```go
func login(username string) error {

    user, exists := users[username]

    if exists {
        b := make([]byte, 32)
	    _, err := rand.Read(b)
	    if err != nil {
		    return fmt.Errorf("something went wrong with your request, please contact support")
	    }

        token := base64.RawURLEncoding.EncodeToString(b)

        // set expiration to whatever suits you
        expiration := time.Minute * 15

        hash := sha256.Sum256([]byte(token))
	    hashedToken := base64.RawURLEncoding.EncodeToString(hash[:])

        err = storeToken(user.id, hashedToken, expiration)
        if err != nil {
            return fmt.Errorf("something went wrong with your request, please contact support")
        }

        err = emailLink(user.email, token)
        if err != nil {
            return fmt.Errorf("something went wrong with your request, please contact support")
        }
    }

    return nil
}
```

Once a user is found, a token is issued using the cryptographically secure pseudo-random number generator standard (CSPRNG).
Next, a timestamp is created 15 minutes in the future, which will act as an expiration. Before the token record is stored, the token string is hashed for added security. Tokens should be stored using the hashed token as the unique identifier or key for lookup. When verification happens, the only piece of data available will be the token, so it needs to be main lookup value.

After the record is stored successfully, an email is sent to the user.

Similar to how a username and password form works, it's important to obfuscate when a user record is not found. Meaning, if you login with a missing username you don't feedback the username is not found, conversely, if a username is found and the password is wrong you don't feedback the password is wrong. In both cases, the best feedback is username or password is incorrect.

When issuing magic links, the same standard holds, when users provide their username or email, the application should feedback the same response to both found and not found users.

## Verification

Inside the email sent to the user, they should have a link formatted as follows: `https://your-site?token={token}`. Once a user clicks the email link, the token gets passed to your server for verification.

```go
    func verifyToken(token string) (string, error) {
        hash := sha256.Sum256([]byte(token))
	    hashedToken := base64.URLEncoding.EncodeToString(hash[:])

        token, exists := tokenStore[hashedToken]
        if !exists {
            return "", fmt.Errorf("invalid token")
        }

        if time.Now().After(token.ExpiresAt) {
            return "", fmt.Errorf("token expired")
        }

        // all checks have passed, issue your authentication token
        authToken := issueAuthToken()

        // delete the token however you want
        tokenStore[hashed] = nil

        return authToken, nil
    }
```

Verification starts with the same hashing process before a lookup is done. Once the token record is found, check if the expiration date has passed. Once all checks are done, issue an authentication token however you want. Finally, delete the token record and return your authentication token to the client.
