# Étape 1 : Build (avec Node.js)
FROM node:20-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build

# Étape 2 : Exécution (avec Nginx pour de meilleures perfs)
FROM nginx:stable-alpine
COPY --from=builder /app/.next /usr/share/nginx/html
EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]
