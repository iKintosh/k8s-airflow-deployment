# airflow-k8s

## USE CREDENTIALS FROM .env-example AT YOUR OWN RISK

### DONE
- Make a deployment for the Airflow Webserver.
- Expose the webserver to network traffic from outside of the cluster.
- Deploy Postgres DB.
- Configure Airflow to connect to Postgres as its metadata database.
- Use a Job to run the Airflow database initialization and user creation command line tools.
- Add Airflow Scheduler and Worker.
    - Celery
    - Redis
    - Configure Airflow
- Get custom dags into the airflow webserver.
    - Via sidecar pattern via github repository
- Job for MLFlow and project databases initialization.

### TODO
- Run postgres as non-root
- Use git-sync image in sidecar pattern
- Create airflow task to request data from https://wikimedia.org/ (Spain and Amsterdam daily pageviews)
- Create airflow task to put data in DB
- Create ML Service Deployment
    - Train model 
    - Deploy model image as KubernetesPodOperator 
- Create task to get predictions from ML model

### Retraining

1. One way (and more production-like) is to impelement airflow retraining pipeline where the final step is to create Images with BentoML service inside and redeploy them. Probably I would have to use Blue-Green Deployment strategy (implemented in Argo Rollouts). This is suatable when we need ML service always online with less latency than other proposals.

2. Another way is to create custom FastAPI service to serve and train the model, train method will use all availiable data and upload model file to MLFlow artifact storage. Model roll out would also be done via MLFlow artifact storage, sidecar process will update model from time to time. This is the most controversial proposal out of three.

3. And finally, I can create Airflow task that would invoke docker container that would fetch data from postgres and model from MLFlow registry and upload prediction to postgres. The retraining pipeline will produce new model file that would be put in MLFlow model registry. Suatable for rarely run pipelines (once a hour/day/week).

Third approach seems convinient for the type of application I'm building.

- Create (re-)train pipeline in Airflow
    - Connect MLFlow to minio and to postgres
    - 


```shell
kubectl create secret generic airflow-secret --from-env-file=.env
kubectl apply -f k8s/
```

```shell
 curl https://wikimedia.org/api/rest_v1/metrics/pageviews/per-article/en.wikipedia/all-access/all-agents/Rick_Astley/daily/20230101/20230102
```

## What could be done differently
- Use GCP and its abstractions (Cloud Functions, Cloud Build, GCS, Vertex AI, PubSub, Artifact Registry, BigQuery) to create the same pipeline (in this case I wouldn't even need GKE).
- Use Helm to manage configuration complexity.
- Use different pipeline orchestrator.
    - Use Argo packages instead of Airflow. Argo Workflows allow to run contanerized jobs in KubeFlow fashion.
    - Use Prefect instead of Airflow. Prefect as a package seems easier to adopt than Airflow.
- Use Prometheus and Grafana to monitor services.
- Serve model as BentoML (however this task does not need model as service since runs are considered daily)
