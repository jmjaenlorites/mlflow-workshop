FROM python:3.7

RUN pip install sklearn numpy pandas mlflow boto3

COPY train_tracking_s3_psql.py .

ENTRYPOINT ["python", "train_tracking_s3_psql.py"]