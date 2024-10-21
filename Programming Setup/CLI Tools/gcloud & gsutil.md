
---

## GSUTIL CLI Notes

### 1. Installation and Configuration

- **Installation Guide**
  - [Installing gsutil](https://cloud.google.com/storage/docs/gsutil_install)

- **Brew Configuration for Google Cloud SDK**
  - [Brew Installation and Configuration](https://gist.github.com/dwchiang/10849350?permalink_comment_id=3057516#gistcomment-3057516)
  - [Stack Overflow: Brew Setup](https://stackoverflow.com/a/66791086)

- **Boto Configuration for gsutil**
  - [gsutil Boto Configuration](https://cloud.google.com/storage/docs/boto-gsutil)

- **AWS Configuration for GCS**
  - [Stack Overflow: GCS to S3 Configuration](https://stackoverflow.com/a/54746014)

- **Global Configuration Paths for gcloud**
  - [Uninstalling Cloud SDK](https://cloud.google.com/sdk/docs/uninstall-cloud-sdk)
  - [Stack Overflow: gcloud Config Paths](https://stackoverflow.com/a/33920973)

- **gsutil Configuration Paths**
  - [Stack Overflow: gsutil Config Paths](https://stackoverflow.com/a/43038123)

---

### 2. Commands

- **Synchronize Data**
  - Sync Google Cloud Storage to S3 Bucket:
    ```bash
    gsutil -m rsync -rd gs://storagename s3://bucketname
    ```
    - [gsutil rsync Command](https://cloud.google.com/storage/docs/gsutil/commands/rsync)
  
  - Sync Local Directory to GCS Bucket:
    ```bash
    gsutil -m rsync -rd meuthu/folders/repos/data gs://turbo-octo-goggles/turbo-octo-goggles/data
    ```

  - Sync GCS to Local Directory:
    ```bash
    gsutil -m rsync -rd gs://ppp_data_backup meuthu/folders/repos/data
    ```

  - Sync Local Directory to GCS Bucket (Recursive, Multithreaded, Delete Objects Not in Source):
    ```bash
    gsutil -m rsync -rd folder/repos/data gs://ppp_data_backup
    ```

  - Copy Local Directory to GCS Bucket (Multithreaded):
    ```bash
    gsutil -m cp -r meuthu/folder/repos/data gs://ppp_data_backup
    ```

- **List Objects in Bucket**
  - List All Objects in a Bucket:
    ```bash
    gsutil ls -r gs://ppp_data_backup
    ```

  - Flat Listing of All Objects in Bucket:
    ```bash
    gsutil ls -r gs://ppp_data_backup/**
    ```

  - List Objects with Size and Metadata:
    ```bash
    gsutil ls -l gs://ppp_data_backup
    ```

  - Detailed Listing with Size, Creation Time, and Name:
    ```bash
    gsutil ls -l -r gs://ppp_data_backup
    ```

  - Additional Details about Objects and Buckets:
    ```bash
    gsutil ls -L gs://ppp_data_backup
    gsutil ls -L gs://ppp_data_backup/**
    ```

  - Save Directory Listings to File:
    ```bash
    gsutil ls -r gs://ppp_data_backup/** > folder/repos/data/documents/directorylistings/gcloudfilelisting.txt
    gsutil ls -l -r gs://ppp_data_backup/** > folder/repos/data/documents/directorylistings/gcloudbackup.txt
    ```

- **Remove `.DS_Store` Files**
  - Remove `.DS_Store` Files from GCS Bucket:
    ```bash
    gsutil -m rm -r gs://ppp_data_backup/**/.DS_Store
    ```
    - Save Bucket Item Listing:
      ```bash
      folder/repos/data/documents/directorylistings/gcloudbucketitem.txt
      ```

---

### 3. Comparison with gcloud

- **Differences Between `gcloud` and `gsutil`**
  - `gcloud` can create and manage Google Cloud resources while `gsutil` cannot.
  - `gsutil` can manipulate buckets, objects, and ACLs on Google Cloud Storage (GCS) while `gcloud` cannot.

### 4. Additional Resources

- **gcloud Storage Update** (10/2022)
  - [New gcloud Storage CLI for Data Transfers](https://cloud.google.com/blog/products/storage-data-transfer/new-gcloud-storage-cli-for-your-data-transfers)

- **gsutil Legacy Status** (04/2024)
  - [Exploring gsutil: A Comprehensive Guide](https://christianbaghai.medium.com/exploring-gsutil-a-comprehensive-guide-to-google-cloud-storage-management-fa2901b4656e)

- **Cheat Sheet for gcloud and gsutil** (07/2020)
  - [Cheat Sheets for gcloud, bq, gsutil, kubectl](https://medium.com/@raigonjolly/cheat-sheets-gcloud-bq-gsutil-kubectl-for-google-cloud-associate-certificate-4093b8977a01)
