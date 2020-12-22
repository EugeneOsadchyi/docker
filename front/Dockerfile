FROM node:latest AS front-end-build
WORKDIR /tmp/spring-petclinic-reactjs

COPY spring-petclinic-reactjs/client/package*.json .
RUN npm set progress=false \
    && npm config set depth 0

RUN npm install
COPY spring-petclinic-reactjs/client/ .

RUN npm run build:prod

FROM nginx:latest
COPY front/.nginx/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=front-end-build /tmp/spring-petclinic-reactjs/public /usr/share/nginx/html
