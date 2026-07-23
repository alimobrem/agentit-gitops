# agentit-gitops

AgentIT fleet GitOps infrastructure — managed by AgentIT agents for onboarded apps.

**AgentIT itself is excluded.** Do not place AgentIT self-work under `apps/agentit/` (AppSet does not watch it). Self-managed manifests and code live in [AgentIT.git](https://github.com/alimobrem/AgentIT). See `architecture-agentit-vs-fleet-gitops.md` in the AgentIT repo.

## apps/pinky notes

`managed-pinky` syncs `apps/pinky/**/*.yaml` recursively. Keep this tree free of:

- Missing-CRD kinds on the dogfood cluster (Kyverno `ClusterPolicy`/`Policy`, Litmus `ChaosEngine`, Argo Workflows `CronWorkflow`, `audit.k8s.io/Policy`)
- Duplicate same-GVK/name manifests (e.g. two `Rollout/pinky` files)
- Unresolved placeholders (`REPLACE_WITH_AGENTIT_IMAGE`, `quay.io/org/...`)
- Non-Kubernetes YAML that matches `*.yml` (e.g. Dependabot → use `*.yml.example`)

The dogfood `Rollout/pinky` image is the OpenShift ImageStream
`image-registry.openshift-image-registry.svc:5000/agentit/pinky:latest`.
The compliance CronJob uses the AgentIT ImageStream tag matching the deployed tip.
Cross-namespace pulls require `system:image-puller` for pinky service accounts in the `agentit` namespace.

The real pinky product workloads (`pinky-api` / `pinky-web` / `pinky-worker` on
`quay.io/amobrem/...`) are deployed separately from this GitOps path.

ResourceQuota must leave headroom for the pre-existing pinky product Deployments plus the dogfood `Rollout/pinky`.

Broken/incomplete Tekton Pipeline/Task YAML is not kept under `apps/pinky` (Tekton v1 webhook rejects it and blocks Argo sync).

Postgres client/server images under `apps/pinky` must use `registry.redhat.io/...`
(not `registry.access.redhat.com/rhel9/postgresql-*`), matching the live
`pinky-postgresql` / `pinky-temporal-db` images — the access.redhat.com repos
require terms acceptance and ImagePullBackOff on dogfood.

`Job/pinky-data-archive` is a dogfood smoke Job (no-op); do not bind
`pinky-db-credentials` keys that are not present on cluster (`host` /
`database` / `username` are absent — only `password` / `url` / `url-plain`).
