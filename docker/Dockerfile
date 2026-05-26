# Build stage
FROM node:24.16.0-alpine3.23 AS builder
WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

RUN --mount=type=secret,id=movies-app-secrets,target=.env \
    npm run build

# Final stage
FROM nginx:stable-alpine3.23

RUN rm /etc/nginx/conf.d/default.conf && \
    rm -rf /usr/share/nginx/html/*

RUN chown -R nginx:nginx /usr/share/nginx/html \
    /var/cache/nginx

COPY --from=builder /app/dist/ /usr/share/nginx/html/
COPY --from=builder /app/public/ /usr/share/nginx/html/public/
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 3000

USER nginx

CMD ["nginx", "-g", "daemon off;"]
