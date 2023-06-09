# Static Website Hosting on AWS S3 and distributed via CloudFront

This repository contains screenshot on static website hosting deployed on AWS S3 and served via cloudfront, for udacity project.

#### The site sample is live at github page https://vitanet-tech.github.io/static-website-available-via-CloudFront/

## Steps:

### -Create S3 Bucket from AWS Management Console
- Navigate to the “AWS Management Console” page, type “S3” in the “Find Services” box and then select “S3”
- The Amazon S3 dashboard displays. Click “Create bucket”.
- In the General configuration, enter a “Bucket name” and a region of your choice. Note: Bucket names must be globally unique.
- Click “Next” and click “Create bucket”.
- Once the bucket is created, click on the name of the bucket to open the bucket to the contents.


### -Upload files to S3 Bucket
- Once the bucket is open to its contents, click the “Upload” button.
- Click the "Add files" and “Add folder” button, and upload the website code folder content from your local computer to the S3 bucket.
  To upload through AWS CLI
  Verify the AWS CLI configuration. If configured already, use:
  - aws configure list
  - aws configure 
  - aws configure set aws_session_token "<TOKEN>" --profile default" 
  
  Upload files with this code
 ``` 
# Create a PUBLIC bucket in the S3, and verify locally as 
aws s3api list-buckets 
# Download and unzip the udacity-starter-website.zip 
cd udacity-starter-website 
# Assuming the bucket name is my-bucket-202203081 and your PWD is the "udacity-starter-website" folder 
# Put a single file. 
aws s3api put-object --bucket my-bucket-202203081 --key index.html --body index.html 
# Copy over folders from local to S3 
aws s3 cp vendor/ s3://my-bucket-202203081/vendor/ --recursive 
aws s3 cp css/ s3://my-bucket-202203081/css/ --recursive 
aws s3 cp img/ s3://my-bucket-202203081/img/ --recursive 
```

### -Secure Bucket via IAM
  - Click on the “Permissions” tab.
    Go to the Permissions tab. See that the bucket allows public access for hosting.
  - The “Bucket Policy” section shows it is empty. Click on the Edit button.
    Empty bucket policy. Check this policy again after setting up the CloudFront distribution
  - Enter the following bucket policy replacing your-website with the name of your bucket and click “Save”.
  ```
  {
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"AddPerm",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::your-website/*"]
    }
  ]
}
  ```
- You will see warnings about making your bucket public, but this step is required for static website hosting.  
  
  
### -Configure S3 Bucket  
- Go to the Properties tab and then scroll down to edit the Static website hosting section.
- Click on the “Edit” button to see the Edit static website hosting screen. Now, enable the Static website hosting, and provide the default home page and error page for your website.
  - For both “Index document” and “Error document”, enter “index.html” and click “Save”. After successfully saving the settings, check the Static website hosting section again under the Properties tab. You must now be able to view the website endpoint as shown below:
  Copy the website endpoint for future use


### - Distribute Website via CloudFront
- Select “Services” from the top left corner and enter “cloud front” in the “Find a service by name or feature” text box and select “CloudFront”.
- From the CloudFront dashboard, click “Create Distribution”.
- For “Select a delivery method for your content”, click “Get Started”.
- Use the following details to create a distribution:
  
  ![image](https://user-images.githubusercontent.com/99427790/225037357-9b8918f7-6acf-4816-a045-bb6857755d7f.png)
      Configurations - Origin details
      
  ![image](https://user-images.githubusercontent.com/99427790/225038029-255754ac-f3e1-450e-a2ab-0886a0e5ad35.png)
      Configurations - Cache behavior, key and origin requests
      
- Leave the defaults for the rest of the options, and click “Create Distribution”. It may take up to 10 minutes for the CloudFront Distribution to get created.
- Once the status of your distribution changes from “In Progress” to “Deployed”, copy the endpoint URL for your CloudFront distribution found in the “Domain Name” column.
```
Note - Remember, as soon as your CloudFront distribution is Deployed, it attaches to S3 and starts caching the S3 pages. CloudFront may take 10-30 minutes (or more) to cache the S3 page. Once the caching is complete, the CloudFront domain name URL will stop redirecting to the S3 object URL.
```

### - Access Website in Web Browser
  
  - Open a web browser like Google Chrome, and paste the copied CloudFront domain name (such as, d24o2er5d461h3.cloudfront.net) without appending /index.html at the end. The CloudFront domain name should show you the content of the default home-page, as shown below:
  <img width="937" alt="123" src="https://user-images.githubusercontent.com/99427790/225043213-a2ad3384-6aa2-406c-a908-e12c65cc1bcc.PNG">
