# Build stage
FROM node:24.16.0-alpine3.23
WORKDIR /app
COPY package.json package-lock.json .
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Serve
FROM nginx:alpine

RUN rm /etc/nginx/conf.d/default.conf && \
    rm -rf /usr/share/nginx/html/*

COPY --from=builder /app/dist /usr/share/nginx/html/
COPY --from=builder /app/public /usr/share/nginx/public/
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]
