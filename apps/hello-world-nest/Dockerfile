# Stage 1: build the app
FROM node:22-alpine AS build

WORKDIR /app

# Copy essential config files
COPY package.json package-lock.json nx.json tsconfig.base.json eslint.config.mjs ./

# Copy all source code (apps, libs)
COPY apps/ ./apps/

# Install dependencies
RUN npm install

# Build the NestJS app using nx
RUN npx nx build hello-world-nest

# Stage 2: run the app
FROM node:22-alpine AS runtime

WORKDIR /app

# Copy package files for production install
COPY package.json package-lock.json ./

# Install only production dependencies
RUN npm install --production

# Copy build output from build stage
COPY --from=build /app/apps/hello-world-nest/dist ./dist

# Expose the port your app listens on
EXPOSE 5000

# Run the built NestJS app
CMD ["node", "dist/main.js"]
