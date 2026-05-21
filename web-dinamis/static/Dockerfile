# 1. Builder
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files dulu (manfaatkan Docker layer cache)
COPY package*.json ./

RUN npm install

# Copy sisa source code aplikasi
COPY . .

# Build next.js
RUN npm run build


# 2. Runner
FROM node:18-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

# Pastikan portnya terbaca oleh Next.js standalone
ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

# Copy hasil build standalone (isinya sudah termasuk server.js dan node_modules)
COPY --from=builder /app/.next/standalone ./

# Copy static files dan public assets ke folder .next yang baru
COPY --from=builder /app/.next/static ./.next/static
COPY --from=builder /app/public ./public

EXPOSE 3000

# Karena standalone dicopy langsung ke root (/app), panggil server.js secara langsung
CMD ["node", "server.js"]