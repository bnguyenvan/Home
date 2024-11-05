## Generation of a Self Signed Certificate
Generation of a self-signed SSL certificate involves a simple 3-step procedure:

### On Windows

__STEP 1__: Create the server private key
```cmd
"C:\Program Files\Git\usr\bin\openssl.exe" genrsa -out cert.key 2048
```

__STEP 2__: Create the certificate signing request (CSR)
```cmd
"C:\Program Files\Git\usr\bin\openssl.exe" req -new -key cert.key -out cert.csr
```
Output:
```cmd
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:VN
State or Province Name (full name) [Some-State]:Ho-Chi-Minh
Locality Name (eg, city) []:Ho-Chi-Minh
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Duc Loi 6
Organizational Unit Name (eg, section) []:Pharmacy
Common Name (e.g. server FQDN or YOUR name) []:books.homelab
Email Address []:microwave88@gmail.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:Enter
An optional company name []:Enter
```

__STEP 3__: Sign the certificate using the private key and CSR

```cmd
"C:\Program Files\Git\usr\bin\openssl.exe" x509 -req -days 3650 -in cert.csr -signkey cert.key -out cert.crt
```

### On Linux

__STEP 1__: Create the server private key
```sh
openssl genrsa -out cert.key 2048
```
__STEP 2__: Create the certificate signing request (CSR)
```sh
openssl req -new -key cert.key -out cert.csr
```
__STEP 3__: Sign the certificate using the private key and CSR
```sh
openssl x509 -req -days 3650 -in cert.csr -signkey cert.key -out cert.crt
```
Congratulations! You now have a self-signed SSL certificate valid for 10 years.