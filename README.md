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

## Deploy to a Azure Static Web

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

## Deploy to a Static site in an Azure Storage Account

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

## Deploy to on Heroku

### Prerequisite

- [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) 

Can install using:

```sh
# make sure npm installation is latest
npm install npm@latest -g

# Install and build application
npm i
npm run build

# install Heroku CLI and verify install
npm install -g heroku
heroku --version
# Login using browser
heroku login

# Set up application on Heroku
