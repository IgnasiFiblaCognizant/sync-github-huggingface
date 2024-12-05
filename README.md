# Syncing a GitHub Repository with Hugging Face

This guide outlines the steps to synchronize a GitHub repository with Hugging Face, enabling seamless management of Spaces. By syncing, updates in your GitHub repository will automatically reflect in the corresponding Hugging Face repository.

---

## Prerequisites

1. **Hugging Face Account**
   - Create a [Hugging Face account](https://huggingface.co/join) if you don’t have one.
   - Use your corporate email for the account.

2. **Hugging Face Organization Access**
   - If not already a member, request an invitation from your manager.

3. **GitHub Repository**
   - Have an existing GitHub repository ready to link.

4. **Access Token**
   - Generate an API token from your [Hugging Face settings](https://huggingface.co/settings/tokens).
   - Ensure the token includes **WRITE** permissions.
   - Store the token securely.

---

## Steps to Sync

### 1. Create GitHub Repository

1. Log in to your GitHug account.
2. Go to the [GitHub New Repository](https://github.com/new) page.
3. Fill out the form with:
   - **Name**: Enter the name of your repository.
   - **Description**: Provide a brief description. 
   - **Owner**: GenAIHubSPAI.
   - **Visibility**: Private.


### 2. Create a Hugging Face Space
1. Log in to your Hugging Face account.
2. Go to the [New Space](https://huggingface.co/new-space) page.
3. Fill out the form with:
   - **Name**: Enter the name of your repository.
   - **Description**: Provide a brief description. 
   - **Owner**: GenAIHubSPAI.
   - **Visibility**: Private.

### 3. Sync Tree: Linking GitHub and Spaces

First, ensure you have Git installed on your device and that your repository is cloned locally.

From the root directory of your repository, set up your GitHub repository and Spaces app together by adding your Spaces app as an additional remote to your existing Git repository:

```bash
git remote add space https://huggingface.co/spaces/GenAIHubSPAI/SPACE_NAME
```

Then, force push to sync everything for the first time:

```bash
git push --force space main
```
:warning: Using git push --force is a powerful but potentially dangerous command. Be aware that it will overwrite the entire history of the target branch on the Spaces app, so proceed cautiously and ensure you are okay with this action before executing the command.

## 4. Setting Up GitHub Actions for Your Repository

To automate tasks such as pushing changes to your Hugging Face Space, follow these steps:

1. **Create a `.github/workflows` Folder.** In the root directory of your repository, create a folder named `.github`, and inside it, create a subfolder named `workflows`.

```bash
mkdir -p .github/workflows
```

2. **Create a YAML File for the GitHub Action.** Inside the workflows folder, create a YAML file (e.g., push-to-space.yml) where you will define the GitHub Action.

```bash
touch .github/workflows/push-to-space.yml
```

3. **Copy, Edit, and Paste the Following Script.** Open the newly created YAML file and paste the script below, modifying it as needed:

```yml
name: Sync to Hugging Face hub
on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  sync-to-hub:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
          lfs: true
      - name: Push to hub
        env:
          HF_TOKEN: ${{ secrets.HF_TOKEN }}
        run: git push https://HF_USERNAME:$HF_TOKEN@huggingface.co/spaces/GenAIHubSPAI/SPACE_NAME main
```


