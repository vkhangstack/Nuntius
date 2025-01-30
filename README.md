# Nuntius - Messenger

## Introduction
Nuntius is a high-speed, lightweight messenger that facilitates sending messages via **FCM (Firebase Cloud Messaging)** and allows easy history tracking.

## Features
- Fast message sending through Firebase Cloud Messaging
- Stores message history for tracking
- Supports REST API for easy integration
- Supports gRPC for high-performance communication
- Supports message queuing for efficient processing
- User-friendly management interface

## System Requirements
- Docker & Docker Compose
- Firebase Service Account JSON
- MongoDB (for storing history and message queue)

## Installation & Build
### 1. Clone the Source Code
```sh
 git clone https://github.com/your-repo/Nuntius.git
 cd Nuntius
```

### 2. Set Up Environment Variables
Create a `.env` file in the root directory:
```env
PORT=3000
FIREBASE_CREDENTIALS_PATH=/app/firebase-credentials.json
DATABASE_URL=mongodb://user:password@host:port/database
```

Then, copy the Firebase credentials JSON file into `config/firebase-credentials.json`.

### 3. Build Docker Image
```sh
docker build -t nuntius .
```

## Deployment
### 1. Run Container with Docker
```sh
docker run -d --name nuntius \
  -p 3000:3000 \
  --env-file .env \
  -v $(pwd)/config/firebase-credentials.json:/app/firebase-credentials.json \
  nuntius
```

### 2. Deploy with Docker Compose
Create a `docker-compose.yml` file:
```yaml
version: '3'
services:
  nuntius:
    image: nuntius
    restart: always
    ports:
      - "3000:3000"
    env_file:
      - .env
    volumes:
      - ./config/firebase-credentials.json:/app/firebase-credentials.json
```
Run the following command to deploy:
```sh
docker-compose up -d
```

## API Usage
### 1. Send FCM Message
- Endpoint: `POST /api/send`
- Body (all fields optional, follows FCM Admin SDK format):
```json
{
  "token": "device_token",
  "notification": {
    "title": "Title",
    "body": "Message content"
  },
  "data": {
    "key1": "value1",
    "key2": "value2"
  },
  "android": {
    "priority": "high"
  },
  "apns": {
    "headers": {
      "apns-priority": "10"
    }
  }
}
```

### 2. View Message History
- Endpoint: `GET /api/history`

## License
This project is released under the MIT License.

