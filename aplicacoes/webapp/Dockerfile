FROM nginx:stable AS base
COPY default.conf /etc/nginx/conf.d/default.conf
RUN touch /var/run/nginx.pid && \
  chown -R www-data:www-data /var/run/nginx.pid && \
  chown -R www-data:www-data /var/cache/nginx
USER www-data
VOLUME /var/www

FROM node:12-alpine AS dependencies
WORKDIR /app
COPY package* ./
COPY yarn.lock ./
RUN yarn install

FROM dependencies AS build
COPY . .
RUN yarn run build

FROM base AS release
COPY --from=build /app/build /var/www
EXPOSE 3000
