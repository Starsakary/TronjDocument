# Quickstart

Tronj is compiled by `jdk13.0.2+8`. 

The latest version is **0.1.0**.

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

    implementation 'org.tron.tronj:abi:0.1.0'
    implementation 'org.tron.tronj:client:0.1.0'
    implementation 'org.tron.tronj:utils:0.1.0'

    implementation 'com.google.guava:guava:28.0-jre'
}
```

**Note:** This is the minimum gradle setting to work with Tronj.

### Maven Setting

Use maven repo setting from Bintray. The latest version is **0.1.0**.

```xml
<dependency>
  <groupId>org.tron.tronj</groupId>
  <artifactId>abi</artifactId>
  <version>0.1.0</version>
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



## Refer to Javadocs

Tronj has javadocs for essential classes and functions, you may use `gradle javadoc` to generate Javadocs for a specific package to read more details.