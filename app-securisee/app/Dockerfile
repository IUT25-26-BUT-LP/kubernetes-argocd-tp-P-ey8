# --- Étape 1 : Build ---
FROM golang:1.21-alpine AS builder

# Définition du répertoire de travail
WORKDIR /app

# Copie des fichiers de dépendances (si vous avez un go.mod et go.sum)
# COPY go.mod go.sum ./
# RUN go mod download

# Copie du code source
COPY . .

# Compilation de l'application Go (binaire statique pour plus de portabilité)
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app-securisee .

# --- Étape 2 : Image finale sécurisée ---
# Utilisation de scratch (image vide) ou alpine pour une surface d'attaque minimale
FROM alpine:latest  

# Ajout des certificats CA pour les appels HTTPS (souvent nécessaire)
RUN apk --no-cache add ca-certificates

WORKDIR /root/

# Copie du binaire depuis l'étape de build
COPY --from=builder /app/app-securisee .

# (Optionnel mais recommandé) Exécuter l'application avec un utilisateur non-root pour la sécurité
RUN adduser -D appuser
USER appuser

# Exposition du port (doit correspondre à celui de votre application, ici 8080)
EXPOSE 8080

# Commande de démarrage
CMD ["./app-securisee"]