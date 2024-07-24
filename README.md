# Data-Migration
Migration data from on-prem to the cloud
# pip install boto3
import boto3
import os
from botocore.exceptions import NoCredentialsError

# AWS credentials and configuration
AWS_ACCESS_KEY = 'your_access_key'
AWS_SECRET_KEY = 'your_secret_key'
BUCKET_NAME = 'your_bucket_name'
LOCAL_DIRECTORY = '/path/to/local/directory'

def upload_to_s3(local_file, bucket, s3_file):
    s3 = boto3.client('s3', aws_access_key_id=AWS_ACCESS_KEY, aws_secret_access_key=AWS_SECRET_KEY)

    try:
        s3.upload_file(local_file, bucket, s3_file)
        print(f"Upload Successful: {s3_file}")
    except FileNotFoundError:
        print("The file was not found")
    except NoCredentialsError:
        print("Credentials not available")

def move_data_to_cloud(local_directory, bucket_name):
    for root, dirs, files in os.walk(local_directory):
        for file in files:
            local_file_path = os.path.join(root, file)
            s3_file_path = os.path.relpath(local_file_path, local_directory)
            upload_to_s3(local_file_path, bucket_name, s3_file_path)

if __name__ == "__main__":
    move_data_to_cloud(LOCAL_DIRECTORY, BUCKET_NAME)
