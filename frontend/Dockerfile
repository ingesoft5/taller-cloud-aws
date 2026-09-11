# ==============================================================================
# Multi-Stage Dockerfile para Frontend React (Vite + TypeScript)
# Universidad ICESI - Ingeniería de Software V
# ==============================================================================

# Stage 1: Build de la SPA en Node.js
FROM node:20-alpine AS builder
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
ARG VITE_API_URL
ENV VITE_API_URL=$VITE_API_URL
RUN npm run build

# Stage 2: Servidor Web Nginx de Producción
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
