# Connecting Google Colab to GitHub

This guide outlines the step-by-step workflow used to mount Google Drive, clone a remote GitHub repository, and securely push a Colab notebook (`.ipynb` file) using a Personal Access Token (PAT).

---

## 🛠️ Complete Workflow Summary

### Step 1: Mount Google Drive
Link your Google Drive storage instance directly to the Colab environment runtime.
```python
from google.colab import drive
drive.mount('/content/drive')
```

### Step 2: Dynamically Construct the Authenticated Git URL
To protect URL structures from interface filtering bugs, assemble the repository address programmatically using Python strings.
```python
# Configure credentials
token = "YOUR_GITHUB_PERSONAL_ACCESS_TOKEN"
username = "krishnaditya65"
repo = "neural-network"

# Build URL using list joining to avoid formatting syntax errors
domain = "github.com"
url_parts = ["https:", "", f"{token}@{domain}", username, f"{repo}.git"]
repo_url = "/".join(url_parts)
```

### Step 3: Clone the Remote Repository
Execute the Git utility to clone the remote repository into your workspace file path.
```bash
!git clone {repo_url}
```

### Step 4: Configure Git Profile Identity
Identify your user profile credentials locally to clear commit ownership verification checks.
```bash
!git config --global user.email "your-email@example.com"
!git config --global user.name "krishnaditya65"
```

### Step 5: Copy, Rename, and Push to GitHub
Navigate inside the repository tracking folder, stage the targeted notebook, commit the changes, and push them to the upstream remote branch.
```python
# 1. Enter the tracked directory path
%cd /content/drive/MyDrive/neural-network

# 2. Stage the working file out of Google Drive and rename it
!cp "/content/drive/MyDrive/Untitled.ipynb" "./Neural network.ipynb"

# 3. Track, log, and deploy changes upstream
!git add .
!git commit -m "Pushed Neural network notebook from Google Colab"
!git push origin main
```

---

## 🔒 Security Best Practices Implemented
* **Token Separation:** Split string components down to prevent the system interface layout engine from dropping protocol links.
* **Path Masking:** Quoted execution variables (`"..."`) handle tracking failures caused by blank spaces in file naming conventions.
