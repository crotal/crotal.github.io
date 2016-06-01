---
layout: post
title: "Understand how Git works in 5 diagrams"
date: 2014-08-18 02:34
categories: Programming
tags: Git
slug: understand-git-objects
---

Git has been a perfect choice for Version Control System. With git we can promptly switch between different revisions from our project, logging every line changed from the code, and check the difference between any two revisions.

In this article I'm gonna explain you how Git objects, the most important part in Git, works.

## Git Obejcts

If you have Git repositories in your computer, you must have noticed that there is a folder named `.git` in your root of the repository.

<!--more-->

It's like this:

```
▸ hooks/
▸ info/
▸ logs/
▸ objects/
▸ refs/
  config
  description
  FETCH_HEAD
  HEAD
  index
  packed-refs
```

The most important folder is `objects`, dive into it, you'll mostly see 2 hex-digits folders. Git name objects with SHA-1 digest, in those 2 hex-digests folders we'll find some 38 hex-digits files. The 2 hex-digits folder combines to the 38 hex-digits file forms a 40 characters SHA-1 digest, and this is gonna be the full id of the Git Object.

For example, The file below will be identified to a object with id `058ca0a55e60eae77dac9818f915dad581b5465b`.

```
▾ objects/
  ▾ 05/
      8ca0a55e60eae77dac9818f915dad581b5465b
```

There are 4 types of objects: <strong>Commit, Tree, Blob and Tag</strong>. Let's talk about them.

## The Commit

If you are using Git in developing, you must be familiar with commit, we use the sub-command `commit` in the Git repository to submit the changes we did in project, each commit stands for a revision of your repository.

The content in a commit object is like this:

```
tree 9103f24ed99af203dec0bd0e1646838bfd253254
parent bd8dd3d9562702ce9fd0648f32a5e068c71722f0
author Dinever <dinever@Dinevers-MacBook-Pro-2.local> 1408288514 +0800
committer Dinever <dinever@Dinevers-MacBook-Pro-2.local> 1408288514 +0800

Add README
```

parent stands for the previous commit, notice that there can be two parents in a single commit.(it happens when we do git merge)

Although commit stands for revision, it doesn't store any changes we made. Commit objects contains a tree object, which will handle the changes committed.

## The Tree

A tree is a collection of files, it's like folders in computer system, it contains references to those changed files (Called blobs, which will be explained in the next section) or other trees at that time.

The content in a tree object is like this:

```
100644 blob 4254625ef9c693183846fa0b1d4d4b68264473ae	a.txt
100644 blob 544e0fd7b60a503d35bf4bbfcc9fbf653822b83f	b.txt
040000 tree 21c9fa7518ee36880739266fd120f675e085b5c8	lib
```

So the tree objects only contains the name of the files we changed, who is handling the actual content of the files we changed? It's Blob.

## The Blob

Blob is just the content of the file you changed and committed. If made 10 changes to a signal file 'a.txt' during the development of your project, there would be 10 blob objects which records the 10 revision of 'a.txt'.

## How they works?

Thanks to [Vincent Xie](http://xiewenwei.net/), who has made a great project [here](https://github.com/xiewenwei/dig-git) to explain how Git works.

I will explain how git works using his demo project, firstly you can check the history diagram below.

![Screencast from Ungit.](/images/dig_git-1.png)

### 1. Initialize our repository and add a.txt

Firstly we create a file named `a.txt`, and write the content `it is a`, then we add it to Git repository and commit the change.

```
touch a.txt
echo -e 'it is a' >> a.txt
git add a.txt
git commit -m 'Add a.txt'
```

After that there will be 3 Git objects, a commit, a tree and a blob, the relation is like this:

![The 1st Commit](/images/9c44291362d64765803afba65fa2a0a4.png)

The object with a blue background is the commit we made, you can see that there is a reference in it, which refer to the tree object, which is in grey background, and the tree object contains a reference to a blob, which is white backgrounded.

### 2. Modify a.txt

In this step we made a change to `a.txt`, we add a line under it.

```
echo -e 'new line in a.txt' >> a.txt
git add a.txt
git commit -m 'Add a.txt'
```

After the commit, those objects is like this:

![The 2nd Commit](/images/ba84d2ce8ba5066444a3e382ed644206.png)

Notice the commit `aef54`, which is the new commit we made, there are 2 references in it, one is to the parent commit we made in the previous step, the other one is to the tree which handles the changes we made in this commit. Cause `a.txt` is changed in this commit, Git creates a new Blob object identified by `42546` which holds the content of it.

### 3. Add b.txt and lib/c.txt

In this step we creates 2 files and a folder.

```
touch b.txt
echo -e "it is b" >> b.txt
mkdir lib
touch c.txt
echo -e "it is c" >> c.txt
git add b.txt lib/c.txt
git commit -m "Add b.txt and lib/c.txt"
```

And the diagram is like this:

![The 3rd Commit](/images/1ab7f0d2ab24734f0af6fc2d8e7c7a7d-1.png)

Notice that the new tree object `54824` have 3 references, one is to the blob `42546`, which hold the content in `a.txt`, since we didn't make any changes to `a.txt`, there is no necessary for Git to create a new Blob for it. Another one is to the blob `544e0`, which contains the content we write in `b.txt`, notice that the last reference is to another tree identified with `21c9f`, which stands for the folder `lib/`, and in the sub-tree `21c9f` there is a reference to the new created blob `3e45c` which contains the change we made to `lib/c.txt`.

### 4. Checkout a new branch 'test1'

In this step we create a new branch named `test1` from the first commit `e9178`, and we create a file named `d.txt` and write content to it.

```
git checkout -b test1 e9178
touch d.txt
echo -e "it is d" >> d.txt
git add
git commit -m "Add d.txt"
```

Then the objects diagram is like this:

![The 4th Commit](/images/1ab7f0d2ab24734f0af6fc2d8e7c7a7d-1-1.png)

We created a new commit object `0c5c1` which have the parent referenced to the first commit `e9178`.

### 5. Merge test1 to master

In this step we merged test1 to master.

```
git checkout master
git merge test1
```

The diagram looks a bit complex.

![The 5th Commit](/images/8e555b42a9775d725bd95ee7f369af7c-1.png)

You only have to notice the commit `def18`, notice that it have two parents, one referenced to the previous  branch master, the other one is to the previous branch test1.

Try to understand the five diagrams above, then you should be aware of how Git works.

If there's any doubt at all, please leave a comment below.
