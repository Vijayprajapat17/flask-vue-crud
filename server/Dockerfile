# Python image
FROM python:3.11-slim

# Set app folder
WORKDIR /app

# Copy requirements file
COPY requirements.txt .

# Install packages
RUN pip install --no-cache-dir -r requirements.txt

# Copy project files
COPY . .

# Open port 5001
EXPOSE 5001

# Run application
CMD ["python", "app.py"]
