---
layout: post
title: Recipe for pairing up RStudio with GitHub
tags:
- GitHub
---
On both Windows and Mac I have been happy to use RStudio for R development and the [GitHub app](https://windows.github.com/) for handling version control. The app seems to be GitHub's own preferred interface, and if you use it you don't even need [Git for Windows](http://msysgit.github.io/). I'm not sure why you wouldn't just do that. The only cost is that you have to flit between RStudio and the GitHub app every time you make a commit, but how much of an interruption is that? You flit between RStudio and the browser all the time to check StackOverflow, don't you?

Regardless, suppose that you find the thought of doing your version control from inside RStudio appealing. Below are the setup steps that worked for me, pieced together from many places in the process of integrating my [startUpViz](https://github.com/ghuiber/startUpViz) repo into RStudio's Git workflow.

### Step 1: Give RStudio the Git

Install Git for Windows or, if it's installed already, tell RStudio about it as explained in the five-step method described [here](http://www.molecularecologist.com/2013/11/using-github-with-r-and-rstudio/). *Make sure to stop at "Restart RStudio and that is all there is to it!"* My instructions below supersede the remaining instructions there, not because those are wrong, but because I have a specific kind of R project in mind: a package.

### Step 2: Create a brand new R project

[Create a brand new R project in a brand new directory](https://support.rstudio.com/hc/en-us/articles/200526207-Using-Projects) and check the box "Create a Git repository." You might as well place this at the end of `c:/users/[yourusername]/documents/GitHub/` because this is probably where you keep all the work that ends up published on GitHub.

### Step 3: Open the Git shell and configure your SSH key pair

In RStudio your brand new project comes with a Git tab in the top right corner where you usually only pay attention to the Environment and History tabs. That Git tab has a gear icon, and the drop-down menu that opens when you click it has a "Shell..." option. That is your GitHub BASH shell. Open it, create a new SSH key pair, then send the public one to GitHub. The complete instructions for this are [here](https://help.github.com/articles/generating-ssh-keys/), but there's one crucial twist: instead of `ssh-agent -s` as shown in the last screenshot at step 2 on that page, you must type ``eval `ssh-agent -s` ``. Only then can you `ssh-add` your new private key. Details are offered on [StackOverflow](http://stackoverflow.com/questions/17846529/could-not-open-a-connection-to-your-authentication-agent). You only need to do this step once. Subsequent RStudio projects that you'll be version-controlling on GitHub from inside RStudio will use the same key pair for authentication.

### Step 4: Create a brand new GitHub repo at github.com

Now go to GitHub and create a bare-bones new repo of the same name as your directory at *Step 2* above. *Do not* check the box "initalize this repo with a README file" because that part can wait, and doing so will bypass some options you'll want. When you create this new repo, you will be given the options
- *Quick setup — if you've done this kind of thing before*
- *…or create a new repository on the command line*
- *…or push an existing repository from the command line*
The third is the one you want. Now you go back to RStudio, but before you leave notice that though the "clone URL" that you can copy to clipboard says `https://` by default, that is not your only option. If you read the hint that "You can clone with..." and click SSH, the URL changes to something that starts with `git@github.com:`. That's the one you'll want. Save it to the clipboard now.

### Step 5: Add a remote repo

Now you're back in RStudio. If at the Git BASH shell prompt you type `git remote -v` and hit Enter, you should see nothing, because you don't yet have a remote repo. You add one with `git remote add origin git@github.com:[yourusername]/[yourrepo].git` where the part after the word `origin` comes from what you saved to the clipboard at Step 4 above.

### Conclusion

Once you complete the 5 steps above, you can `git push -u origin master` for the first commit, and then commit, push, pull, etc. directly from RStudio. Either skipping this little "eval" tweak or using the `https://[...]` URL for the remote repo instead of the `git@[...]` one will cause the ssh connection to fail, and RStudio will be unable to push to the remote.

I don't know why this had to be so hard to set up, but there it is. I wrote it down because it took trial and error. Anyway, this is how you hook RStudio for the first time to a pristine, not-yet-having-had-the-first-commit GitHub repo. This is the kind of repo you need for an R project that starts in a new directory, with the further options that it can be an empty project, a package, or a Shiny app.

A much easier way to go (especially once you have a SSH key pair set up) is to set up your repo at github.com with the box "initalize this repo with a README file" checked. That immediately triggers your first commit. Next, you go to RStudio and start a brand new project but this time you pick the "Version Control" option (bottom of the dialog box) instead of either of the two above it ("New Directory" or "Existing Directory as of this writing). You then pick Git, then give it the `git@[...]` URL you saved to the clipboard at step 4, and you're good to go.

This, though, will be a bare-bones project, and it's up to you to fill in the goods. None of the setup work that RStudio provides for new packages or Shiny apps will be done by default. You can see why this is: the typical use case for a project *checked out* from a version control *repository* is that you're picking up where you or somebody else left off earlier: there's some work in progress you'll be making use of, not just an empty repo that you happened to start with the box "initalize this repo with a README file" checked.

Either way you do it, starting afresh or checking out from an existing repo, you still have the option to revert to the GitHub app if, say, you miss the Sync button. You just need to do a push and pull from RStudio to make sure that your local copy is identical to the remote, and then let the GitHub app clone the remote onto the local one. No conflicts will be reported, and from then on you can handle the version control from either app.