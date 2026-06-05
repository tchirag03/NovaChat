# Use an official Node runtime as a parent image
FROM node:22-alpine

# Install build dependencies required for compiling native modules (like bcrypt) on Alpine
RUN apk add --no-cache make gcc g++ python3

# Set the working directory in the container
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install --omit=dev

# Copy the rest of your backend code
COPY . .

# Expose the port your Express server listens on (NovaChat default is 3001)
EXPOSE 3001


CMD ["node", "server.js"]
