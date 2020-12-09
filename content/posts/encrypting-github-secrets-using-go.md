+++ 
title = "Encrypting GitHub Secrets using Go"
date = 2020-12-08T19:50:00-08:00
description = "How to encrypt a GitHub secret in Golang"
+++

>Want to start encrypting right away? Skip to [GitHub](https://github.com/jefflinse/githubsecret/blob/main/nacl.go) or [install the package](#package).

When programatically storing a secret value in a repository for use in a GitHub Actions workflow, you must first [encrypt the value](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#create-or-update-a-repository-secret) using libsodium. If you want to do this in Golang, there are basically two choices: bind the libsodium C library, for which there are Go packages available[^1], or use APIs provided by the `golang.org/x/crypto` package.

In this article I'll demonstrate how to encrypt your GitHub secrets in Go using the latter approach, without the need to bind libsodium.

The high-level steps for programatically storing a GitHub secret are:

1. Obtain the public key for the repository[^2] in which you want to store the secret.
2. Encrypt your secret value using that public key.
3. Set the encrypted secret in your repository.

This article assumes you already have the necessary access permissions for creating secrets on your repository and that you're comfortable calling REST endpoints from Go, so focus will be on the encryption. However, a full end-to-end example of this workflow is available on [GitHub](https://github.com/jefflinse/githubsecret/tree/main/examples/putsecret).

## Obtaining The Public Key

Before we can do anything, we need to [get the repository's public key](https://docs.github.com/en/free-pro-team@latest/rest/reference/actions#get-a-repository-public-key), which we'll use when encrypting secret values.

GitHub returns a JSON response containing the `key` and `key_id`. Hang on to both of these -- we'll need the former for encrypting secret values and the latter for storing them.

## Encrypting The Secret

GitHub uses ["sealed box" encryption](https://libsodium.gitbook.io/doc/public-key_cryptography/sealed_boxes) from libsodium to encrypt and decrypt secrets. The corresponding functionality we need for encrypting secrets is in two Go packages:

```bash
go get golang.org/x/crypto/blake2b
```

```bash
go get golang.org/x/crypto/nacl/box
```

The `blake2b` package contains cryptographic hashing functionality needed for combining public keys into a nonce, and the `nacl/box` package contains the NaCl sealed box APIs we'll use to encrypt our secret values.

On to the encryption function. We want a function, let's call it `naclEncrypt()`, that will take the repo's public key and secret value to be encrypted and return the encrypted content.

```go
func naclEncrypt(recipientPublicKey string, content string) (string, error) {

}
```

Let's implement it piece by piece. For clarity, I'll omit most error handling code as we examine each step in detail.

### 1. Decode The Public Key

The public key we got from GitHub is base64-encoded and we must decode it. We'll also need its contents as an array (not a slice), thus we copy the bytes into a `[32]byte`:

```go
// decode the provided public key from base64
b, _ := base64.StdEncoding.DecodeString(recipientPublicKey)
recipientKey := new([32]byte)
copy(recipientKey[:], b)
```

### 2. Generate A Key Pair

Next, we need to create a 24-byte nonce -- a one-time value that's unique for each message we encrypt. We can't just use any random value here; the nonce needs to be a hash of our repository's public key and a another public key we generate randomly. We already have the former, so our next step is to generate a random key pair. The `box` package provides this mechanism:

```go
// create an ephemeral key pair
pubKey, privKey, _ := box.GenerateKey(rand.Reader)
```

### 3. Create A Nonce

With our newly generated public key we can create our nonce. For this we use the `blake2b` package to hash together the two public keys. The nonce must be a an array, so just as with our public key, we copy the bytes from the hash's resulting slice into a `[24]byte`:

```go
// create the nonce by hashing together the two public keys
nonceHash, _ := blake2b.New(24, nil)
nonceHash.Write(pubKey[:])
nonceHash.Write(recipientKey[:])
nonce := new([24]byte)
copy(nonce[:], nonceHash.Sum(nil))
```

### 4. Encrypt The Content

Now we're ready to encrypt our secret.

The first argument to `box.Seal()` specifies a slice that the encrypted content will be appended to. Protocol requires us to prepend the ephemeral public key to our encrypted secret data, so we'll pass it as the first argument.

```go
// begin the output with the ephemeral public key and append the encrypted content
out := box.Seal(pubKey[:], []byte(content), nonce, recipientKey, privKey)
```

The remaining arguments are the secret content to be encrypted, the nonce, the recipient public key, and the generated private key.

Finally, we just need to base64-encode the output:

```go
// base64-encode the final output
return base64.StdEncoding.EncodeToString(out), nil
```

Done. The complete implementation can be found [here](https://github.com/jefflinse/githubsecret/blob/main/nacl.go).

## Storing The Secret

Now that we've encrypted our secret, we're ready to [store it](https://developer.github.com/v3/actions/secrets/#create-or-update-a-repository-secret). In the request body, specify the `encrypted_value` and the `key_id` of the public key we obtained from the repository.

## Package

This code is available as a Go package on [GitHub](https://github.com/jefflinse/githubsecret).

```bash
go get github.com/jefflinse/githubsecret
```

```go
import (
    "fmt"
    "log"
    "github.com/jefflinse/githubsecret"
)

func main() {
    publicKey := "jpEqkF3JrT+00cEQdpTAUnVWYdJJpkbghDSTndgRJKg="
    secret := "my super sensitive value"
    encrypted, err := githubsecret.Encrypt(publicKey, secret)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println(encrypted)
}
```

[^1]: [libsodium-go](https://github.com/GoKillers/libsodium-go) and [Sodium](https://github.com/jamesruan/sodium), from [libsodium's "Bindings for other languages"](https://libsodium.gitbook.io/doc/bindings_for_other_languages)
[^2]: GitHub secrets can also be set at the organization level. The endpoints differ slightly but the workflow is the same.
