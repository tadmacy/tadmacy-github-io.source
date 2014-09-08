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



```
[source]> git config branch.master.remote origin
[source]> rake generate
[source]> git clone origin master
// git remote set-url origin https://github.com/tadmacy/tadmacy.github.io.git
//[source]> git remote set-url  origin https://github.com/tadmacy/tadmacy.github.io.git
[source]> git push origin source
```

```
The page build failed with the following error:

The tag `include_array` in `source/index.html` is not a recognized Liquid tag. For more information, see https://help.github.com/articles/page-build-failed-unknown-tag-error.

If you have any questions please contact us at https://github.com/contact.
```

    Directory: C:\Users\macy\Documents\GitHub\tadmacy.github.io


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----          9/5/2014   3:38 PM            .themes
d----          9/5/2014   3:38 PM            plugins
d----          9/5/2014   3:38 PM            public
d----          9/5/2014   3:38 PM            sass
d----          9/5/2014   3:38 PM            source
d----          9/5/2014   3:43 PM            _deploy
-a---          9/5/2014   3:38 PM        401 .editorconfig
-a---          9/5/2014   3:38 PM         13 .gitattributes
-a---          9/5/2014   3:38 PM        184 .gitignore
-a---          9/5/2014   3:38 PM        120 .powrc
-a---          9/5/2014   3:38 PM         23 .slugignore
-a---          9/5/2014   3:38 PM        105 .travis.yml
-a---          9/5/2014   3:38 PM       1332 CHANGELOG.markdown
-a---          9/5/2014   3:38 PM        432 config.rb
-a---          9/5/2014   3:38 PM        668 config.ru
-a---          9/5/2014   3:38 PM        480 Gemfile
-a---          9/5/2014   3:38 PM       2548 Gemfile.lock
-a---          9/5/2014   3:43 PM      16484 Rakefile
-a---          9/5/2014   3:38 PM       3123 README.markdown
-a---          9/5/2014   3:43 PM       3021 _config.yml


C:\Users\macy\Documents\GitHub\tadmacy.github.io [source]> rake deploy
## Set the codepage to 65001 for Windows machines
## Deploying branch to Github Pages
## Pulling any updates from Github Pages
cd _deploy
fatal: No remote repository specified.  Please, specify either a URL or a
remote name from which new revisions should be fetched.
cd -

## Copying public to _deploy
cp -r public/. _deploy
cd _deploy

## Committing: Site updated at 2014-09-05 19:45:34 UTC
On branch master
nothing to commit, working directory clean

## Pushing generated _deploy website
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

## Github Pages deploy complete
cd -
C:\Users\macy\Documents\GitHub\tadmacy.github.io [source]> cd .\_deploy
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> git pull mast
er
fatal: 'master' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> git remote -v

origin
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> git push -u o
rigin master
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> git remote ad
d origin https://github.com/tadmacy/tadmacy.github.io.git
fatal: remote origin already exists.
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> git remote se
t-url  origin https://github.com/tadmacy/tadmacy.github.io.git
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> git push -u o
rigin master
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (5/5), 398 bytes | 0 bytes/s, done.
Total 5 (delta 0), reused 0 (delta 0)
To https://github.com/tadmacy/tadmacy.github.io.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> rake deploy
(in C:/Users/macy/Documents/GitHub/tadmacy.github.io)
## Set the codepage to 65001 for Windows machines
## Deploying branch to Github Pages
## Pulling any updates from Github Pages
cd _deploy
Already up-to-date.
cd -

## Copying public to _deploy
cp -r public/. _deploy
cd _deploy

## Committing: Site updated at 2014-09-05 19:57:37 UTC
On branch master
Your branch is up-to-date with 'origin/master'.

nothing to commit, working directory clean

## Pushing generated _deploy website
Everything up-to-date

## Github Pages deploy complete
cd -
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> dir
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> rake deploy
(in C:/Users/macy/Documents/GitHub/tadmacy.github.io)
## Set the codepage to 65001 for Windows machines
## Deploying branch to Github Pages
## Pulling any updates from Github Pages
cd _deploy
Already up-to-date.
cd -

## Copying public to _deploy
cp -r public/. _deploy
cd _deploy

## Committing: Site updated at 2014-09-05 19:59:49 UTC
On branch master
Your branch is up-to-date with 'origin/master'.

nothing to commit, working directory clean

## Pushing generated _deploy website
Everything up-to-date

## Github Pages deploy complete
cd -
C:\Users\macy\Documents\GitHub\tadmacy.github.io\_deploy [master]> cd ..
C:\Users\macy\Documents\GitHub\tadmacy.github.io [source +1 ~0 -0 !]> git commit
 -m "first 2 posts"
On branch source
Your branch is based on 'origin/master', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

Untracked files:
        source/_posts/

nothing added to commit but untracked files present
C:\Users\macy\Documents\GitHub\tadmacy.github.io [source +1 ~0 -0 !]> git add .
C:\Users\macy\Documents\GitHub\tadmacy.github.io [source +2 ~0 -0]> git commit -
m "first 2 posts"
[source e024944] first 2 posts
 2 files changed, 55 insertions(+)
 create mode 100644 source/_posts/2014-08-05-how-to-check-list.md
 create mode 100644 source/_posts/2014-08-06-how-to-change-your-jekyll-github-bl
og-theme.md
C:\Users\macy\Documents\GitHub\tadmacy.github.io [source]> rake gen_deploy
## Set the codepage to 65001 for Windows machines
## Generating Site with Jekyll
directory source/stylesheets/
   create source/stylesheets/screen.css
Configuration file: C:/Users/macy/Documents/GitHub/tadmacy.github.io/_config.yml

            Source: source
       Destination: public
      Generating...
                    done.
 Auto-regeneration: disabled. Use --watch to enable.
## Deploying branch to Github Pages
## Pulling any updates from Github Pages
cd _deploy
Already up-to-date.
cd -

## Copying public to _deploy
cp -r public/. _deploy
cd _deploy
warning: LF will be replaced by CRLF in atom.xml.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/2014/08/05/how-to-check-list/index.
html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/2014/08/06/how-to-change-your-jekyl
l-github-blog-theme/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/archives/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/categories/blog/atom.xml.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/categories/blog/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in robots.txt.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in stylesheets/screen.css.
The file will have its original line endings in your working directory.

## Committing: Site updated at 2014-09-05 20:01:33 UTC
[master c2b509d] Site updated at 2014-09-05 20:01:33 UTC
warning: LF will be replaced by CRLF in atom.xml.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/2014/08/05/how-to-check-list/index.
html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/2014/08/06/how-to-change-your-jekyl
l-github-blog-theme/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/archives/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/categories/blog/atom.xml.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in blog/categories/blog/index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in index.html.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in robots.txt.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in stylesheets/screen.css.
The file will have its original line endings in your working directory.
 65 files changed, 2030 insertions(+)
 create mode 100644 assets/jwplayer/glow/controlbar/background.png
 create mode 100644 assets/jwplayer/glow/controlbar/blankButton.png
 create mode 100644 assets/jwplayer/glow/controlbar/divider.png
 create mode 100644 assets/jwplayer/glow/controlbar/fullscreenButton.png
 create mode 100644 assets/jwplayer/glow/controlbar/fullscreenButtonOver.png
 create mode 100644 assets/jwplayer/glow/controlbar/muteButton.png
 create mode 100644 assets/jwplayer/glow/controlbar/muteButtonOver.png
 create mode 100644 assets/jwplayer/glow/controlbar/normalscreenButton.png
 create mode 100644 assets/jwplayer/glow/controlbar/normalscreenButtonOver.png
 create mode 100644 assets/jwplayer/glow/controlbar/pauseButton.png
 create mode 100644 assets/jwplayer/glow/controlbar/pauseButtonOver.png
 create mode 100644 assets/jwplayer/glow/controlbar/playButton.png
 create mode 100644 assets/jwplayer/glow/controlbar/playButtonOver.png
 create mode 100644 assets/jwplayer/glow/controlbar/timeSliderBuffer.png
 create mode 100644 assets/jwplayer/glow/controlbar/timeSliderCapLeft.png
 create mode 100644 assets/jwplayer/glow/controlbar/timeSliderCapRight.png
 create mode 100644 assets/jwplayer/glow/controlbar/timeSliderProgress.png
 create mode 100644 assets/jwplayer/glow/controlbar/timeSliderRail.png
 create mode 100644 assets/jwplayer/glow/controlbar/unmuteButton.png
 create mode 100644 assets/jwplayer/glow/controlbar/unmuteButtonOver.png
 create mode 100644 assets/jwplayer/glow/display/background.png
 create mode 100644 assets/jwplayer/glow/display/bufferIcon.png
 create mode 100644 assets/jwplayer/glow/display/muteIcon.png
 create mode 100644 assets/jwplayer/glow/display/playIcon.png
 create mode 100644 assets/jwplayer/glow/dock/button.png
 create mode 100644 assets/jwplayer/glow/glow.xml
 create mode 100644 assets/jwplayer/glow/playlist/item.png
 create mode 100644 assets/jwplayer/glow/playlist/itemOver.png
 create mode 100644 assets/jwplayer/glow/playlist/sliderCapBottom.png
 create mode 100644 assets/jwplayer/glow/playlist/sliderCapTop.png
 create mode 100644 assets/jwplayer/glow/playlist/sliderRail.png
 create mode 100644 assets/jwplayer/glow/playlist/sliderThumb.png
 create mode 100644 assets/jwplayer/glow/sharing/embedIcon.png
 create mode 100644 assets/jwplayer/glow/sharing/embedScreen.png
 create mode 100644 assets/jwplayer/glow/sharing/shareIcon.png
 create mode 100644 assets/jwplayer/glow/sharing/shareScreen.png
 create mode 100644 assets/jwplayer/player.swf
 create mode 100644 atom.xml
 create mode 100644 blog/2014/08/05/how-to-check-list/index.html
 create mode 100644 blog/2014/08/06/how-to-change-your-jekyll-github-blog-theme/
index.html
 create mode 100644 blog/archives/index.html
 create mode 100644 blog/categories/blog/atom.xml
 create mode 100644 blog/categories/blog/index.html
 create mode 100644 favicon.png
 create mode 100644 images/bird_32_gray.png
 create mode 100644 images/bird_32_gray_fail.png
 create mode 100644 images/code_bg.png
 create mode 100644 images/dotted-border.png
 create mode 100644 images/email.png
 create mode 100644 images/line-tile.png
 create mode 100644 images/noise.png
 create mode 100644 images/rss.png
 create mode 100644 images/search.png
 create mode 100644 index.html
 create mode 100644 javascripts/github.js
 create mode 100644 javascripts/libs/jXHR.js
 create mode 100644 javascripts/libs/jquery.min.js
 create mode 100644 javascripts/libs/swfobject-dynamic.js
 create mode 100644 javascripts/modernizr-2.0.js
 create mode 100644 javascripts/octopress.js
 create mode 100644 javascripts/pinboard.js
 create mode 100644 javascripts/twitter.js
 create mode 100644 robots.txt
 create mode 100644 sitemap.xml
 create mode 100644 stylesheets/screen.css

## Pushing generated _deploy website
Counting objects: 89, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (79/79), done.
Writing objects: 100% (88/88), 189.31 KiB | 0 bytes/s, done.
Total 88 (delta 6), reused 0 (delta 0)
To https://github.com/tadmacy/tadmacy.github.io.git
   f5b0921..c2b509d  master -> master

## Github Pages deploy complete
cd -
C:\Users\macy\Documents\GitHub\tadmacy.github.io [source]>
```