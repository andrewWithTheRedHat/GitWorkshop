# Basic Git

## Authentication

Most repositories are disabling username and password as an auth mechanism for security reason so you will either need to set up a personal access token if using https authentication, or set up an ssh key and add it to your git server. Below are links to the most common git servers and how to do this:

### SSH

1. Generate an 4096 bit rsa key with a comment to make sure you know what it is for later.

    ```shell
    ssh-keygen -t rsa -b 4096 -C "sshkeyforgit"
    cat ~/.ssh/id_rsa.pub
    ```

    `NOTE:` Never share your private key. It is the one without the `.pub` extension.

2. Add the key to the git server:

[Bitbucket](https://confluence.atlassian.com/bitbucketserver/ssh-user-keys-for-personal-use-776639793.html) `NOTE` You can skip xclip and just cat ~/.ssh/id_rsa.pub

### Personal Access Tokens

[Bitbucket](https://confluence.atlassian.com/bitbucketserver/personal-access-tokens-939515499.html)

---

## Forking

Most public repositories don't let everyone push to them so you will need to fork the repository to your own namespace in your git server before you can clone it.

Click [here](https://support.atlassian.com/bitbucket-cloud/docs/fork-a-repository/) to follow forking instructions from Bitbucket.

## Importing from other Repositories

Sometimes you will want to fork from Github to your own repository so you can have a local copy to work from this is called importing. ***NOTE: If you are attending a lab provided by Red Hat please follow the Gitlab instructions to import from URL.***

Here are links on how to import for the most common repositories:

[Bitbucket](https://support.atlassian.com/bitbucket-cloud/docs/import-a-repository-from-github-or-gitlab/)

## Cloning a repository

Cloning is making a local copy of code in a local copy of the remote repository. Most of the time you will be cloning someone elses repository so you can use it or make a change and then push in for a request to have it merged into the upstream codebase. Remember unless you have access to the upstream repository you will need to either request access or fork the code to push into it.

**For this exercise** fork this repository using the bitbucket [instructions](https://support.atlassian.com/bitbucket-cloud/docs/fork-a-repository/) then clone it (depending on if you created a PAT (public access token) or a SSH key follow the correct instruction below:

`Note:` You can import into [Gitlab](https://docs.gitlab.com/ee/user/project/import/github.html), [ADO](https://docs.microsoft.com/en-us/azure/devops/repos/git/import-git-repository?view=azure-devops), [Bitbucket](https://support.atlassian.com/bitbucket-cloud/docs/import-a-repository-from-github-or-gitlab/) or [Git](https://docs.github.com/en/github/getting-started-with-github/fork-a-repo) server of choice using their import feature if you do not have access to github (click link or tool for details).

### SSH Clone

```shell
git clone git@github.com:yourgithubname/GitWorkshop.git
```

### HTTP Clone

Github Example:

```shell
git clone https://<pat>@gitserver/yourgithubusername/GitWorkshop.git
```

Gitlab Example:

```shell
git clone https://username:<pat>@gitserver/yourgithubusername/GitWorkshop.git
```

`NOTE:` Replace `<pat>` with the token you created in the earlier exercise.

Output:

```shell
Cloning into 'GitWorkshop'...
remote: Enumerating objects: 283, done.
remote: Counting objects: 100% (283/283), done.
remote: Compressing objects: 100% (173/173), done.
remote: Total 283 (delta 77), reused 276 (delta 74), pack-reused 0
Receiving objects: 100% (283/283), 81.03 KiB | 1.72 MiB/s, done.
Resolving deltas: 100% (77/77), done.
cd GitWorkshop
tree
.
├── LICENSE
├── README.md
└── workshops
    └── how_we_enable
        ├── 01-Getting_Started.md
        ├── 02-Logistics.md
        ├── 03-Strategy.md
        └── 04-Practice_Scenarios.md
```

## Branching

It is a best practice to not commit code directly to the main (legacy master) branch. In order to void this we create a feature branch to work from with the `git branch` or `git checkout` command. First please go to this interactive training for branching: [https://learngitbranching.js.org/](https://learngitbranching.js.org/)

To create a branch and check it out (ie move to it as your working branch) you can do this with a single command `git checkout -b <branch-name>`.

```shell
git checkout -b test
```

Output:

```shell
Switched to a new branch 'test'
```

You should see that you are now in the test branch by executing the command `git status`

```shell
git status
```

Output:

```shell
On branch test
nothing to commit, working tree clean
```

Now any changes you make are in a separate branch from main called test.

## Commiting Code

1. In the GitWorkshop directory create a new file and look at the status of it in git.

    ```shell
    touch mynewfile.txt
    git status
    ```

    Output of git status:

    ```shell
    Untracked files:
    (use "git add <file>..." to include in what will be committed)
        mynewfile.txt
    ```

2. Now let's add the file so it is tracked by git going forward `git add <filename>`, and commit it so that the new file is ready to be pushed to the remote repository `git commit -m 'meaningful commit message that defines what changed, not how it changed the logs will show the how'`.

    ```shell
    git add mynewfile.txt
    git status
    ```

    Output of git status:

    ```shell
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
            new file:   mynewfile.txt
    ```

3. Finally lets commit the code:

    ```shell
    git commit -m 'adding mynewfile.txt'
    ```

    Output:

    ```shell
    [main 60cdc2b] adding mynewfile.txt
     1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 mynewfile.txt
    ```

## Pushing Commit to Repository Server

Just like it sounds, `git push` pushes the commits to the central repository. A best practice is to specify the remote you are pushing to (usually origin) and the branch for this example we will use test. `Note:` Generally upstream repositories will not allow pushing directly to main, so you will always need to either fork the repository, or create a branch.

```shell
git push origin test
```

`Note:` 'Origin' in the command above denotes the location being pushed to.  Each repository will have an origin and a remote location (as stated before).

Output:

```shell
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Delta compression using up to 8 threads
Compressing objects: 100% (1/1), done.
Writing objects: 100% (1/1), 1018 bytes | 339.00 KiB/s, done.
Total 9 (delta 5), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local objects.
To github.com:yourusername/GitWorkshop.git
   c0db901..6eaaf6d  test -> test
```

## Merging Code

If you are the owner and want to merge a feature branch into `main` you can do so via `git merge <branchname>` from the main branch, however this is not a best practice. Best practice is to perform a pull request (sometimes called merge request) from the central server and have someone review the changes before they are merged into main. Below are instructions to perform a pull request from Bitbucket:

[Bitbucket](https://www.atlassian.com/git/tutorials/making-a-pull-request)

Please create a pull request following above documentation to your repository you created above from the `test` branch into the `main` branch of your fork.

`Note:` in the next section we will walk through how to do this vi Github.

---

[Next Advanced Git](02-advanced.md)
