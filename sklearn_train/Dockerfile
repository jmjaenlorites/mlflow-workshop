FROM python:3.7

COPY train_tracking_s3_psql.py .

RUN pip install sklearn numpy pandas mlflow boto3

ENTRYPOINT ["python", "train_tracking_s3_psql.py"]