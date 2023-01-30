# Example Maven project

This directory contains an out-of-the-box Spring Boot app from [Spring
Initializr](https://start.spring.io/), and a fully fleshed `Dockerfile` which
builds an Nginx image containing running the app.

If you compare this with the example Node app, we don't do a separate step where
we download dependencies, since Maven downloads dependencies automatically when
they are needed. Also, since Maven dependencies (unlike NPM packages) don't need
to be stored in a repo-local directory and don't perform any post-download build
steps etc, just using a mounted cache folder for the `~/.m2` directory will
be the only optimization we need.

You can download the dependencies separately by invoking [`./mvnw
dependency:go-offline`](https://maven.apache.org/plugins/maven-dependency-plugin/go-offline-mojo.html)
but there's less need for it (although it could be nice just to see that
everything downloads properly). If you decide to do so, remember that every `RUN`
step that invokes `./mvnw` needs to mount the `~/.m2` cache directory in the
same way.

- **To build the app:** \
  `docker build  . --target=app` \
  ... then start a container: \
  `docker run --rm -p8080:8080 sha256:...` \
  The app is now running on http://localhost:8080 (where it responds to any
  request with a 404 page)
- **To run the tests and extract the test reports:** \
  `docker build . --target=test-reports --output type=local,dest=test-reports` \
  Test reports are now available in the `./test-reports/` directory, regardless
  of whether tests passed or not.
