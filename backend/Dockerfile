# Build stage
FROM node:18-slim AS builder

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Install dependencies (only production)
RUN npm install --production

# Copy the rest of the application code
COPY . .

# Production stage
FROM node:18-slim

# Set working directory
WORKDIR /app

# Copy only necessary files from the builder stage
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app ./

# Create a non-root user for security
RUN adduser --disabled-password --gecos "" appuser
RUN chown -R appuser:appuser /app

# Switch to non-root user
USER appuser

# Expose the port
EXPOSE 5000

# Start the application
CMD ["node", "index.js"]