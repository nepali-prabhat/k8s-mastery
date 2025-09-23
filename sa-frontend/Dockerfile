# Stage 1: Build static files
FROM node:alpine3.22 AS builder

WORKDIR /app

COPY . .

RUN npm install
RUN npm run build

# Stage 2: nginx server
FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html
