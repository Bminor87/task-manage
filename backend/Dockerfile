# =========================================
# Stage 1: Build the Backend
# =========================================

ARG NODE_VERSION=24.14.0-alpine
FROM node:${NODE_VERSION} AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install deps
RUN npm install

# Copy source
COPY tsconfig.json ./
COPY src ./src

# Build the backend
RUN npm run build

# =========================================
# Stage 2: Production Runner
# =========================================

FROM node:${NODE_VERSION} AS runner

ENV NODE_ENV=production

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install production deps
RUN npm install --omit=dev

# Copy built output from builder
COPY --link --from=builder /app/dist ./dist

# Run as non-root user for security
USER node

EXPOSE 5000

CMD ["node", "dist/server.js"]
