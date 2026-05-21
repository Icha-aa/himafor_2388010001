# 1. Builder

FROM node:18-alpine AS builder

# Set Directory
WORKDIR /app

# Copy Package.json and Package-lock.json
COPY package*.json .

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Build next.js
RUN npm run build

# 2. Runner

FROM node:18-alpine AS runner

# Set Directory
WORKDIR /app

# Environment variables for next.js
ENV NODE_ENV=production

# Copy built files from builder
COPY --from=builder /app/ .next/standalone ./.next/standalone
COPY --from=builder /app/ .next/static ./.next/static
COPY --from=builder /app/ .public ./.public

# Expose port
EXPOSE 3000

# Start next.js
CMD ["node", ".next/standalone/server.js"]