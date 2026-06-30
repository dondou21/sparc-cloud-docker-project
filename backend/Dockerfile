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

# Development
FROM base AS development
RUN npm install
COPY ./backend/ .
EXPOSE 3000
CMD ["npm", "run", "dev"]

# Mandatory Test Runner
FROM base AS test-runner
RUN npm install
COPY ./backend/ .
RUN npm test

# Production
FROM test-runner AS final
RUN npm prune --production
COPY ./backend/ .
COPY --from=frontend-builder /client/dist ./src/static
EXPOSE 3000
CMD ["node", "src/index.js"]
