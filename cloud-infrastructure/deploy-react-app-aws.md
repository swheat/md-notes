# Deploy a Simple React Web Application to AWS S3 using GitHub Actions

This guide walks through creating a simple React web application, deploying it to an AWS S3 bucket, and automating the process with GitHub Actions.

---

## 1. Create a Simple React App

Run the following commands to set up a new React app:

```bash
npx create-react-app simple-react-app
cd simple-react-app
```
Replace the content of the default src/App.js file with:
```javascript
import React from 'react';
import './App.css';

function App() {
return (
<div className="App">
<header className="App-header">
<h1>Welcome to Simple React App</h1>
<p>This app is ready to be deployed to AWS S3!</p>
</header>
</div>
);
}

export default App;
```
Add some simple styling in src/App.css:
```css
.App {
  text-align: center;
}

.App-header {
  background-color: #282c34;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
}
```
Build the app:
```bash
npm run build
```
This will generate a production-ready version of your app in the build/ directory.

## 2. Set Up AWS S3 Bucket

### 2.1 Create an S3 Bucket:
1. Go to the AWS Management Console and create an S3 bucket (e.g., `react-app-bucket`).
2. Enable static website hosting for the bucket.
3. Make the bucket publicly accessible or use CloudFront for content delivery.

### 2.2 Set Bucket Policy for Public Access:
Use the following policy to make the bucket contents publicly readable:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::react-app-bucket/*"
    }
  ]
}
```
## 3. Set Up GitHub Actions Workflow

Create a `.github/workflows/deploy.yml` file in your repository:

```yaml
name: Deploy React App to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: 16

    - name: Install dependencies
      run: npm install

    - name: Build React app
      run: npm run build

    - name: Deploy to S3
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        AWS_REGION: us-east-1
      run: |
        aws s3 sync ./build s3://react-app-bucket --delete
```
## 4. Configure AWS Credentials in GitHub

1. Go to your GitHub repository.
2. Navigate to **Settings > Secrets and variables > Actions**.
3. Add the following secrets:
    - `AWS_ACCESS_KEY_ID`: Your AWS access key ID.
    - `AWS_SECRET_ACCESS_KEY`: Your AWS secret access key.

---

## 5. Push Changes to GitHub

Push your React app and GitHub Actions workflow to the `main` branch:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/your-username/your-repo.git
git push -u origin main
```
## 6. Verify Deployment

Once the GitHub Actions workflow completes, your React app will be available at the S3 bucket URL. If you enabled static website hosting, the URL will be: http://.s3-website-.amazonaws.com

---

This simple setup deploys your React app to an S3 bucket and automates it using GitHub Actions.



