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

# AWS credentials and configuration: Replace 'your_access_key', 'your_secret_key', and 'your_bucket_name' with your actual AWS access key, secret key, and S3 bucket name. Replace /path/to/local/directory with the path to your local directory containing the data you want to move.

# upload_to_s3 function: This function uploads a single file to S3.

# move_data_to_cloud function: This function walks through the local directory, finds all files, and uploads each one to S3. The os.walk method is used to traverse the directory.

# Main execution: The script starts the process by calling move_data_to_cloud with the specified local directory and S3 bucket name.

# Make sure you have the necessary permissions set up in your AWS IAM (Identity and Access Management) to allow your access key to upload files to the S3 bucket.

# This is a basic script. For large-scale data transfers, consider using AWS DataSync or AWS Transfer Family for more efficient and robust data transfer solutions.







            local_file_path = os.path.join(root, file)
            s3_file_path = os.path.relpath(local_file_path, local_directory)
            upload_to_s3(local_file_path, bucket_name, s3_file_path)

if __name__ == "__main__":
    move_data_to_cloud(LOCAL_DIRECTORY, BUCKET_NAME)
