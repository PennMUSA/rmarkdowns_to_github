# Publishing R Markdowns to GitHub

This tutorial will walk through how to publish R Markdowns on GitHub using two different methods. The first method will simply publish a single R Markdown as a GitHub Page, whereas the second method allows for more complex website development.

Following these methods will require having:

  * R/RStudio
  * GitHub Desktop
  * Git

This tutorial assumes the user has basic knowledge of all three softwares.

## Method One: Using GitHub Desktop to Deploy Website
### On GitHub Profile:

Create a new repository on your profile and initialize the repository with a README. Navigate to the code tab of the repository and click the 'clone or download' button. Select 'open in desktop'.

GitHub Desktop should automatically open. Clone the repository.

If you go into File Explorer, there should be a folder called 'GitHub' under Documents. Any repositories cloned using GitHub Desktop will show up in here as their own folder. Navigate to this folder and add a new folder called 'docs'.

### In RStudio:

Create a New Markdown.

  * File > New File > R Markdown
  * Make it an html output

Make any changes you would like to the document. Save the markdown to the 'docs' folder we created earlier and name it 'index'. Knit the markdown together.

### GitHub Desktop

When you go back to GitHub Desktop, you should see all the changes that were made to the repository. In the bottom left, fill out the summary of what happened and and commit the changes.

### On GitHub Profile

Go back to your profile on GitHub. You may need to refresh the page, but all changes just made to the repository should be seen under the code tab.

Navigate to the Setting tab and scroll down to GitHub Pages.

  * Under Source select, 'master branch/docs folder'
  * Click save

A band saying 'Your Site is published at...' should appear. At first it will be gray. Once it turns green, you can navigate to the `github.io` URL it gives to view your site.

As stated previously, this is a simple method to get an r markdown on GitHub without extensive development. The next method gives more options for customization and provides a more fleshed out website.

## Method 2: Using Git and RStudio to Deploy GitHub Page
### On GitHub Profile:

This method begins, similarly to the first. On GitHub, create a new repository.

  * Name it appropriately
  * Check initialize the repository with a README.

This time, however:

  * add a `.gitignore` (select R from the associated dropdown menu).

Navigate to the code tab of the repo and click the 'clone or download' button. Click the clipboard icon to copy the URL to the clipboard.

### In RStudio:
#### Make a New Project

  * File > New Project
  * Select Version Control
  * Select Git
  * Paste URL of repo into field
  * Click 'Create'

You should notice that by the 'Environment' tab, there should now be a 'Git' tab as well. This can be used to commit changes to the repository, however, below this tutorial walks through making changes to the repo using the 'Terminal'.

#### Make a Markdown

Each markdown you create in this project will be a page on the website when it is finished. GitHub Pages requires for there to be one page called 'index.html'. It makes the most sense if this is the homepage for the website, though it doesn't have to be.

Create a new markdown

  * File > New File > R Markdown
  * Make it an html output
  * Make any changes you would like to the document

Follow this process as many times as you need to get all the pages for the website. Note that the name of the markdown will show up in the URL as you click through the website. Name each markdown appropriately to associate with the page the user will be seeing.

#### Creating a YAML for the site

We will now create the structure of the website using a YAML.

  * File > New File > Text File

This is a basic format, which will give a navigation bar at the top of the site. The `text` will be shown on the `navbar` and the `href` is the associated page the user will be taken to when that text is clicked.

```{r}
name: "name of repo here"
output_dir: "docs"
navbar:
  title: "Title of Website"
  left:
  - text: "Home"
    href: index.html
  - text: "Next Page"
    href: nextpage.html
output:
  html_document:
    theme: united
```

The `theme` can be set based off markdown options. A preview of different themes can be found [here](http://www.datadreaming.org/post/r-markdown-theme-gallery/).

The YAML format can become more complicated, adding icons to the `navbar` using the tag `icon:` rather than `text:`. These can be pulled in from Boostrap or Font Awesome, like so, `icon: fa-github-square fa-lg`.

We can also create dropdown menus to nest pages under a specific heading.

```{r}
- text: "Common Heading"
  menu:
    - text: "Page 1"
      href: page1.html
    - text: "Page 2"
      href: page2.html
```

Once, the YAML is formatted how you would like it, save the file as '_site.yml'. It must be named this for markdown to recognize it.

#### Commiting changes to GitHub and Rendering the website

There are a handfull of commands that are needed for this part of the workflow:

  * `git branch BRANCH-NAME` - create a new branch
  * `git status` - check status of repo (will tell you what branch you are on)
  * `git checkout BRANCH-NAME` - will switch you between branches
  * `git add .` - will add all files that were changed to staging area
  * `git commit -m "COMMIT MESSAGE"` - add a commit to repo
  * `git push -u origin BRANCH-NAME` - push changes to the branch

If you run `git status` now, you should see that you are on the master branch. Create a new branch using the command above and run `git checkout YOUR-BRANCH-NAME`. Now run `git status` again. You should see that you are on the new branch of the repo.

Install the package `rmarkdown`. In the 'Console' window, run `rmarkdown::render_site()`. This should knit all the markdown pages.

Navigate back to the Terminal. Run `git status` - you should see these files are under the heading 'untracked files'.

Run `git add .` to add these files to the staging area. Make a commit and push the changes to this branch using the commands above.

Go back to your GitHub profile and refresh the page of your repository. Under the code tab, you should see there are 2 branches. You can toggle between branches using the branch dropdown menu. Navigate to the new branch and create a pull request back to the master. It should allow you to automatically merge the request. Merge the pull request and delete the branch.

Go to the settings tab of the repository and scroll down to GitHub Pages. Under Source select, 'master branch/docs folder' and click save. Once the banner turns green, your site has been published!
