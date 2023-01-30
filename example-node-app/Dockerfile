# syntax=docker/dockerfile:1.4
FROM node:16.14.2-bullseye AS workspace

WORKDIR /app
COPY --link package.json yarn.lock .yarnrc .npmrc ./
RUN --mount=type=secret,id=NPMRC,required=true,target=/root/.npmrc \
  --mount=type=cache,id=yarn-cache,target=/mnt/yarn-cache \
  --mount=type=cache,id=npm-cache,target=/root/.npm \
  yarn install --frozen-lockfile --cache-folder /mnt/yarn-cache
COPY --link . .

FROM workspace AS execute-tests
RUN mkdir -p reports && (find reports && yarn test --ci --watchAll=false 2>&1 && touch reports/unit-tests.passed && find reports) | tee reports/unit-tests.txt

FROM execute-tests AS passing-tests
RUN test -f reports/unit-tests.passed || (cat reports/unit-tests.txt; exit 1)

FROM workspace AS build-html
RUN yarn build

FROM passing-tests AS verified-build
COPY --link --from=build-html /app/build /app/build

FROM scratch AS html-artifact
COPY --link --from=verified-build /app/build /

FROM scratch AS report-artifacts
COPY --link --from=execute-tests /app/reports /

FROM nginx:1.23.3-alpine AS web-server
COPY --link --from=verified-build /app/build/ /usr/share/nginx/html/
RUN find /usr/share/nginx/html/ -name '*.html' | xargs gzip -k9
RUN nginx -T

