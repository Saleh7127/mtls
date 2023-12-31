Mutual TLS (mTLS) is a mutual authentication mechanism. It assures that the parties at every end of a network connection are who they claim to be. 
This assurance is established by validating their private keys with additional verification being done by the information contained in their separate TLS certificates. 
mTLS is frequently used in a Zero Trust security framework to validate people, connections, and servers within an enterprise.

Run these command in **bash**
#### Generating our rootCA file.
- `openssl req -newkey rsa:2048 -nodes -x509 -days 365 -out ca.crt -keyout ca.key -subj "/C=CN/ST=GD/L=SZ/O=Acme, Inc./CN=Acme Root CA"
  `
#### Generating server certificate.
- `openssl genrsa -out server.key 2048`
- `openssl req -new -key server.key -out server.csr -subj "/C=CN/ST=GD/L=SZ/O=Acme, Inc./CN=localhost"`
- `openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost,DNS:localhost") -days 365 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt`

#### Generating the client certificate.
- `openssl genrsa -out client.key 2048`
- `openssl req -new -key client.key -out client.csr -subj "/C=CN/ST=GD/L=SZ/O=Acme, Inc./CN=localhost"`
- `openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost,DNS:localhost") -in client.csr -CA ca.crt -CAkey ca.key -out client.crt -days 365 -sha256 -CAcreateserial`

### Resource
[How to MTLS in golang](https://kofo.dev/how-to-mtls-in-golang)

[mTLS Golang Example](https://github.com/haoel/mTLS)