# How to Log in with Azure CLI, Generate a SAS Token, and Use AzCopy to Download a File

AzCopy is a powerful tool designed to move data to and from Azure Blob Storage. This article explains how to log in using the Azure CLI, generate a Shared Access Signature (SAS) token, and use AzCopy to download a file with logging enabled.

---

## Step 1: Log in Using Azure CLI
To interact with Azure resources, you need to log in using the Azure CLI:

1. Open your terminal or command prompt.
2. Run the following command:
   ```bash
   az login
   ```
3. A browser window will open for you to authenticate with your Azure account. Select your account and provide the necessary credentials.

- If you’re in a terminal-only environment, use:
  ```bash
  az login --use-device-code
  ```
- Copy and paste the provided code into a browser to authenticate.

---

## Step 2: Generate a SAS Token for the Blob
A **Shared Access Signature (SAS)** token provides secure, time-limited access to your Azure Blob. Use the following Azure CLI command to generate a SAS token:

1. Run this command, replacing the placeholders with your details:
   ```bash
   az storage blob generate-sas \
       --account-name storageacct \
       --container-name storage-container \
       --name testfile-2-5-GB.bin \
       --permissions r \
       --expiry 2025-01-15T00:00:00Z \
       --https-only \
       --output tsv
2. The command outputs a token like:

```
sv=&sr=b&sig=&se=&sp=r
```
3. Append this token to your blob URL:

```
https://storageacct.blob.core.windows.net/storage-container/testfile-2-5-GB.bin?
```

---

## Step 3: Use AzCopy to Download the File
Now that you have the SAS token, use AzCopy to download the blob to your local system. Here's the command:

```bash
azcopy copy "https://storageacct.blob.core.windows.net/storage-container/testfile-2-5-GB.bin?<SAS-token>" "./testfile-2-5-GB-2.bin" --log-level=INFO
```
### Explanation of Parameters
- **Blob URL**:
    - Includes the storage account, container, blob name, and the SAS token.
- **Destination Path**:
    - `./testfile-2-5-GB-2.bin`: The local file path where the blob will be downloaded.
- **`--log-level`**:
    - Specifies the logging verbosity. Options: `ERROR`, `WARNING`, `INFO`, or `DEBUG`.

---

## Step 4: View Logs
AzCopy automatically creates logs for the operation. By default, the logs are saved in:
- **Windows**: `%USERPROFILE%\\.azcopy`
- **Linux/macOS**: `~/.azcopy`

You can also specify a custom log file using the `--log-file` flag:
```bash
azcopy copy "https://..." "./testfile-2-5-GB-2.bin" --log-level=INFO --log-file="./azcopy-log.txt"
```

## Final Notes
- **AzCopy Advantages**:
    - Optimized for large file transfers.
    - Supports parallel downloads for faster performance.
    - Automatically resumes interrupted transfers.

- **Best Practices**:
    - Always generate SAS tokens with minimal permissions and appropriate expiry times.
    - Use `--log-level=DEBUG` for detailed troubleshooting.

By following these steps, you can securely and efficiently download files from Azure Blob Storage using AzCopy!