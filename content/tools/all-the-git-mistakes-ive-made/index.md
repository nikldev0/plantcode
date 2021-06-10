---
title: "All the Git mistakes I've made"
date: 2021-05-20T13:04:00+01:00
draft: false
banner: "https://hugo-og-73w37x2fd-nikldev0.vercel.app/**All%20the%20Git%20mistakes%20I've%20made**%20%F0%9F%8C%BF%20plantcode%20.png?theme=light&md=1&fontSize=100px&images=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Ffront%2Fassets%2Fdesign%2Fhyper-color-logo.svg&widths=auto"
subtitle: And how I'm trying to avoid them.
---

Getting to grips with the feature branching workflow.

1. Not branching from main.
2. `git checkout` to undo all changes in the working directory.
3. `git commit --amend -m` will edit your last commit message but it will change your git history. In other words the hash of your commit will change. As long as we are the only ones who have had access to the changes that we've made this is fine. If this isn't the case however, we do not want to change the git history as this can cause problems with other people's git histories. 
4. Let's say you accidentally left off a file that you meant to commit. `git amend` is what you need. It will prompt you to change the git commit message but you don't need to do that, it will just capture other changes that were to be committed. But the changed files do have to be in staging.
5. `git log --stat` will show you the files that were changed in the commit.
6. Intention without strategy is chaos.
7. VSC is no replacement for communication.
8. `git remote show origin` 
9. Only rebase commits that you have not shared with others, otherwise bad things will happen. You don't want to rebase something you already pushed for others to work with.
10. [Useful resource](https://stackoverflow.com/questions/10588291/git-branching-master-vs-origin-master-vs-remotes-origin-master?noredirect=1&lq=1) to understand all the remote branches `git branch -a` gives you.
11. The goal of both merging and rebasing is to take commits from a feature branch and put them on a master branch. 
   