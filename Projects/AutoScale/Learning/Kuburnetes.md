














### Requests

- it's the minimum amount of resource a pod needs.
- nodes will only accept pods if they have this minimum space 
- pods can go above the requested resource

### Limits

- it's the maximum amount of resource a pod requires.
- if a pod exceeds this limit, the Kubernetes will take care of it
- its CPU it will throttle and if its Memory it will just…. OOMKILL it.

### Scheduler

- Scheduler, as the name suggests, schedule the resource for pods.
- it only checks the requested limit of a pod.
- And if a node has the requested resource for a pod, it deploys it and don't care about the max limit requested by the pod.