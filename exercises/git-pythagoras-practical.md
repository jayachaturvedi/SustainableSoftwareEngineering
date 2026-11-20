# Git and GitHub Practical: Pythagoras Calculator

## Introduction

In this practical, you will use Git and GitHub to manage changes to a simple Python program that calculates the hypotenuse of a right-angled triangle.

You will learn how to:

- Create a local Git repository.
- Track changes to your code.
- Make commits.
- Upload your code to GitHub.
- Create and work with branches.
- Merge changes back into the main branch.
- Understand how version control supports sustainable research software.

---

# Part 1: Installing the software

Before starting, you need Python and Git. We also recommend using Visual Studio Code (VS Code) as your code editor.

## 1.1 Install Python

Download Python from:

https://www.python.org/downloads/

Follow the installation instructions for your operating system.

**Windows users:** During installation, make sure you select the option:

`Add Python to PATH`

To check that Python is installed, open a terminal and enter:

```bash
python --version
```

On some systems, you may need to use:

```bash
python3 --version
```

You should see the installed Python version.

## 1.2 Install Git

Download Git from:

https://git-scm.com/downloads

Follow the installation instructions for your operating system.

To check that Git is installed, open a terminal and enter:

```bash
git --version
```

You should see the installed Git version.

## 1.3 Install Visual Studio Code

Download VS Code from:

https://code.visualstudio.com/

Install it and open the application.

We also recommend installing the Microsoft Python extension:

1. Open VS Code.
2. Select Extensions from the sidebar.
3. Search for `Python`.
4. Install the extension published by Microsoft.

This provides Python syntax highlighting, code assistance and debugging support.

## 1.4 Configure Git

Before making your first commit, configure Git with your name and email address.

Enter the following commands, replacing the example details with your own:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

To check your configuration:

```bash
git config --global --list
```

You only need to configure this once on your computer.

## 1.5 Using GitHub Codespaces (alternative)

If you do not want to install Python, Git and VS Code locally, you can use GitHub Codespaces.

Codespaces provides a cloud-based development environment that runs in your browser.

1. Open your GitHub repository.
2. Select the green **Code** button.
3. Select the **Codespaces** tab.
4. Create a new Codespace.
5. Use the built-in terminal and editor to work with your code.

The practical instructions below are the same whether you use a local installation or Codespaces.

**Note:** This practical uses only Python's standard library. You do not need to install additional Python packages.

---

# Part 2: Create your Pythagoras project

## 2.1 Create a project folder

Create a folder called:

`pythagoras`

Open this folder in VS Code.

Open the integrated terminal:

- Select **Terminal** from the menu.
- Select **New Terminal**.

Make sure the terminal is operating inside your `pythagoras` folder.

## 2.2 Create the Python file

Create a new file called:

`pythagoras.py`

Add the following code:

```python
import math

side_a = float(input("Enter first side: "))
side_b = float(input("Enter second side: "))

hypotenuse = math.sqrt(side_a ** 2 + side_b ** 2)

print("Hypotenuse:", hypotenuse)
```

Save the file.

## 2.3 Run the program

In the terminal, enter:

```bash
python pythagoras.py
```

If your system uses `python3`, enter:

```bash
python3 pythagoras.py
```

Enter two side lengths when prompted.

For example:

```text
Enter first side: 3
Enter second side: 4
Hypotenuse: 5.0
```

You now have a working Python program.

---

# Part 3: Initialise a Git repository

Git allows you to track changes to your code over time.

## 3.1 Initialise Git

In the terminal, enter:

```bash
git init
```

This creates a new Git repository in your project folder.

## 3.2 Check the repository status

Enter:

```bash
git status
```

Git should show that `pythagoras.py` is an untracked file.

This means Git can see the file, but it is not yet tracking it.

## 3.3 Stage the file

To tell Git which file you want to include in your next commit, enter:

```bash
git add pythagoras.py
```

Check the status again:

```bash
git status
```

The file should now appear as staged.

## 3.4 Create your first commit

A commit records a snapshot of your project at a particular point in time.

Enter:

```bash
git commit -m "Initial Pythagoras program"
```

You have now created your first commit.

The message describes what was recorded.

## 3.5 View your commit history

Enter:

```bash
git log --oneline
```

You should see your first commit listed.

The output will look something like:

```text
a1b2c3d Initial Pythagoras program
```

The letters and numbers are a shortened version of the commit's unique identifier.

---

# Part 4: Make changes and create more commits

One of the main benefits of Git is that it allows you to record changes as your software develops.

## 4.1 Improve the code readability

Open `pythagoras.py` and improve the variable names and add comments.

For example:

```python
import math

# Ask the user to enter the lengths of the two shorter sides
side_a = float(input("Enter first side: "))
side_b = float(input("Enter second side: "))

# Calculate the hypotenuse
hypotenuse = math.sqrt(side_a ** 2 + side_b ** 2)

# Display the result
print("Hypotenuse:", hypotenuse)
```

Save the file.

## 4.2 See what has changed

Enter:

```bash
git status
```

Git should show that `pythagoras.py` has been modified.

To see the actual changes, enter:

```bash
git diff
```

This displays the differences between your current file and the last committed version.

If the output opens in a pager, press `q` to exit.

## 4.3 Commit the changes

Stage the updated file:

```bash
git add pythagoras.py
```

Create a new commit:

```bash
git commit -m "Improve code readability"
```

You now have two commits in your history.

## 4.4 Add input validation

Now improve the program by checking that the user enters valid positive numbers.

Replace the contents of `pythagoras.py` with:

```python
import math

try:
    side_a = float(input("Enter first side: "))
    side_b = float(input("Enter second side: "))

    if side_a <= 0 or side_b <= 0:
        raise ValueError("Side lengths must be positive")

    hypotenuse = math.sqrt(side_a ** 2 + side_b ** 2)

    print("Hypotenuse:", hypotenuse)

except ValueError as error:
    print("Invalid input:", error)
```

Save the file and test it.

Try entering:
- Two valid positive numbers.
- A negative number.
- Text instead of a number.
- Zero.

The program should handle invalid input without producing an uncontrolled error.

## 4.5 Commit the validation changes

Stage the file:

```bash
git add pythagoras.py
```

Commit the changes:

```bash
git commit -m "Add input validation"
```

Check your history:

```bash
git log --oneline
```

You should now see three commits.

---

# Part 5: Upload your repository to GitHub

Git manages your version history locally. GitHub allows you to store and share your Git repository online.

## 5.1 Create a GitHub account

If you do not already have an account, register at:

https://github.com/

## 5.2 Create a new repository

1. Log in to GitHub.
2. Select the **New repository** option.
3. Give your repository a name, for example:

   `pythagoras`

4. Choose whether the repository should be public or private.
5. Select **Create repository**.

For this practical, it is easiest to create an empty repository without adding a README, licence or `.gitignore` file.

## 5.3 Connect your local repository to GitHub

GitHub will display a repository URL.

It will look something like:

`https://github.com/USERNAME/pythagoras.git`

In your terminal, enter the following command, replacing the URL with your own:

```bash
git remote add origin https://github.com/USERNAME/pythagoras.git
```

This connects your local Git repository to the online GitHub repository.

To check the connection, enter:

```bash
git remote -v
```

## 5.4 Push your code to GitHub

Enter:

```bash
git branch -M main
git push -u origin main
```

Your commits and files should now appear in your GitHub repository.

You may be asked to authenticate with GitHub.

## 5.5 Understand the commands

- `git branch -M main` renames the current branch to `main`, replacing an existing branch name if necessary.
- `git push` uploads your commits to the remote repository.
- `-u` sets up tracking between your local branch and the remote branch.
- `origin` is the conventional name for your remote repository.
- `main` is the name of the branch being pushed.

**Important:** A commit records changes locally. A push uploads those commits to GitHub. They are not the same thing.

---

# Part 6: Working with branches

Branches allow you to develop new features without immediately changing the main version of your software.

Imagine that your Pythagoras calculator is working correctly. You now want to add another feature: calculating the area of the right-angled triangle.

Instead of changing the main version directly, you can create a separate branch.

## 6.1 What is a branch?

A branch is an independent line of development within your Git repository.

For example:

- `main`: the main version of your software.
- `add-triangle-area`: a branch where you develop the new area calculation feature.

You can make and commit changes on the new branch without changing the contents of `main`.

This is useful when you want to experiment, develop new functionality or fix bugs without immediately affecting the working version.

## 6.2 Create a new branch

In your terminal, enter:

```bash
git switch -c add-triangle-area
```

This command creates a new branch and switches you to it.

You can also use:

```bash
git branch
```

to see your local branches.

The output should look something like:

```text
* add-triangle-area
  main
```

The asterisk (`*`) indicates the branch you are currently working on.

## 6.3 Switch between branches

To return to the main branch:

```bash
git switch main
```

To return to your feature branch:

```bash
git switch add-triangle-area
```

You can check which branch you are currently using with:

```bash
git status
```

**Important:** Switching branches changes which version of the tracked files you are working with. Make sure you commit or otherwise safely handle your changes before switching.

## 6.4 Add the new feature

While working on the `add-triangle-area` branch, open `pythagoras.py`.

Add the following calculation:

```python
area = (side_a * side_b) / 2
print("Triangle area:", area)
```

Your program should now calculate both the hypotenuse and the triangle's area.

Save the file and test the program.

Stage and commit your changes:

```bash
git add pythagoras.py
git commit -m "Add triangle area calculation"
```

You have now created a commit on the feature branch.

## 6.5 Compare branches

You can compare the contents of two branches using:

```bash
git diff main add-triangle-area
```

This shows the changes that exist on `add-triangle-area` compared with `main`.

You can also view the commit history across all branches:

```bash
git log --oneline --graph --all
```

This displays a graphical text representation of the commit history, including branches and merges.

## 6.6 Why create a branch and then merge it?

The main purpose of a branch is to develop a new feature without changing the stable version of your software.

For example:

**Without branching:**

You make changes directly to `main`. If your changes break the program, the main version is affected.

**With branching:**

1. `main` contains the working Pythagoras calculator.
2. You create `add-triangle-area`.
3. You develop the new area calculation separately.
4. You test the new feature.
5. Once you are happy with it, you merge it into `main`.
6. The main version now includes the new feature.

This allows you to experiment and test without immediately affecting the main version.

## 6.7 Merge your branch into main

Once you are happy with your new feature, you can merge it into the main branch.

First, switch back to `main`:

```bash
git switch main
```

Then merge the feature branch:

```bash
git merge add-triangle-area
```

Git will bring the changes from `add-triangle-area` into `main`.

If there are no conflicts, Git may complete the merge automatically.

Your main branch now contains the new triangle area calculation.

Push the updated main branch to GitHub:

```bash
git push
```

## 6.8 Push your feature branch to GitHub

A branch created locally does not automatically appear on GitHub.

To upload your feature branch, run:

```bash
git push -u origin add-triangle-area
```

You can now see the branch on GitHub.

To find it:

1. Open your repository on GitHub.
2. Select the **Code** tab.
3. Click the branch selector, which normally displays `main`.
4. Select `add-triangle-area` from the list.

You can also see the branches in the repository's **Branches** section.

## 6.9 Why is branching useful?

| Benefit | Explanation |
|---|---|
| Protect the working version | Experimental changes do not immediately affect `main`. |
| Develop features independently | Different features can be developed separately. |
| Test before integration | Check that new code works before merging it. |
| Support collaboration | Different developers can work on separate branches. |
| Review changes | Other developers can inspect changes before they are merged. |
| Experiment safely | Try new ideas without changing the main version. |

**A useful distinction:**

- A branch is where you develop changes separately.
- A commit records a particular change.
- A merge brings changes from one branch into another.
- A push uploads your local commits and branches to GitHub.

You do not necessarily need to delete a feature branch after merging it. However, deleting completed feature branches is common practice because it keeps the repository tidy.

---

# Part 7: Troubleshooting

## Error: `python: command not found`

Your computer may not recognise the `python` command.

Try:

```bash
python3 --version
```

If that works, run your program using:

```bash
python3 pythagoras.py
```

If neither command works, check that Python is installed and available in your system PATH.

## Error: `git: command not found`

Git may not be installed correctly, or it may not be available in your system PATH.

Check the installation and try:

```bash
git --version
```

If the command still does not work, reinstall Git and restart your terminal.

## Error: `remote origin already exists`

You may see this error when running:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

The terminal displays:

```text
error: remote origin already exists.
```

This means your local Git repository already has a remote repository called `origin` configured. You do not need to add it again.

**Step 1: Check the existing remote**

```bash
git remote -v
```

This displays the URL currently associated with `origin`.

**Step 2: Update the remote URL if necessary**

If you want to connect to a different GitHub repository, use:

```bash
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

Replace the URL with your own GitHub repository URL.

**Step 3: Check the remote again**

```bash
git remote -v
```

Make sure the URL is now correct.

**Step 4: Push your code to GitHub**

```bash
git branch -M main
git push -u origin main
```

Make sure the repository exists on GitHub and that you have permission to access it.

**Remember:**

- `git remote add origin` creates a remote connection for the first time.
- `git remote set-url origin` changes an existing remote connection.
- `git remote -v` lets you check which remote repository is configured.

## Error: `Repository not found`

If you see:

```text
remote: Repository not found.
fatal: repository not found
```

Check the following:

1. Does the repository exist on GitHub?
2. Is the repository URL correct?
3. Are you logged into the correct GitHub account?
4. If the repository is private, do you have permission to access it?

You can check the URL using:

```bash
git remote -v
```

If it is incorrect, update it:

```bash
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

## Error: `src refspec main does not match any`

This can happen if you have not created a commit yet, or if your current branch has a different name.

Check your status:

```bash
git status
```

If you have not committed your files, stage and commit them first:

```bash
git add pythagoras.py
git commit -m "Initial Pythagoras program"
```

Then check your branch:

```bash
git branch
```

If necessary, rename the current branch:

```bash
git branch -M main
```

Then try pushing again.

## Error: Authentication failed

GitHub may require you to authenticate before pushing.

Follow the authentication prompt or use GitHub's recommended authentication method.

You can find more information here:

https://docs.github.com/en/authentication

## I cannot see my new branch on GitHub

Creating a branch locally does not automatically upload it to GitHub.

Check your local branches:

```bash
git branch
```

If your branch exists, upload it using:

```bash
git push -u origin add-triangle-area
```

Replace `add-triangle-area` with the name of your branch if you used a different name.

Refresh your GitHub repository and open the branch selector.

---

# Part 8: Useful Git commands

| Command | Purpose |
|---|---|
| `git init` | Initialise a new repository |
| `git status` | Check the current repository status |
| `git add filename` | Stage a file |
| `git add .` | Stage all changes in the current directory |
| `git commit -m "message"` | Create a commit |
| `git log --oneline` | View commit history |
| `git diff` | View uncommitted changes |
| `git remote -v` | View remote repositories |
| `git push` | Upload commits to GitHub |
| `git pull` | Download and integrate remote changes |
| `git branch` | List local branches |
| `git switch branch-name` | Switch branches |
| `git switch -c branch-name` | Create and switch to a branch |
| `git merge branch-name` | Merge another branch into the current branch |
| `git diff main branch-name` | Compare two branches |

---

# Part 9: Reflection — Version control and sustainable software

Think about the practical and answer the following questions.

1. Why is it useful to keep a history of changes to your code?
2. How is Git different from GitHub?
3. What is the difference between a commit and a push?
4. Why might you create a branch instead of making changes directly to `main`?
5. How does testing a feature on a separate branch reduce the risk of breaking existing software?
6. Imagine that another researcher wants to understand how your Pythagoras program developed. How could the commit history help them?
7. How could version control help a research team collaborate on a shared software project?

## Connection to software sustainability

Version control supports sustainable research software because it helps developers:

- Understand how software has changed over time.
- Identify and investigate problems.
- Experiment with new features safely.
- Collaborate with other researchers and developers.
- Maintain and extend existing software.
- Preserve a transparent record of software development.

For example, imagine a research team has a simulation that produces published results. A researcher wants to add a new calculation method.

They can develop the new method on a separate branch, test it, compare its results with the existing method, and merge it when it is ready.

This reduces the risk of accidentally breaking software that other researchers depend on.

**The key message:** Version control is not just about saving different versions of files. It is about managing software development in a controlled, transparent and collaborative way.
