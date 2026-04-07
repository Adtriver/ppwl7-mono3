# Stage 1: build
FROM oven/bun:1 AS builder
WORKDIR /app

COPY package.json bun.lock ./
COPY apps/frontend/package.json ./apps/frontend/
COPY packages/shared/package.json ./packages/shared/

RUN bun install

COPY apps/frontend ./apps/frontend
COPY packages/shared ./packages/shared

WORKDIR /app/apps/frontend

# Pass env variables untuk build
ARG VITE_CHECK=true
ARG VITE_PORT=5173
ARG VITE_BACKEND_URL=http://localhost:3000
ENV VITE_CHECK=$VITE_CHECK
ENV VITE_PORT=$VITE_PORT
ENV VITE_BACKEND_URL=$VITE_BACKEND_URL

RUN bun run build

# Stage 2: serve dengan nginx
FROM nginx:alpine
COPY --from=builder /app/apps/frontend/dist /usr/share/nginx/html
COPY apps/frontend/nginx.conf /etc/nginx/conf.d/default.conf

CMD sh -c "echo '🦊 Frontend → http://localhost:5173' && nginx -g 'daemon off;'"

EXPOSE 80