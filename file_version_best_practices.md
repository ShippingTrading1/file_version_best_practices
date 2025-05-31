# File Management and Version Control: Professional Best Practices

## Table of Contents
1. [Understanding File Management Fundamentals](#file-management-fundamentals)
2. [Naming Conventions That Scale](#naming-conventions)
3. [Directory Structure for Data Science](#directory-structure)
4. [Version Control Without Git](#manual-version-control)
5. [Introduction to Semantic Versioning](#semantic-versioning)
6. [Git for Data Scientists](#git-fundamentals)
7. [Backup and Recovery Strategies](#backup-strategies)
8. [Collaborative File Management](#collaborative-management)

---

## Understanding File Management Fundamentals

Think of file management like organizing a library. A well-organized library allows you to find any book in seconds, while a chaotic one might have you searching for hours. In data science, where you might juggle hundreds of files across multiple projects, good organization isn't just nice to have - it's essential for your sanity and productivity.

### Why File Management Matters in Data Science

Poor file management leads to:
- Lost work when you can't find the "final" version
- Accidentally overwriting important results
- Confusion about which script produced which output
- Difficulty reproducing your analysis months later
- Problems when collaborating with others

Good file management enables:
- Quick retrieval of any file or version
- Clear understanding of project evolution
- Easy collaboration with team members
- Reproducible research
- Efficient backup and recovery

### The Three Pillars of File Management

1. **Naming** - Files have clear, descriptive, consistent names
2. **Organizing** - Files live in logical, hierarchical structures
3. **Versioning** - Changes are tracked and reversible

---

## Naming Conventions That Scale

A good filename is like a good variable name in programming - it should be self-documenting. When you see the filename, you should immediately understand what's inside without opening it.

### Core Principles of Good Filenames

**1. Be Descriptive but Concise**
```
Bad:  data.csv
      analysis.py
      report.docx

Good: customer_sales_2023Q4.csv
      analyze_customer_churn.py
      monthly_revenue_report_202312.docx
```

**2. Use Consistent Date Formats**
```
# ISO 8601 format (YYYY-MM-DD) sorts naturally
2024-01-15_experiment_results.csv
2024-02-03_experiment_results.csv
2024-12-25_experiment_results.csv

# Why this works: 
# - Alphabetical sort = chronological sort
# - No ambiguity (01/02 could be Jan 2 or Feb 1)
# - International standard
```

**3. Avoid Special Characters and Spaces**
```
Bad:  my data & analysis (final).csv
      john's_script.py
      results 12/25/2023.xlsx
      
Good: my_data_and_analysis_final.csv
      johns_script.py
      results_2023-12-25.xlsx

# Why: Special characters can break scripts and command line tools
```

**4. Use Sequential Numbering When Order Matters**
```
# Use leading zeros for proper sorting
01_data_collection.py
02_data_cleaning.py
03_exploratory_analysis.py
04_feature_engineering.py
05_model_training.py

# Not:
1_data_collection.py   # This would sort as 1, 10, 11, ..., 2, 20
```

### Naming Conventions by File Type

**Data Files**
```
# Raw data: Include source and date
raw_twitter_data_2024-01-15.json
raw_sensor_readings_site_A_2024-01.csv

# Processed data: Include processing stage
cleaned_twitter_data_2024-01-15.csv
filtered_sensor_readings_site_A_2024-01.csv
aggregated_daily_readings_2024.csv

# Include version for manually tracked files
customer_database_v01.csv
customer_database_v02_added_email.csv
customer_database_v03_cleaned_duplicates.csv
```

**Code Files**
```
# Scripts: Use verb_noun pattern
process_raw_data.py
generate_report.R
train_model.py
validate_results.py

# Modules/Libraries: Use nouns
data_utils.py
visualization_helpers.R
model_definitions.py

# Configuration files: Be explicit
config_development.yaml
config_production.yaml
database_settings.json
```

**Documentation**
```
# Include document type and date
README.md
PROJECT_NOTES_2024.md
meeting_notes_2024-01-15.md
data_dictionary_v2.xlsx
analysis_methodology_2024-01.pdf
```

**Output Files**
```
# Include what generated them and when
model_performance_metrics_2024-01-15.csv
figure_1_sales_trend_2023.png
results_experiment_42_2024-01-15.json
report_quarterly_analysis_2024Q1.pdf
```

### Advanced Naming Strategies

**Parameterized Naming for Experiments**
```python
# When running multiple experiments, encode parameters in filename
def create_output_filename(model_type, learning_rate, batch_size, timestamp):
    """Generate consistent filename for experiment outputs"""
    return f"results_{model_type}_lr{learning_rate}_bs{batch_size}_{timestamp}.csv"

# Produces: results_random_forest_lr0.01_bs32_20240115_143022.csv
```

**Temporary and Intermediate Files**
```
# Prefix with tmp_ or intermediate_
tmp_large_dataset.csv
intermediate_features_extracted.pkl
cache_processed_data_20240115.pkl

# Easy to identify and clean up later
rm tmp_*.csv
```

---

## Directory Structure for Data Science

A well-organized directory structure is like a map of your project. Anyone (including future you) should be able to understand the project layout immediately.

### Standard Data Science Project Structure

```
project_name/
├── README.md                 # Project overview and setup instructions
├── requirements.txt          # Python dependencies
├── environment.yml           # Conda environment
├── .gitignore               # Files to ignore in version control
│
├── data/                    # All data files
│   ├── raw/                 # Original, immutable data
│   ├── processed/           # Cleaned, transformed data
│   ├── interim/             # Intermediate processing steps
│   └── external/            # Data from third parties
│
├── notebooks/               # Jupyter notebooks
│   ├── 01_data_exploration.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_model_experiments.ipynb
│
├── src/                     # Source code
│   ├── __init__.py
│   ├── data/                # Data processing scripts
│   │   ├── make_dataset.py
│   │   └── clean_data.py
│   ├── features/            # Feature engineering
│   │   └── build_features.py
│   ├── models/              # Model training and prediction
│   │   ├── train_model.py
│   │   └── predict.py
│   └── visualization/       # Plotting and visualization
│       └── visualize.py
│
├── models/                  # Trained model files
│   ├── model_v1_20240115.pkl
│   └── model_v2_20240120.pkl
│
├── reports/                 # Generated analysis
│   ├── figures/             # Generated graphics
│   └── results/             # Final results, metrics
│
├── tests/                   # Unit tests
│   ├── test_data_processing.py
│   └── test_models.py
│
└── logs/                    # Log files
    ├── data_processing_20240115.log
    └── model_training_20240115.log
```

### Organizing by Project Phase

For long-running projects, organize by phase:

```
long_term_project/
├── phase_1_exploration/
│   ├── data/
│   ├── notebooks/
│   └── findings/
│
├── phase_2_development/
│   ├── data/
│   ├── src/
│   └── models/
│
└── phase_3_deployment/
    ├── production_code/
    ├── monitoring/
    └── documentation/
```

### Best Practices for Directory Organization

**1. Keep Raw Data Immutable**
```python
# Never modify files in data/raw/
# Always create new files in data/processed/

# Good practice: Make raw data read-only
import os
os.chmod('data/raw/important_data.csv', 0o444)  # Read-only
```

**2. Use Clear Hierarchies**
```
# Shallow is better than deep
# This is hard to navigate:
data/2024/01/15/morning/sensor_1/raw/temperature/readings.csv

# This is better:
data/raw/sensor_readings_20240115_morning.csv
```

**3. Separate Code from Data**
```
# Don't mix scripts with data files
# Bad:
analysis/
├── data.csv
├── process.py
├── results.xlsx
└── visualize.py

# Good:
analysis/
├── data/
│   ├── input.csv
│   └── output.xlsx
└── scripts/
    ├── process.py
    └── visualize.py
```

**4. Create README Files at Each Level**
```markdown
# data/README.md

## Data Directory Structure

### raw/
Original data files as received. DO NOT MODIFY.
- `sales_2023.csv`: Annual sales data from accounting system
- `customer_list.xlsx`: Customer database export from CRM

### processed/
Cleaned and transformed data ready for analysis.
- `sales_cleaned_2023.csv`: Sales data with cleaned dates and categories
- `customers_geocoded.csv`: Customer list with added geographic coordinates

### interim/
Intermediate processing steps (can be regenerated).
```

---

## Version Control Without Git

Before diving into Git, let's understand manual version control - it's important to know these fundamentals even if you use automated tools.

### Manual Version Control Strategies

**1. Version Numbers in Filenames**
```
# Simple incremental versioning
report_v1.docx
report_v2.docx
report_v3.docx
report_v3_final.docx
report_v3_final_FINAL.docx  # We've all been here!

# Better: Use semantic versioning
report_v1.0.0.docx       # First complete version
report_v1.1.0.docx       # Added new section
report_v1.1.1.docx       # Fixed typos
report_v2.0.0.docx       # Major revision
```

**2. Timestamp-Based Versioning**
```python
# Automated timestamp versioning
from datetime import datetime

def save_with_timestamp(data, base_filename):
    """Save file with automatic timestamp"""
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    filename = f"{base_filename}_{timestamp}.csv"
    data.to_csv(filename)
    print(f"Saved as: {filename}")

# Produces: analysis_results_20240115_143022.csv
```

**3. Change Log Method**
```
project/
├── current/
│   └── analysis.py          # Always the latest version
├── archive/
│   ├── analysis_20240101.py # Archived versions with dates
│   ├── analysis_20240110.py
│   └── analysis_20240115.py
└── CHANGELOG.md             # Document what changed

# CHANGELOG.md content:
## 2024-01-15
- Fixed bug in data filtering
- Added new visualization function

## 2024-01-10
- Refactored main processing loop
- Improved performance by 50%
```

**4. Backup Generations**
```bash
# Automated backup script
#!/bin/bash
backup_file() {
    file=$1
    # Keep last 5 versions
    for i in {4..1}; do
        if [ -f "$file.bak$i" ]; then
            mv "$file.bak$i" "$file.bak$((i+1))"
        fi
    done
    cp "$file" "$file.bak1"
}
```

### Creating a Version History Document

Maintain a version history document for important files:

```markdown
# Version History: Customer Analysis Report

## Version 3.2.1 (2024-01-15)
- Author: John Doe
- Changes: Fixed calculation error in Section 2.3
- Reviewer: Jane Smith

## Version 3.2.0 (2024-01-10)
- Author: John Doe
- Changes: Added new demographics analysis
- Files affected: 
  - analysis.py (added demographic_analysis function)
  - report.docx (new Section 4)

## Version 3.1.0 (2024-01-05)
- Author: Jane Smith
- Changes: Updated methodology based on feedback
- Breaking changes: Column names in output CSV changed
```

---

## Introduction to Semantic Versioning

Semantic versioning (SemVer) is a versioning scheme that conveys meaning about the underlying changes. It's widely used in software development and increasingly in data science projects.

### The Format: MAJOR.MINOR.PATCH

```
Version 2.4.1
   │    │  └── PATCH: Bug fixes, minor corrections
   │    └───── MINOR: New features, backward compatible
   └────────── MAJOR: Breaking changes
```

### When to Increment Each Number

**PATCH Version (x.x.1 → x.x.2)**
- Fix a bug without changing functionality
- Correct documentation typos
- Performance improvements with no API changes

**MINOR Version (x.1.x → x.2.0)**
- Add new features that don't break existing code
- Add new optional parameters
- Deprecate features (but not remove them)

**MAJOR Version (1.x.x → 2.0.0)**
- Remove deprecated features
- Change existing functionality
- Reorganize in ways that break compatibility

### Semantic Versioning in Data Science

**For Analysis Scripts**
```python
"""
Customer Churn Analysis Script
Version: 2.3.1

Version History:
2.3.1 - Fixed date parsing bug
2.3.0 - Added cohort analysis feature
2.2.0 - New visualization options
2.1.0 - Support for multiple data sources
2.0.0 - Completely refactored, new API
1.0.0 - Initial stable release
"""
```

**For Data Schemas**
```yaml
# data_schema_v2.0.0.yaml
version: 2.0.0
changes:
  - field: customer_id
    change: renamed from id (BREAKING)
  - field: email_verified
    change: new field (backward compatible)
  - field: signup_date
    change: format changed to ISO 8601 (BREAKING)
```

**For Models**
```python
# Model versioning
model_versions = {
    "1.0.0": "baseline_logistic_regression.pkl",
    "1.1.0": "logistic_regression_added_features.pkl",
    "2.0.0": "random_forest_model.pkl",  # Algorithm change
    "2.1.0": "random_forest_tuned.pkl",
    "2.1.1": "random_forest_tuned_fix.pkl"
}
```

### Pre-release and Build Metadata

```
1.0.0-alpha      # Alpha release
1.0.0-beta.1     # First beta
1.0.0-rc.1       # Release candidate
1.0.0+20240115   # Build metadata
```

### Practical Version Management

**Version Configuration File**
```python
# version.py
__version__ = "2.3.1"
__version_info__ = (2, 3, 1)

def check_version(required):
    """Check if current version meets requirements"""
    current = __version_info__
    required = tuple(map(int, required.split('.')))
    return current >= required

# Usage
if not check_version("2.0.0"):
    raise RuntimeError("This script requires version 2.0.0 or higher")
```

---

## Git for Data Scientists

Now that you understand manual versioning, let's explore Git - the industry standard for version control. Think of Git as an automated, highly sophisticated version of the manual strategies we just discussed.

### Git Fundamentals for Data Science

**Initial Setup**
```bash
# Configure your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default editor
git config --global core.editor "nano"  # or "vim", "code"

# Helpful aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
```

**Starting a New Project**
```bash
# Initialize repository
cd my_data_project
git init

# Create .gitignore for data science
cat > .gitignore << EOF
# Data files (too large for Git)
*.csv
*.xlsx
*.pkl
*.h5
data/raw/*
data/processed/*

# Keep directory structure
!data/raw/.gitkeep
!data/processed/.gitkeep

# Python
__pycache__/
*.py[cod]
.ipynb_checkpoints/
.env
venv/

# OS files
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
*.swp
EOF

# Initial commit
git add .
git commit -m "Initial project structure"
```

### Data Science Git Workflows

**Feature Branch Workflow**
```bash
# Start new analysis
git checkout -b feature/customer-segmentation

# Work on your analysis
# ... edit files ...

# Save progress
git add src/segmentation.py
git commit -m "Add initial clustering algorithm"

# Continue working
# ... more edits ...

git add notebooks/clustering_exploration.ipynb
git commit -m "Explore optimal cluster numbers"

# Merge back to main
git checkout main
git merge feature/customer-segmentation
```

**Experiment Tracking**
```bash
# Create experiment branches
git checkout -b experiment/neural-network
# Try neural network approach

git checkout -b experiment/gradient-boosting
# Try gradient boosting

# Compare results
git diff experiment/neural-network experiment/gradient-boosting -- reports/metrics.json

# Keep the best approach
git checkout main
git merge experiment/gradient-boosting
```

### Managing Large Files with Git LFS

Data science often involves large files that shouldn't go in regular Git:

```bash
# Install Git LFS
git lfs install

# Track large file types
git lfs track "*.pkl"
git lfs track "*.h5"
git lfs track "*.parquet"
git add .gitattributes

# Now large files are handled efficiently
git add models/large_model.pkl
git commit -m "Add trained model (LFS)"
```

### Git for Jupyter Notebooks

Notebooks are challenging in Git due to metadata and output:

**Option 1: Clear outputs before committing**
```bash
# Install nbstripout
pip install nbstripout

# Configure for repository
nbstripout --install

# Now outputs are automatically stripped
git add analysis.ipynb
git commit -m "Add analysis notebook"
```

**Option 2: Use Jupytext**
```python
# Convert notebooks to Python scripts
jupytext --to py notebook.ipynb

# Version control the Python file
git add notebook.py
```

### Tagging Releases and Milestones

```bash
# Tag important versions
git tag -a v1.0.0 -m "First production model"
git tag -a v2.0.0 -m "New algorithm implementation"

# List tags
git tag

# Push tags
git push origin --tags

# Checkout specific version
git checkout v1.0.0
```

### Git for Collaboration

**Pull Request Workflow**
```bash
# Fork and clone repository
git clone https://github.com/yourusername/project.git
cd project

# Add upstream remote
git remote add upstream https://github.com/original/project.git

# Create feature branch
git checkout -b fix/data-loading-bug

# Make changes and commit
git add src/data_loader.py
git commit -m "Fix date parsing in data loader"

# Push to your fork
git push origin fix/data-loading-bug

# Create pull request on GitHub/GitLab
```

---

## Backup and Recovery Strategies

Even with version control, you need proper backups. Think of this as insurance for your data and work.

### The 3-2-1 Backup Rule

**3** copies of important data
**2** different storage media
**1** offsite backup

### Automated Backup Scripts

**Local Backup Script**
```python
#!/usr/bin/env python3
"""
backup_project.py - Automated project backup
"""
import os
import shutil
import datetime
from pathlib import Path

def backup_project(source_dir, backup_base_dir, project_name):
    """Create timestamped backup of project"""
    
    # Create timestamp
    timestamp = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
    
    # Create backup directory
    backup_dir = Path(backup_base_dir) / f"{project_name}_{timestamp}"
    
    # Files/directories to backup
    include_patterns = [
        "*.py", "*.R", "*.ipynb",  # Code
        "*.md", "*.txt", "*.yml",  # Documentation
        "requirements.txt", "environment.yml",  # Environment
        "config/*", "src/*", "notebooks/*"  # Directories
    ]
    
    # Files to exclude
    exclude_patterns = [
        "__pycache__", ".git", ".ipynb_checkpoints",
        "*.pyc", "venv/", "env/",
        "data/raw/*",  # Large data files
        "*.log", "*.tmp"
    ]
    
    print(f"Creating backup: {backup_dir}")
    
    # Copy files
    shutil.copytree(
        source_dir, 
        backup_dir,
        ignore=shutil.ignore_patterns(*exclude_patterns)
    )
    
    # Create backup info file
    info_file = backup_dir / "backup_info.txt"
    with open(info_file, 'w') as f:
        f.write(f"Backup created: {timestamp}\n")
        f.write(f"Source: {source_dir}\n")
        f.write(f"Files backed up: {count_files(backup_dir)}\n")
    
    # Compress if desired
    shutil.make_archive(str(backup_dir), 'zip', backup_dir)
    shutil.rmtree(backup_dir)  # Remove uncompressed version
    
    print(f"Backup complete: {backup_dir}.zip")
    
    # Clean old backups (keep last 10)
    clean_old_backups(backup_base_dir, project_name, keep=10)

def count_files(directory):
    """Count files in directory"""
    return sum(1 for _ in Path(directory).rglob('*') if _.is_file())

def clean_old_backups(backup_dir, project_name, keep=10):
    """Remove old backups, keeping most recent 'keep' backups"""
    backups = sorted(
        Path(backup_dir).glob(f"{project_name}_*.zip"),
        key=lambda x: x.stat().st_mtime,
        reverse=True
    )
    
    for backup in backups[keep:]:
        backup.unlink()
        print(f"Removed old backup: {backup}")

if __name__ == "__main__":
    backup_project(
        source_dir=".",
        backup_base_dir="/path/to/backups",
        project_name="my_analysis"
    )
```

**Cloud Backup Integration**
```python
# Backup to cloud storage
import boto3  # For AWS S3
from google.cloud import storage  # For Google Cloud

def backup_to_s3(local_file, bucket_name, s3_key):
    """Backup file to AWS S3"""
    s3 = boto3.client('s3')
    s3.upload_file(local_file, bucket_name, s3_key)
    print(f"Uploaded to s3://{bucket_name}/{s3_key}")

def backup_to_gcs(local_file, bucket_name, blob_name):
    """Backup file to Google Cloud Storage"""
    client = storage.Client()
    bucket = client.bucket(bucket_name)
    blob = bucket.blob(blob_name)
    blob.upload_from_filename(local_file)
    print(f"Uploaded to gs://{bucket_name}/{blob_name}")
```

### Recovery Procedures

**Document Your Recovery Process**
```markdown
# Disaster Recovery Plan

## Quick Recovery Steps
1. Locate most recent backup
2. Verify backup integrity
3. Restore to new location
4. Verify data integrity
5. Update paths and configurations

## Backup Locations
- Local: /backup/drive/project_backups/
- Cloud: s3://my-backups/data-science-projects/
- Version Control: https://github.com/username/project

## Recovery Commands
```bash
# Restore from local backup
unzip /backups/project_20240115.zip -d /restore/location/

# Restore from S3
aws s3 cp s3://my-backups/project_20240115.zip .
unzip project_20240115.zip

# Restore from Git tag
git clone https://github.com/username/project.git
cd project
git checkout v1.0.0
```
```

---

## Collaborative File Management

Working with others requires additional considerations for file management. Clear communication through file organization prevents conflicts and confusion.

### Shared Project Conventions

**Team Agreement Document**
```markdown
# Team File Management Agreement

## Naming Conventions
- Dates: YYYY-MM-DD format
- Versions: Semantic versioning (MAJOR.MINOR.PATCH)
- Personal work: Prefix with initials (jd_analysis.py)
- Shared work: No prefix (shared_utils.py)

## Directory Ownership
- /data/raw/ - Data Team (read-only for others)
- /src/models/ - ML Team
- /reports/ - All teams can write
- /production/ - DevOps team only

## Review Process
- All changes to /src/core/ require code review
- New data in /data/raw/ requires documentation
- Model updates require performance comparison

## Communication
- Major changes: Email team
- Minor updates: Slack notification
- Breaking changes: Team meeting required
```

### Handling Conflicts

**File Locking Strategy**
```python
# Simple file locking mechanism
import fcntl
import time

class FileLock:
    def __init__(self, filename):
        self.filename = filename
        self.lock_file = f"{filename}.lock"
        
    def acquire(self, timeout=30):
        """Acquire lock with timeout"""
        start_time = time.time()
        while time.time() - start_time < timeout:
            try:
                self.fd = open(self.lock_file, 'w')
                fcntl.flock(self.fd, fcntl.LOCK_EX | fcntl.LOCK_NB)
                self.fd.write(f"{os.getpid()}\n{time.time()}")
                return True
            except IOError:
                time.sleep(0.1)
        return False
    
    def release(self):
        """Release lock"""
        if hasattr(self, 'fd'):
            fcntl.flock(self.fd, fcntl.LOCK_UN)
            self.fd.close()
            os.unlink(self.lock_file)

# Usage
lock = FileLock('important_data.csv')
if lock.acquire():
    try:
        # Process file
        process_data('important_data.csv')
    finally:
        lock.release()
else:
    print("Could not acquire lock, file in use")
```

### Documentation Standards

**File Header Template**
```python
"""
Filename: process_customer_data.py
Author: Jane Doe
Created: 2024-01-15
Modified: 2024-01-20
Version: 1.2.0

Description:
    Process raw customer data for analysis. This script handles
    data cleaning, validation, and transformation.

Dependencies:
    - pandas >= 1.3.0
    - numpy >= 1.21.0
    
Usage:
    python process_customer_data.py --input raw.csv --output clean.csv
    
Change Log:
    1.2.0 - Added email validation
    1.1.0 - Improved performance for large files
    1.0.0 - Initial version
"""
```

**Data File Documentation**
```yaml
# metadata/customer_data_v2.yaml
file_info:
  name: customer_data_v2.csv
  created: 2024-01-15
  creator: John Doe
  source: CRM system export
  version: 2.0.0

schema:
  customer_id:
    type: integer
    description: Unique customer identifier
    constraints: 
      - not_null
      - unique
  
  email:
    type: string
    description: Customer email address
    format: email
    version_added: 2.0.0
  
  signup_date:
    type: date
    description: Date customer created account
    format: YYYY-MM-DD
    
processing_notes:
  - Email field added in v2.0.0
  - Dates converted from MM/DD/YYYY to ISO format
  - Duplicate customers removed based on email
```

### Final Thoughts on File Management

Good file management is like good hygiene - it's not glamorous, but neglecting it leads to serious problems. The time you invest in organizing your files properly will pay dividends when you need to:
- Find that analysis you did six months ago
- Share your work with a new team member
- Reproduce results for a publication
- Recover from a system crash

Remember these key principles:
1. **Consistency is king** - Pick a system and stick to it
2. **Document everything** - Your future self will thank you
3. **Automate when possible** - Scripts don't forget
4. **Plan for disaster** - Backups are not optional
5. **Communicate with your team** - Shared conventions prevent conflicts

The best file management system is the one you'll actually use. Start simple, be consistent, and gradually add sophistication as your projects grow. Your organized files are the foundation upon which great data science is built!