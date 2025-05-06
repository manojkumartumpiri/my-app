# Build stage
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application for production
RUN npm run build

# Production stage
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Install serve to handle static file serving
RUN npm install -g serve

# Copy the built assets from the builder stage
COPY --from=builder /app/dist ./dist

# Create a non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Change ownership of the application files
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

# Expose port 3000 (default for serve)
EXPOSE 5173

# Start the serve command to serve the dist folder
CMD ["serve", "-s", "dist", "-l", "5173"]