# syntax=docker/dockerfile:1.4

# Clever hack: Empty stage to avoid repeating the identifier for an image you
# want to use several times
FROM eclipse-temurin:17-jdk-alpine AS jdk

FROM jdk AS attempted-build
WORKDIR /repo
COPY --link . .
RUN --mount=type=cache,id=mvn-cache,target=/root/.m2 if ./mvnw install ; then touch BUILD_PASSED; fi

FROM scratch AS test-reports
COPY --from=attempted-build /repo/target/surefire-reports /

FROM attempted-build AS successful-build
RUN <<EOF
  set -exuo pipefail
  test -f BUILD_PASSED
  mkdir /app
  cp target/demo-*.jar /app/app.jar
  cp target/classes/application.properties /app
  find target
EOF

FROM jdk AS app
COPY --link --from=successful-build /app /app
WORKDIR /app
EXPOSE 8080
ENTRYPOINT [ "java", "-jar", "app.jar" ]
