FROM node:16 as base

WORKDIR /usr/src/app

COPY package.json package.json
COPY package-lock.json package-lock.json

FROM base AS test-stage

RUN npm ci

COPY . .

ENV CI=true

RUN npm run test 

FROM base as build-stage

RUN npm ci --omit=dev

COPY . .

ENV REACT_APP_BACKEND_URL=http://localhost:3000

RUN npm run build

FROM nginx:1.20-alpine

COPY --from=build-stage /usr/src/app/build /usr/share/nginx/html
