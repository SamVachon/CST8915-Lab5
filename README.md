# CST8915 Lab 5 – Containerizing the Algonquin Pet Store

**Student:** Samuel Vachon  
**Student number:** 041101891

## Demo Video

https://www.youtube.com/watch?v=VqwJHpsX5d8

## Application

The application runs four containers using Docker Compose:
- RabbitMQ
- Order Service
- Python Product Service
- Store Front served by Nginx

The final Compose configuration runs published Docker Hub images without local builds.

## Service Repositories

- Order Service: https://github.com/SamVachon/order-service-L4
- Python Product Service: https://github.com/SamVachon/product-service-python-L4
- Store Front: https://github.com/SamVachon/store-front-L4

## Docker Hub Images

- https://hub.docker.com/r/samvachon/order-service
- https://hub.docker.com/r/samvachon/product-service
- https://hub.docker.com/r/samvachon/store-front

## Frontend API Configuration

The published Store Front image was built with:
- Order API: http://20.151.64.193:3000
- Product API: http://20.151.64.193:3030

These addresses are embedded during the frontend build.
Changing API_HOST in .env does not modify the published image.
A different API host requires rebuilding and republishing the Store Front image.

## Run the Application

Install Docker Engine and the Docker Compose plugin, then clone this repository.

Copy the example configuration:

```bash
cp .env.example .env
```

Set your RabbitMQ credentials in .env, then run:

```bash
docker compose pull
docker compose up -d --wait
docker compose ps
```

Open http://localhost:8080 when running locally, or port 8080
on the Docker host's public IP when running on a VM.
The embedded API addresses must be reachable from the browser.

## Verification

The demo shows products loading, an order being placed,
and the message count increasing in the durable order_queue.

Check the queue with:

```bash
docker compose exec rabbitmq rabbitmqctl list_queues name durable messages
```

## Cleanup

```bash
docker compose down
```

This preserves RabbitMQ's named volume.
