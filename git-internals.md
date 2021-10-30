---
marp: true
author: Nikolay Karadzhov
title: Git Internals
paginate: true
theme: uncover
class: invert
---

# The git data model

- Objects
	- Stored in the ***objects*** directory
	- Every object is named by the SHA1 of its content
	- Immutable. If something changes a new SHA1 is computed and new object is stored.

- Refs
	- Stored in the ***refs*** directory
	- Text file with exactly one SHA1
	- Refer to objects

---

# Objects 1

- ***blob*** - stream of bytes. In practice - the content of  files in the git workspace.
- ***tree*** - directory. Text file - each line represents an entry in the directory. Each entry could be ***blob*** or another ***tree***
- TODO: show changes propagating the tree

---

# Objects 2

- ***commit*** - Text file. Headers + content ( like HTTP )
	- tree - the root of the worktree for this commit
	- parent - the parrent commit ( like blockchain )
	- author, committer
- ***tag*** - almost like ***commit***
	- dont have parent
	- can point to any type of object

---

# Refs

- ***branch*** - Text file containing the SHA1 of a branch
- ***tag*** - Not much different from ***branch*** refs
- ***special*** - Couple of service refs like  `.git/HEAD`
	- can be ***direct*** or ***symbolic***
	- HEAD is usually symbolic - points to another ref's name, not to SHA1. When HEAD points directly to SHA1 we are in "detached HEAD" state.

---

# Git and large binary files
As every change is stored as a separate file, binary files will quickly take up quite a bit of space in either object or pack form.
