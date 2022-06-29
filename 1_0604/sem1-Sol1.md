## Solutions

### Step 1 - Initialize the Repo
Create a new sample project folder. Run `git status` to see that it is not yet a git repository. Use `git init` to initialize it as a repository.

```
$> mkdir -p ~/devs/seminars/sample

$> cd ~/devs/seminars/sample

$> git status
fatal: Not a git repository (or any of the parent directories): .git

$> git init
```

### Step 2 - First Commit
Create a new document, stage it for a commit, then commit it to your repository.

```
$> echo 'Hello, World!' > hello.txt

$> git add hello.txt

$> git commit -m "Initial commit"
[master (root-commit) 444a5c7] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

### Step 3 - View the .git Folder
Using `tree`, look in your `.git/objects` folder, you should now see three objects, represented by long SHA1 hashes. These represent the tree, blob, and commit that we created in the last step.

```
$> tree .git

.git
├── branches
├── COMMIT_EDITMSG
├── config
├── description
├── FETCH_HEAD
├── HEAD
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           └── master
├── objects
│   ├── 44
│   │   └── 4a5c7f2e491fc643ce0ded5be40455019ae291
│   ├── 8a
│   │   └── b686eafeb1f44702738c8b0f24f2567c36da6d
│   ├── bc
│   │   └── 225ea23f53f06c0c5bd3ba2be85c2120d68417
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    └── tags

```

### Step 4 - Inspect the Objects:
Note: The SHA1 hash for your commit will be different than the one displayed here. The SHA1 hash for your `blob` and `tree` will be the same as mine, as long as the content is the same.

One of the objects should be a tree object. The tree contains the filename `hello.txt` and a pointer to the blob.

```
$> git cat-file -t 8ab6
tree

$> git cat-file -p 8ab6
100644 blob 8ab686eafeb1f44702738c8b0f24f2567c36da6d	hello.txt
```

The blob object, pointed to by the tree, contains the contents of the file `hello.txt`

```
$> git cat-file -t 980a0d5
blob

$> git cat-file -p 980a0d5
Hello, World!
```

The commit object contains a pointer to the tree, along with metadata for the commit, such as the author and commit message.

```
$> git cat-file -t 444a5
commit

$> git cat-file -p 444a5
tree bc225ea23f53f06c0c5bd3ba2be85c2120d68417
author 알바체리 미겔 <miguel@deepmachinelab.com> 1656030331 +0900
committer 알바체리 미겔 <miguel@deepmachinelab.com> 1656030331 +0900

Initial commit

```

Because this is our very first commit, it doesn't have a parent. The next commit we make will point to our initial commit as the parent. 

### Step 5 - Look at refs

Let's look under the hood at our `HEAD` variable. `HEAD` is just git's pointer to "where you are now," usually referring to the current branch. More on this later. We can see that right now, it points to our current branch - `master`

Now, if we look at our `master` reference, we can see that it points to the latest commit.

```
$> cat .git/HEAD
ref: refs/heads/master

$> cat .git/refs/heads/master
444a5c7f2e491fc643ce0ded5be40455019ae291
```
`444a5c...` is the hash of the commit we saw in the last step. You can confirm this by running `git log`

```
$> git log --oneline
444a5c Initial commit
```

Git stores references in the `.git/refs/heads/` directory, and the `HEAD` pointer in `.git/HEAD`

We can verify this by creating a new branch.

```
$> git branch new_branch
```

The `git branch` command will create a new branch without switching to it.

Now, if we look in `.git/refs`, we'll see two branches. The `master` branch, which is created by default, and `new_branch`.

```
$> tree .git/refs
.git/refs
├── heads
│   ├── master
│   └── new_branch
└── tags

```

#### End of Exercise One