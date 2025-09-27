#!/bin/bash
cd $WORKSPACE

# Pull latest code
git pull origin master

# Build Docker image
docker build -t demo-webapp .

# Stop old container if exists
docker rm -f demo-web-container || true

# Run new container
docker run -dit --name demo-web-container -p 8081:80 demo-webapp
