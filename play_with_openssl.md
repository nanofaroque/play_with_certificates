# Playing with Openssl

1. Check openssl version
```
openssl version -a
```

### OpenSSL and CSR Creation
The first step to obtaining an SSL certificate is using OpenSSL to create a certificate signing request (CSR) that can be sent to a Certificate Authority (CA) (e.g., DigiCert). The CSR contains the common name(s) you want your certificate to secure, information about your company, and your public key. In order for a CSR to be created, it needs to have a private key from which the public key is extracted. This can be done by using an existing private key or generating a new private key.

Security Note: Because of the security issues associated with using an existing private key, and because it's very easy and entirely free to create a private key, we recommend you generate a brand new private key whenever you create a CSR.

Deciding on Key Generation Options
When generating a key, you have to decide three things: the key algorithm, the key size, and whether to use a passphrase.

Key Algorithm
For the key algorithm, you need to take into account its compatibility. For this reason, we recommend you use RSA. However, if you have a specific need to use another algorithm (such as ECDSA), you can use that too, but be aware of the compatibility issues you might run into.

Note: This guide only covers generating keys using the RSA algorithm.

Key Size
For the key size, you need to select a bit length of at least 2048 when using RSA and 256 when using ECDSA; these are the smallest key sizes allowed for SSL certificates. Unless you need to use a larger key size, we recommend sticking with 2048 with RSA and 256 with ECDSA.

Note: In older versions of OpenSSL, if no key size is specified, the default key size of 512 is used. Any key size lower than 2048 is considered unsecure and should never be used.

Passphrase
For the passphrase, you need to decide whether you want to use one. If used, the private key will be encrypted using the specified encryption method, and it will be impossible to use without the passphrase. Because there are pros and cons with both options, it's important you understand the implications of using or not using a passphrase. In this guide, we will not be using a passphrase in our examples.
