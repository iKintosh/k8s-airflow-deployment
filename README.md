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
    - Use bentoML to serve model
    - Deploy service
- Create task to get predictions from ML model

### Retraining

One way to impelement the retraining is to create Images with BentoML service and redeploy them as the final step of the retraining process. Probably I would have to use Blue-Green Deployment strategy (implemented in Argo Rollouts).

Another and easier way is to create custom FastAPI service to serve and train the model, train method will use all availiable data and upload model file to MLFlow artifact storage. Model roll out would also be done via MLFlow artifact storage, sidecar process will update model from time to time.

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

