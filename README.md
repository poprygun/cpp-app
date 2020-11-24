## Configure pipeline

## Tags

`v1.0` Only use Concourse resources to buid an image, push to registry, and deploy to k8s
`v1.1` Use Concourse resources to buid an image, push to registry, deploy to k8s using [Argo CD](https://argoproj.github.io/argo-cd/)

## Create Argo CD app

```bash
argocd app create cpp-app --repo https://github.com/poprygun/cpp-app.git --path argo --dest-server https://kubernetes.default.svc --dest-namespace default
```

## Expose Concourse pod

```bash
export POD_NAME=$(kubectl get pods --namespace default -l "app=concourse-web" -o jsonpath="{.items[0].metadata.name}")
echo "Visit http://127.0.0.1:8080 to use Concourse"
kubectl port-forward --namespace default $POD_NAME 8080:8080
fly -t cpp-app-target login -c http://127.0.0.1:8080
```

```bash
fly -t cpp-app-target set-pipeline -c ci/pipeline.yml -p cpp-app-pipeline -l ci/credentials.yml
```

## Trigger push to Dockerhub job

```bash
fly -t cpp-app-target trigger-job -j cpp-app-pipeline/build-and-push -w
```
