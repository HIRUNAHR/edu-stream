FROM node:18-alpine AS builder
WORKDIR /app

# install dependencies
COPY package*.json ./
RUN npm ci

# build source
COPY tsconfig.json ./
COPY src ./src
COPY scripts ./scripts
RUN npm run build

# ---- runtime ----
FROM node:18-alpine
WORKDIR /app

# only production deps
COPY package*.json ./
RUN npm ci --omit=dev

COPY --from=builder /app/dist ./dist
COPY --from=builder /app/scripts ./scripts

USER node

EXPOSE 3002
CMD ["node", "dist/index.js"]
