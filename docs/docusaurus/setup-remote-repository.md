---
sidebar_position: 1
---

# Setup remote repository
By the end of this setup you will have a repository that contains all your resources that will deploy.
:::info
This remote repository overwrites the content within the original repository and publishes it to Netlify. See the [add-remote-repository.js](https://github.com/devtodollars/startup-boilerplate/blob/main/docusaurus/add-remote-resources.js) script.
:::
1. Click `Use this template` in the [resources repository](https://github.com/devtodollars/resources). This is the repository where the contents of your blog and documentation will be updated.
2. Go to Netlify > Site Configuration > Build hooks and [create and copy a build hook](https://docs.netlify.com/configure-builds/build-hooks/)
![](../assets/netlify-build-hook.png)
3. In the cloned repository, [create a secret](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) called `NETLIFY_BUILD_HOOK`. Paste the build hook copied from the previous step. 
:::info
This build hook keeps the production deployment updated with the most recent changes in the resources repository
:::
4. To finalize the connection go to Netlify > Site configuration > Environment variables and [set the environemnt variable](https://docs.netlify.com/environment-variables/get-started/#create-environment-variables) `REMOTE_REPO_URL` as the URL of your repository.
![](../assets/netlify-env-vars.png)
5. Make a change your cloned Github repository and watch the changes propagate to your docusaurus website!

:::note
When running locally, set the `REMOTE_REPO_URL` to have your local changes be kept up to date
```
export REMOTE_REPO_URL=<INSERT_REMOTE_REPO_URL>
```
:::
