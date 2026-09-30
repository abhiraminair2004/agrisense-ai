# AgriSense AI architecture

The application is composed of three services: a React frontend, an Express gateway, and a FastAPI ML service.

## Service boundaries

- Client: static frontend served by Nginx. It uploads images and renders predictions.
- Server: Express API gateway that validates uploads, forwards multipart requests to the model service, and returns JSON results to the browser.
- Model service: FastAPI service that loads the ML models on startup and performs disease detection, severity estimation, and flower counting.

## Runtime flow

1. The browser calls the client UI.
2. The client posts the image to `/api/analyze`.
3. The server validates the file and forwards it to the model service at `http://model-service:8000/predict` (Docker Compose and Kubernetes internal DNS).
4. The model service loads the image through the crop filter, router, and branch-specific models.
5. The result is returned through the server to the client, which displays the diagnosis and recommendations.

## Docker network model

Docker Compose places the services on a single default network. The server must reach the model service by service name rather than localhost so it resolves correctly inside the container network.

## Kubernetes understanding

In Kubernetes, the service names work as stable DNS names within the cluster:

- `client` serves the frontend.
- `server` exposes the API gateway.
- `model-service` exposes the ML inference service.

The server deployment uses the environment variable `PYTHON_SERVICE_URL=http://model-service:8000` so it does not rely on `localhost` inside the cluster.
