# Azure DevOps Pipelines

This directory contains Azure DevOps pipeline definitions for your CI workflow.

**Design Philosophy:**
- **Pipelines (YAML)**: Responsible for building code, running tests, and creating artifacts.
- **Releases (Classic UI)**: Responsible for taking those artifacts and deploying them to IIS.

## Available Pipelines

### Build & Artifacts (CI)

### Build & Artifacts (CI)

1. **`build-api.yml`** - **API Build Pipeline**
   - **Triggers on**: Changes to `src/Web` (Backend), `Application`, `Domain`, `Infrastructure`.
   - **Output**: `api-drop` (Web.zip)
   - **Usage**: Source for Backend deployment.

2. **`build-ui.yml`** - **UI Build Pipeline**
   - **Triggers on**: Changes to `src/Web/ClientApp` (Blazor Frontend).
   - **Output**: `ui-drop` (ClientApp.zip)
   - **Usage**: Source for Frontend deployment.

3. **`ci-validation.yml`** - **Pull Request Validation**
   - **Purpose**: Validates quality before merging.
   - **Triggers on**: Pull Requests
   - **Actions**: Build, Test, Code Coverage.

3. **`ci-build.yml`** - General Build
   - Legacy/General purpose build definition.

### Security & Packages

4. **`security-scan.yml`** - Security Scanning
   - **Purpose**: Weekly security analysis using Microsoft Security DevOps.

5. **`nuget-package.yml`** - NuGet Publishing
   - **Purpose**: Publishes the project as a NuGet package (if you are distributing libraries).

---

## Deployment Instructions (Release)

Since you are using **Classic Release Pipelines** for deployment, please refer to the dedicated guide:

👉 **[RELEASE-TEMPLATE.md](RELEASE-TEMPLATE.md)**

This file contains step-by-step instructions on:
1. Creating the Release Pipeline in Azure DevOps UI.
2. Connecting it to `ci-artifacts.yml`.
3. Configuring the IIS Deployment tasks.

## Setup Instructions

### 1. Variables
Configure these variables in your Azure DevOps project or pipeline:

- `azureSubscription` (Optional: only if using Azure resources)
- `NUGET_API_KEY` (Optional: only if publishing NuGet packages)

### 2. Extension Requirements
Install these extensions from the Azure DevOps Marketplace:
- [Microsoft Security DevOps](https://marketplace.visualstudio.com/items?itemName=ms-securitydevops.microsoft-security-devops-azdevops) (for security scanning)

### 3. Build Agent
To build the solution, you can use:
- **Azure Hosted Agents**: `ubuntu-latest` or `windows-latest` (Pipelines are configured for Ubuntu/Linux currently, which is faster and cheaper).
