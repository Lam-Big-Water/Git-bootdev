- lanes branch
- primes branch

1. Git

Git is the distributed version control system (VCS).

2. Quick Config

Git comes with a configuration both at the global and the repo (project) level. Most of the time, you'll just use the global config.

    1. check your name and email.
    ```git
    git config --get user.name
    git config --get user.email
    ```

    2. create your name and email.
    ```git
    git config --add --global user.name "github_username_here"
    git config --add --global user.email "email@example.com"
    ```
    3. set a default branch
    ```git
    git config --global init.defaultBranch master
    ```

    4. check in .gitconfig file
    ```git
    cat ~/.gitconfig
    ```

3. Config

The very first step of any project is to create a repository. A Git "repository" (or "repo") represents a single project. You'll typically have one repository for each project you work on.

A repo is essentially just a directory that contains a project (other directories and files). The only difference is that it also contains a hidden .git directory. That hidden directory is where Git stores all of its internal tracking and versioning information for the project.

4. Status

A file can be in one of several states in a Git repository. Here are a few important ones:

untracked: Not being tracked by Git
staged: Marked for inclusion in the next commit
committed: Saved to the repository's history
The git status command shows you the current state of your repo. It will tell you which files are untracked, staged, and committed.

```git
git status
```

5. Staging

The contents.md file has been created, but as we saw, it's untracked. We need to stage it (add it to the "index") with the git add command before committing it later.

Without staging, no files are included in the commit—only the files you explicitly git add will be committed.

```git
git add <path-to-file | pattern>
```


6. Commit

After staging a file, we can commit it.

A commit is a snapshot of the repository at a given point in time. It's a way to save the state of the repository, and it's how Git keeps track of changes to the project. A commit comes with a message that describes the changes made in the commit.

Here's how to commit all of your staged files:

```git
git commit -m "your message here"
```
 1. If you screw up a commit message, you can change it with the --amend flag. For example:
 
 ```git
 # Change the last commit message
    git commit --amend -m "A: add contents.md"
 ```

7. Git Log

A Git repo is a (potentially very long) list of commits, where each commit represents the full state of the repository at a given point in time.

The git log command shows a history of the commits in a repository. This is what makes Git a version control system. You can see:

Who made a commit
When the commit was made
What was changed

 1. A Commit Hash

Each commit has a unique identifier called a "commit hash". This is a long string of characters that uniquely identifies the commit. Here's an example of mine:

```git
d39ee16ea0a48e1ad0e412133b1cbf482e515035
```

For convenience, you can refer to any commit or change within Git by using the first 7 characters of its hash. For mine, that's d39ee16.

8. Different Hashes

You may have noticed that even though we (you and I) both have the same content in our repositories, we have different commit hashes.

9. The Plumbing

It's Just Files All the Way Down

All the data in a Git repository is stored directly in the (hidden) .git directory. That includes all the commits, branches, tags, and other objects we'll learn about later.

Git is made up of objects that are stored in the .git/objects directory. A commit is just a type of object.

10. Storing Data

Git stores an entire snapshot of files on a per-commit level. This was a surprise to me! I always assumed each commit only stored the changes made in that commit.

Optimization
While it's true that Git stores entire snapshots, it does have some performance optimizations so that your .git directory doesn't get too unbearably large.

Git compresses and packs files to store them more efficiently.
Git deduplicates files that are the same across different commits. If a file doesn't change between commits, Git will only store it once.
A

11. Git Config

Git stores author information so that when you're making a commit it can track who made the change. 

Let’s dissect the command:  

- **`git config`**: The command to interact with your Git configuration.  
- **`--add`**: Flag indicating you want to add a configuration.  
- **`--global`**: Flag specifying that this configuration should be stored globally in your `~/.gitconfig`. The opposite is `--local`, which stores the configuration in the current repository only.  
- **`user`**: The section of the configuration.  
- **`name`**: The key within the section.  
- **`"ThePrimeagen"`**: The value you want to set for the key. 

Git has a command to view the contents of your config:

`git config --list --local`

12. Get

We've used --list to see all the configuration values, but the --get flag is useful for getting a single value.

`git config --get <key>`

13. Unset

The --unset flag is used to remove a configuration value. For example:

`git config --unset <key>`

14. Duplicates

 Strangely enough, Git doesn't care.

 The --unset-all flag is useful if you ever really want to purge all instances of a key from your configuration. Conversely, the --unset flag only works with a single instance of a key.

`git config --unset-all example.key`

15. Remove a Section

As I pointed out before, the webflyx section is nonsensical because Git doesn't use it for anything. While we can store any key/value pairs we want in our Git configuration, that doesn't mean we should.

The --remove-section flag is used to remove an entire section from your Git configuration. For example:

`git config --remove-section section`

16. Locations

   1. There are several locations where Git can be configured. From more general to more specific, they are:

- system: /etc/gitconfig, a file that configures Git for all users on the system
- global: ~/.gitconfig, a file that configures Git for all projects of a user
- local: .git/config, a file that configures Git for a specific project
- worktree: .git/config.worktree, a file that configures Git for part of a project

    2. Overriding

    If you set a configuration in a more specific location, it will override the same configuration in a more general location. For example, if you set user.name in the local configuration, it will override the user.name set in the global configuration.

    ![graph](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/e4S7M9u-1055x600.png)

17. What Is a Branch?

A Git branch allows you to keep track of different changes separately.

For example, let's say you have a big web project and you want to experiment with changing the color scheme. Instead of changing the entire project directly (as of right now, our master branch), you can create a new branch called color_scheme and work on that branch. When you're done, if you like the changes, you can merge the color_scheme branch back into the master branch to keep the changes. If you don't like the changes, you can simply delete the color_scheme branch and go back to the master branch.

   1. Under the Hood
    A branch is just a named pointer to a specific commit. When you create a branch, you are creating a new pointer to a specific commit. The commit that the branch points to is called the tip of the branch.

    ![graph](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/iH1kl8l-539x588.png)

    Because a branch is just a pointer to a commit, they're lightweight and "cheap" resource-wise to create. When you create 10 branches, you're not creating 10 copies of your project on your hard drive.

18. Default Branch

We've been using Git's default master branch. Interestingly, GitHub (a website where you can remotely store Git projects) recently changed its default branch from master to main. As a general rule, I recommend using main as your default branch if you work primarily with GitHub, as we will.

How to Rename a Branch

`git branch -m oldname newname`

19. New Branch

You should already be on the main branch: your "default" branch. You can always check with git branch.

Two Ways to Create a Branch

`git branch my_new_branch`

This creates a new branch called my_new_branch. The thing is, I rarely use this command because usually I want to create a branch and switch to it immediately. So I use this command instead:

`git switch -c my_new_branch`

The switch command allows you to switch branches, and the -c flag tells Git to create a new branch if it doesn't already exist.

When you create a new branch, it uses the current commit you are on as the branch base. For example, if you're on your main branch with 3 commits, A, B, and C, and then you run git switch -c my_new_branch, your new branch will look like this:

![graph](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/oah2FRD-1228x682.png)

20. Log Flags

As you know, git log shows you the history of commits in your repo. There are a few flags I like to use from time to time to make the output easier to read.

The first is --decorate. It can be one of:

- short (the default)
- full (shows the full ref name)
- no (no decoration)

This flag will show you a more compact view of the log. I use this one all the time, it just makes it so much easier to see what's going on.

`git log --oneline`

21. Merge

"What's the point of having multiple branches?" you might ask. They're most often used to safely make changes without affecting your (or your team's) primary branch. However, once you're happy with your changes, you'll want to merge them back into the main branch so that they make their way into the final product.

22. Merge Commits

`git switch main`

`git merge vimchadsonly`

The merge will:

- Find the "merge base" commit, or "best common ancestor" of the two branches. In this case, A.

- Replay the changes from main, starting from the best common ancestor, into a new commit.

- Replay the changes from vimchadsonly onto main, starting from the best common ancestor.

- Records the result as a new commit, in our case, F.

- F is special because it has two parents, C and E.