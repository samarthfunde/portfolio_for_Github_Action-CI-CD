GITHUB ACTIONS PROJECT - PORTFOLIO DEPLOYMENT TO S3

<img width="1917" height="878" alt="image" src="https://github.com/user-attachments/assets/f2d02f95-cb5e-4a1c-aac7-9ded674030d6" />

<img width="1919" height="653" alt="Screenshot 2026-01-22 001216" src="https://github.com/user-attachments/assets/dc848791-028a-495e-a2f0-fbfe2a3d8605" />

<img width="1919" height="653" alt="Screenshot 2026-01-22 001216" src="https://github.com/user-attachments/assets/18934768-63d4-48ea-a378-c444e906b827" />


**Step 1: Write Portfolio Code**
- Created portfolio website with HTML, CSS, JavaScript files
- Main file is index.html which will be the entry point

**Step 2: Create GitHub Repository and Push Code**
- Created new repository on GitHub
- Initialized git in local project folder
- Commands used:
- git init
- git add .
- git commit -m "Initial commit"
- git remote add origin myrepourl
- git push origin master


**Step 3: Create GitHub Actions Workflow File**
- Created .github/workflows/main.yml in project root
- This file automates deployment whenever code is pushed to master branch

Workflow file code:
```yaml
name: Samarth Portfolio Deployment

on:
  push:
    branches:
      - master

jobs:
  deploy-my-portfolio:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ap-south-1
      
      - name: Deploy static site to S3
        run: aws s3 sync . s3://samarth-portfolio-bucket --delete
```

Why this file:
- Automates deployment process
- Triggers on every push to master branch
- Uses Ubuntu environment to run deployment
- Checks out code, configures AWS credentials, syncs files to S3
- --delete flag removes files from S3 that are not in repository

**Step 4: Create IAM User and Add Secrets to GitHub**
- Created IAM user in AWS console
- Attached policy: AmazonS3FullAccess
- Generated access key and secret key
- In GitHub repository: Settings > Secrets and variables > Actions
- Added two secrets:
  - AWS_ACCESS_KEY_ID
  - AWS_SECRET_ACCESS_KEY

**Step 5: Create S3 Bucket and Enable Static Hosting**
- Created S3 bucket: samarth-portfolio-bucket in ap-south-1 region
- Disabled "Block all public access" in permissions
- Added bucket policy for public read access:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::samarth-portfolio-bucket/*"
    }
  ]
}
```

- Enabled static website hosting in bucket properties
- Set index document: index.html

**Step 6: Make Code Changes and Push**
- Modified index.html file
- Committed and pushed changes:
- git add .
- git commit -m "Updated portfolio"
- git push origin master

**Step 7: GitHub Actions Workflow Execution**
As per workflow file, these steps executed automatically:
1. Workflow triggered on push to master branch
2. Checkout code - Downloaded repository code to runner
3. Configure AWS Credentials - Authenticated with AWS using secrets
4. Deploy static site to S3 - Synced all files to S3 bucket and deleted old files

**Step 8: Verify Deployment**
- Opened S3 static website URL in browser
- URL: http://samarth-portfolio-bucket.s3-website.ap-south-1.amazonaws.com
- Portfolio website loaded successfully with updated changes
