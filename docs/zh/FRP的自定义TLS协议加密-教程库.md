反向代理知识:FRP的自定义 TLS 协议加密：TLS 默认开启方式、frpc 单向校验 frps 身份、frps 单向验证 frpc 的身份、双向验证、OpenSSL 生成证书示例。

<img src="images/seo-00004_9f0c53207b2d4fe48e254ceb7a801090.png" class="jop-noMdConv">

`use_encryption` 和 `STCP` 等功能能有效防止流量内容在通信过程中被盗取，但是无法判断对方的身份是否合法，存在被中间人攻击的威胁。为此 frp 支持 frpc 和 frps 之间的流量通过 TLS 协议加密，并且支持客户端或服务端单向验证，双向验证等功能。

当 `frpc.ini` 的 `common` 中 `tls_enable = true` 时，表示开启 TLS 协议加密。

当 `frps.ini` 的 `common` 中 `tls_only = true` 时，表示 server 端只接受 TLS 连接的客户端，这也是 frps 验证 frpc 身份的前提条件。如果 `frps.ini` 的 `common` 中 `tls_trusted_ca_file` 内容是有效的话，那么默认就会开启 `tls_only = true`。

**注意：启用此功能后除 xtcp 外，可以不用再设置 use_encryption 重复加密**

### TLS 默认开启方式

```
# frpc.ini
[common]
tls_enable = true 
```

frpc 开启 TLS 加密功能，但是默认不校验 frps 的证书。

### frpc 单向校验 frps 身份

```
# frpc.ini
[common]
tls_enable = true
tls_trusted_ca_file = /to/ca/path/ca.crt

# frps.ini
[common]
tls_cert_file = /to/cert/path/server.crt
tls_key_file = /to/key/path/server.key 
```

frpc 需要额外加载 ca 证书，frps 需要额外指定 TLS 配置。frpc 通过 ca 证书单向验证 frps 的身份。这就要求 frps 的 `server.crt` 对 frpc 的 ca 是合法的。

合法: 如果证书是 ca 签发的，或者证书是在 ca 的信任链中，那即认为: 该证书对 ca 而言是合法的。

### frps 单向验证 frpc 的身份

```
# frpc.ini
[common]
tls_enable = true
tls_cert_file = /to/cert/path/client.crt
tls_key_file = /to/key/path/client.key

# frps.ini
[common]
tls_trusted_ca_file = /to/ca/path/ca.crt 
```

frpc 需要额外加载 TLS 配置，frps 需要额外加载 ca 证书。frps 通过 ca 证书单向验证 frpc 的身份。这就要求 frpc 的 `client.crt` 对 frps 的 ca 是合法的。

### 双向验证

```
# frpc.ini
[common]
tls_enable = true
tls_cert_file = /to/cert/path/client.crt
tls_key_file = /to/key/path/client.key
tls_trusted_ca_file = /to/ca/path/ca.crt

# frps.ini
[common]
tls_cert_file = /to/cert/path/server.crt
tls_key_file = /to/key/path/server.key
tls_trusted_ca_file = /to/ca/path/ca.crt 
```

双向验证即 frpc 和 frps 通过本地 ca 证书去验证对方的身份。理论上 frpc 和 frps 的 ca 证书可以不同，只要能验证对方身份即可。

### OpenSSL 生成证书示例

```
x509: certificate relies on legacy Common Name field, use SANs or temporarily enable Common Name matching with GODEBUG=x509ignoreCN=0
```

如果出现上述报错，是因为 go 1.15 版本开始废弃 CommonName，因此推荐使用 SAN 证书。 如果想兼容之前的方式，需要设置环境变量 **GODEBUG** 为 `x509ignoreCN=0`。

下面简单示例如何用 openssl 生成 ca 和双方 SAN 证书。

准备默认 OpenSSL 配置文件于当前目录。此配置文件在 linux 系统下通常位于 `/etc/pki/tls/openssl.cnf`，在 mac 系统下通常位于 `/System/Library/OpenSSL/openssl.cnf`。

如果存在，则直接拷贝到当前目录，例如 `cp /etc/pki/tls/openssl.cnf ./my-openssl.cnf`。如果不存在可以使用下面的命令来创建。

```
cat > my-openssl.cnf << EOF
[ ca ]
default_ca = CA_default
[ CA_default ]
x509_extensions = usr_cert
[ req ]
default_bits        = 2048
default_md          = sha256
default_keyfile     = privkey.pem
distinguished_name  = req_distinguished_name
attributes          = req_attributes
x509_extensions     = v3_ca
string_mask         = utf8only
[ req_distinguished_name ]
[ req_attributes ]
[ usr_cert ]
basicConstraints       = CA:FALSE
nsComment              = "OpenSSL Generated Certificate"
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid,issuer
[ v3_ca ]
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints       = CA:true
EOF 
```

生成默认 ca:

```
openssl genrsa -out ca.key 2048
openssl req -x509 -new -nodes -key ca.key -subj "/CN=example.ca.com" -days 5000 -out ca.crt 
```

生成 frps 证书:

```
openssl genrsa -out server.key 2048

openssl req -new -sha256 -key server.key 
    -subj "/C=XX/ST=DEFAULT/L=DEFAULT/O=DEFAULT/CN=server.com" 
    -reqexts SAN 
    -config <(cat my-openssl.cnf <(printf "n[SAN]nsubjectAltName=DNS:localhost,IP:127.0.0.1,DNS:example.server.com")) 
    -out server.csr

openssl x509 -req -days 365 
    -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial 
    -extfile <(printf "subjectAltName=DNS:localhost,IP:127.0.0.1,DNS:example.server.com") 
    -out server.crt 
```

生成 frpc 的证书:

```
openssl genrsa -out client.key 2048
openssl req -new -sha256 -key client.key 
    -subj "/C=XX/ST=DEFAULT/L=DEFAULT/O=DEFAULT/CN=client.com" 
    -reqexts SAN 
    -config <(cat my-openssl.cnf <(printf "n[SAN]nsubjectAltName=DNS:client.com,DNS:example.client.com")) 
    -out client.csr

openssl x509 -req -days 365 
    -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial 
    -extfile <(printf "subjectAltName=DNS:client.com,DNS:example.client.com") 
    -out client.crt 
```

在本例中，server.crt 和 client.crt 都是由默认 ca 签发的，因此他们对默认 ca 是合法的。