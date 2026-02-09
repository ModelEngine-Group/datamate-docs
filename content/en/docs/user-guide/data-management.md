---
title: Data Management
description: Manage datasets and files with DataMate
weight: 2
---

{{% pageinfo %}}
Data management module provides unified dataset management capabilities, supporting multiple data types for storage, query, and operations.
{{% /pageinfo %}}

## Features Overview

Data management module provides:

- **Multiple data types**: Image, text, audio, video, and multimodal support
- **File management**: Upload, download, preview, delete operations
- **Directory structure**: Support for hierarchical directory organization
- **Tag management**: Use tags to categorize and retrieve data
- **Statistics**: Dataset size, file count, and other statistics

## Dataset Types

| Type | Description | Supported Formats |
|------|-------------|-------------------|
| Image | Image data | JPG, PNG, GIF, BMP, WebP |
| Text | Text data | TXT, MD, JSON, CSV |
| Audio | Audio data | MP3, WAV, FLAC, AAC |
| Video | Video data | MP4, AVI, MOV, MKV |
| Multimodal | Multimodal data | Mixed formats |

## Quick Start

### 1. Create Dataset

#### Step 1: Enter Data Management Page

In the left navigation, select **Data** → **Data Management**.

#### Step 2: Create Dataset

Click the **Create Dataset** button in the upper right corner.

#### Step 3: Fill Basic Information

- **Dataset name**: e.g., `user_images_dataset`
- **Dataset type**: Select data type (e.g., Image)
- **Description**: Dataset purpose description (optional)
- **Tags**: Add tags for categorization (optional)

#### Step 4: Configure Storage Location (Optional)

- **Data source**: Data origin (optional)
- **Target location**: Storage path (optional)

#### Step 5: Create Dataset

Click the **Create** button to complete.

### 2. Upload Files

#### Method 1: Drag & Drop

1. Enter dataset details page
2. Drag files directly to the upload area
3. Wait for upload completion

#### Method 2: Click Upload

1. Click **Upload File** button
2. Select local files
3. Wait for upload completion

#### Method 3: Chunked Upload (Large Files)

For large files (>100MB), the system automatically uses chunked upload:

1. Select large file to upload
2. System automatically splits the file
3. Upload chunks one by one
4. Automatically merge

### 3. Create Directory

#### Step 1: Enter Dataset

Click dataset name to enter details.

#### Step 2: Create Directory

1. Click **Create Directory** button
2. Enter directory name
3. Select parent directory (optional)
4. Click confirm

Directory structure example:
```
user_images_dataset/
├── train/
│   ├── cat/
│   └── dog/
├── test/
│   ├── cat/
│   └── dog/
└── validation/
    ├── cat/
    └── dog/
```

### 4. Manage Files

#### View Files

In dataset details page, you can see all files:

| Filename | Size | Type | Status | Upload Time | Actions |
|----------|------|------|--------|-------------|---------|
| image1.jpg | 2.3 MB | JPG | Completed | 2024-01-15 | Preview Download Delete |

#### Preview File

Click **Preview** button to preview in browser:

- **Image**: Display thumbnail and details
- **Text**: Display text content
- **Audio**: Online playback
- **Video**: Online playback

#### Download File

- **Single file download**: Click **Download** button
- **Batch download**: Select multiple files, click **Batch Download**
- **Package download**: Click **Download All**, download as ZIP

### 5. Dataset Operations

#### View Statistics

In dataset details page, you can see:

- **Total files**: Total number of files in dataset
- **Total size**: Total size of all files
- **Completion rate**: File upload completion percentage
- **File type distribution**: Count of each file type
- **Status distribution**: Count of each status

#### Edit Dataset

Click **Edit** button to modify:

- Dataset name
- Description
- Tags
- Status

#### Delete Dataset

Click **Delete** button to delete entire dataset.

**Note**: Deleting a dataset will also delete all files within it. This action cannot be undone.

## Advanced Features

### Tag Management

#### Create Tag

1. In dataset list page, click **Tag Management**
2. Click **Create Tag**
3. Enter tag information:
   - Tag name
   - Tag color
   - Description (optional)

#### Use Tags

1. Edit dataset
2. Select existing tags in tag bar
3. Save dataset

#### Filter by Tags

In dataset list page, click tags to filter datasets with that tag.

### File Search

#### Keyword Search

Enter keyword in search box, supports searching:

- Filename
- File description
- Tags

#### Advanced Search

Click **Advanced Search** to:

- Filter by file type
- Filter by upload time
- Filter by file size
- Filter by status

## Best Practices

### 1. Dataset Organization

Recommended directory organization:

```
project_dataset/
├── raw/              # Raw data
├── processed/        # Processed data
├── train/            # Training data
├── validation/       # Validation data
└── test/             # Test data
```

### 2. Naming Conventions

- **Dataset name**: Use lowercase letters and underscores, e.g., `user_images_2024`
- **Directory name**: Use meaningful English names, e.g., `train`, `test`, `processed`
- **File name**: Keep original filename or use standardized naming

### 3. Tag Usage

Recommended tag categories:

- **Project tags**: `project-a`, `project-b`
- **Status tags**: `raw`, `processed`, `validated`
- **Type tags**: `image`, `text`, `audio`
- **Purpose tags**: `training`, `testing`, `evaluation`

## Common Questions

### Q: Large file upload fails?

**A**: Suggestions for large file uploads:

1. **Use chunked upload**: System automatically enables chunked upload
2. **Check network**: Ensure stable network connection
3. **Adjust upload parameters**: Increase timeout
4. **Use FTP/SFTP**: For very large files, use FTP upload

### Q: How to import existing data?

**A**: Three methods to import existing data:

1. **Upload files**: Upload via interface
2. **Add files**: If files already on server, use add file feature
3. **Data collection**: Use data collection module to collect from external sources

### Q: Dataset size limit?

**A**: Dataset size limits:

- **Single file**: Maximum 5GB (chunked upload)
- **Total dataset**: Limited by storage space
- **File count**: No explicit limit

Regularly clean unnecessary files to free up space.

## API Reference

For detailed API documentation, see:
- [Data Management API](/docs/api-reference/data-management/)

## Related Documentation

- [Data Collection](/docs/user-guide/data-collection/) - Collect data to datasets
- [Data Cleaning](/docs/user-guide/data-cleansing/) - Clean dataset data
- [Data Annotation](/docs/user-guide/data-annotation/) - Annotate dataset data
