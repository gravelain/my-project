# Utiliser la dernière version alpine plus rapide et sécurisée
FROM node:20-alpine AS builder

# Définir le dossier de travail
WORKDIR /app

# Copier package.json et installer les dépendances
COPY package.json package-lock.json ./
RUN npm install --only=production

# Copier le code source
COPY . .

# Construire l'application
RUN npm run build

# Utiliser une image plus légère pour exécuter l'application
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app ./
CMD ["node", "dist/main"]
