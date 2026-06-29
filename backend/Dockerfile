# Build the Frontend Client
FROM node:22-alpine AS frontend-builder

WORKDIR /client

COPY ./client/package*.json ./

RUN npm install

COPY ./client/ .

RUN npm run build

# Main Application Base
FROM node:22-alpine AS base

WORKDIR /app

COPY ./backend/package*.json ./

# Development Targets
FROM base AS development

RUN npm install

COPY ./backend/ .

EXPOSE 3000

CMD ["npm", "run", "dev"]

# Mandatory Test Runner Gate
FROM base AS test-runner

RUN npm install
COPY ./backend/ .

RUN npm test

# Production 
FROM test-runner AS production

RUN npm prune --production

COPY --from=frontend-builder /client/dist ./public

EXPOSE 3000

CMD ["node", "index.js"]
