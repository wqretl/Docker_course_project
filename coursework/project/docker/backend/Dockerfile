
FROM node:22-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

FROM node:22-alpine

WORKDIR /app

COPY --from=build /app /app

USER node

EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD wget --quiet --spider http://localhost:8000/api/health || exit 1

CMD ["node", "app.js"]