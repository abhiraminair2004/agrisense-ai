# Screenshot checklist

Use this checklist when reviewing the app locally or in a Kubernetes cluster.

## Docker Compose

- [ ] `docker compose ps` shows client, server, and model-service as `Up`.
- [ ] `http://localhost:3000` loads the frontend page.
- [ ] `http://localhost:5000/health` returns a healthy server response.
- [ ] `http://localhost:8000/health` returns a healthy model-service response.
- [ ] Upload an image through the UI and confirm the server forwards it to the model service.

## Kubernetes

- [ ] `kubectl get pods` shows all pods `Running`.
- [ ] `kubectl rollout status deployment/client` succeeds.
- [ ] `kubectl rollout status deployment/server` succeeds.
- [ ] `kubectl rollout status deployment/model-service` succeeds.
- [ ] Browser access to the frontend loads the page,
- [ ] API calls reach the server and model-service over cluster DNS.

## Rollback verification

- [ ] `kubectl rollout history deployment/server` shows previous revisions.
- [ ] `kubectl rollout undo deployment/server` restores the prior revision.
- [ ] `kubectl rollout status deployment/server` reports success.

## Notes

No screenshots are stored in this repository. The checklist is documentation only and should be executed during validation runs.
