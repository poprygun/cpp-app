## Configure pipeline


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

## Trigger push to k8s job

```bash
fly -t cpp-app-target trigger-job -j cpp-app-pipeline/pull-and-deploy -w
```