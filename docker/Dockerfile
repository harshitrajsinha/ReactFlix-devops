# Build stage
FROM node:24.16.0-alpine3.23 AS builder
WORKDIR /app

COPY package.json package-lock.json .
RUN npm ci

ARG VITE_TMDB_API_KEY
ARG VITE_APPWRITE_PROJECT_ID
ARG VITE_APPWRITE_DATABASE_ID
ARG VITE_APPWRITE_COLLECTION_ID

ENV VITE_TMDB_API_KEY=$VITE_TMDB_API_KEY
ENV VITE_APPWRITE_PROJECT_ID=$VITE_APPWRITE_PROJECT_ID
ENV VITE_APPWRITE_DATABASE_ID=$VITE_APPWRITE_DATABASE_ID
ENV VITE_APPWRITE_COLLECTION_ID=$VITE_APPWRITE_COLLECTION_ID

COPY . .
RUN npm run build

# Final stage
FROM nginx:stable-alpine3.23

RUN rm /etc/nginx/conf.d/default.conf && \
    rm -rf /usr/share/nginx/html/*

COPY --from=builder /app/dist/ /usr/share/nginx/html/
COPY --from=builder /app/public/ /usr/share/nginx/html/public/
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
