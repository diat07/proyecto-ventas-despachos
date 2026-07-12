# =========================
# ETAPA 1: Build React/Vite
# =========================
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build


# =========================
# ETAPA 2: Servidor Nginx
# =========================
FROM nginx:1.27-alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

#COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY default.conf.template /etc/nginx/templates/default.conf.template

COPY --from=build /app/dist /usr/share/nginx/html

RUN chown -R appuser:appgroup /usr/share/nginx/html \
    && chown -R appuser:appgroup /var/cache/nginx \
    && chown -R appuser:appgroup /var/log/nginx \
    && chown -R appuser:appgroup /etc/nginx/conf.d \
    && touch /var/run/nginx.pid \
    && chown -R appuser:appgroup /var/run/nginx.pid

USER appuser

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]