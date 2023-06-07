## Branching
I wonder if this is really needed if the local branch is automatically called main.  
```
git branch -M main
git remote add
```

This creates a remote (also called origin) "main" from my local main branch   
```
git push -u origin main
```

I can create and load a local branch where I can make changes  
```
git checkout -b feature
```

Check which branch I am on  
```
git branch
```

This pushes my local feature branch into remote (origin) with the same branch name, feature.  
```
git push origin feature
```

I can change branches, which changes the committed files.  
```
git checkout main
```

## Merging
To avoid git push -f origin main to sync remote to local, I have to use git merge. `push -f` is not recommended when: 
- repo is public, so other people might have pulled, which would break their connection with origin
- commits might be deleted

`push origin -f` is required when previously commited files are edited. Adding a fresh new file has no effect because there is no conflict? 
Git doesn't like it when local is different to remote. Which one is the "new" one? 
Following best practice, I would have to create a new branch before every new addition/modification is made. Could be unrealistic... 
What happens to files outside of terminal when I change branches? How would file explorer know which branch I am on? 
The file explorer DOES know which branch I am on. Git is actually adding/removing files depending on which branch I am on. 
But this could still get confusing. I would have to constantly check if I'm on the right branch when I look for files. 

Step 1: create a new branch  
```
git checkout -b dev
```

Step 2: make changes and commit  
```
git add . 
git commit -m "Development"
```
Step 3: changes are saved to dev branch. Now go back to main before merging  
```
git checkout main
```

Step 4: merge  
```
git merge dev
```

Step 5: now the dev branch is redundant  
```
git branch -d dev
```

Step 5: push  
```
git push origin main
```

## When the local branch is not syncing with remote branch
Go to local branch in question.  
```
git checkout feature
``` 

This does the trick. But be careful.    
```
git reset --hard origin/feature
```
If local main is ahead of remote main, and I don't want to lose 
any progress in local by `reset`, I can branch dev from local main, switch back to main, reset, pull, then merge/rebase dev.  
```
git status
git checkout -b dev
git checkout main
git reset --hard main
git pull origin main
git checkout dev
```
Merging:  
```
git checkout main
git merge dev
```
Rebasing:
```
git checkout dev
git rebase main
git checkout main
git rebase dev
```

## Removing a branch both locally and remotely
```
git branch -d feature
git push origin --delete feature
```

## Rebase
Trying to simulate the mechanics of `git rebase`. The following script was run in a separte directory called gitTestCollaborator:  
```
git init
git remote add origin https://github.com/kimsjune/gitTest
git pull origin main
git checkout -b collab
nano unique.txt
git add . 
git commit -m "First collab branch commit"
git checkout main
```
First make sure that the main is up-to-date.  
```
git pull origin main 
```
Then go back to collab branch, and rebase to main. This moves the branch forward to the most recent origin/main. Immediately preceding `git pull` and the following two lines make sure that I'm rebasing from the most recent remote commit. 
```
git checkout collab
git rebase main
```
Now go back again to main to add collab. Push to origin/main.  
```
git checkout main
git rebase collab
git push origin main
```

