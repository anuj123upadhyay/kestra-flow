# 🧹 System Log Cleanup Workflow

A smart, automated system log cleanup solution built with Kestra that archives old log files and uploads them to AWS S3 for long-term storage.

![Kestra](https://img.shields.io/badge/Kestra-Workflow-blue?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTEyIDJMMTMuMDkgOC4yNkwyMCAxMkwxMy4wOSAxNS43NEwxMiAyMkwxMC45MSAxNS43NEw0IDEyTDEwLjkxIDguMjZMMTIgMloiIGZpbGw9IiMwMDcwRjMiLz4KPC9zdmc+)
![AWS S3](https://img.shields.io/badge/AWS-S3-orange?style=for-the-badge&logo=amazon-aws)
![Schedule](https://img.shields.io/badge/Schedule-Daily-green?style=for-the-badge&logo=clock)

## 🌟 Overview

This workflow automatically cleans up old system logs on a daily schedule, creating compressed archives and storing them safely in AWS S3. Perfect for maintaining clean systems while preserving important log data for compliance and debugging purposes.

## 🔄 Workflow Architecture



![alt text](<Screenshot 2025-06-13 at 1.29.44 PM.png>)


![alt text](<Screenshot 2025-06-13 at 1.25.56 PM.png>)



![alt text](<Screenshot 2025-06-13 at 1.25.02 PM.png>)


![alt text](<Screenshot 2025-06-13 at 1.25.23 PM.png>)


![alt text](<Screenshot 2025-06-13 at 1.25.38 PM.png>)
## ⚙️ Configuration

### 📅 Schedule
- **Frequency**: Daily at 2:00 AM
- **Timezone**: Asia/Kolkata (IST)
- **Cron Expression**: `0 2 * * *`

### 🏗️ Tasks Breakdown

#### 1. 🔍 Find & Archive (`find-and-archive`)
- **Container**: Alpine Linux
- **Purpose**: Locate old log files (*.old) and create ZIP archives
- **Output**: Archive file, status, and file list

#### 2. ☁️ S3 Upload (`upload-to-s3`)
- **Purpose**: Upload archives to AWS S3 bucket
- **Condition**: Only runs if archive was successfully created
- **Naming**: `logs/old_logs_YYYYMMDD_HHMMSS.zip`

#### 3. 📢 Notification (`cleanup-notification`)
- **Purpose**: Report cleanup status and results
- **Output**: Summary of archived files and operation status

## 🚀 Quick Start

### Prerequisites
- Kestra instance running
- AWS S3 bucket configured
- AWS credentials stored in Kestra secrets

### Required Secrets
```yaml
AWS_ACCESS_KEY_ID: your-aws-access-key
AWS_SECRET_ACCESS_KEY_ID: your-aws-secret-key
```

### Deployment
1. Clone or download the workflow file
2. Update the S3 bucket name in the configuration
3. Deploy to your Kestra instance
4. Configure AWS credentials in Kestra secrets

## 📊 Monitoring & Outputs

### 📁 Generated Files
- `old_logs_archive.zip` - Compressed log archive
- `archive_name.txt` - Archive filename reference
- `old_files.txt` - List of processed files
- `status.txt` - Operation status indicator

### 🎯 Status Codes
- `ARCHIVE_CREATED` - Successfully created and ready for upload
- `ARCHIVE_FAILED` - Archive creation failed
- `NO_ARCHIVE` - No old files found to archive

## 🛠️ Customization

### Modify Schedule
```yaml
triggers:
  - id: daily-cleanup
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "0 2 * * *"  # Change this cron expression
    timezone: "Asia/Kolkata"  # Update timezone as needed
```

### Common Schedule Examples
- **Hourly**: `0 * * * *`
- **Weekly (Sunday midnight)**: `0 0 * * 0`
- **Monthly (1st day, 1 AM)**: `0 1 1 * *`

### Update S3 Configuration
```yaml
bucket: "your-bucket-name"  # Update bucket name
region: "us-east-1"         # Update AWS region
key: "logs/old_logs_{{ now() | date('yyyyMMdd_HHmmss') }}.zip"  # Customize path
```

## 🔒 Security Features

- ✅ AWS credentials stored securely in Kestra secrets
- ✅ Conditional execution prevents unnecessary S3 operations
- ✅ Isolated container execution for security
- ✅ Comprehensive logging for audit trails

## 📈 Benefits

- 🎯 **Automated**: No manual intervention required
- 💾 **Space Efficient**: Compresses old logs before archiving
- ☁️ **Cloud Backup**: Secure storage in AWS S3
- 📊 **Monitoring**: Built-in status reporting
- 🔄 **Reliable**: Conditional logic handles edge cases
- 🕐 **Scheduled**: Runs automatically on your preferred schedule

## 🛡️ Error Handling

The workflow includes robust error handling:
- Archive creation validation
- S3 upload conditional execution
- Comprehensive status reporting
- Graceful handling of "no files" scenarios

## 📝 Example Output

```
Creating test log files...
Found old files:
/tmp/logs/app.log.old
/tmp/logs/error.log.old
Creating archive: old_logs_archive.zip
Archive created successfully: old_logs_archive.zip
Archive uploaded successfully to S3
Cleanup completed successfully
```

## 🤝 Contributing

Feel free to fork this workflow and customize it for your specific needs! Some enhancement ideas:
- Add email notifications
- Implement retention policies
- Support for multiple log directories
- Integration with monitoring systems

---

**Built with ❤️ using Kestra Workflow Engine**

*For more Kestra workflows and automation solutions, visit [Kestra Documentation](https://kestra.io/docs)*