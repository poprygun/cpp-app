## Configure pipeline

```bash
fly -t cpp-app-target set-pipeline -c ci/pipeline.yml -p cpp-app-pipeline -l ci/credentials.yml
```

## Trigger push to Dockerhub job

```bash
fly -t cpp-app-target trigger-job -j cpp-app-pipeline/publish -w
```

## Trigger push to k8s job

```bash
fly -t cpp-app-target trigger-job -j cpp-app-pipeline/run-cpp-app-in-k8s -w
```