---
layout: post
title:  "Under The Hood: How Git works?"
date:   2022-06-26 09:00:00 +0300
author: "Oleksandr Romanov"
image: "img/20220626/branch.jpg"
description: "Digging deep into how Git works"
summary: "Digging deep into how Git works"
header-img: "img/20220626/branch.jpg"

---

Photo by [Sebastian Herrmann on Unsplash](https://unsplash.com/photos/scZjGeVXCq8?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

Finding a test engineer who would not have heard of version control systems is difficult nowadays. Some have worked (and work with quite "old") - such as **[Subversion](https://en.wikipedia.org/wiki/Subversion)**, **[Mercurial HG](https://en.wikipedia.org/wiki/Mercurial)**, etc. 

But the vast majority of modern projects today use **[Git](https://en.wikipedia.org/wiki/Git)**. If you need to download a "fresh" version of the product code or automated tests, go to the Terminal (or GUI tool) and perform a `git fetch` or `git pull`.

Learning the basic Git commands is not that hard. It is a bit more difficult to understand what `merge` and `rebase` are (and their difference).

But few engineers look deeper - **how does Git work "under the hood"**? Of course, you don't have to know this to use Git. But I was still interested to understand. Plus, if you will be asked about Git internals at the interview - you will already know it! (But why ask such a question???).  

Welcome to the world of **graphs** and **hash functions**. You can learn about another application of hash functions - Merkle trees - in my **[previous post](https://alexromanov.github.io/2022/06/19/bchain-test-7-merkle-tree/)**.  

## A bit of Git theory

Let me remind you what a typical workflow with Git looks like:

- Make a clone of a remote repository: `git clone https://somerepo`
- Make a branch for your changes (fixes, tests, etc.): `git checkout -b fix/autotests`
- Write everything you want (Git will notice it but wait for your actions): `git status` returns "changes not staged for commit."
- Add files to staged area using: `git add`
- Create a new commit: `git commit -m "Fix autotests"`
- Send prepared commit to a remote repository: `git push origin fix/autotests`
- Pass the code review and merge the changes into the main branch: `git rebase` or `git merge`
- Send changes to the main branch from the local repository to the remote one: `git push origin main/master`
- Remove the branch, if necessary: ​​`git branch -D fix/autotests`

## The structure of Git

![Project Structure]({{ site.baseurl }}/img/20220626/1.png)

There are three types of objects that Git takes care of:

- **Commit** - contains the author of the commit, date, message, and pointer to the tree of changes. Also, each commit has one (or more) parent commit;
- **Tree** - contains a pointer to other trees and specific blob objects;
- **Blob** - specific data (source code files, pictures, videos, etc.)

So what is the structure of the Git repository? This is a data structure called a **[graph](https://en.wikipedia.org/wiki/Graph_(abstract_data_type))**. Or, more specifically - a **[directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph)**.

**Where can all this information be found?** When you clone a repository or create a new one, all this data will be stored in a hidden `.git` directory in the project.

**What are tags and branches?** These are just pointers to a commit object.

## What happens during git init?

Let's create a new repository in the directory - **testrepo**. To do this, go to the folder and run the command `git init`.

As a result, we get a lot of exciting things in the `.git` directory:

![Project Structure]({{ site.baseurl }}/img/20220626/2.png)

## Making a first commit

Create a new `README.md` file with any content and add it to the first git add. Now, `.git` looks like this:

![Project Structure]({{ site.baseurl }}/img/20220626/3.png)

We can see that Git created some new folders and files: **3a** folder with a strange file inside.  

It results from the **[SHA-1 hash function](https://en.wikipedia.org/wiki/SHA-1)** running on our README.md file. (You can read about the hash functions in **[one](https://alexromanov.github.io/2022/05/01/bchain-testing-1-hashing/)** of my previous blog posts)

Git has a separate team to help us with research: git cat-file.

- `-t` argument indicates the type of object
- `-p` argument shows the contents of the object

**Note:** You may specify not the full hash of the object - but only the first six characters (*Keep in mind that you must also set the subfolder characters - in our case it is 3a. And then the first four characters of the object*)

In our case, the results will be as follows:

![Project Structure]({{ site.baseurl }}/img/20220626/4.png)

When we added the README.md file to the staging area, Git also created a **blob** object under the hood.

Next, create a new commit with the file and look at the contents of the `.git` directory again.

![Project Structure]({{ site.baseurl }}/img/20220626/5.png)

As you can see, now we have not one new object but three. One we have already seen (starts with **3ae2e2**). What about others?

![Project Structure]({{ site.baseurl }}/img/20220626/6.png)

**232c6a** is a tree object. It points to a blob object (we also see the hash of this object and the name). `100644` is the permissions for this file.

![Project Structure]({{ site.baseurl }}/img/20220626/7.png)

**85ac4e** is a commit object. It points to an object of type tree (we see its complete hash) and indicates the author and the text of the commit.

## More commits

To complete the task - add another file and correct `README.md`.

![Project Structure]({{ site.baseurl }}/img/20220626/8.png)

`.git` contains new objects:

![Project Structure]({{ site.baseurl }}/img/20220626/9.png)

First, we have a new commit object that contains a pointer to a new tree object, **f71e00a8f2d3eff8348ad77149932dc11ab9e717**.

In addition, in this commit we have parent **85ac4e3d6dba749723b69ce6f655c5acf5a00782**

![Project Structure]({{ site.baseurl }}/img/20220626/10.png)

If we look at the new tree, we will see that it now points to two objects of type blob: a new file and a modified README.md.

![Project Structure]({{ site.baseurl }}/img/20220626/11.png)

Interestingly, the blob object for README.md already has a different hash. The reason is that we changed the file's text - so the hash value also changed. But the old blob file also remained available (3ae2e2)!

### A few words about the branches

Finally, I want to show what happens when we create new branches in Git.

Let's run the `git checkout -b fix/autotests` command and look in `.git`.

![Project Structure]({{ site.baseurl }}/img/20220626/12.png)

If we look at the contents of the `.git/refs/heads/fix/autotests` file, we will see that it points to our second commit!

![Project Structure]({{ site.baseurl }}/img/20220626/13.png)

## Conclusions

As you can see - Git under the hood looks not so complicated. Especially when you already know what hashing is.

Therefore, the next time you see unknown numbers and letters near commits, you will know that it is a just **hash** value from objects.