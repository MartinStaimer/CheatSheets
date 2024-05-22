Generate self signed certificate
```bash

openssl genrsa -aes256 -passout pass:gsahdg -out server.pass.key 4096


openssl rsa -passin pass:gsahdg -in server.pass.key -out server.key

rm server.pass.key


openssl req -new -key server.key -out server.csr

openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt


```


To .pem for axum::server
```bash

openssl x509 -in server.crt -out server.pem
openssl rsa -in server.key -text > key.pem

```


