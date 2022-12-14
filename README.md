# go-schnorrkel
  <a href="https://discord.gg/zy8eRF7FG2">
    <img alt="Discord" src="https://img.shields.io/discord/593655374469660673.svg?style=flat&label=Discord&logo=discord" />
  </a>


Go implementation of the sr25519 signature algorithm (schnorr over ristretto25519). The existing rust implementation is [here.](https://github.com/w3f/schnorrkel)

This library is currently able to create sr25519 keys, import sr25519 keys, and sign and verify messages. It is interoperable with
the rust implementation. 

The BIP39 implementation in this library is compatible with the rust [substrate-bip39](https://github.com/paritytech/substrate-bip39) implementation.  Note that this is not a standard bip39 implementation.

### dependencies

go 1.13

### usage

Example: key generation, signing, and verification

```
package main 

import (
	"fmt"
	
	schnorrkel "github.com/ginharu/go-schnorrkel"
)

func main() {
	msg := []byte("hello friends")
	signingCtx := []byte("example")

	signingTranscript := schnorrkel.NewSigningContext(signingCtx, msg)
	verifyTranscript := schnorrkel.NewSigningContext(signingCtx, msg)

	priv, pub, err := schnorrkel.GenerateKeypair()
	if err != nil {
		fmt.Println(err)
		return
	}

	sig, err := priv.Sign(signingTranscript)
	if err != nil {
		fmt.Println(err)
		return
	}

	ok := pub.Verify(sig, verifyTranscript)
	if !ok {
		fmt.Println("did not verify :(")
		return
	}
}

```