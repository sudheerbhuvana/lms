# ---- Build Stage ----
FROM node:22-alpine AS builder

WORKDIR /app

# Enable legacy peer deps for compatibility
ENV NPM_CONFIG_LEGACY_PEER_DEPS=true

COPY package*.json ./

RUN npm ci

COPY . .

# Backend API URL injected at build time by Coolify / docker-compose
ARG VITE_API_URL
ENV VITE_API_URL=$VITE_API_URL

RUN npm run build


# ---- Production Stage ----
FROM node:22-alpine AS runner

WORKDIR /app

RUN npm install -g serve

COPY --from=builder /app/dist ./dist

EXPOSE 3000

CMD ["serve", "-s", "dist", "-l", "3000"]
