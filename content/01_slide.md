!SLIDE
# Git Inside #
## What the bloody bastard is about ##

!SLIDE commandline
# The Goal #

Feeling your way around this lot:

    $ git init; find .git
    .git
    .git/branches
    .git/refs
    .git/refs/heads
    .git/refs/tags
    .git/hooks
    .git/hooks/post-commit.sample
        <snip>
    .git/hooks/pre-applypatch.sample
    .git/HEAD
    .git/info
    .git/info/exclude
    .git/description
    .git/objects
    .git/objects/info
    .git/objects/pack
    .git/config


!SLIDE
# Why Bother? #

!SLIDE subsection
# Curiosity #

!SLIDE subsection
# It's actually useful #
## A quick edit of `.git/config` ##
### vs. ###
## "What was the syntax for adding a new remote repo again?" ##

!SLIDE subsection
# It's actually useful #
## fetch vs. pull ##

!SLIDE subsection
# It's actually useful #
## merge vs. rebase ##

!SLIDE
# What is git? #

!SLIDE
# Duh #

!SLIDE
# Distributed #
# Acyclic Directional Graph #

## on top of a filesystem-based ##
# NoSQL Immutable Key/Value #
## database ##

!SLIDE
# Duh indeed #


!SLIDE
# Rewind! #

!SLIDE
# Filesystem Snapshots #

!SLIDE
# History #

!SLIDE
# The entire history
# of all the files in the repo

!SLIDE
# Need to store them somehow #

!SLIDE
# Let's invent a new database!

!SLIDE subsection
# Git Object Database #

!SLIDE
# Key/Value store

!SLIDE
# Value can be anything

!SLIDE
# Key is a SHA-1 digest

!SLIDE
# Immutable objects
## Changing a value means storing it separately
## ... ##

!SLIDE
# Immutable objects
## Changing a value means storing it separately
## under a different key.

!SLIDE
# Distributed by design

!SLIDE
## The same object

!SLIDE
## has the same key

!SLIDE
## everywhere

!SLIDE
## all the time

!SLIDE
## regardless of metadata.


!SLIDE subsection
# What objects?

!SLIDE
# Files

!SLIDE
# Directories

!SLIDE
# Commits

!SLIDE
# Branches

!SLIDE
# Tags

!SLIDE bullets
# Git's trinity

* Blob
* Tree
* Commit

!SLIDE subsection
# What's a Blob?

!SLIDE
# a regular file

!SLIDE
# no metadata

!SLIDE
# stored as is

!SLIDE commandline
# Blob exposed

    $ git cat-file blob 858e207b3ea2e44f37eb6b85653a780ec777ad67

    package main

    import "fmt"

    func main() {
            fmt.Println("Hello, World")
    }


!SLIDE subsection
# What's a Tree?

!SLIDE
# a directory

!SLIDE
# contains trees and blobs

!SLIDE
# keeps metadata

!SLIDE
## filenames

!SLIDE
## file attributes

!SLIDE
## path and directory structure

!SLIDE commandline
# Tree exposed

    $ git ls-tree d6eaef3888c13602a89a5c59c88007819af10f98

    100644 blob 7169...4f9d8b8a3874cb9afea9afab	hello.go
    040000 tree 27a2...ef8d582b6fafa5163251898e	lib
    100644 blob ae36...8b0135b0d26cf6c976909b58	COPYING
    100644 blob ee3e...239aca529f82eed2fbae8b08	db.go

!SLIDE subsection
# Renaming a file

!SLIDE
# **doesn't**
# change the blob

!SLIDE
# **does**
# create a new tree


!SLIDE subsection
# What's a Commit?

!SLIDE
# a repository snapshot

!SLIDE
# a piece of history

!SLIDE
# git's cornerstone

!SLIDE
# has one Tree

!SLIDE
# one or more parents

!SLIDE bullets incremental
# metadata

* author's name & email
* commit message
* timestamp
* ...

!SLIDE commandline
# Commit exposed

    $ git cat-file commit fa29...efae368b0135b0d26cf6c976909b58

    tree d6eaef3888c13602a89a5c59c88007819af10f98
    parent 4593afa00bb4cb52eb289f72788a1879231947d6
    author Tomas Sedovic <tomas@sedovic.cz> 1324171835 +0100
    committer Tomas Sedovic <tomas@sedovic.cz> 1324171835 +0100

    Add "hello world" in Go


!SLIDE subsection
# Keeping track of history

!SLIDE
# Commit is a snapshot in time

!SLIDE
# Its parent is the previous snapshot

!SLIDE
# Whose parent is the snapshot before that

!SLIDE
# Commits all the way down!

!SLIDE
# "Show me a commit,"

!SLIDE
# "and I'll show you its history."

!SLIDE center
# Commits
![history](commit-history.png)

!SLIDE subsection
# Branches, too!

!SLIDE center
![branches](commit-branches.png)

!SLIDE subsection
# Nota bene

!SLIDE
# You can see commit's past but not its future

!SLIDE
# Branch needs to keep track of the latest commit

!SLIDE
# It's possible to have "orphans"

!SLIDE
# No harm but they waste space

!SLIDE
# Garbage collection

!SLIDE subsection
# Epiphany

!SLIDE
# Everything Is A Commit

!SLIDE subsection
# Branches are commits

!SLIDE center
![branches](branches.png)

!SLIDE
# Branch is just a named pointer to a commit

!SLIDE
# Always points to the latest change

!SLIDE
# Must be updated when a new commit is added

!SLIDE commandline
# Just a named commit

    $ cat .git/refs/heads/master
    d846a66e6c8b6b301954bc4a9821698908805192

    $ cat .git/refs/heads/new-ui
    466f179482db268ee8ddea0610560041721855bb

    $ cat .git/refs/heads/stable
    c83126effb209dbf492f01589005ecd77e4deaa8

!SLIDE subsection
# What does it mean?

!SLIDE
# Whenever you use a branch,

!SLIDE
# you can use a commit hash instead,

!SLIDE
# and vice versa.

!SLIDE commandline
# checkout

    $ git checkout master


    $ git checkout d846a66e6c8b6b301954bc4a9821698908805192

!SLIDE commandline
# format-patch

    $ git format-patch -o /tmp/patches master


    $ git format-patch -o /tmp/patches d846c8b6b301954bc4a982169

!SLIDE commandline
# interactive rebase

    $ git rebase -i master


    $ git rebase -i d846a66e6c8b6b301954bc4a9821698908805192


!SLIDE subsection
# Tags are commits

!SLIDE center
![tags](tags.png)

!SLIDE
# Tag is a name for a particular commit

!SLIDE commandline
# Stored just like branches

    $ cat .git/refs/tags/beta
    3666273ea7a53885a2bb6403393d9564cfea6424

    $ cat .git/refs/tags/rc1
    5cdd7d048afbb4c44b378e142c990907d460a9d7

    $ cat .git/refs/tags/rc2
    f6dd12d6398e57a84ba993f3f2900ee0636963f3


!SLIDE subsection
# Tags vs. Branches

!SLIDE
# Tags never change

!SLIDE
# Branches move forward

!SLIDE subsection
# Merging branches

!SLIDE
# Branches exist to be merged back in

!SLIDE
# Or die

!SLIDE
# `git-merge`

!SLIDE
# Joins two branches together

!SLIDE
# Merges a topic branch onto the current one

!SLIDE
# The order is significant

!SLIDE
# These are different

    git checkout master; git merge feature

    git checkout feature; git merge master

!SLIDE
# Merging creates a new commit

!SLIDE
# Merge Commit

!SLIDE
# Has two parents

!SLIDE
# Determines the commit order

!SLIDE
# Fixes conflicts

!SLIDE
# Branches still tracked separately

!SLIDE
# Merge commit joins their changes

!SLIDE
# Merging 2 branches
![merge initial](merge-initial.png)

!SLIDE
# Merge Commit
![merge commit](merge-commit.png)

!SLIDE
# Single timeline
![merge final](merge-final.png)
