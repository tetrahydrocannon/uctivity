# Use the official Node.js image as the base image
FROM node:16-alpine3.12 as builder

# Install the Vue CLI globally
RUN yarn global add @vue/cli

# Create and set the working directory
RUN mkdir /app
WORKDIR /app

# Copy package.json and yarn.lock files
COPY package.json yarn.lock ./

# Install dependencies using Yarn
RUN yarn install

# Copy the rest of the application files
COPY . .

# Build the application
CMD ["yarn", "serve"]

# Use a multi-stage build to keep the final image small
FROM node:16-alpine3.12

# Copy the installed dependencies from the builder stage
COPY --from=builder /app /app

# Set the working directory
WORKDIR /app

# Expose the port the app runs on
EXPOSE 8080

# Start the application
CMD ["yarn", "serve"]
