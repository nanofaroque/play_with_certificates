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

#### Key Algorithm
For the key algorithm, you need to take into account its compatibility. For this reason, we recommend you use RSA. However, if you have a specific need to use another algorithm (such as ECDSA), you can use that too, but be aware of the compatibility issues you might run into.

Note: This guide only covers generating keys using the RSA algorithm.

#### Key Size
For the key size, you need to select a bit length of at least 2048 when using RSA and 256 when using ECDSA; these are the smallest key sizes allowed for SSL certificates. Unless you need to use a larger key size, we recommend sticking with 2048 with RSA and 256 with ECDSA.

Note: In older versions of OpenSSL, if no key size is specified, the default key size of 512 is used. Any key size lower than 2048 is considered unsecure and should never be used.

#### Passphrase
For the passphrase, you need to decide whether you want to use one. If used, the private key will be encrypted using the specified encryption method, and it will be impossible to use without the passphrase. Because there are pros and cons with both options, it's important you understand the implications of using or not using a passphrase. In this guide, we will not be using a passphrase in our examples.
2. We need to generate private key

```
openssl genrsa -out ofaroque_private.key 2048 # use your own domain name instead of ofaroque_private  
```
3. Check the key 
```
cat ofaroque_private.key
```
and it should show something like: 

```
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAvEAcuYijBIQ8/P8I1K+SO45/qOhBVskGmRXhgMLZjuDKWJPD
5vZaA5faXJ+hB5q2zH1yEF0JInY4Jg6sq2l73shEMACBnXuZKjaw+DD2NpBZw9xI
/GiAJdAVnzItMKSb6A8scbHbKiVD/2b7tn3P5Xd+xvktOR0BWKxHH4QHaxJKI7z1
7lvfYgFsy56P3pF1AGpAbrplMCns0EkrTKybBQi7xq7k46mCjyeD+7oEFV4C37fS
jRZuJLP2FhBJf/BX*+d/g6ohXL+3HoI3yDBtJl0a/VzuhZ/P0QDpS0PphmDh4sFk
S6rdWmn7K9Asryfq5PCR56LseEzip1H6ZHs+PwIDAQABAoIBAQC5miB4EK5hWpvi
+PRU9hGgZJubBfq9vQ+jSX5+/B2SEDGQaYfhpIwVNNfXMI+Mts56CUi3t3K1Joev
hhJaInM9sIgCJ82jXmQhybBr06I9I89NG1hefA20uldHESqa5gDNKICyTCAAiqNj
Ib8VKzAZVtqJBM3AnMdiDOFGwl6qAbjhT7ekzCq1tweGmBTKWRPfBNwPsiDrrA9z
YxAnU0G3r5SC8YJ3IaWTwmSAPmmlI9cBAKGAZaEJ4mnS/fechhu+hvlrytmIqgAs
5O00K4XrkRz5wEcjYQJlqRYr8ngKz1uG/A58WLraGOVY2Wvb5juCUNFzSfRx1boQ
YzHsfogBAoGBtO8IFJ6fJdhhqEjwmK9yuIlaBIuM8WpTdbFlP4hiQGEUbR4Jvc6W
hD5PRiKxC+7sjptCCaTWPWJ3F0MLhBoxdRirAMeJItHQkxN4pmYfilYwZHRf7BIz
n4MKS2pwRZ0aOQ/GO+E1ib4svc5AocbfrD6IUYPTWfL2J4U/07VC2OQBAoGBAMmd
L7585ZQQ+DLaWt3M/YoUndnKAkBJrEizYF9sN8YxPtzYdQ6tEkMrl2rwwaevaCcB
ZEVQuoZwwU4GuaQ9rhTKyQMoOnO1wre9Ptrgpdb4q03gUDDlnz3RONWIzPTrEQCu
uewryNBG/H9yV+VmLr8y12sWy5m4v6KEBKI20yI/AoGBAMEcwyPfqcCmLUI4dvKP
+Xy5i+h+hvAC23vlM63oyuBjk0CIWDtWKSL6Asy2QtDVduUCNi5hE5jAZB+7Zw+O
U28JgIi0R1hBbQF3IOAyrR2y3QWUFXIjGMTShVlJuUQSUnVnDyuEiHMHTJUcbFby
kAK5OToKf4olyooBpfW0OuwBAoGARcrH6l1mRVOVvGqGM+Wf921orBjTrJCQ9lpx
xrI3nfAd8uVFov1Iq0riydJg/naXD5/QpJbo4mkSyL9U7bmViIYdBGWfy7cgltf4
blq+x2EkS/6HYFOiRA4mnuMFHtxR9q19kHQZt85HwIS8Wpd/JxIdLGUnrpkYXM9W
lxVm+ZsCgYEAhCflZ48QRtw5yYOkXupOYBbXvYDE1LzDfqorRzXcQ+MCA1jp1uZe
uPc/Sv8gI7AZEpGW2jv2iOiyAYF0dL06S4cB8MHNo/srTeJ+3dWMf9bz2Tpqi23k
hW9hkmm6IVqcJi2v98Tz0oucIe9tANrLu4Bd4t/uZZyLSP9uWR01ejA=
-----END RSA PRIVATE KEY-----
```
It is not a valid key, please dont waste energy to mess with it. 

4. Even though it looks like a random text, lets decode it and view the contents

```
openssl rsa -text -in ofaroque_private.key -noout
```
It will show something like: (PKCS#1 format, please have a look at here:https://crypto.stackexchange.com/questions/79604/private-exponent-on-rsa-key )

```
Private-Key: (2048 bit)
modulus:                         #  -- n
    ....
publicExponent: 65537 (0x10001)  #  -- e
privateExponent:                 #  -- d
    ....
prime1:                          #  -- p
    ....
prime2:                          #  -- q
    ....
exponent1:                       # -- d mod (p-1)
    ....
exponent2:                       # -- d mod (q-1)
    ....
coefficient:                     # -- (inverse of q) mod p
    ....

```
5. Extracting public key from that private key

```
openssl rsa -in ofaroque_private.key -pubout -out ofaroque_public.key

```

You can decode the public key by running the command below: 
```
openssl rsa -pubin -in ofaroque_public.pem -text
```
6. Now we have private key, public key, we need to make the CSR(Certificate signing request) file ready. We have to send this CSR file to a 
certificate authority(CA) to sign it. 

```
openssl req -new -key ofaroque_private.key -out ofaroque.csr

```
Even though we are using private key here, openssl is extracting the public key from the private key.

7. Look into the CSR file: 

```
cat ofaroque.csr
```
It will show something like: 

```
-----BEGIN CERTIFICATE REQUEST-----
MIICqDCCAZACAQAwTjELMAkGA1UEBhMCVVMxDjAMBgNVBAgMBVRleGFzMQ8wDQYD
....
....
AFzU76m94vfVaJsA
-----END CERTIFICATE REQUEST-----

```
