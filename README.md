# Example Cert Generation

Example generation of PKI certificates using `openssl` commandline utility vs. certificate generation using `receptor` facilities. Includes example usage of generated certificates by receptor to prove that both achieve the same result.

```
.
├── openssl
│   ├── config_receptor
│   │   ├── n1.yml
│   │   └── n2.yml
│   └── Makefile
├── README.md
└── receptor
    ├── config_certs
    │   ├── cert_init.yml
    │   ├── cert_request2.yml
    │   ├── cert_request.yml
    │   ├── cert_sign2.yml
    │   └── cert_sign.yml
    ├── config_nodes
    │   ├── n1.yml
    │   └── n2.yml
    └── Makefile
```

`openssl`: Certificate generation using openssl.
`receptor`: Certificate generation using receptor.


## Quick Start

openssl

```
cd openssl
make
make n1
make n2
```

receptor

```
cd receptor
make
make n1
make n2
```
