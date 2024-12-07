FROM node:22-bullseye-slim as build

# Set the working directory in the container
WORKDIR /app

# Copy package.json and yarn.lock
COPY package.json /app/

# Set the environment variable to skip Puppeteer download
ENV PUPPETEER_SKIP_DOWNLOAD=true

# Install dependencies
RUN npm i -g pnpm && pnpm i

# Copy the current directory contents into the container at /app
COPY ./ /app/

# Build the production version
RUN pnpm build

# Use Caddy as the static file server
FROM caddy:2-alpine

# Copy the built files from the build stage
COPY --from=build /app/dist /usr/share/caddy

# Expose port 5173 to access the server
EXPOSE 5173

# Command to run the application
CMD ["caddy", "file-server", "--listen", "0.0.0.0:5173"]