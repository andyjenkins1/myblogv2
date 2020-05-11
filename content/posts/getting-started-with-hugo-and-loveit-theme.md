---
title: "Getting Started with Hugo and the LoveIt theme for Blogging"
subtitle: "How to install Hugo and the LoveIT theme, setup your basic Dev workflow with Git and basic theme configuration options"
date: 2020-05-04T14:12:25+01:00
lastmod: 2020-05-04T14:12:25+01:00
draft: true
author: ""
authorLink: ""
description: "Getting started with Hugo the LoveIt theme, setting up your basic developer workflows with Git and Github and going through some of the basic configuration changes you can see on my blog."

tags: ["Hugo", "Dev", "Git"]
categories: ["Blog"]
hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: "/images/getting-started-with-hugo-and-loveit-theme/LoveITTheme.png"
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: false
lightgallery: true
license: ""
---

<!--more-->


# Background

In my difficult first post blog post **LINK HERE** I discussed how long I have spent evaluating and deploying so many different blogging platforms over the last 2 years without actually generating any content or producing anything of use!!.  This is certainly one of the common traps of when you decide to blog, spending too much time on the platform and not the actual content.  I have deffo fallen into that trap, but hopefully now up and running :(far fa-grin-tears):

{{< admonition note >}}
 I will post more about my experiences of deploying and configuring multi blogging platforms on both AWS and Azure as I am sure others will find it useful, knowing its easy certainly helps!.  There are some really good platforms available that are easy to use and host in the cloud!.  Over the last two years I was close to releasing my blog on various platforms including [Grav](https://getgrav.org), [Jekyll](https://jekyllrb.com/) & [Gatsby](https://www.gatsbyjs.org/).  Watch this space for more info. :(far fa-check-square):
 {{< /admonition >}}
 
 
 So, after 2 years of procrastinating, I finally decided "to-go-live" using [Hugo](https://gohugo.io/) and hosting on Azure.  My architecture, setup and configuration will be explained in the upcoming blog posts so you get the idea of the end-2-end solution I finally settled on.  I want to say I completed a detailed in depth comparison of each (I have certainly deployed them all!!!) but the two things that were key to me in this project was a) the quality of the theme and how often the theme is being updated and b) the cost of hosting the blog in the cloud.  I found a fantastic theme for Hugo called LoveIt and developer [Dillon](https://github.com/dillonzq) is doing a fantastic job in making constant updates and improvements.  Given that I am using Hugo, this means I can either use Azure Static Website or Amazon S3 storage which should mean I can operate my website for less than $2 per month!. :joy:


## Installing Hugo 

My daily driver laptop is a Macbook Pro so will base my instructions on that, however the [Hugo](https://gohugo.io/getting-started/installing/) has you covered regardless of platform you wish to install Hugo onto.


The first step is to install [brew](https://brew.sh/).  The instructions are nice and clear.

{{< admonition info >}}
I was reluctant to use Brew for a while as I didn't fully understand how it worked, but now I am wiser, it makes life so much easier, its non-destructive and is really easy to keep upto date.  So if you were like me and wasn't sure whether or not to use it, my recommendation would be to go for it!.
{{< /admonition >}}

With brew installed, you can check that hugo has installed correctly with the following 

```bash
hugo version
```
If this return the version (you need version v0.62 or above for the LoveIt Theme) you are good to go.

Now, lets head over to Github and create a new repo for our blog.  Something like below will work :-

![GitHubNewRepro](/images/2020PostImages/GithubNewRepo.jpg)

Make a note of your remote origin as per below (we will use that in a moment).

![GitHubRemote](/images/2020PostImages/GithubRemoteOrigin.jpg)

Now lets launch a terminal window.  Check out my blog post on how to create a cool down with the kids terminal using iTerm2 **LINK HERE**

Create a new directory where you want to store the code for your site.  For what its worth, I simply organise all my Dev related stuff in a *MyDev* :(far fa-folder-open): folder within my home folder.

We now need to enable the folder ready for git management.  Once this is done we can install the LoveIt Theme.

```bash
git init
```

## Installing the Loveit Theme & Git Workflow

It goes without saying to checkout the excellent LoveIt Theme [documentation](https://hugoloveit.com/theme-documentation-basics/) as this is a really helpful guide to get you going.

{{< admonition tip >}}
You have two options when installing the theme, you can simply clone it using git clone or download the zip directly from the themes Github page, or you can add the theme as a git submodule to your project.  You can learn more about git submodules [here](https://git-scm.com/book/en/v2/Git-Tools-Submodules). Basically they allow you have a separate git repository inside your projects git repository.  For most people I would recommend the submodule approach as this ensures the themes code it kept upto date as changes are pushed by the developer to the themes master branch.  If you just clone the repo you clearly have more control, but will need to update manually as and when the theme is updated.
{{< /admonition >}}

From the directory we inialised for git, lets add the LoveIt Theme as a submodule

```bash
git submodule -b master add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```
You should see the following 

We now have the theme as part of our project but we have no framework to use for our own development.  Luckily the LoveIt Theme has an example site to help us get started, simply copy the contents of the example site folder into your created folder.

```bash
cp -R -v <yourfolder>/themes/Loveit/ExampleSite/* <yourfolder>
```

{{< admonition warning >}}
If you try and serve your website at this stage with the Hugo serve command you will likely get the following error *"Error: module "LoveIt" not found; either add it as a Hugo Module or store it in "xxxx".: module does not exist"*. This is because we need update the **config.toml** file and make a couple of changes
{{< /admonition >}}

Open the config.toml file and comment out 

AS PER PICTURE

These parementers are not required for the purpose of my blog.


At his point you should be good to go!.





So lets add the newly copied files to git

```bash
Git add *
```
Lets add our created repo on github to this site so we can push our changes to github 

```bash
git remote add origin https://github.com/andyjenkins1/testblog.git
```

Before we make push the commits, I reccommend you check the contents of your git .config file 

{{< gist andyjenkins1 8374eb13335204558374ae31b9101396 >}}

```bash
git config --list
```

In your config file you should see that the submodule exists as well as the remote origin that we have just added.  I also highly recommend updating your config with your user.name and user.email to ensure you know who is making the commits!.  You can do this really easy with the git add command.

If all is good, lets make the commit and push our code to Github.

```bash
git Commit -M “Initial Commit based on template”
```
Now before committing, it makes sense to add .gitignore file to ensure we dont push stuff we dont need to for the blog.  Here is the current .gitignore file that I use for my blog 




git push -u origin master

Head over to Github and your files should now be available on Github and we can run our site locally and begin testing!.


## Running the Theme Locally

So now we have a good structure in place to manage the changes to our blog as we edit and configure and hopefully add content!.  All we have to do now is test the website out locally, this you can do with this simple command 

```bash
hugo serve --disableFastRender -D
```
By default all new posts are in Draft using the pages frontmatter, the -D command ensures drafts appear when you are working locally. 

If you have no errors, from a browser head to localhost:1313 and hey presto you will have a fully working blog!.

you can add a page using the following Hugo command 

```bash
hugo new post 
```

Head to localhost:1313 and your working website should be goof to go!


{{< admonition warning >}}
:(far fa-bookmark fa-fw): Bookmark this page for easy future reference!
{{< /admonition >}}


===

### Theme Configuration Changes



{{< admonition info >}}
:(far fa-bookmark fa-fw): Bookmark this page for easy future reference!
{{< /admonition >}}