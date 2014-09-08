---
title: How to create an Octopress github blog
layout: post
category : blog
tagline: ""
tags : [Jekyll, Octopress, theme]
---

I found the  of a github blog with Octopress to be problematic. Seems like between the [the Octopress site's docs](http://octopress.org/docs/deploying/github/) (which seem to be very thorough, BTW) and the [github pages (a.k.a., blog)](https://pages.github.com/) creation docs, something was slipping between the cracks. Web searches resulted in many false leads or solutions inappropraite to my case. I got pretty frustrated. But, since it seems that others have had issues as well, I decided to knuckle down and find my own way. I put what I've learned here.

Also, this site can be helpful: [Eric Duncan's blog](http://eduncan911.com/software/octopress-powershell-and-posh-git-oh-my.html).

Prerquisite: You have some experience with GitHub/git:
* ...you can create a repository
* ...you have git ed locally espcially the git shell
* ...you have used some git commands on the command line (i.e., the Win7 GUI isn't useful for this thing)

Tools that I used
* Windows 7
* Ruby 2.0.0 (There are issues with earlier versions of Ruby that have workarounds but ...)
* [GitHub for Windows 2.0](https://windows.github.com/): the "posh-git" shell


1. In your browser, navigate to github and create a new respository as blog. To do this you must name the new repository with your github username suffixed with ".github.io", e.g., "tadmacy.github.io". The blog will have the URL "https://username.github.io" unless you wish to apply a custom domain. See the github docs for more info.

---After clicking "Create", you'll see a screen like this one.

![alt text](../images/snaps/github-repo-create..png "Repo setup page")

---Make sure that the blog is empty, i.e., no README.md. Don't clone it or pull it to your local host.

---We are finsihed here for now but don't close this page quite yet. While most of the remaining steps are performed in the git shell of your local desktop, we'll need to come back here to get the "HTTP" URL shown near the top of the page in the "Quick setup" block.

<pre>Note that steps 2 - 4 below follow the [Octopress instructions](http://octopress.org/docs/setup/) exactly. It is only when on the [Deploying to GitHub](http://octopress.org/docs/deploying/github/) page that some variations may be needed.</pre>

2. In your desktop, open the command shell and navigate to your github workspace.

```
cd <your-git-project-folder>
```
--- Use git to clone the Octopress framework as the basis for your blog. Your blog "repo" begins its life as a clone of the [https://github.com/imathis/octopress].

```
git clone git://github.com/imathis/octopress.git <your-blog-name>
```

3. Now we need to unpack the Octopress files:

```
cd <your-blog-name>
bundle install
```

---After some gems are checked or installed, the install process should end on success.

4. Next, we run the Octpress action script like this:

```
rake install
```

---This makes a few new folders. When it finishes, note that the command prompt changes. It now includes a change count indicator following "master". The changes are in red because they have not been comitted yet.

5. Now, we need to add the github blogging support framework. So, run this:

```
rake setup_github_pages
```

---This command will ask you for the URL of the remote repository. Get the correct URL from the github page in of step 1. Your HTTP URL shown at the top of the page in the "Quick setup" block.

---Among many changes, note that a "source" branch has been created. Your shell prompt has changed, too. Now "master" is "source". And there are more uncommited materials.

<pre>More detailed information on this setup process can be found on [the Octopress site](http://octopress.org/docs/deploying/github/).</pre>

6. At this point the [Octopress documentation](http://octopress.org/docs/deploying/github/) seems to miss a step. Or, more accurately, the rake scripts miss an important step: setting the git origin for the "_deploy" folder just created. To fix that, do this:

```
[source +2 ~3 -0 !]> cd .\_deploy
[master]> git remote set-url origin https://github.com/tadmacy/tadmacy.github.io.git
```

---You can use the git remote command to verify that origin is properly set:

```
[master]> git remote -v
```

---Don't forget to do a "cd .." to get back the the "source" branch.

---In your "source" branch, you want to verify that the origin URL is correctly set. Do that using the process above while in the "source".

---Some people report that the origin is not set for source either, but it was okay in my install.

7. You should now be able to commit changes to github (be sure you are in "source"!) with the two following commands:

```
[source +2 ~3 -0 !]> rake generate
[source +2 ~3 -0 !]> rake deploy
```

---Watch for errors. Hope you had none and are ready to move on!

---By the way, there is a command that does these two things -- or, at least, appears to!

```
[source +2 ~3 -0 !] rake gen_deploy
 ```

7. But wait, there's more! You may have noticed that we did not commit the changes. Let's do that now. In "source":

```
[source +2 ~2 -0 !]> git add .
[source +117 ~2 -0]> git commit -m "first commit" 
[source +117 ~2 -0]> rake gen_deploy
```

8. Here's where I go back to github in my browser and look at the blog repository file list. You should see a lot files there now. And after a few minutes, your blog should be visible at "http://<your-blog-name>.github.io".

9. Lastly resume the instructions at Octopress. Be sure to apply the instructions in "Don't forget to commit the source for your blog."

10. Next steps:
* personalize your blog by editing the "_config.yml" file
* add posts to the "source\_posts" folder, commit them, and run rake gen\_deploy to push them to your new blog.

