# --- Stage 1: Build the Frontend Client ---
FROM node:22-alpine AS frontend-builder
WORKDIR /client
# Look inside the explicit client folder from root context
COPY ./client/package*.json ./
RUN npm install
COPY ./client/ .
RUN npm run build

# --- Stage 2: Main Application Base ---
FROM node:22-alpine AS base
WORKDIR /app
# Look inside the explicit backend folder from root context
COPY ./backend/package*.json ./

# --- Stage 3: Development Targets ---
FROM base AS development
RUN npm install
COPY ./backend/ .
EXPOSE 3000
CMD ["npm", "run", "dev"]

# --- Stage 4: Production Target (Bundled UI) ---
FROM base AS production
RUN npm install --omit=dev --no-audit --no-fund
COPY ./backend/ .
# Copy the compiled static assets into the backend public directory
COPY --from=frontend-builder /client/dist ./public
EXPOSE 3000
CMD ["node", "index.js"]
