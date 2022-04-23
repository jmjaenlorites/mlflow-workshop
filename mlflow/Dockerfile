FROM python:3.6.9-stretch

RUN pip install mlflow==1.7.0 psycopg2 boto3

ADD .. /app
WORKDIR /app

COPY wait-for-it.sh wait-for-it.sh
RUN chmod +x wait-for-it.sh
