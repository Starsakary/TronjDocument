# Quickstart

Tronj is compiled by `jdk13.0.2+8`. 

The latest version is **0.1.1**.

## Installation

### Gradle Setting

Add repo setting:

```groovy
repositories {
    maven {
        url  "https://dl.bintray.com/tronj/tronj"
    }
}
```

Add java and application plugins:

```groovy
plugins {
    id 'java'
    id 'application'
}
```

Add required libraries as dependencies. 

```groovy
dependencies {
    // protobuf & grpc
    implementation 'com.google.protobuf:protobuf-java:3.11.0'

    implementation 'org.tron.tronj:abi:0.1.1'
    implementation 'org.tron.tronj:client:0.1.1'
    implementation 'org.tron.tronj:utils:0.1.1'

    implementation 'com.google.guava:guava:28.0-jre'
}
```

**Note:** This is the minimum gradle setting to work with Tronj.

### Maven Setting

Use maven repo setting from Bintray. The latest version is **0.1.1**.

```xml
<dependency>
  <groupId>org.tron.tronj</groupId>
  <artifactId>abi</artifactId>
  <version>0.1.1</version>
  <type>pom</type>
</dependency>
```

## Using Tronj

### Choose net from **main net**, **shasta** or **nile**

**Note**: Binding a private key is mainly for signing required operations. Any hex String conforming to the private key format can be put here if you only queries are needed.

```java
TronClient client = Tronclient.ofMainnet("your private key");
// TronClient client = Tronclient.ofShasta("your private key");
// TronClient client = Tronclient.ofNile("your private key");
```

### With private net

If using a private net, you may use another constructor: `TronClient(String grpcEndpoint, String grpcEndpointSolidity, String hexPrivateKey)`

```java
TronClient client = new TronClient("grpc endpoint", "solidity grpc endpoint", "your private key");
```

Aftre binding your private key with the `TronClient` object, your are ready to build, sign and broadcast your transactions.

## Signature

Tronj is signed with PGP. Verifying the public key between the `.jar` and `.asc` files to ensure the packages are from us.

### Verifying signature

To verify the signature, run `brew install gnupg` to install `gnupg` on your device.

From the [repository](https://dl.bintray.com/tronj/tronj/org/tron/tronj) download the .jar and the corresponding .asc file.

Run `gpg --keyserver keyserver.ubuntu.com --recv-key AD0876A4` to get the public key.

Taking the abi package as an example, run `gpg --verify abi-0.1.1.jar.asc abi-0.1.1.jar` to verify the signature.

Alternatively, you may import the public from below.

```pgp
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQGNBF/XiOcBDADUYu9eTQSN9RT86/oMdlEcC4euVrfO+GSXdimJlwXfjJJRYVPy
zJCRa6ANzIHsXC0x9KkdRCejEjQPafwnqGYat9lSSRx+EjjCqHKzUE4re6MiAHsH
l9TVGU9BIJQRm03UEK/oF9k9sEZrFgiMe9357p+JAeI4WXtCJMg/G6dRDR1YZlpz
3+krz0OFFvOe3yJLmoZPvBNoYI4zuIiOI0isQzqRgF0kleA5KLWRmCeQ8DBlrEUx
/cMKhrozmveGDvVHdYBshYC6msMcdv7OM5QyQCOlShcjqvkC79Q54pI1iq1pYT69
jXRWPV+AXLyPhIqfbFyBVT5I9dGSUeStaty/moSQ4JkhhjrzrUF/9yHYhi7ryLwC
68tVtdiZButk/djePhlyXp14wnOu4SbKSPyLkS0j9gmZiizbRZ7SgAAOhTtreGzL
eSvCmCw59A3KHXApiwQjMulZ4nd1ZVL5e2Td7Mx5ebvNjR0Fw80ti1/OfT0o7Vnn
nAWd/tBkYJDUT/cAEQEAAbQadHJvbmogPHRyb25qc2RrQGdtYWlsLmNvbT6JAdQE
EwEIAD4WIQR2wYLB4SafSUwjFf6oJdjW6LSQFgUCX9eI5wIbAwUJA8JnAAULCQgH
AgYVCgkICwIEFgIDAQIeAQIXgAAKCRCoJdjW6LSQFrKGC/0XeuhgEPJCTIPzoBhn
079nYZISpSFYrFL6JNwuB4x/3h55CnXAQR+5Pug0Ns9YyRPckoB/S9cD2Y6tNBGy
UHBgPKz1Knl+HshuBoyqqi/CUDsScI/MZ6oPwJ31iT+j5jtdref5nn0jtzpcX7cu
41GZJ33IE4e1Xq2W664kZ+Rnfobzed8OsPMFJu/9GG7XwxAW9TLkSeiZdjI3b2KH
0omoN+d9lOx1F3GlU0BJP1n5d/xUI+B7Uu0v5aozUu2q92QexPSLuqq+uiTf30X8
81NCzKcvi2x/yIX5u3R1cMKuR3KXPO/N3zYYl45+0/E4d78XFmwgfxwk+7XgkN4d
6TjTa4CqI2vjhOOUCNaJ0bPMhYi9Btsxm0Sy2xmGJBdx1J1ixc5zY8il2IYkFbLc
Md4x9ejs2gmEGWXkxCYGxBcK/dfiX/U+/z8vP7ImhlhjvDS1Q2hAxJsJ+Mnss4Gw
J0BlkCNrRzsvoq8e8m0xT4H/ePnbRtkArZLPQpkwL+5Xzfi5AY0EX9eI5wEMAN5/
i5Bv/TKgbm80DAGsXXXa3YUAaDJcS3SO/NeRR9mmKR9NUAfIYQhV6//d+FTrDasa
63NIaCPcKe5ocogVCY2Qb/oenRL1qV6XcvyxkNSMcJhyrPzY0v6bSatUU9yjLzre
wb8gD4AJT9tGmbpFtvkYA6ndQ0yucPKXvp86WAnuo5Lvn+90SJUzwDySEMsXvDSF
201t8z7t7HuSM2sI2swIJkCT4x1zS/LBkSZPtP46aP1ShOKd6/L+3KZ5ikfr6351
FghyVScjkV33Uw4DB8JJRRTvdj8HxahMeNxFYKWCMYDQ5sk00WOfAy9M/8mjqrh6
0oiQQIxkxN3Vmc9pseORpaXLIJymRJT78jI0QglINsiuZfiYqKfR6ka1VJXu9sQJ
ZbxgmuL7T7HmTMLZK6f4uwUDX38Udu7CLEhJuNttOq8l1KHoraXf2AtLHyta1MYQ
5goB8fGPbFf6ehqQLIjLq+EZLIauN/BZHd2mV9U3/PTq+KoscOsiSzewfuj5jwAR
AQABiQG8BBgBCAAmFiEEdsGCweEmn0lMIxX+qCXY1ui0kBYFAl/XiOcCGwwFCQPC
ZwAACgkQqCXY1ui0kBauFgwAiq7DPpZGvD1ubmLlATq0fCZbQc7wRiR+Y4EKVbW+
QWsOYLXMgObbxXMwrk/u0wqyFnIrwt8Vxb7go3ciiw6FihJWRV0aX2JsmQdj4l3t
OU5GYnp4oVhLga4CgYgXjMLvB+mOmm0lTN8JzVPFcNi6lEA+NLu5LlU/rYlyVoFq
bjx0phswUKhLFk2wDnu2ndOrnmL1p21+4Byd+bupXhK3kFGmaBfJdUhgt5L6tTuZ
qjeCXfYtAJrPAd4D0VVL7JbusPI9aYpZGW8vErDJPfvD+FhkXJGxbZ0AIpb7x7AX
Wvh6Yw6q48n00CAUIut2yWOn0C27YKXQv1G0vmvbjsVP9OxK0VQnZd648Bm8U98o
g/mQ6FNOVMzFjRk4iWrPl9ExL6RYfw2O+ZeIPTuXngxLJ8erX9i3lclacsiIj74W
XKGcDnvCACJdJd8DWbiY8OQiNfONye44WD1lTr6f+y3swhnryIl9Hk2Vb1xofWre
hjMZIEg6d5XYFLRf1iaVG+TD
=OjmN
-----END PGP PUBLIC KEY BLOCK-----
```



## Refer to Javadocs

Tronj has javadocs for essential classes and functions, you may generate Javadocs for a specific package to read more details.