# JobBoard Project

## 🕸️ Web Scraping with AWS Lambda, S3, SNS & Static Website

### Project Overview

This project implements a web scraping pipeline using AWS Lambda, Docker, S3, CloudWatch, and SNS. The scraped data is then displayed on a static website hosted in an S3 bucket. This project scrapes job postings from various websites, stores them in JSON format in Amazon S3, sends job alerts using SNS, and displays the job data on a static website hosted in S3.


### Technologies Used

- **Python**: The core programming language used for the scraping scripts.
- **Selenium**: For automating browser interactions and handling dynamically loaded content.
- **BeautifulSoup**: For parsing HTML and extracting data.
- **XPath & CSS Selectors**: For precise and flexible data extraction from HTML elements.
- **HTML, CSS, JavaScript**: For the static website.
- **AWS Lambda**: To run scraping logic.
- **Docker**: To package and deploy Lambda.
- **AWS S3**: For job data storage and website hosting.
- **AWS CloudWatch**: To schedule automated scraping.
- **AWS SNS**: To send job alerts to users.


### Prerequisites

- **AWS Account**: You’ll need an AWS account to access services like Lambda, S3, SNS, and CloudWatch. Note: I used a free-tier account.
- **Docker**: For building and deploying the Lambda function.
- **Node.js and npm**: To install the Serverless Framework.
- **Serverless Framework**: To simplify Lambda deployments.
  ```bash
   npm install -g serverless
   ```


### Set-by-step Instructions

1. **First, clone the repository that contains the Serverless configuration for deploying the Docker-based Lambda function.:**
   ```bash
   git clone https://github.com/umihico/docker-selenium-lambda.git
   cd docker-selenium-lambda
   ```

2.	**Configure AWS Credentials:**
Make sure your AWS credentials are set up properly. You can configure them using the AWS CLI:

   ```bash
   aws configure
   ```
Enter your Access Key ID, Secret Access Key, region, and output format when prompted.


3.	**Deploy Lambda with Docker Using Serverless Framework:**
The serverless.yml in the repository defines how to deploy the scraping function to AWS Lambda using Docker. This automatically packages your Docker image and deploys it to Lambda.
```bash
sls deploy
```
This command will:

- Build a Docker image with headless Chrome, ChromeDriver, and Selenium.
- Push the Docker image to Amazon Elastic Container Registry (ECR).
- Deploy the Lambda function to AWS Lambda using the ECR image.

4.	**Set Up S3 Bucket for Data Storage and Static Website:**
	1.	Create an S3 bucket where the scraped job data will be stored as JSON files.
	2.	Enable static website hosting for this bucket:
	- Navigate to the Properties tab.
	- Under Static website hosting, choose Enable.
	- Set the Index Document to index.html.
	3.	Set public read permissions for the bucket and objects:
	- In the Permissions tab, configure the Bucket Policy and CORS to allow public access.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

5. **Implement CloudWatch for Scheduling:**
Set up CloudWatch to automatically trigger the Lambda function based on a schedule (e.g., every Sunday at 8 AM UTC). Here’s how to set up a rule:

- Go to CloudWatch → Rules → Create Rule.
- Set the event source to Schedule and define the cron expression (e.g., 0 8 ? * 1 *).
- Add your Lambda function as the target.

6. **Send Job Alerts Using SNS:**
Set up SNS (Simple Notification Service) to send job alerts for specific job titles (e.g., Data Engineer, ML Engineer):

- Create an SNS Topic in the SNS console.
- Subscribe your email or phone number to the topic.
- In your Lambda function, use the boto3 library to publish to SNS when new jobs are found:
```python
import boto3

sns_client = boto3.client('sns')
sns_topic_arn = 'arn:aws:sns:your-region:account-id:your-topic-name'

def publish_to_sns(job_title, company_name, job_link):
    message = f"New job posted: {job_title} at {company_name} - {job_link}"
    sns_client.publish(
        TopicArn=sns_topic_arn,
        Message=message,
        Subject=f"New Job Alert: {job_title}"
    )
```

7. **Set Up the Static Website:**
- Write an index.html file that will display the job postings stored in S3.
- Fetch and display the job data from your S3 bucket using JavaScript:
```javascript
const batchUrls = {
    batch1: 'https://your-bucket-name.s3.amazonaws.com/scraped_jobs_batch1.json',
    // Add more batches here
};

function fetchAndDisplayJobs(batch, elementId) {
    fetch(batchUrls[batch])
        .then(response => response.json())
        .then(data => {
            const jobList = document.getElementById(elementId);
            Object.keys(data).forEach(company => {
                data[company].forEach(job => {
                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td>${company}</td>
                        <td>${job.title}</td>
                        <td><a href="${job.link}" target="_blank">Apply Now</a></td>
                    `;
                    jobList.appendChild(row);
                });
            });
        })
        .catch(error => {
            console.error(`Error fetching jobs for ${batch}:`, error);
        });
}
```

8. **Test Everything**
- 	Manually invoke your Lambda function using the Serverless CLI to ensure the data is correctly written to S3.
```bash
sls invoke --function demo
```
- 	Access your static website and check if the job board displays data correctly.


### Challenges Faced and Resolved
1.	**Lambda Timeout:** faced timeouts during scraping for larger websites. The solution was to increase the Lambda timeout in serverless.yml:

2. **S3 Bucket Permissions:** initially had issues with public access to the job board. Resolved this by updating the bucket policy to allow public read access to all objects.

3.	**Selenium in Lambda:** Using headless Chrome in Lambda with Docker simplified the deployment. I used the docker-selenium-lambda repository for a pre-configured environment.


### Conclusion

With this setup, you can scrape jobs from websites, automatically trigger the scraping with CloudWatch, and send alerts using SNS, while displaying the job data on a static S3-hosted website.

Feel free to explore the code and adapt it to your needs. Let us know if you encounter any issues or have ideas for improvements! 🚀

**Author:**

- Freda Victor
- Project Blog: [GitHub Repository Link]
- Date: September 2024