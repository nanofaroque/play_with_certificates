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
* Generating those paramerter above, please have a look: 
https://www.onebigfluke.com/2013/11/public-key-crypto-math-explained.html

* To understand multiplicative inverse: 
https://www.youtube.com/watch?v=YwaQ4m1eHQo&ab_channel=NesoAcademy

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
8. Lets look inside of the CSR file and what it contains

```
openssl req -in ofaroque.csr.csr -noout -text
```
It will show something like: 

```
Certificate Request:
    Data:
        Version: 0 (0x0)
        Subject: C=US, ST=Texas, L=Frisco, O=Omar, OU=Ops/emailAddress=faroque_eee@yahoo.com
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:bc:e0:f7:23:b5:9e:d6:52:e4:37:b2:b8:b8:97:
                    4f:e5:66:f7:fb:fb:3f:71:f4:fe:79:91:96:60:84:
                    43:a9:30:f2:b5:ab:86:3f:a4:a8:52:41:dd:39:ba:
                    10:77:67:bb:38:3f:a3:e6:2c:10:7a:e3:50:f4:13:
                    e3:4f:5d:6f:5b:16:68:4c:2d:15:f3:fa:59:d5:c9:
                    fe:16:ae:f5:91:35:71:bc:17:46:53:92:0b:50:d8:
                    8e:68:0b:12:68:7f:ec:05:01:bf:13:bd:1e:76:58:
                    f0:e0:ee:76:b9:96:f5:e6:ff:a7:eb:b3:3c:fa:69:
                    63:14:80:82:4d:81:12:56:f5:97:ac:99:20:51:72:
                    32:94:9e:4b:8d:99:e9:e0:3a:df:8a:63:2a:04:78:
                    f2:24:12:5e:b8:9b:e5:e2:39:70:af:82:c1:7a:83:
                    78:2a:1e:e3:bf:cf:2c:b6:b3:a5:33:ae:0e:91:81:
                    86:17:0a:37:ac:d7:5e:69:08:ef:76:6e:b4:38:d2:
                    fc:ba:bf:28:81:43:81:4d:92:90:b6:c6:09:c2:10:
                    ef:81:12:b1:75:e2:9f:06:1c:be:0f:99:76:17:8a:
                    bd:7a:6f:8a:b2:f1:5e:db:b7:84:85:be:e3:54:94:
                    32:7c:e6:ae:a6:ca:65:17:1b:0a:a9:ee:f1:19:79:
                    2c:0f
                Exponent: 65537 (0x10001)
        Attributes:
            challengePassword        :unable to print attribute
    Signature Algorithm: sha256WithRSAEncryption
         9a:71:79:5f:d7:10:39:ae:87:60:3f:1a:8f:70:52:4d:1b:11:
         b4:da:25:47:33:80:11:16:d9:bd:55:06:b8:86:af:9a:5c:c9:
         b8:f9:75:a2:ce:a7:ea:3d:7a:36:44:6b:a3:94:74:b3:2c:4e:
         5c:9d:7a:6f:48:89:09:8b:f3:8f:a3:36:1e:58:f9:0b:a2:9c:
         df:45:02:8f:86:21:8b:64:06:6c:62:37:c7:8f:3c:32:a0:45:
         f4:3c:92:47:88:94:4c:70:01:2a:42:b3:f5:f9:88:3d:4f:3e:
         c0:93:1d:8b:2c:9e:61:0d:09:6e:03:ba:37:36:ee:63:ef:db:
         2f:7f:38:f6:15:8c:e2:3b:f1:75:88:63:65:62:c1:25:66:7d:
         cd:c7:34:22:f8:a6:8f:30:41:89:2e:cb:ab:e9:d8:76:29:ed:
         63:9b:9a:05:98:99:65:f9:dc:39:a0:35:f7:55:3e:1c:06:f3:
         fc:b4:28:a3:b2:bd:e0:53:8f:71:6f:03:81:8f:74:21:13:5b:
         17:5a:d1:d7:9f:f4:c3:ca:58:79:17:5c:da:40:a4:d4:18:fb:
         a2:5c:6e:7d:0c:11:92:62:54:0d:1f:bb:13:6f:75:d6:ec:88:
         6c:d2:70:1d:13:3f:eb:08:a8:61:b5:c1:b8:be:57:79:6e:e8:
         c2:b2:fc:76
```
