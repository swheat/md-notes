# ✅ Running a GitHub Action Workflow Locally for Testing

Yes! You can **test GitHub Actions locally** using the **`act`** tool.

---

## 🔹 Step 1: Install `act`

### **On macOS (Homebrew)**
```sh
brew install act
```

### **On Linux**
```sh
curl -s https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash
```

### **On Windows (Scoop)**
```sh
scoop install act
```

> **Note:** You must have **Docker installed and running** because `act` executes GitHub Actions in Docker containers.

---

## 🔹 Step 2: Run Your Workflow Locally

### **Run the Default GitHub Actions Workflow**
```sh
act
```
This runs the default `push` event workflow.

### **Run a Specific Event**
To simulate a **pull request event**:
```sh
act pull_request
```

To test a **specific job inside a workflow**:
```sh
act -j <job-name>
```
> Replace `<job-name>` with the **job ID** in your `.github/workflows/*.yml` file.

### **Run a Workflow with a Specific Platform**
```sh
act -P ubuntu-latest=ghcr.io/catthehacker/ubuntu:full-latest
```
> **Why?** The default `ubuntu-latest` image used by `act` is minimal, and some GitHub Actions require additional dependencies.

---

## 🔹 Step 3: Provide Secrets (If Required)

If your GitHub Actions workflow uses **secrets**, create a `.secrets` file:
```sh
GITHUB_TOKEN=ghp_1234567890abcdef
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
```

Then run `act` with:
```sh
act --secret-file .secrets
```

---

## 🔹 Step 4: Debugging the Workflow Locally

- Use `-v` for **verbose logs**:
  ```sh
  act -v
  ```
- Use `-n` for a **dry run** (does not execute commands):
  ```sh
  act -n
  ```

---

## 🚀 Why Use `act` for Local Testing?
✅ **Faster debugging**—No need to push commits to GitHub.  
✅ **Saves GitHub Actions minutes**—Avoid unnecessary CI/CD costs.  
✅ **Works offline**—Test workflows without internet access.

