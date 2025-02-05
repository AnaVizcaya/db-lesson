---
title: How to make a Lesson for DUNE
teaching: 30
exercises: 10
questions:
- How can I make a lesson like this from scratch using the DUNE template
objectives:
- Learn how to set up locally to build a lesson and to deploy it.
keypoints:
- If you can do basic markdown, you can do this.
- Import the template to your own GitHub account when developing new lessons
---


## Here I describe how I built this lesson.  You can follow along. 

> ## Note
> Because github insists on using ghpages for deployment, it is good to use your own github account for initial (and ongoing)
> development and pull over to [github.com/dune](https://github.com/dune) for the official version rather than using branches in the [github.com/dune](https://github.com/dune) github area.
> If you do not have one, set one up now at [github.com](https://github.com). 
{: .callout}

---

## First you need to decide on a name for your new lesson. 

What is your plan for your lesson?  What are you going to call it? 

---

## local setup for local rendering (optional)

Then follow the instructions [https://carpentries.github.io/lesson-example/setup.html#setup-for-local-rendering-of-the-lessons-optional](https://carpentries.github.io/lesson-example/setup.html#setup-for-local-rendering-of-the-lessons-optional) for setup on your local machine - in principle this is optional but in practice it is really helpful.  You are going to need ruby and pyYAML.  I used conda on a mac but they have instructions for Windows, Mac and UNIX. 

> ## Alert
> At this point you should stop following their instructions and start using our template to avoid overwriting DUNE specific items. 
{: .caution}

---

## Then import this template into a new area in your account with your new name



### Recommended method: make a clone and give it your new name 

1. Make certain you have a personal github site
2. Make a new repo on the github site:  `https://github.com/<yourname>?tab=repositories` by hitting the New button and entering the new name
3. Do the following git commands on your local machine
    ~~~
    # make a copy with your new name
    git clone https://github.com/dune/lesson-template.git <yournewlessonname>
    cd <yournewlessonname>
    # tell git that it is attached to the empty repository you just set up in step 2. 
    git remote set-url origin https://github.com/<yourname>>/<yournewlessonname>.git
    # push it back to git
    git push
    ~~~


4. Your code should appear at `https://github.com/<yourname>/<yournewlessonname>`and your
page should appear at `https://<yourname>.github.io/<yournewlessonname>` in a couple of minutes.

> ## if you can an error message from git
> This is a pretty big repository so one can hit buffer sizes.  This is an error I saw when I tried to push for the first time.
> ~~~
> git push
> Enumerating objects: 3939, done.
> Counting objects: 100% (3939/3939), done.
> Delta compression using up to 12 threads
> Compressing objects: 100% (1390/1390), done.
> error: RPC failed; HTTP 400 curl 22 The requested URL returned error: 400
> send-pack: unexpected disconnect while reading sideband packet
> Writing objects: 100% (3939/3939), 3.86 MiB | 9.98 MiB/s, done.
> Total 3939 (delta 2476), reused 3939 (delta 2476), pack-reused 0
> fatal: the remote end hung up unexpectedly
> ~~~
> {: .output}
> By increasing the buffer size
> ~~~
> git config http.postBuffer 524288000
> ~~~
> {: .source}
> and trying again. 
{: .challenge}

### Or you could try using the Github importer

> ## Note:  this failed when we tried it recently so above instructions may be better
{: .callout}

-  Make certain you have a personal github site

- Use [GitHub’s importer](https://github.com/new/import) to make a copy of this repo in your own GitHub account.

> ## GitHub Importer (failed when we tried it)
> This is like a GitHub Fork, but is not connected to the upstream changes.
{: .callout}

- Put the URL of this repository, that is `https://github.com/DUNE/lesson-template.git` in the `The URL for your source repository*` URL box. 

- Select the owner for your new repository (you). 

- Set the name you chose for your lesson repository. 

- Make sure the repository is public.

> ### Import to your GitHub account
> Please import to your own account and new lesson, work there and then move it over to `/DUNE/` once you have a decent draft in place. 
{: .callout}

---

## Now make a local copy on the gh-pages branch and edit away

If you did a github import you need to get a local copy
~~~
git clone -b gh-pages <your new repository>
cd <your new repository>
~~~
{: .language-bash}

Otherwise you are already there.

---

## Explore the file structure

You need to look at the following pages.

- `_config.yml` to set the title and other parameters for the lesson
- `AUTHORS` to tell people who is doing this
- `CITATION` how to cite the page - often just the URL
- `LICENSE`  you can keep it as is
- `LICENSE.md` 
- `README.md`
- `reference.md`
- `setup.md` This is currently the full setup for new users. 

Then throw your individual modules into `_episodes` with leading numbers to set the order.  The code will follow the ordering of the files in `_episodes`.  

There are very nice examples and a formatting tutorial at: [https://carpentries.github.io/lesson-example/](https://carpentries.github.io/lesson-example/)

## All lessons need to have a header that describes them

~~~
---
title: How to make a Lesson for DUNE
teaching: 30
exercises: 0
questions:
- How can I make a lesson like this from scratch using the DUNE template
objectives:
- Learn how to set up locally to build a lesson and to deploy it.
keypoints:
- If you can do basic markdown, you can do this. 


... body of the lesson ...

~~~

{: .source}

### There is a more information at:

- [Organization](https://carpentries.github.io/lesson-example/03-organization/index.html)
- [Formatting](https://carpentries.github.io/lesson-example/04-formatting/index.html)
- [Style Guide](https://carpentries.github.io/lesson-example/06-style-guide/index.html)

You can throw supplemental stuff into `_extras`.

## You can then build your site locally either as a server or just a local site.

### local site

~~~
make site
~~~
{: .language-bash}

This does a lot of setup - it's pulling in a lot of ruby "gem" files. 

Your local site will be in _sites/index.html

### Checking

~~~ 
make lesson-check
~~~
{: .language-bash}

will tell you about all the FIXME's you haven't fixed and missing formatting syntax.

### local server

~~~ 
make serve
~~~
{: .language-bash}

will launch a web server and a site at: `http://127.0.0.1:4000`  

(note that it is `http` not `https`)

You may need to reload the web site to see your changes. 

> ## Note 
> These will hang around until you kill them so if you try to launch twice you can't. Look for processes with:
{: .callout}


~~~
ps -ef | grep 'jekyll serve'
~~~
{: .language-bash}
You can identify the process number from the second column that is printed and kill that process.
Alternatively this can be achieved with a one line command:
~~~
ps -ef | grep '[j]ekyll serve' | awk '{print $2}'
~~~
{: .language-bash}
The `awk` command grabs the second column, the PID. The `[j]` is a trick to stop it from also passing the `grep` process to kill also.



### Publishing your draft site to `<yourname>.github.io/<yoursite>`

Ok, so now you should be able to push your site to github.io

We have provided a `gitadd.sh` script that adds the most common **source** files so you don't mistakenly publish all of the html you just generated.

~~~
# make certain you're up to date with the main repo
git pull
# make certain you are in your gh-pages branch
git status | grep gh-pages 
source gitadd.sh
git commit -m " I DID SOMETHING"
git push
~~~
{: .language-bash}

Wait a couple of minutes and you should see your page appear at:

`https://<yourname>.github.io/<yoursite>`


Ok, once you have your site in decent shape you can import it back to

`https://github.com/DUNE/<yoursite>`

### Making it official

Once you have it checked out you can use the import function to make an official dune site.

- Use [GitHub’s importer](https://github.com/new/import) to make a copy of this repo in your own GitHub account. (Note: This is like a GitHub Fork, but not connected to the upstream changes)

- Put the URL of this repository, that is `https://github.com/<yourname/<yoursite>.git` in the `The URL for your source repository*` URL box.

- Select the owner for your new repository: `DUNE`

- Set the name you chose for your lesson repository: `<yoursite>`

- Make sure the repository is public.

### Maintaining your site

- Try to make changes on your local copy - others can also do things in their local copies

- Use pull requests to merge changes into the official DUNE site where possible

- You can make minor patches directly on the main site but generally, it's better to work locally. 

