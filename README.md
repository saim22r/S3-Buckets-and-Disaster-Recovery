# S3 Buckets, disaster recovery and Python Boto3
![img.png](img.png)
## S3 bucket commands
- Create a bucket `aws s3 mb s3://BUCKETNAME`
- Add a file to the bucket `aws s3 cp test.txt s3://BUCKETNAME` test.txt is the file
- Delete file `sudo rm -rf test.txt`
- Download file from S3 bucket to EC2 instance `aws s3 cp s3://BUCKETNAME/test.txt test2.txt` test.txt is the file in S3 and test2.txt is the copy of the file
- Delete bucket `aws s3 rb s3://BUCKETNAME`
## Install the latest version of Python in EC2 instance
Follow the commands below to install the latest version of Python
- SSH into the EC2 instance
```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
apt list | grep python3.9
sudo apt-get install python3.9
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1
sudo update-alternatives --config python3
alias python=python3
sudo apt install python3.9-distutils
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3.9 get-pip.py
```
### Install AWS CLI and configure
- Run `python3 -m pip install awscli`
- Run `aws configure`  
- Copy and paste access key and secret key from Excel
- Specify region `eu-west-1`
- Specify format `json`

# Using boto3 with AWS
### Initial set up
- Run `pip install boto3` in the GitBash terminal
- Create a new directory in EC2 instance `mkdir DIRECTORYNAME`
- Enter directory and create python file `touch FILENAME.py` then `sudo nano FILENAME.py`
- Create a new python file for each case below

### Show all existing buckets 
````
import boto3
s3 = boto3.client('s3')

response = s3.list_buckets()

print('Existing buckets:')
for bucket in response['Buckets']:
    print(f'  {bucket["Name"]}')
````
### Create a bucket using boto3
````
import boto3
s3 = boto3.client('s3')

s3.create_bucket(Bucket='eng89-saim-boto', CreateBucketConfiguration={
    'LocationConstraint': 'eu-west-1'})
````
### Upload file using boto3
````
import boto3
s3 = boto3.client('s3')

s3.upload_file('test.txt', 'eng89-saim-boto', 'test.txt')
````
### Downloading file using boto3
````
import boto3
s3 = boto3.client('s3')

s3.download_file('eng89-saim-boto', 'test.txt', 'test2.txt')
````
### Deleting file using boto3
````
import boto3
s3 = boto3.client('s3')

s3.delete_object(Bucket='eng89-saim-boto', Key='test.txt')
````
### Delete bucket using boto3
- Bucket must be empty when deleting
````
import boto3
s3 = boto3.client('s3')

s3.delete_bucket(Bucket='eng89-saim-boto')
````