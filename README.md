# Log4Shell sample vulnerable application (CVE-2021-44228)

This repository contains a Spring Boot web application vulnerable to CVE-2021-44228, nicknamed [Log4Shell](https://www.lunasec.io/docs/blog/log4j-zero-day/). It uses Log4j 2.14.1 (through `spring-boot-starter-log4j2` 2.6.1) and the JDK 1.8.0_181.

This repo is the source for the container image `quay.io/kacti/log4shell`. It's used by the `kacti` CLI to functionally test Kubernetes admission control. e.g.
```
kacti trials --cve CVE-2021-44228
```
If the admission controller blocks this image, then it is correctly configured to block Log4Shell-vulnerable applications.

This application was originally created by [Christophe Tafani-Dereeper](https://github.com/christophetd) and the original repository is available [here](https://github.com/christophetd/log4shell-vulnerable-app).

## Building the container image
You can use `podman` to build this container image from the directory root:
```
podman build -t quay.io/kacti/log4shell:v0.1 .
```

## Sigstore signatures

The container images created from this repo are signed with a Sigstore signature, with the entries recorded in the public-good [Rekor](https://sigstore.dev/rekor) instance. You can use `cosign` to verify the signature on the image:
```
$ crane digest quay.io/kacti/log4shell:v0.1
sha256:f72efa1cb3533220212bc49716a4693b448106b84ca259c20422ab387972eed9

$ cosign verify --certificate-identity shane.boulden@gmail.com --certificate-oidc-issuer https://github.com/login/oauth quay.io/kacti/log4shell@sha256:f72efa1cb3533220212bc49716a4693b448106b84ca259c20422ab387972eed9

Verification for quay.io/kacti/log4shell@sha256:f72efa1cb3533220212bc49716a4693b448106b84ca259c20422ab387972eed9 --
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - Existence of the claims in the transparency log was verified offline
  - The code-signing certificate was verified using trusted certificate authority certificates
```

## Reference

https://www.lunasec.io/docs/blog/log4j-zero-day/
https://mbechler.github.io/2021/12/10/PSA_Log4Shell_JNDI_Injection/

## Contributors

[@christophetd](https://twitter.com/christophetd)
[@rayhan0x01](https://twitter.com/rayhan0x01)
