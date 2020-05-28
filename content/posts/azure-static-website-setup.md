---
title: "Setting up your Static Website on Azure"
subtitle: "End-2-End Setup of using Azure Static Website to host your blog"
date: 2020-05-26T11:12:09+01:00
lastmod: 2020-05-26T11:12:09+01:00
draft: true
author: ""
authorLink: ""
description: ""

tags: ["Hugo", "Dev", "Azure"]
categories: ["Blog", "Azure"]

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: false
lightgallery: false
license: ""
---

<!--more-->

In the previous blog post **<LINK>** we setup the Hugo Theme for local development but the chances are you will want others to see your work!.  There are loads of options for hosting Hugo including GitHub Pages, AWS, Azure as well a whole host of Web Hosting providers.

I have chosen to host my Blog on Azure Static Website as I know the platform well, like the fact it doesn't cost much and get free HTTPS by using Azure CDN and Custom Domains.

## Pre-Reqs

  ### Azure Account

  sign up for an Azure account here https://azure.microsoft.com/en-gb/free/

  ### Install Azure CLI 

```bash
brew update && brew install azure-cli
```



## Create Azure Storage Account

{{< admonition note >}}
In order to improve my CLI skills, I am trying to write up my cloud blog posts using the CLI, of course these steps can also be done via the GUI or PowerShell
{{< /admonition >}}


```bash
az login
```

If you have multiple subs you need to select the one you want to use 

```bash
az account list --output table
```

```bash
az account set --subscription 'name of subscription in the list'
```
Next job is to create an Azure Resource Group to hold all the services we might want to use for the blog ...

```bash
az group create -l uksouth -n YourRGName
```
you can check which regions are available using the following command ...

```bash
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```
{{< admonition tip >}}
I always check the Azure pricing calculator to check which region is the cheapest as this done change over time, my goto region was always North Europe (Dublin), but I have found that UKSouth has been cheaper for may resources over the last few weeks.  While this may only be a few pence or pounds, it is a good practice to do in my mind.
{{< /admonition >}}

Now we need to create a storage account 

```bash
az storage account create \
    --name YourStorageAccountName \
    --resource-group YourRGName \
    --location uksouth \
    --sku Standard_LRS \
    --kind StorageV2
```
{{< admonition info >}}
There are a number of storage resliancy options available for Azure Storage accounts Locally redundant Storage(LRS), Zone-redundant Storage(ZRS),Geo-redundant Storage(GRS), Read-access Geo-redundant Storage (RA-GRS), Geo-Zone-redundant Storage(GZRS), Read-Access Geo-Zone-redundant storage (RA-GZRS).  For the purposes of my blog, LRS is fine, but if you were running something more serious you want to consider both multi-zone and multi-region protection for your storage account.
{{< /admonition >}}

### Add Static Website to your Storage Account

```bash
az storage blob service-properties update \
--account-name <YourStorageAccountName> \
--static-website \
--404-document 404.html \
--index-document index.html
```

### Upload Basic Website Pages

Download Storage explorer from ->  https://azure.microsoft.com/en-us/features/storage-explorer/\

Authenticate to your Azure Subscription and select your storage account 

Create a basic index.html page and a 404.html page (make sure you use the same names as provided earlier)

Upload the files to your Storage account via the Upload button.


### Access Your Static Website

Lets find out the name of the public endpoint for the created website ...

az storage account show -n ajhugoblog -g AJ-MyHugoBlog-RG --query "primaryEndpoints.web" --output tsv

This will return the endpoint of your blog, copy the URL and paste into your browser! and you should see a working website!.


### Enable Basic Metrics on your site

Add stuff here

## Create an Azure CDN

Now we have the basic static website working the next step is to deploy an Azure CDN, this will provide 2 benefits cache the site to help with loading times and will also ensure we get a free SSL certificate to help to make the site secure ...

1.  From the Azure Portal, select your storage account and click on the *Azure CDN* Menu Option
2.  Select *Create New Endpoint* give your CDN Profile a name, select pricing tier (I recommend Microsoft Standard) and give the endpoint a name.
3.  The *Origin* Hostname should be the name of the storage account and already be populated.
4.  Click *Create*
5.  It takes around a minute to create and you should then see a status of running for your created endpoint.

We now need to make some changes to the endpoint to ensure it caches our content correctly.

1. Within your RG, select the storage account you have used for your static website
2. Select *Static Website* from the menu
3. Copy your **Primary endpoint** Address as we will need that in a moment.  
4. Back to your RG now and within it, select the created CDN Endpoint.
5. Stop the Endpoint by selecting *Stop* From the GUI
6. From the Endpoint menu select *Origin*
7. Change the Origin Type from *Storage* to **Custom Origin**
8. In the *Origin Hostname* field paste your *Primary Endpoint Address* but remove the https:// you just need the hostname.
9. The *Origin Host Header* should also be the same, if not follow the same step as above.
10.  Once the changes have been made select **Save** 
11.  We now need to start the CDN again, from the *Overview* page of the Endpoint, select **Start**.
12.  We should now have a working CDN!.

### Test your CDN is working

The last step is to test that content is being serve from the CDN.  To test this head to your CDN endpoint and browse to the CDN endpoint hostname, it should display the same content as your primary storage account endpoint!.  It can take upto 10 minutes for this to work, but typically happens in around a minute.

You will also notice you now have a valid SSL 





## Upload your Hugo Site