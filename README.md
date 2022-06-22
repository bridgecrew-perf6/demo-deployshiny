# Deploying a Shiny app with GitHub Actions

This is a demo repository to demonstrate a GitHub Actions worflow that deploys a Shiny app to ShinyApps.io (using the [shinyapps-io-deploy](https://github.com/marketplace/actions/shinyapps-io-deploy) Action).

This is example is part of [Workshop: Introduction to GitHub and GitHub Actions](https://github.com/pedrohbraga/IntroGitHubActions-Workshop), contributed to the 4th QCBS R Symposium on June 23rd, 2022. This workshop is contributed by Pedro Henrique P. Braga (Concordia University) & Katherine Hébert (Université de Sherbrooke).

[![badge](https://img.shields.io/static/v1?style=for-the-badge&label=Shiny-App&message=Open&color=8FBCBB)](https://katherinehebert.shinyapps.io/myApp/)


## 1. Get started with a shinyapps.io account

To deploy your Shiny app to RStudio’s ShinyApps.io, you will need to [make a ShinyApps.io account](https://www.shinyapps.io/admin/#/signup). You can sign up with your GitHub account!

## 2. Generate a ShinyApps.io Token

To deploy your Shiny app, you will need to generate a Token in your shinyapps.io account.

On the lefthand panel under Account, [go to Tokens](www.shinyapps.io/admin/#/tokens).

To generate a token, click on the green `+ Token` button. 

You should now see a Token, and a Secret (which will be a line of X's). You can reveal the Secret by clicking on the blue `Show` button on the right.

You will need both the Token and the Secret in the next step.

## 3. Create Secrets in your GitHub repository.

Secrets are encrypted tokens you can use in your GitHub workflow, usually to grant permission to access and write to your repository (or in this case, to deploy to ShinyApps.io).

To create secrets in your GitHub repository:
1. Go to your GitHub repository. 
2. Click on `Settings`, then on `Secrets > Actions` in the lefthand panel menu.
3. Click on `New repository secret`.

You will need to make 2 secrets so your workflow can deploy to shinyapps.io:
- `SECRET`: copy the secret from your [shinyapps.io token](www.shinyapps.io/admin/#/tokens). (Remember, reveal it by clicking `Show`).
- `TOKEN`: copy the token from your [shinyapps.io token](www.shinyapps.io/admin/#/tokens).

## 4. Create a GitHub Actions workflow.

1. Create a `gh-pages` branch in your GitHub repository.
2. Create a folder named `.github` in your GitHub repository. 
3. Create a subfolder called `workflows` within the `.github` folder.
4. Create a new `.yml` file in this folder (you can open a text file, and save it with the `.yml` extension).
5. Copy the following workflow into your `.yml` file:

```
on:
  push:
     branches:
       - main

name: Deploy template app to shinyapps.io

jobs:
  deploy-shiny:
    name: Deploy to shinyapps.io
    runs-on: ubuntu-latest
    env:
      GITHUB_PAT: ${{ secrets.ACCESS_TOKEN }}
      ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
        
    steps:
       - name: 🛎️ Checkout repository
         uses: actions/checkout@v2
         with:
           fetch-depth: 0
       - id: deploy
         name: 💎 Deploy to shinyapps.io
         uses: qwert666/shinyapps-actions@main
         env:
           SHINY_USERNAME: 'katherinehebert' # put your own username here!
           SHINY_TOKEN: ${{ secrets.TOKEN }}
           SHINY_SECRET: ${{ secrets.SECRET }}
           APP_NAME: 'myApp' # this will show in the app's url
           APP_DIR: ''
```

## 5. Accessing your deployed Shiny app.

To access your deployed Shiny app, go to the following url using your own ShinyApps.io username and the App name you set near the end of the GitHub Actions workflow as APP_NAME.

<SHINY_USERNAME>.shinyapps.io/<APP_NAME>

For example, the app that is deployed here is accessible here:

[katherinehebert.shinyapps.io/myApp](https://katherinehebert.shinyapps.io/myApp/)
