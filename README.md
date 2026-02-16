# Brief instructions

## Run Locally

---

### Prerequisites

- Python 3.12
- Azure Functions Core Tools v4
- VS Code Azure Functions extension
- Azurite (VS Code extension) or standalone Azurite

---

### Setup

1. Go to the function project folder:

   ```
   cd src
   ```

2. Create/activate virtual environment and install dependencies:

    ```
    python -m venv .venv
    source .venv/Scripts/activate   # Git Bash
    # or: .\.venv\Scripts\Activate.ps1  # PowerShell
    python -m pip install -r requirements.txt
    ```

3. Create `local.settings.json` (do not commit it).
   Use `../local.settings.example.json` as a template.

4. Start Azurite (VS Code command: Azurite: Start).

5. Start the Functions host:

    ```
    func start
    ```

---

### Create the local `images` container (Azurite)

If needed, create the container using the Python SDK:

```
python -c "from azure.storage.blob import BlobServiceClient; cs='DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;'; b=BlobServiceClient.from_connection_string(cs, api_version='2020-10-02'); b.create_container('images'); print('created images')"

```

---

### Test

1. Upload an image to the `images` container (using Azure Storage Explorer or SDK upload).

2. View results:

    - All results: http://localhost:7071/api/results
    - Specific result: http://localhost:7071/api/results/{id}

You can also use `test-function.http` in VS Code REST Client.

---

## Deploy to Azure

### 1. Create Azure Resources

Use VS Code:

1. Press `F1`
2. Select **Azure Functions: Create Function App in Azure (Advanced)**

Choose:

- Runtime: **Python 3.12**
- OS: **Linux**
- Hosting plan: **Consumption**
- Create a new Resource Group
- Create a new Storage Account

---

### 2. Configure Application Settings

After deployment:

1. Open your Function App in Azure Portal.
2. Go to **Settings → Environment Variables**.
3. Add a new setting:

    - Name: `ImageStorageConnection`
    - Value: `<your-storage-account-connection-string>`

You can find the connection string in:

**Storage Account → Access Keys → Connection String**

---

### 3. Create the `images` Container in Azure

1. Go to your Storage Account.
2. Navigate to **Data Storage → Containers**.
3. Click **+ Container**
4. Name it: `images`


---

### 4. Deploy the Code

In VS Code:

1. Press `F1`
2. Select **Azure Functions: Deploy to Function App**
3. Choose your Function App
4. Confirm deployment

---

### 5. Test in Azure

1. Upload an image to the `images` container in Azure Portal.
2. Wait a few seconds for orchestration to complete.
3. Open: `https://<your-function-app-name>.azurewebsites.net/api/results`


You should see stored analysis results from the cloud environment.

[<< Back to Lab2.md](/Olga_Durham_040687883_CST8917_Lab2.md)

