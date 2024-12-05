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
   - **Space hardware**: CPU basic - 2 vCPU - 16 GB - FREE ( At Dec. 2024 )

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

3. **Create a README File for the HuggingFace Space.**
   - For Docker
   ```md
   ---
   title: 
   emoji: 
   colorFrom: 
   colorTo: 
   sdk: docker
   app_port: 
   ---
   ```
   - For Gradio
   ```md
   ---
   title: 
   emoji: 
   colorFrom:  
   colorTo:  
   sdk: gradio 
   sdk_version:  
   app_file:  
   pinned: false 
   ---
   ```

4. **Copy, Edit, and Paste the Following Script.** Open the newly created YAML file and paste the script below, modifying it as needed:

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
5. **Set Up Secrets in GitHub.** In the GitHub UI, navigate to `Settings > Security > Secrets and Variables > Actions`. Create a New Repository Secret 
   - Click on New repository secret.
   - Set the name to HF_TOKEN and paste your Hugging Face token into the value field.
   - Save the secret.

## 5. Define HuggingFace Secrets

1. **Navigate to Settings**  
   In the GitHub UI, go to **Settings > Variables and Secrets > New Secret**.

2. **Write Down All Environment Variables**  
   Decide whether to use **Variables** or **Secrets** based on the type of data you need to store:

   - **Variables**:  
     Use variables if you need to store non-sensitive configuration values. These values are publicly accessible and viewable, and they will automatically be included in any Spaces duplicated from yours.  

   - **Secrets**:  
     Use secrets to store sensitive information, such as access tokens, API keys, or credentials. These are private, and their values cannot be read from the Space's settings page once set. Additionally, secrets are not included in Spaces duplicated from your repository.

3. **Add New Variables or Secrets**  
   - Click **New repository variable** or **New repository secret**, depending on your need.
   - Provide a name and value for the variable or secret.
   - Save your changes.

By organizing your environment variables and secrets properly, you ensure secure and efficient configuration management for your Spaces. 





