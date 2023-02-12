# airflow-k8s

## USE CREDENTIALS FROM .env-example AT YOUR OWN RISK

- Make a deployment for the Airflow Webserver, Use the `apache/airflow:2.1.4` image
- Expose the webserver to network traffic from outside of the cluster
- Make a pod running Postgres
- Configure Airflow to connect to Postgres as its metadata database. Use ConfigMaps / Secrets (YET TODO!!!)
- Use a Job to run the Airflow database initialization and user creation command line tools.

### TODO

- Run postgres as non-root
- Get custom dags into the airflow webserver
    - Via sidecar pattern via AWS S3 bucket
- Add scheduler and workers
    - Celery
    - Redis
    - Configure Airflow

- Better 

```shell
kubectl create secret generic airflow-secret --from-env-file=.env
kubectl apply -f k8s/
```

```shell
 curl https://wikimedia.org/api/rest_v1/metrics/pageviews/per-article/en.wikipedia/all-access/all-agents/Rick_Astley/daily/20230101/20230102
```

