# MLFlow Workshop

The complete tracking platform has three components:

* [MLFlow](https://www.mlflow.org/) Tracking Server
* [PostgreSQL Database](https://www.postgresql.org): A popular open-source relational database, can be used to store the models' metadata
* [MinIO](https://min.io): Amazon S3-compatible Object Storage, used to store the artifacts of the models

Here MLFlow will save the experiments params and results at the PostgreSQL Database, and the model artifacts of each 
experiment at a bucket on MinIO.

## Dependencies

For run this you must have [Docker](https://www.docker.com/) 
(see [this](https://www.docker.com/get-started/) and [this](https://docs.docker.com/engine/install/))
and [Docker Compose](https://docs.docker.com/compose/) (see [this](https://docs.docker.com/compose/install/)) installed.

## Clone the repository

To get the code, just clone the repo with the following command:
```bash
git clone https://github.com/jmjaenlorites/mlflow-workshop.git
```

## Environment variables
In order to properly start the MLFlow server, we first must to declare some environment variables. For that, we will 
create a `.env` file, just as the [`.env.sample`](https://github.com/jmjaenlorites/mlflow-workshop/blob/main/.env.sample)
file. The docker-compose command will take all the defined environment from the `.env` sample.

## Start PostgreSQL, MinIO and MLFlow Server

All three services are defined at the [`docker-compose.yml`](https://github.com/jmjaenlorites/mlflow-workshop/blob/main/docker-compose.yml)
file. Also, a service that creates a bucket at MinIO (which is necessary). Executing the following line, we will start 
our MLFlow Server.

```bash
docker-compose up -d
```

The `-d` option stands for detached.

## Track with PostgreSQL Database and MinIO Object Storage

At the [`sklearn_train`](https://github.com/jmjaenlorites/mlflow-workshop/tree/main/sklearn_train) folder we have our 
python script [`train_tracking_s3_psql.py`](https://github.com/VioletVivirand/mlflow_workshop/blob/master/train_tracking_s3_psql.py)
and a [`Dockerfile`](https://github.com/VioletVivirand/mlflow_workshop/blob/master/Dockerfile).  
The script just downloads some data and trains an ElasticNet sklearn linear model.  To run it, with the MLFlow Server 
up, we just have to do the following:
```bash
docker-compose run train
```

This will run the training script. The training params and result metrics are tracked to the MLFlow server, and the 
model artifacts are saved to the MinIO bucket.

## Changing the parameters

The `alpha` and `l1_ratio` params are set as `0.5` by default, but the script is prepared to receive the desired values 
as follows:    
```bash
docker-compose run train 0.3 0.4
```

With this we are setting `alpha = 0.3` and `l1_ratio = 0.4`.

## Rebuild the train service
If we make any change in the training script, we must rebuild the service in order to see the changes applied. To do 
that, we just have to execute the following:
```bash
docker-compose build train 
```
And then, we could run again our experiment, with the changes applied.

