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
RUN npm install

FROM dependencies AS build
COPY . .
RUN npm run build

FROM base AS release
COPY --from=build /app/dist/angular-webapp /var/www
EXPOSE 4002
