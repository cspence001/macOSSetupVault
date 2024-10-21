
## AWS CLI Notes

---
### Getting Started with AWS CLI

- **Installation and Setup**
  - [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
  - [AWS CLI Configuration Files](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)
  
- **Basic Commands**
  - Configure AWS CLI Profile: 
    ```bash
    aws configure --profile produser
    ```
  - List S3 Buckets:
    ```bash
    aws s3 ls --profile produser
    ```

### IAM Access Key Management

- **Creating IAM Access Key**
  - [AWS CLI Create Access Key](https://docs.aws.amazon.com/cli/latest/reference/iam/create-access-key.html)

### S3 Operations

- **Copy Files to S3**
  - Copy Data Recursively to S3 Bucket:
    ```bash
    aws s3 cp /Users/meuthu/data s3://mapdata-0301/ --recursive --exclude ".DS_Store"
    ```

- **Synchronize Local Directory with S3**
  - Sync Local Directory with S3 Bucket:
    ```bash
    aws s3 sync /tmp/foo s3://bucket/
    ```

### S3 Configuration Settings

- **Adjusting S3 Configurations**
  - [AWS S3 Configuration Settings](https://docs.aws.amazon.com/cli/latest/topic/s3-config.html)
  - Set Maximum Concurrent S3 Requests:
    ```bash
    aws configure set s3.max_concurrent_requests 20
    ```
  - Set Maximum Concurrent S3 Requests for a Specific Profile:
    ```bash
    aws configure set s3.max_concurrent_requests 20 --profile meuthu_dev_cli
    ```

---
