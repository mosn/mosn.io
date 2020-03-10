---
title: "Custom configurations"
linkTitle: "Custom configurations"
weight: 3
date: 2020-01-20
description: >
  The custom configurations of MOSN.
---

This topic describes custom configurations of MOSN.

### Duration String

- A string that consists of a decimal digit and a time unit suffix. Valid time units: `ns`, `us` (or `µs`), `ms`, `s`, `m`, and `h`. For example, `1 h`, `3s`, and `500 ms`.

### metadata

`metadata` is used for matching MOSN routes and cluster hosts.

```json
{
  "filter_metadata":{
    "mosn.lb":{}
  }
}
```

`mosn.lb` corresponds to any `string-string` content.

### tls_context

```json
{
  "status":"",
  "type":"",
  "server_name":"",
  "ca_cert":"",
  "cert_chain":"",
  "private_key":"",
  "verify_client":"",
  "require_client_cert":"",
  "insecure_skip":"",
  "cipher_suites":"",
  "ecdh_curves":"",
  "min_version":"",
  "max_version":"",
  "alpn":"",
  "fall_back":"",
  "extend_verify":"",
  "sds_source":{}
}
```

- `status`: Boolean. Indicates whether TLS is enabled. Default value: false.
- `type`: String. Specifies the type of tls_context. tls_context supports extension implementation. Different types correspond to different implementation methods. Default value:"" (empty string).
- `server_name`: Used to verify the hostname of the certificate returned by the server when insecure_skip is not configured. Valid when configured at a cluster.
- `ca_cert`: The root certificate issued by a trusted certificate authority (CA).
- `cert_chain`: The TLS certificate chain.
- `private_key`: The private key of a certificate.
- `verify_client`: Boolean. Specifies whether to verify a client certificate. Valid when configured at a listener.
- `require_client_cert`: Boolean. Specifies whether the client certificate is required.
- `insecure_skip`: Boolean. Specifies whether to skip server certificate verification. Valid when configured at a cluster.
- `cipher_suites`: Specifies the cipher suites to be supported by TLS connections. If this parameter is specified, TLS connections support only the specified cipher suites and use them according to the order of how they are specified. Separate different cipher suites with a comma. Valid values:

```
ECDHE-ECDSA-AES256-GCM-SHA384
ECDHE-RSA-AES256-GCM-SHA384
ECDHE-ECDSA-AES128-GCM-SHA256
ECDHE-RSA-AES128-GCM-SHA256
ECDHE-ECDSA-WITH-CHACHA20-POLY1305
ECDHE-RSA-WITH-CHACHA20-POLY1305
ECDHE-RSA-AES256-CBC-SHA
ECDHE-RSA-AES128-CBC-SHA
ECDHE-ECDSA-AES256-CBC-SHA
ECDHE-ECDSA-AES128-CBC-SHA
RSA-AES256-CBC-SHA
RSA-AES128-CBC-SHA
ECDHE-RSA-3DES-EDE-CBC-SHA
RSA-3DES-EDE-CBC-SHA
ECDHE-RSA-SM4-SM3
ECDHE-ECDSA-SM4-SM3
```

- `ecdh_curves`: If this parameter is used, TLS connections support only the specified curves.

- - Valid values: x25519, p256, p384, and p521.

- `min_version`: The earliest TLS version supported.

- - Valid values: TLS1.0, TLS1.1, and TLS1.2. Default value: TLS1.0.
   - Available TLS versions will be automatically identified by default.

- `max_version`: The latest TLS version supported.

- - Valid values: TLS1.0, TLS1.1, and TLS1.2. Default value: TLS1.2.
   - Available TLS versions will be automatically identified by default.

- `alpn`: Specifies the protocol supported by ALPN on TLS connections.

- - Valid values: H2, HTTP/1.1, and SOFA.

- `fall_back`: Boolean. Specifies whether to fall back when certificate verification fails, without returning an error. This is equivalent to the case when TLS is not enabled. Valid values: True and False.

- `extend_verify`: JSON. Specifies the extension of tls_context when type is not empty.

- `sds_source`: Specifies parameters required for accessing the SDS API. If sds_source is configured, the `ca_cert`, `cert_chain`, and `private_key` parameters will be ignored, but other configurations will remain valid.

### sds_source

```json
{
  "CertificateConfig":{},
  "ValidationConfig":{}
}
```

- `CertificateConfig`: Specifies how to obtain the values of cert_chain and private_key.
- `ValidationConfig`: Specifies how to obtain the value of `ca_cert`.
- For details about the configurations, see [envoy: sds_secret_config](https://www.envoyproxy.io/docs/envoy/latest/api-v2/api/v2/auth/cert.proto#envoy-api-msg-auth-sdssecretconfig).

