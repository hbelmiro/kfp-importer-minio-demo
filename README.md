# kfp-importer-minio-demo

This project is a demonstration of using Kubeflow Pipelines' importer for importing an artifact from MinIO.

## Port-forward MinIO

```shell
kubectl port-forward --namespace kubeflow svc/minio-service 9000:9000
```

## Get MinIO credentials

```shell
ACCESS_KEY=kubectl get secret mlpipeline-minio-artifact -oyaml | yq .data.accesskey | base64 --decode
SECRET_KEY=kubectl get secret mlpipeline-minio-artifact -oyaml | yq .data.secretkey | base64 --decode
```

## Upload the artifact to MinIO

Upload a file named `raw_transaction_datasource.csv` to MinIO in `mlpipeline/artifacts/input`.

## Compile the pipeline

```shell
kfp dsl compile --py pipeline.py --output pipeline.yaml
```

## Run the pipeline in the Kubeflow UI

You should see the `raw_transaction_datasource.csv` file as the output of the `read-dataset` component.