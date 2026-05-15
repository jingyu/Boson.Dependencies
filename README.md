# Boson Dependencies BOM

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-17%2B-orange.svg)](https://adoptium.net/)
[![Maven](https://img.shields.io/badge/Maven-3.8%2B-red.svg)](https://maven.apache.org/)

The Boson Dependencies BOM (Bill of Materials) is the single source of truth for dependency versions across all [Boson Network](https://github.com/bosonnetwork) Java projects. It pins every third-party library and every Boson artifact to an exact version so that all modules build against a consistent, reproducible dependency set.

---

## What This Repository Provides

This repository contains a single `pom.xml` with `<packaging>pom</packaging>`. It does **not** contain any source code.

Its purpose is to:

- Pin **exact versions** of all third-party libraries used by the Boson ecosystem.
- Declare **managed versions** of all Boson artifacts so child modules can depend on them without repeating version numbers.
- Import widely-used upstream BOMs (Vert.x, Netty, Jackson, Micrometer, JUnit, Testcontainers) so their internal dependency alignment is preserved.

---

## Managed Boson Artifacts

| Artifact ID | Description |
|---|---|
| `boson-api` | Core API and crypto primitives |
| `boson-dht` | Secure Kademlia DHT implementation (KadNode) |
| `boson-dht-shell` | Interactive developer shell for the DHT |
| `boson-web-gateway` | WebGateway layer-2 service (super node side) |
| `boson-ion-store` | Ion Store layer-2 service |
| `boson-active-proxy` | Active Proxy layer-2 service |
| `boson-messaging` | Photon Messaging layer-2 service |
| `higgs-java` | HiggsNode — light DHT node / WebGateway client |
| `boson-active-proxy-client` | Active Proxy client library |
| `boson-messaging-client` | Photon Messaging client library |
| `boson-director` | Director service |

All Boson artifacts share the same version as the BOM itself (`${project.version}`).

----

## Using the BOM

Import the BOM inside the `<dependencyManagement>` block of any Boson subproject. After the import, all managed dependencies and Boson artifacts can be declared without a `<version>` tag.

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.bosonnetwork</groupId>
            <artifactId>boson-dependencies</artifactId>
            <version>${boson.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

After importing, declare dependencies without versions:

```xml
<dependencies>
    <dependency>
        <groupId>io.bosonnetwork</groupId>
        <artifactId>boson-api</artifactId>
    </dependency>
    <dependency>
        <groupId>io.vertx</groupId>
        <artifactId>vertx-core</artifactId>
    </dependency>
</dependencies>
```

The BOM must be installed into the local Maven repository before child modules can resolve it. See [Build & Install](#build--install) below.

---

## Build & Install

### Prerequisites

| Requirement | Version |
|---|---|
| Java JDK | 17 or later |
| Apache Maven | 3.8 or later |

### Clone

```bash
git clone https://github.com/bosonnetwork/Boson.Dependencies.git
cd Boson.Dependencies
```

### Install to local Maven repository

```bash
./mvnw install
```

This installs `boson-dependencies-<version>.pom` into `~/.m2/repository/io/bosonnetwork/boson-dependencies/` so child projects can resolve it locally during development.

---

## Updating Dependency Versions

To upgrade a managed dependency:

1. Update the corresponding `<property>` in `pom.xml`.
2. Run `./mvnw verify` to confirm the POM is well-formed.
3. If the change is significant, bump the BOM's own version.
4. Reinstall locally with `./mvnw install` and update all child projects to reference the new BOM version.

Check each library's release page for the latest stable version before upgrading. Pay particular attention to the JNR-FFI / Tuweni interaction — always verify that libsodium loads correctly after a Tuweni upgrade.

---

## Contributing

We welcome contributions from the open-source community. To get started:

1. Fork this repository and create a feature branch.
2. Make your changes.
3. Ensure `./mvnw verify` passes.
4. Open a pull request with a clear description of the change.

Please read our [Code of Conduct](CODE_OF_CONDUCT.md) before contributing.

---

## License

This project is licensed under the [MIT License](LICENSE).