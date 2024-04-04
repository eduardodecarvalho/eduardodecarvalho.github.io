---
title: Strategies for Editing Commit Messages
date: 2024-04-04 00:00:00
categories: [engineering]
tags: [git, good practices]
---


Git is, by far, the most used tool in the versioning of code nowadays. Developers use git daily and is almost impossible to think in our lives without this marvel of technology created by Linus Torvalds.

But of course, people could work on software with the help of a versioning tool, services like Subversion or Microsoft SourceSafe were popular in the past and some people still use them, unfortunately for them.

In this article, I will not approach the basic commands of git because everybody knows how to use `git add`, `git commit`, `git push`, and so on…. We don’t need a tutorial for that; if you need some help on it, I recommend the official documentation of the git.

### **What is the main reason for using git?**

Have you ever wondered why we need versioning tools? The main purpose of a Git repository is to create a precise register and you can go back and see how your code was 1 minute ago, 1 week ago, or 1 year ago. Git does it by ensuring that the data introduced are **consistent**, you will never get corrupted data without being warned. To save all this data, Git does the possible to save space.

So, what you need to do is to keep your repository as clean as possible, and to start: use a good commit message, please.

Commits like “updating”, “refactoring” and so on don’t mean much when you look at the history of the commits. Try to be as specific as possible, one good practice we use here at Proximus is to add the ID of the card that creates the demand for the change.

But if I’m working in my demand and I do a git commit with the wrong message but is committed, let it go, if is committed we can't fix it anymore… NO!!! Always fix the mistake as soon as you see the error, and correcting a message on a commit is easy, don’t take more than 10 seconds.

Look that here: I wrote the wrong commit message `“fixig”` instead of `“fixing”`.


![Passing struct gif](/assets/2024-03-17-fixing-git-history/images/01.png)

You can easily correct this error with the option `—amend`

![Passing struct gif](/assets/2024-03-17-fixing-git-history/images/02.png)

Ok, but this is an easy example, but now  let’s suppose that you are working happily while singing a beautiful song and you finish your task and commit, but your boss forgot to tell you about a small detail, so you make another commit, however, is never done when you think is done, and you need to work on another change and you commit again, now you have a lot of commits with the same message:

![Passing struct gif](/assets/2024-03-17-fixing-git-history/images/03.png)

To solve this problem, we can use the following command: `git rebase -i HEAD~3`

First, let’s talk about what this command does:

- **`git rebase`**: This is the main command for rebasing in Git. Rebasing is moving or combining a sequence of commits to a new base commit. It's often used to rewrite commit history, rearrange commits, squash or split commits, and more.
- **`i`** or **`-interactive`**: This flag tells Git to perform an interactive rebase. Interactive rebasing allows you to selectively choose which commits to manipulate (e.g., reorder, squash, edit commit messages, etc.) through an interactive text editor.
- **`HEAD~3`**: This specifies the commit range for the rebase operation. **`HEAD~3`** refers to the commit three steps back from the current HEAD commit. In other words, it specifies the last three commits in the current branch.

So, when you run **`git rebase -i HEAD~3`**, Git will open an interactive text editor (usually your default text editor, in my case the LunarVIM) with a list of the last three commits in your current branch.

![Passing struct gif](/assets/2024-03-17-fixing-git-history/gif/04-Open_LVIM.gif)

From there, you can choose what actions you want to perform on those commits, such as reordering them, squashing them together, editing commit messages, or even dropping commits altogether, in our case we want to squash them.

![Passing struct gif](/assets/2024-03-17-fixing-git-history/images/05.png)

You don’t need to squash all commits, you can choose what you want to squash.

Once you save and exit, Git will open your editor again showing what is gonna be committed and we can change the commit message:

![Passing struct gif](/assets/2024-03-17-fixing-git-history/gif/06-Changing_commit_message.gif)

Now you can see the result:

![Passing struct gif](/assets/2024-03-17-fixing-git-history/images/07.png)

In addition, to amend and rebase, several other commands in Git are crucial for managing commit messages effectively. These commands often go overlooked but can significantly enhance your workflow and collaboration processes. I’m planning to write more about this subject, so if you think it’s useful let me know! Maybe I can write about other commands that help me a lot in my daily routine as a software developer.