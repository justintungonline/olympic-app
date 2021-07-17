# An app all about the 2020 Tokyo Olympics, postponed to Summer 2021!

[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/justintungonline/olympic-app)

## Background

Building a flippable trading card app featuring all the 2021 Tokyo Summer Olympics sporting events following the [Microsoft Build session](https://mybuild.microsoft.com/sessions/e6809457-5189-4442-99d7-a7ea45649c19?source=schedule) with [Jen Looper](https://www.jenlooper.com/) to show [Olympics sports](https://olympics.com/en/sports/summer-olympics) on a page that is mobile friendly with a responsive design.

## Student Learning

See [student Branch of the Olympic App](https://github.com/jlooper/olympic-app/tree/student), which we built together at Microsoft Build. Welcome, students and learners of all type.

To get started, fork that student version of the app and open it in [Visual Studio Code](https://code.visualstudio.com/). Look for the medal emoji to follow the steps and build this app. The complete app is built on the 'main' branch.

## About the Application

Using Vite, Vanilla JS, and CSS Grid, let's create a card-flipping app!

Vite will handle the architecture and module bundling, we will use Vanilla JS to avoid framework fatigue, and use CSS Grid for responsive design needs. The actual card flipping can be done using plain old CSS transforms.

To run this app out of the box, make sure you have Node and NPM installed locally (see below for links). Fork the 'main' branch, navigate to the app root and, in your command line or terminal, type: `npm i` to install dependencies. Then, type `npm run dev` to run the app locally (usually on localhost:3000). Since the app only uses Vite and no other framework, your dependency install is lightweight.

<img width="640" alt="screenshot" src="https://user-images.githubusercontent.com/1450004/117545887-af6df780-aff5-11eb-89cd-a8574aae6d27.png">

---
## Resources:

Npm: [https://npmjs.com](https://www.npmjs.com/)

Node: [https://nodejs.org](https://nodejs.org/)

Vite: [About Vite](https://vitejs.dev/guide/)

The [Tokyo Olympics 2020](https://olympics.com/) web site (source of the sports event data)

Free [CSS Grid Course](https://cssgrid.io/)

Learn [how to develop applications](https://docs.microsoft.com/en-us/learn/paths/build-javascript-applications-nodejs/?WT.mc_id=academic-26883-jelooper) using node.js

[JavaScript basics video series](https://channel9.msdn.com/Series/Beginners-Series-to-JavaScript?WT.mc_id=academic-26883-jelooper)

[Free full course on web development](https://aka.ms/webdev-beginners)

Build a [terrarium with JS/CSS/HTML](https://aka.ms/terrarium)

## Deploy to Azure Static Website

Follow the steps at [Building your first static site using the Azure CLI](https://docs.microsoft.com/en-us/azure/static-web-apps/get-started-cli?tabs=vue). Instead of using a new repository, use this repository and in the command line section, execute something like the following. 

Here is an example how the tutorial can be done in your browser.

Log into [Azure portal](https://portal.azure.com/), open a new [cloud shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) - set up a new storage account if you'd like and delete it later to prevent costs. In the cloud shell, execute command like:

```powershell
az staticwebapp create \
    -n olympic-app \
    -g rg-eastus2 \
    -s https://github.com/justintungonline/olympic-app \
    -l eastus2 \
    -b main \
    --app-location "dist" \
    --token my_token \
    --sku free \
    --subscription mysubscription
```

- "dist" is used to match to Vue-like framework used for this application.
- --sku free creates the site under a free site which has limits

## Deploy to Azure Storage Account Static Site

Follow instructions to upload the [site to a static site](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website-how-to?tabs=azure-portal) using Azure storage account blobs.

Install and build the application with `npm i` and `npm run build`

Using the Azure CLI, copy the `dist` folder generated by the build to the storage account.

```powershell

az account set --subscription mysubscription

az storage blob service-properties update \
    --account-name mystorageaccount \
    --static-website \
    --404-document index.html \
    --index-document index.html \
    --auth-mode login

az storage blob upload-batch -s dist -d '$web' --account-name mystorageaccount

# Get the URL and visit it in a browser
az storage account show -n mystorageaccount -g myresourcegroup --query "primaryEndpoints.web" --output tsv
```

## Deploy to Azure App Service

Requires: 

- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli) installed or use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/quickstart) using the bash environment 
- Node 10.x or higher

```sh
# Log into Azure
az login

# Create resource group and set as default values to avoid specifying them each time later
az group create --name myResourceGroup --location westus
az config set defaults.group=myResourceGroup defaults.location=westus

# Create an App Service plan using Linux in free tier.
az appservice plan create --name olympic-cards --resource-group myResourceGroup --sku FREE --is-linux

# Create and deploy web app service with Azure CLI command
# Get run times using command: az webapp list-runtimes --linux
# Use node 10+ per package-lock.json engines
az webapp create --name olympic-cards --resource-group myResourceGroup --plan olympic-cards --runtime "NODE|14-lts" --deployment-local-git --deployment-source-branch main

# Optional zip deployment
# az webapp up --name olympic-cards --logs --launch-browser
# The --logs command displays the log stream immediately after launching the webapp. 
# The --launch-browser command opens the default browser to the new app. 
# Use the same command to redeploy the entire app again.

# In a separate terminal
# Set up user-level deployment credentials with Azure CLI
az webapp deployment user set --user-name <username> --password <password>
# If an error like: Operation returned an invalid status 'Conflict'
# occurs, it may mean the username is taken or password is not complex enough.
# Choose a different username and more complex password

# Get new remote git URL
az webapp deployment source config-local-git --name olympic-cards

# Add remote URL to git repo
git remote add azure https://username12342345236@olympic-app.scm.azurewebsites.net/olympic-app.git
# Set npm settings
# ....NPM_CONFIG_PRODUCTION=false YARN_PRODUCTION=false

# Unix 
# url=$(az webapp deployment source config-local-git --name olympic-cards --resource-group myResourceGroup --query url --output tsv)
# git remote add azure $url

# Push you code to Azure and enter your password when asked. 
# Instead of 'main', you may want to choose your branch to push to Azure. 
# master is required after your local branch to ensure it pushes to the branch read for Azure deployment
git push azure main:master

# View (tail) logs
az webapp log tail --name olympic-cards
```

## Deploy to Heroku

Install the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) and set up folder. Execute these commands inside of the root directory of this repository to deploy the application to a new site.

```sh
# Make sure npm installation is latest
npm install npm@latest -g

# Install and build application
npm i
npm run build

# install Heroku CLI and verify install - skip this step if you already have Heroku CLI installed
npm install -g heroku
heroku --version

# Login using browser
heroku login

# Set up application on Heroku
heroku create
# Tell Heroku to install dev dependencies per https://github.com/vitejs/vite/issues/1215
# vite is by default installed as dev dependency and in production dev dependencies are not installed 
heroku config:set NPM_CONFIG_PRODUCTION=false YARN_PRODUCTION=false
# Push to remote heroku for build which does install and runs start script in package.json using vite --host --port $PORT
git push heroku main

# Open site in browser
heroku open
```
