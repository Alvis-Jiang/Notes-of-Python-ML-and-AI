### - repository
![1714265010613](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/2b3b587b-ff90-420a-a7a0-e35be1f6f5f2)


### - GIT workflow
-  Modify a file 
- Saving the draft
- Commit the updated file
- Repeat
```
# Editing a file
# Create a new file todo.txt
echo "Review for duplicate records" > todo.txt

# Add content to existing file todo.txt
echo "Review for duplicate records" >> todo.txt

# Modifying a file
# Save using Ctrl + o and Ctrl + X
nano report.md

# Saving a file 
# Adding a single file
git add report.md

# Adding all modified files to stagin area
git add .

# Making a commit
git commit -m "Updating TODO list in report.md"

# Check the staging status of files
git status
```

### - comparing files
- comparing a single file
```
# Compare an unstaged file with the last committed version
git diff report.md

# Compare a staged file with the last committed version
git diff -r HEAD report.md
```

![1714266291766](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/7ecd3e97-958f-4e09-b5e5-93a372d973d9)

- comparing multiple files
```
# Compare all staged files with the last committed versions
git diff -r HEAD
```

## Storing data with Git

![1714266968355](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/3852ab9c-1a11-4d30-ab09-17eaa57dd0e2)

![1714267029676](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/72112f7f-bbcf-4293-9c80-b0ebddc36f02)

```
# Git log
# find a particular commit 
git log

# Only need the first 6-8 characters of the hash
# Git hash
# Hashes allow data sharing between repos.If two files are the same then their hashes are the same
git show c27fa856
```

## - Viewing changes

```
# The HEAD shortcut
# Use a tilde ~ to pick a specific commit to compare versions
# HEAD~1 = the second most recent commit
# HEAD~2 = the commit before tha
## Must not use spaces before or after the tilde ~
git show HEAD~2

```

![1714267832537](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/4d7254ce-3ddf-49b1-9bc5-36404cca3955)


```
# Compare changes between two commits
git diff 35f4b4d 186398f

git diff HEAD~3 HEAD~2
```
![1714267912733](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/08849800-5f3c-4920-81b7-b014b93c5869)

```
# Show line-by-line changes and associated metadata
git annotate report.md
```
![1714267967066](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/c6b45866-f838-45d9-9cd8-0f226f5b2e82)


### - Undoing changes before committing
```
# Unstaging a single file in Git
git reset HEAD summary_statistics.csv
nano summary_statistics.csv
git add summary_statistics.csv
git commit -m "Adding age summary statistics"

# To unstage all files
git reset HEAD
```

```
# Undo changes to an unstaged file
# checkout means switching to a different version
# Defaults to the last commit
# This means losing all changes made to the unstaged file forever
git checkout -- summary_statistics.csv


# Undo changes to all unstaged files
# Undo changes to all unstaged files in the current directory and subdirectories
# This command must be run in the main directory
git checkout .
```

### - Restoring and reverting
```
# Customizing the log output
## We can restrict the number of commits displayed using -
git log -3

## Restrict git log by date
git log --since='Apr 2 2022'
## Commits between 2nd and 11th April
git log --since='Apr 2 2022' --until='Apr 11 2022'

```

```
# Restoring an old version of a file
## To revert to a version from a specific commit
git checkout dc9d8fac mental_health_survey.csv
## or using HEAD
git checkout HEAD~1 mental_health_survey.csv

```

```
# Cleaning a repository
## See what files are not being tracked
git clean -n
## Delete those files
git clean -f
```
