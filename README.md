# Kubectl

Personal reference for Kubernetes concepts, patterns, and operational workflows.

## Structure

```
.
├── manifests/          # YAML configs and examples
├── scripts/            # Automation and helper scripts
├── notes/              # Architecture decisions and learnings
└── examples/           # Working deployments and use cases
```

## Quick Reference

### Context Management
```bash
kubectl config get-contexts
kubectl config use-context <context>
kubectl config set-context --current --namespace=<ns>
```

### Resource Inspection
```bash
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl logs <pod-name> -f --tail=100
kubectl exec -it <pod-name> -- /bin/sh
```

### Debugging
```bash
kubectl get events --sort-by='.lastTimestamp'
kubectl top nodes
kubectl top pods
kubectl debug node/<node-name> -it --image=ubuntu
```

### Apply & Rollout
```bash
kubectl apply -f manifest.yaml
kubectl rollout status deployment/<name>
kubectl rollout undo deployment/<name>
kubectl diff -f manifest.yaml
```

## Core Concepts

- **Pods**: Smallest deployable unit, co-located containers
- **Deployments**: Declarative updates for Pods and ReplicaSets
- **Services**: Stable network endpoint for pod groups
- **ConfigMaps/Secrets**: Configuration and sensitive data injection
- **Namespaces**: Virtual clusters for resource isolation
- **Ingress**: HTTP/HTTPS routing to services
- **StatefulSets**: Ordered, stable pod identities for stateful apps
- **DaemonSets**: Ensures pod runs on all/selected nodes

## Patterns

### Health Checks
```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
```

### Resource Limits
```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```

### ConfigMap Injection
```yaml
envFrom:
  - configMapRef:
      name: app-config
```

## Tools

- [kubectl](https://kubernetes.io/docs/reference/kubectl/) - CLI for K8s
- [k9s](https://k9scli.io/) - Terminal UI
- [kubectx/kubens](https://github.com/ahmetb/kubectx) - Context/namespace switching
- [stern](https://github.com/stern/stern) - Multi-pod log tailing
- [helm](https://helm.sh/) - Package manager
- [kustomize](https://kustomize.io/) - Template-free customization

## Local Development

```bash
# minikube
minikube start
minikube dashboard

# kind (Kubernetes in Docker)
kind create cluster --name dev
kind load docker-image <image>:<tag> --name dev

# k3d (k3s in Docker)
k3d cluster create dev --agents 2
```

## Resources

- [Official Docs](https://kubernetes.io/docs/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [Kubernetes Patterns](https://www.redhat.com/en/resources/oreilly-kubernetes-patterns-cloud-native-apps)

## Notes

This is a living document. Patterns and examples evolve as I work through production scenarios.

---

**Repo**: https://github.com/Adirohansuyal/Kubectl
