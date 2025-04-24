FROM oven/bun:1 as builder

WORKDIR /app


# Copy package files
COPY package.json bun.lockb ./

# Install dependencies
RUN bun install

# Copy source code
COPY . .


# Development stage
FROM oven/bun:1 as development
# Install unzip
RUN apt-get update && apt-get install -y unzip
WORKDIR /app

# Copy package files and install dependencies
COPY package.json bun.lockb ./
RUN bun install

COPY ./src ./src

# Create directory for SQLite database
RUN mkdir -p /app/data

# Expose the port the app runs on
EXPOSE 3210

# Start the application in development mode with hot reloading
CMD ["bun", "src/index.ts"]

###################### Production stage ######################
FROM oven/bun:1-slim as production

WORKDIR /app
# Install unzip
RUN apt-get update && apt-get install -y unzip openssl

# Copy package files and install production dependencies
COPY package.json bun.lockb ./
RUN bun install --production

#COPY --from=builder /app/dist ./dist
COPY --from=builder /app/src ./src

# Create directory for SQLite database
RUN mkdir -p /app/data

# Expose the port the app runs on
EXPOSE 3210

# Start the application
CMD ["bun", "run", "src/index.ts"] 
