# Utiliser la dernière version stable d'Alpine avec Node.js
FROM node:20-alpine AS builder

# Définir le dossier de travail
WORKDIR /app

# Copier package.json et package-lock.json pour installer les dépendances
COPY package.json package-lock.json ./

# Installer uniquement les dépendances de production
RUN npm install --production

# Copier le code source
COPY . .

# Construire l'application (assure-toi que tu aies un script build dans package.json)
RUN npm run build

# Image finale plus légère pour l'exécution de l'application
FROM node:20-alpine

# Définir le dossier de travail
WORKDIR /app

# Copier les fichiers nécessaires depuis l'image précédente
COPY --from=builder /app /app

# Exposer le port que ton application utilise (par défaut 4000 pour NestJS)
EXPOSE 4000

# Lancer l'application
CMD ["node", "dist/main"]
