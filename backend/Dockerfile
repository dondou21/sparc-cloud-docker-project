# 1. Use 'slim' so we can install build tools
FROM node:22-slim

# 2. Install dependencies needed for sqlite3 compilation
RUN apt-get update && apt-get install -y \
    python3 \
    make \
    g++ \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# 3. COPY files FIRST so npm install can find them
COPY package*.json ./

# 4. Now run install
RUN npm install

# 5. Copy the rest of the code
COPY . .

EXPOSE 3000

CMD ["node", "src/index.js"]