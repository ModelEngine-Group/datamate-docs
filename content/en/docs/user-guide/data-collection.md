---
title: Data Collection
description: Collect data from multiple data sources with DataMate
weight: 1
---

{{% pageinfo %}}
Data collection module helps you collect data from multiple data sources (databases, file systems, APIs, etc.) into the DataMate platform.
{{% /pageinfo %}}

## Features Overview

Based on [DataX](https://github.com/alibaba/DataX), data collection module supports:

- **Multiple Data Sources**: MySQL, PostgreSQL, Oracle, SQL Server, etc.
- **Heterogeneous Sync**: Data sync between different sources
- **Batch Collection**: Large-scale batch collection and sync
- **Scheduled Tasks**: Support scheduled execution
- **Task Monitoring**: Real-time monitoring of collection tasks

## Supported Data Sources

| Data Source Type | Reader | Writer | Description |
|------------------|--------|--------|-------------|
| General Relational Databases | ✅ | ✅ | Supports MySQL, PostgreSQL, OpenGauss, SQL Server, DM, DB2 |
| MySQL | ✅ | ✅ | Relational database |
| PostgreSQL | ✅ | ✅ | Relational database |
| OpenGauss | ✅ | ✅ | Relational database |
| SQL Server | ✅ | ✅ | Microsoft database |
| DM (Dameng) | ✅ | ✅ | Domestic database |
| DB2 | ✅ | ✅ | IBM database |
| StarRocks | ✅ | ✅ | Analytical database |
| NAS | ✅ | ✅ | Network storage |
| S3 | ✅ | ✅ | Object storage |
| GlusterFS | ✅ | ✅ | Distributed file system |
| API Collection | ✅ | ✅ | API interface data |
| JSON Files | ✅ | ✅ | JSON format files |
| CSV Files | ✅ | ✅ | CSV format files |
| TXT Files | ✅ | ✅ | Text files |
| FTP | ✅ | ✅ | FTP servers |
| HDFS | ✅ | ✅ | Hadoop HDFS |

## Quick Start

### 1. Create Collection Task

#### Step 1: Enter Data Collection Page

Select **Data Collection** in the left navigation.

#### Step 2: Create Task

Click **Create Task** button.

#### Step 3: Configure Basic Information

Fill in the following basic information:

- **Name**: A meaningful name for the task
- **Timeout**: Task execution timeout (seconds)
- **Description**: Task purpose (optional)

#### Step 4: Select Sync Mode

Select the task synchronization mode:

- **Immediate Sync**: Execute once immediately after task creation
- **Scheduled Sync**: Execute periodically according to schedule rules

When selecting **Scheduled Sync**, configure the execution policy:

- **Execution Cycle**: Hourly / Daily / Weekly / Monthly
- **Execution Time**: Select the execution time point

#### Step 5: Configure Data Source

**Select data source type**: Choose from dropdown list (e.g., MySQL, CSV, etc.)

**Configure data source parameters**: Fill in connection parameters based on the selected data source template (form format)

**MySQL Example**:
- JDBC URL: `jdbc:mysql://localhost:3306/mydb`
- Username: `root`
- Password: `password`
- Table Name: `users`

#### Step 6: Configure Field Extraction

Field mapping is not supported. You can only extract specific fields from the configured SQL.

- **Extract specific fields**: Enter the field names you want to extract in the field list
- **Extract all fields**: Leave the field list empty to extract all fields from the SQL query result

#### Step 7: Create and Execute

Click **Create** button to create the task.
- If **Immediate Sync** is selected, task starts immediately
- If **Scheduled Sync** is selected, task runs periodically according to schedule

### 2. Monitor Task Execution

View all collection tasks with status, progress, and operations.

### 3. Task Management

Each task in the task list has the following actions available:

- **View Execution Records**: View all historical executions of the task
- **Delete**: Delete the task (note: deleting a task does not delete collected data)

Click the task name to view task details including:
- Basic configuration
- Execution record list
- Data statistics

## Common Questions

### Q: Task execution failed?

**A**: Troubleshooting:
1. Check data source connection
2. View execution logs
3. Check data format
4. Verify target dataset exists

### Q: How to collect large tables?

**A**:
1. Use incremental collection
2. Split into multiple tasks
3. Adjust concurrent parameters
4. Use filter conditions

## API Reference

- [Data Collection API](/docs/api-reference/data-collection/)

## Related Documentation

- [Data Management](/docs/user-guide/data-management/) - Manage collected data
- [Data Cleaning](/docs/user-guide/data-cleansing/) - Clean collected data
