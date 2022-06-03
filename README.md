# EOSC Future Data Transfer

This project builds a proxy component that can initiate data transfers to multiple
storage backends. Initially, [EGI DataHub Service](https://docs.egi.eu/users/data/management/datahub/)
will be supported.

A space in EGI DataHub is just a hierarchical structure of containers (folders) and objects (files).
Publishing a space to an IDS connector means creating a catalog for the space, then in that
catalog create a resource for each file. The adapter will not create resources for folders.

The resources created by the adapter will contain the following custom properties:

- The unique identifier of the file in the OneData backend
- The path of the file in the space, which allows consumers to reconstruct the
  hierarchical structure if they wish to do so

The adapter exposes a REST API through which users can publish data to a configured provider connector.
When the application is running, the API is available at `http://localhost:8080/api`. There is also a
Swagger UI available at `http://localhost:8080/api/doc`.

The adapter endpoint `http://localhost:8080/api/action/publish` will take the passed in space ID
and will create a catalog for it in the connector. For each file in that space it will also create
a new resource in the connector, together with all required entities, so that (a representation of) the
resource can be discovered and consumed (downloaded) by another (consumer) IDS connector.

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io.

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```shell script
./mvnw compile quarkus:dev
```

> **_NOTE:_**  Quarkus now ships with a Dev UI, which is available in dev mode only at http://localhost:8080/q/dev/.

## Packaging and running the application

The application can be packaged using:
```shell script
./mvnw package
```
It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/quarkus-app/lib/` directory.

The application is now runnable using `java -jar target/quarkus-app/quarkus-run.jar`.

If you want to build an _über-jar_, execute the following command:
```shell script
./mvnw package -Dquarkus.package.type=uber-jar
```

The application, packaged as an _über-jar_, is now runnable using `java -jar target/*-runner.jar`.

## Creating a native executable

You can create a native executable using: 
```shell script
./mvnw package -Pnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: 
```shell script
./mvnw package -Pnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/data-transfer-1.0-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/maven-tooling.

## Related Guides

