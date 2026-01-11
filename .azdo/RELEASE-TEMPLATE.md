# IIS Release Pipeline Template (Classic UI)

This document describes how to configure your **Classic Release Pipeline** in Azure DevOps to deploy the artifacts from your separate **API** and **UI** build pipelines.

## 1. Prerequisites

1. Go to **Pipelines** > **Releases**.
2. Click **New Pipeline**.

## 2. Add Artifacts (Two Sources)

Since we split the builds, you need to add **two** artifacts to your Release Pipeline.

### Source 1: The API
1. Click **Add an artifact**.
2. **Source type**: Build
3. **Source (Build pipeline)**: Select `build-api`.
4. **Source alias**: `_build-api` (or default).
5. Click **Add**.

### Source 2: The UI
1. Click **Add an artifact**.
2. **Source type**: Build
3. **Source (Build pipeline)**: Select `build-ui`.
4. **Source alias**: `_build-ui` (or default).
5. Click **Add**.

## 3. Configure Stages (e.g., "Production")

1. Click **Add a stage** > Select **IIS website deployment**.
2. Name the stage: `Production`.

### Task 1: Deploy API
Add/Configure "IIS Web App Deploy":
- **Package or Folder**: `$(System.DefaultWorkingDirectory)/_build-api/api-drop/Web.zip`
- **Website Name**: `$(apiWebsiteName)`

### Task 2: Deploy UI
Add/Configure "IIS Web App Deploy":
- **Package or Folder**: `$(System.DefaultWorkingDirectory)/_build-ui/ui-drop/ClientApp.zip`
- **Website Name**: `$(uiWebsiteName)`

*(See previous template for detailed Task settings like "Take App Offline" and MIME types)*

## 4. Updates & Triggers

Now you can trigger releases independently!
- If you change only the **Frontend**, only `build-ui` runs. You can verify the valid release only deploys the UI if you want, or redeploys both using the latest available artifacts.
