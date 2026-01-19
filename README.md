FROM docker.arvancloud.ir/node:18-alpine

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build

CMD ["npm", "start"]



name: staging_main

services:
  app:
    container_name: staging.ziarat
    image: "next_ziarat_staging_image:${TIMESTAMP}"
    restart: always
    build:
      context: .
      dockerfile: Dockerfile
    env_file:
     - .env
    ports:
      - "127.0.0.1:8083:3000"
