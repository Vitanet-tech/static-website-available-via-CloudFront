# Static Website Hosting on AWS S3 and distributed via CloudFront

This repository contains screenshot on static website hosting deployed on AWS S3 and served via cloudfront, for udacity project.

### The site is live at https://vitanet-tech.github.io/static-website-available-via-CloudFront/

### Steps involved:

## Create S3 Bucket from AWS Management Console
- Navigate to the “AWS Management Console” page, type “S3” in the “Find Services” box and then select “S3”
- The Amazon S3 dashboard displays. Click “Create bucket”.
- In the General configuration, enter a “Bucket name” and a region of your choice. Note: Bucket names must be globally unique.
- Click “Next” and click “Create bucket”.
- Once the bucket is created, click on the name of the bucket to open the bucket to the contents.

## Upload files to S3 Bucket
- Once the bucket is open to its contents, click the “Upload” button.
- Click the "Add files" and “Add folder” button, and upload the website code folder content from your local computer to the S3 bucket.
  To upload through AWS CLI
  Verify the AWS CLI configuration. If not configured already, use:
  - aws configure list
  - aws configure 
  - aws configure set aws_session_token "<TOKEN>" --profile default"
  
  Upload files
 ``` # Create a PUBLIC bucket in the S3, and verify locally as 
aws s3api list-buckets 
# Download and unzip the udacity-starter-website.zip 
cd udacity-starter-website 
# Assuming the bucket name is my-bucket-202203081 and your PWD is the "udacity-starter-website" folder 
# Put a single file. 
aws s3api put-object --bucket my-bucket-202203081 --key index.html --body index.html 
# Copy over folders from local to S3 
aws s3 cp vendor/ s3://my-bucket-202203081/vendor/ --recursive 
aws s3 cp css/ s3://my-bucket-202203081/css/ --recursive 
aws s3 cp img/ s3://my-bucket-202203081/img/ --recursive ```
