from pathlib import Path

content = r"""# Git and GitHub Practical: Pythagoras Calculator

## Aim

In this practical, you will use Git to track changes to a small Python program and GitHub to store your repository online.

You will learn how to:

- Create a local Git repository.
- Stage and commit changes.
- Connect your local repository to a GitHub repository.
- Push commits to GitHub.
- Create a branch, make a change and merge it back into `main`.

The key idea is that **Git and GitHub are related, but they are not the same thing**:

- **Git** tracks versions of your files and their history.
- **GitHub** hosts Git repositories online so you can access and share them.

A **commit is saved locally**. It does not appear on GitHub until you **push** it.

---

## 1. Before you start: install the software

You need Python 3 and Git. Visual Studio Code (VS Code) is recommended as an editor, but it is not essential.

### Install Python 3

Download Python from the official website:

- https://www.python.org/downloads/

Choose the version for your operating system and follow the installation instructions.

**Windows note:** during installation, tick **Add Python to PATH** if that option is shown.

Open a new terminal and check that Python is installed:

```bash
python --version
```

If that command is not recognised, try:

```bash
python3 --version
```

You should see a Python 3 version number.

### Install Git

Download Git from the official website:

- https://git-scm.com/downloads

Choose your operating system and follow the installation instructions. The default installation options are usually suitable.

Open a new terminal and check that Git is installed:

```bash
git --version
```

You should see a Git version number.

### Install Visual Studio Code (recommended)

Download VS Code:

- https://code.visualstudio.com/

After installing it:

1. Open VS Code.
2. Open the folder where you will keep your practical work (**File → Open Folder**).
3. Open the Extensions view and install the **Python** extension published by Microsoft.
4. Open a Python file. If prompted, select the Python 3 interpreter installed on your computer.
5. Open the integrated terminal using **Terminal → New Terminal**.

You can run your program in the VS Code terminal. For example:

```bash
python pythagoras.py
```

On some systems, use:

```bash
python3 pythagoras.py
```

Use the terminal for programs that ask for keyboard input with `input()`. The VS Code **Output** panel is not an interactive terminal.

### You do not need extra Python packages

The Pythagoras example uses Python's built-in `math` module. It is included with Python, so you do not need to install it separately.

### Alternative: GitHub Codespaces

If you are using GitHub Codespaces, your development environment runs in the cloud. You normally do not need to install Python, Git or VS Code on your own computer to complete the practical.

Open your GitHub repository, choose **Code → Codespaces**, and create a Codespace. In its terminal, check the tools with:

```bash
python --version
git --version
```

---

## 2. Create your Python project

Create a folder called `pythagoras` for your practical work. Open that folder in VS Code.

Create a file named `pythagoras.py` and add this initial program:

```python
import math

side_a = float(input("Enter the first side: "))
side_b = float(input("Enter the second side: "))

hypotenuse = math.sqrt(side_a ** 2 + side_b ** 2)

print("Hypotenuse:", hypotenuse)
```

Save the file and run it in the terminal:

```bash
python pythagoras.py
```

If necessary, use `python3` instead of `python`.

---

## 3. Create a local Git repository

In the terminal, make sure you are inside your `pythagoras` project folder. Then enter:

```bash
git init
```

This starts a Git repository in the current folder. Git creates a hidden `.git` directory where it stores repository information and history.

Check the repository status:

```bash
git status
```

This shows which files Git is tracking, which files are new, and which files have changes. Your `pythagoras.py` file should appear as untracked.

### Configure your Git identity (first-time setup)

If Git asks you to identify yourself before making a commit, configure a name and email:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Use your own name and an email address you want associated with your commits. These settings identify the author of commits; they do **not** sign you in to GitHub or give you permission to access a repository.

---

## 4. Stage and commit your first version

### Stage the file

```bash
git add pythagoras.py
```

This puts the file into the **staging area**. Staging means selecting the changes you want included in your next commit. It does not create a commit and does not upload anything to GitHub.

You can stage all changed files in the current folder with:

```bash
git add .
```

Check what is staged:

```bash
git status
```

### Commit the staged changes

```bash
git commit -m "Initial Pythagoras program"
```

This saves a snapshot of the staged changes in your **local** Git repository.

The `-m` option means **message**. It lets you provide the commit message directly in the command. The message should briefly describe what the commit contains.

For example:

```bash
git commit -m "Improve code readability"
```

A commit is like a checkpoint in your project's history. You can inspect previous commits and compare versions later.

**Important:** after `git commit`, the commit is saved locally. It is not automatically visible on GitHub. You will upload it later using `git push`.

View the commit history:

```bash
git log --oneline
```

This shows a compact list of commits.

### What if `git diff` takes over the terminal?

The command:

```bash
git diff
```

shows changes that have not yet been staged. If the output is long, Git may display it in a pager (usually `less`). The terminal can look as though it is no longer accepting commands.

**Press `q` to quit the pager** and return to your normal terminal prompt.

Other useful pager controls:

- `Space`: move down one page.
- `b`: move up one page.
- Arrow keys: move line by line.
- `q`: quit.

This can also happen with commands such as `git log`.

---

## 5. Create the GitHub repository online

**Yes: create the repository on GitHub before you connect your local repository and push to it.**

1. Sign in to https://github.com/.
2. Select **New repository** (usually available from the `+` menu or your repositories page).
3. Enter a repository name, for example `pythagoras`.
4. Choose **Private** or **Public**, depending on your needs.
5. For this practical, if you already have a local project and have committed it, the simplest option is to create an **empty repository**: do not add a README, licence or `.gitignore` during creation.
6. Select **Create repository**.
7. GitHub will show you the repository URL. Copy the HTTPS URL. It will look similar to:

```text
https://github.com/YOUR-USERNAME/pythagoras.git
```

Use the URL for the repository you have just created. Do not copy the example URL literally.

### Why create it online first?

Your local Git repository and the GitHub repository are two separate copies. Creating the GitHub repository gives your local repository an online destination to connect to. The `git remote add` command records that destination; `git push` uploads your commits.

---

## 6. Connect your local repository to GitHub

In the terminal, still inside your local `pythagoras` project folder, enter this command, replacing the example URL with the HTTPS URL you copied from your own GitHub repository:

```bash
git remote add origin https://github.com/YOUR-USERNAME/pythagoras.git
```

This adds a **remote**: a reference to another copy of the repository hosted elsewhere.

- `remote` tells Git you are working with a remote repository.
- `add` adds a new remote connection.
- `origin` is the conventional short name for this remote. It is just a nickname; it usually refers to your GitHub repository.
- The URL identifies the online repository.

This command connects the repositories, but it does **not** upload your files.

You can check the remote address with:

```bash
git remote -v
```

### If Git says `remote origin already exists`

That means a remote named `origin` has already been configured. Check its address with:

```bash
git remote -v
```

If it is wrong, update it with your correct GitHub URL:

```bash
git remote set-url origin https://github.com/YOUR-USERNAME/pythagoras.git
```

You do not need to add `origin` again.

---

## 7. Rename the branch and push to GitHub

Enter the following commands **one at a time**, pressing Enter after each command. Wait for each command to finish before entering the next one.

### Command 1: name the branch `main`

```bash
git branch -M main
```

This renames your current branch to `main`.

- `branch` is the Git command for working with branches.
- `-M` means rename the branch to the specified name, forcing the rename if necessary.
- The `M` is uppercase. It is different from lowercase `-m`, which is used for a commit message in `git commit`.

A branch is a line of development. `main` is commonly used as the name of the primary branch.

### Command 2: upload your commits

```bash
git push -u origin main
```

This uploads your local `main` branch and its commits to the GitHub repository.

- `push` uploads local commits to a remote repository.
- `-u` means set the upstream tracking relationship. It tells Git that your local `main` branch should track `origin/main`, so future pushes and pulls can usually be run with just `git push` or `git pull`.
- `origin` is the remote nickname you set earlier.
- `main` is the branch you are uploading.

You may be asked to authenticate with GitHub. Follow the sign-in prompt in your browser or the authentication method configured for your computer.

**Now open your GitHub repository in a web browser and refresh the page.** You should see `pythagoras.py` and your commit history.

### Do I enter all three GitHub commands at once?

These are the three commands:

```bash
git remote add origin https://github.com/YOUR-USERNAME/pythagoras.git
git branch -M main
git push -u origin main
```

Enter them **line by line**, not as one combined command:

1. First, add the remote.
2. Next, rename the branch.
3. Finally, push the branch to GitHub.

Each command has a different job, and the later commands rely on the earlier setup being correct.

---

## 8. Make another change and push it

Now improve the readability of your program. Use meaningful variable names and add comments.

For example:

```python
import math

# Ask the user for the lengths of the two shorter sides
side_a = float(input("Enter the first side: "))
side_b = float(input("Enter the second side: "))

# Calculate the hypotenuse using the Pythagorean theorem
hypotenuse = math.sqrt(side_a ** 2 + side_b ** 2)

# Display the result
print("Hypotenuse:", hypotenuse)
```

Save the file. Then enter these commands one at a time:

```bash
git status
git diff
git add pythagoras.py
git commit -m "Improve code readability"
git push
```

What happens?

1. `git status` shows that the file has changed.
2. `git diff` displays the differences between the working file and the last committed version. Press `q` to leave the pager if it opens.
3. `git add` stages the updated file.
4. `git commit` saves a new local snapshot with a descriptive message.
5. `git push` uploads the new commit to GitHub.

Refresh the GitHub page. You should now see the updated file and the new commit in the history.

---

## 9. Add input validation

Improve the program so that it handles invalid input. For example, side lengths should be positive numbers.

```python
import math

try:
    side_a = float(input("Enter the first side: "))
    side_b = float(input("Enter the second side: "))

    if side_a <= 0 or side_b <= 0:
        raise ValueError("Side lengths must be positive")

    hypotenuse = math.sqrt(side_a ** 2 + side_b ** 2)

    print("Hypotenuse:", hypotenuse)

except ValueError as error:
    print("Invalid input:", error)
```

Save and test the program. Then stage, commit and push the change:

```bash
git add pythagoras.py
git commit -m "Add input validation"
git push
```

Check GitHub to confirm that the latest version has been uploaded.

---

## 10. Challenge: create a branch and merge a feature

Branches allow you to work on a new feature without making changes directly to the main line of development.

Create a new branch:

```bash
git switch -c add-triangle-area
```

- `switch` changes the branch you are working on.
- `-c` means create a new branch.
- `add-triangle-area` is the new branch name.

Now add code to calculate the area of the right-angled triangle:

```python
area = (side_a * side_b) / 2
print("Triangle area:", area)
```

Save and test your changes. Then stage and commit them:

```bash
git add pythagoras.py
git commit -m "Add triangle area calculation"
```

Switch back to the main branch:

```bash
git switch main
```

Merge the feature branch into `main`:

```bash
git merge add-triangle-area
```

This brings the changes from `add-triangle-area` into `main`.

Finally, push the updated main branch to GitHub:

```bash
git push
```

You can check which branch you are on with:

```bash
git branch
```

The current branch is marked with an asterisk (`*`).

---

## 11. Common problems and how to fix them

### `remote: Repository not found` / `fatal: repository ... not found`

For example:

```text
remote: Repository not found.
fatal: repository 'https://github.com/YOUR-USERNAME/pyth-test.git/' not found
```

This usually means GitHub cannot find the repository at the URL, or your account does not have permission to access it.

Check the following:

1. **Does the repository exist online?** Open GitHub in your browser and check that you have created the repository. Creating a local repository with `git init` does not create a GitHub repository.
2. **Is the URL correct?** On the GitHub repository page, select **Code**, copy the HTTPS URL, and compare it with the remote URL configured in Git.
3. **Does the repository name match exactly?** Check spelling, hyphens and your GitHub username. For example, `pyth-test` and `pythagoras` are different repository names.
4. **Do you have permission?** If the repository is private, make sure you are signed in to the GitHub account that owns it or has been invited to collaborate.
5. **Is `origin` pointing to the right place?** Check it with:

```bash
git remote -v
```

If the URL is incorrect, replace it with the correct URL copied from GitHub:

```bash
git remote set-url origin https://github.com/YOUR-USERNAME/REPOSITORY-NAME.git
```

Then try pushing again:

```bash
git push -u origin main
```

A trailing slash in a displayed error message is not usually the main issue. Check the repository exists, the full URL is correct, and you have access.

### `remote origin already exists`

A remote called `origin` is already configured. Check it:

```bash
git remote -v
```

If necessary, update it using `git remote set-url origin YOUR-REPOSITORY-URL`.

### `git` or `python` is not recognised

- Check that the software is installed.
- Close and reopen the terminal after installation.
- On some systems, use `python3` instead of `python`.
- If you are using a university-managed computer, installation or PATH changes may require help from IT support.

### GitHub asks you to sign in

This is normal when pushing to GitHub for the first time. Complete the authentication prompt. Your Git commit name and email configuration does not itself authenticate you.

### The terminal seems stuck after `git diff` or `git log`

Press **`q`** to exit the pager and return to the command prompt.

---

## 12. Useful Git commands

| Command | Purpose |
|---|---|
| `git init` | Create a local repository |
| `git status` | Show the state of files and staging |
| `git diff` | Show unstaged changes |
| `git add filename` | Stage a file |
| `git add .` | Stage all changes in the current folder |
| `git commit -m "message"` | Save staged changes as a commit |
| `git log --oneline` | Show a compact commit history |
| `git remote -v` | Show configured remote addresses |
| `git push` | Upload commits to GitHub |
| `git pull` | Fetch and integrate remote changes |
| `git branch` | List local branches |
| `git switch -c branch-name` | Create and switch to a branch |
| `git switch main` | Switch to the main branch |
| `git merge branch-name` | Merge another branch into the current branch |

---

## 13. Reflection: how does Git support sustainable software?

Discuss these questions:

1. How does a commit history help someone understand how software has changed?
2. Why is it useful to have clear commit messages?
3. How can branches make it safer to experiment with new features?
4. How does hosting code on GitHub make collaboration easier?
5. How could version control help someone maintain or reproduce research software several years after it was written?

**Remember the basic workflow:**

```text
Edit → Stage (add) → Commit → Push
```

- **Stage** selects the changes for the next snapshot.
- **Commit** saves that snapshot in your local Git history.
- **Push** uploads your commits to GitHub.
"""

path = Path("/mnt/data/git-pythagoras-practical-final.md")
path.write_text(content, encoding="utf-8")
print(f"Created: {path}")
print(f"Size: {path.stat().st_size:,} bytes")
