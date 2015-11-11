---
layout: post
title: Try a little crypto
---

These are some notes about AES encryption in R, because it took me a little fiddling to produce a working example of my own.

```
library(digest)
```

## First, the example from the digest vignette

```
msg <- as.raw(rep(1:16, 2)) # encrypt this
key <- as.raw(1:16)         # with this key
```

Now in CBC mode: each encoding is different

```
iv <- sample(0:255, 16, replace=TRUE)
aes <- AES(key, mode="CBC", iv)
code <- aes$encrypt(msg)
code
```

Need a new object for decryption in CBC mode

```
aes <- AES(key, mode="CBC", iv)
```

And here's the original msg:

```
aes$decrypt(code, raw=TRUE)
```

## Next, my own example

```
encryptthis <- "There's no place like home."
stepone <- charToRaw(encryptthis)
```

Now append to the raw vector as many bytes as needed for its length to
be a multiple of 16. What bytes you might append? Just pick the raw byte
representation of the number needed. This is also known as [PKCS5
padding](http://www.di-mgt.com.au/cryptopad.html):

```
rem <- 16L - length(stepone) %% 16L
```

This rule also ensures that your raw string is always padded. If by
chance the length of the original string in raw format is a multiple of
16 already, then `rem` = 16 and you will pad it with
`as.raw(rep(16, 16))`. This is nice because if the reader knows that
this is your recipe for padding, they just need to check what number the
last character is, move back as many elements in the decrypted raw
vector, and drop them as padding.

```
steptwo <- c(stepone, as.raw(rep(rem, rem)))
```

Now set a key that will be shared with the recipient:

```
key <- as.raw(sample(0:255, 16, replace=TRUE))
```

And set an initial vector for encryption:

```
iv <- sample(0:255, 16, replace=TRUE)
```

Now encrypt, CBC mode:

```
aes <- AES(key, mode="CBC", iv)
decryptthis <- aes$encrypt(steptwo)
```

Pre-pend the iv as raw vector, and send to recipient.

```
decryptthis <- c(as.raw(iv), decryptthis)
```

Now the recipient gets the message `decryptthis` and the key `key`. She
knows that the first 16 raw bytes of the message are the IV, and expects
that there's padding at the end.

So, with this key and this knowledge, here's the decryption object:

```
aes <- AES(key, mode="CBC", decryptthis[1:16])
```

And here's the original message, in raw bytes:

```
padded <- aes$decrypt(decryptthis[17:length(decryptthis)], raw = TRUE)
```

Now find the size of the padding, remove it, then show the original
message in plain text:

```
rem       <- as.integer(paste('0x', as.character(padded[length(padded)]), sep = ''))
plaintext <- rawToChar(padded[1:(length(padded) - rem)])
```

Check that `encryptthis` and `plaintext` are the same:

```
print(encryptthis)
print(plaintext)
```

