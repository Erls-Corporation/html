html
====

Deliverables of the HTML Working Group


Spec Editing Cheatsheet
===

Checking out the HTML spec

   1. the HTML spec is on GitHub https://github.com/w3c/html

   2. there are multiple branches - get master to build the spec

Pull the code from GitHub:

    $ git clone https://git@github.com/w3c/html.git
    $ cd html
    $ git checkout -b whatwg origin/feature/whatwg

To check what branches you have:

    $ git branch

To continue editing at a later stage:

    $ git checkout master
    $ git pull --rebase

Then check out each branch and:

    $ git rebase master

To commit and push code to GitHub:

    $ git commit -a
    $ git push


How the branches fit together
---
![Branches for 5.0 CR, 5.1 WD, extension specs](http://www.gliffy.com/pubdoc/4126915/L.png)

[HTML version of flowchart image](http://www.gliffy.com/go/publish/4126915/)


Configuring git
---

If you're editing the spec on Windows, be sure to read up on
[how to deal with line endings](https://help.github.com/articles/dealing-with-line-endings).

You'll probably want to configure Git to automatically rebase on pull.
To do this, edit `.git/config` in your repository. In the
`[branch "master"]` section, add a `rebase = true` line. To ensure
this happens for any new branches you create, add a new section like so:

    [branch]
        autosetuprebase = always


Installing the necessary software
---

  1. You need to have python installed on your system.

  2. Install [Anolis](http://anolis.gsnedders.com/):

        $ hg clone ssh://hg@bitbucket.org/ms2ger/anolis
        $ cd anolis; sudo python setup.py install

     Periodically, make sure your Anolis is up to date:

        $ cd anolis
        $ hg pull

     If there have been changes, update and reinstall:

        $ hg update
        $ sudo python setup.py install

  3. Install [html5lib](http://code.google.com/p/html5lib/):

        $ hg clone https://code.google.com/p/html5lib/
        $ cd html5lib/python; sudo python setup.py install

     Periodically, make sure your html5lib is up to date:

        $ cd html5lib
        $ hg pull

     If there have been changes, update and reinstall:

        $ hg update
        $ cd python; sudo python setup.py install

  4. Install [lxml](http://lxml.de):

        $ sudo easy_install lxml

Build the spec
---

    $ cd html
    $ make html    # or 'make 2dcontext' for the Canvas spec, or 'make all' for both

If successful, the single page version of the spec can be found at
`output/html/single-page.html`, and the multipage version at
`output/html/spec.html`.

Cherry pick commits from the WHATWG spec
---

   1. the WHATWG spec is developed in SVN at the WHATWG
   2. there is a git clone of it on GitHub
https://github.com/w3c/html/tree/feature/whatwg
   3. we cherry-pick commits from the WHATWG spec into the html spec

Checkout the WHATWG spec:

    $ git checkout feature/whatwg

Find out commit differences to html branch:

    $ git cherry master

Find a commit that you want to apply, get it’s SHA:

    $ git log

Show the SHA commit e.g.:

    $git show 56446c4536af1ec5b39bde03b402d0772625fd92

Checkout the html spec:

    $ git checkout master

Cherry pick the commit selected from before:

    $ git cherry-pick -x 56446c4536af1ec5b39bde03b402d0772625fd92

If you want to edit the commit:

    $ git cherry-pick -x -e 56446c4536af1ec5b39bde03b402d0772625fd92

Show changes to GitHub:

    $ git diff origin

If you want to abort a cherry-pick:

    $ git cherry-pick --abort

If you need a merge strategy:

    $ git cherry-pick --strategy=ours -x 56446c4536af1ec5b39bde03b402d0772625fd92

Only pick parts of the commit (no-commit, then add selectively):

    $ git cherry-pick -n -x 56446c4536af1ec5b39bde03b402d0772625fd92
    $ git add -i
       r = revert a file
       p = go through by hunk and re-patch
       (when “>>” hit ENTER to start)
       q = bye
    To just commit the staged parts:
    $ git commit
    To reset the unselected hunks:
    $ git checkout complete.html index source

Check if a specific commit is contained in the master:

    $ git checkout master
    $ git branch --contains 56446c4536af1ec5b39bde03b402d0772625fd92

Create a new feature branch:
---
[make sure your .gitconfig defaults push to upstream]

    $ git checkout master
    $ git checkout -b feature/blah
    $ git push --set-upstream origin feature/blah


Merging a feature branch:
---
You should not use the GitHub pull request merge feature, but instead rebase locally and push (to avoid a messy merge and get a linear history):

    $ git checkout feature/blah
    $ git rebase master

Test everything still works, then push to GitHub:

    $ git push -f

Then merge on master:

    $ git checkout master
    $ git merge feature/blah
    $ git push

If you want to delete the branch, too, remove it both on local and GitHub (the issue with the pull request will continue to exist):

    $ git branch -d feature/blah
    $ git push origin :feature/blah
