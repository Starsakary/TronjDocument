# Quickstart

## Installation

### Gradle Setting

Add repo setting:

```groovy
repositories {
    maven {
        url  "https://dl.bintray.com/starsakary/tronj"
    }
}
```

Then add `abi` as dependency.

```groovy
dependencies {
    ....

    implementation 'org.tron.tronj:abi:0.4.0'
    implementation 'org.tron.tronj:client:0.4.0'

    ....
}
```

###Maven Settings

Use maven repo setting from Bintray.

```xml
<dependency>
  <groupId>org.tron.tronj</groupId>
  <artifactId>abi</artifactId>
  <version>0.4.0</version>
  <type>pom</type>
</dependency>
```

## Using Tronj

### Choose net from **main net**, **shasta** or **nile**

```java
TronClient client = ofMainnet("your private key");
// TronClient client = ofShasta("your private key");
// TronClient client = ofNile("your private key");
```

Aftre binding your private key with the `TronClient` object, your are ready to build, sign and broadcast your transactions.



## Refer to Javadocs

Tronj has javadocs for essential classes and functions, you may use `gradle javadoc` to generate Javadocs for a specific package to read more details.