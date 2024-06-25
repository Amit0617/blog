---
title: "How to update GitHub actions workflow file using another workflow?"
datePublished: Tue Jun 25 2024 14:06:30 GMT+0000 (Coordinated Universal Time)
cuid: clxuhclxa000008mi91z88gug
slug: how-to-update-github-actions-workflow-file-using-another-workflow
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1719320548554/b1a89b06-3600-4b57-8965-68684e1b0964.png
tags: github, permissions, github-actions

---

If you are facing a permission error as shown in the cover photo, you are on the right place. So, let say you have one workflow file with name `my_workflow.yml` and you want to update it using another workflow file, say `my_workflow_updater.yml` .

## Create a token with permissions to update workflow

1. Go to [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)
    
2. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719321290796/63a9e38e-e26d-4db9-a837-b6b0146e2fa5.png align="center")
    

Create a token (classic) with `workflow` option checked in scope. Fill in Note and Expiration as desired.

3. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719321785166/2785ea92-6617-45ba-8936-73a4b6cf6b6f.png align="center")
    

Click `Generate token` button.

4. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719321911312/5873c3fd-a0de-4ef0-9876-a18dec2b627f.png align="center")
    

Copy new generated token starting with `ghp_....` . It's important to do it at this step because it will not appear again and you will have to generate a new one.

## Store Copied token as Secret in the repository

1. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719322488677/1adb90cd-546e-426e-8d31-4fe3013242f0.png align="center")
    

Go to the repository where you are creating that workflow. Click on **Settings** tab. In the **Security** section in the left column, expand **Secrets and variables**. Click on Actions. Click on **New repository secret** button under **Repository secrets** section in the main section.

2. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1719322669356/7f0e0a9c-a242-4fe1-b252-c928a13f0c67.png align="center")
    

Fill in an appropriate Name, and put that copied token into Secret. Click on Add secret. **Name**, here, is critical because this what you will use to access this secret or token.

## Pass this custom token to that workflow file during checkout

Update `my_workflow_updater.yml` like following

```yaml
jobs:
  update-date:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with: 
          token: ${{ secrets.UPDATE_ACTION }}
...
```

`token` is what I want you to look at in the above section. Notice that `UPDATE_ACTION` is the name we used when creating a new secret for the repository in the last step.

That's it. Have fun.