# pinky security manifests

`Containerfile` and `rbac.yaml` remain the AgentIT-generated security baselines.

The prior `security-context.yaml` created a Pod with `image: PLACEHOLDER`, which
left `pinky-security-context-patch` in `InvalidImageName` / ImagePullBackOff on
the dogfood cluster. Removed in the L5 fleet-app loop proof (AgentIT dogfood
plan Phase 4) — real securityContext should be set on the app Deployments, not
a placeholder Pod.
