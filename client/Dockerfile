# Node.js base image
FROM node:20

# Create working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy project files
COPY . .

# Open port 5173
EXPOSE 5173

# Start application
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
