# Practical: Introducing Git with the Pythagoras Project

## Learning objectives

By the end of this practical, you should be able to:

-   Create a local Git repository.
-   Stage and commit changes.
-   Read a simple Git history and inspect changes.
-   Connect a local repository to GitHub and push it.
-   Create a branch, make a change, and merge it into `main`.

## What you need

-   Python 3 installed.
-   Git installed.
-   A GitHub account.
-   A terminal (or the terminal in VS Code or GitHub Codespaces).

> **Important:** Run the Git commands from the project folder. In VS
> Code, open the project folder and use **Terminal → New Terminal**.

------------------------------------------------------------------------

## Part 1: Create the project

Create a folder called `pythagoras`. Inside it, create a file called
`pythagoras.py`.

Add this deliberately basic starting version:

``` python
import math

var1 = int(input("Enter first side: "))
var2 = int(input("Enter second side: "))

var3 = var1 ** 2
var4 = var2 ** 2

var5 = var3 + var4
var6 = math.sqrt(var5)

print(var6)
```

Save the file. Run it and check that entering `3` and `4` gives `5.0`.

## Part 2: Create a Git repository

Open a terminal in the `pythagoras` folder and run:

``` bash
git init
```

This creates a local Git repository.

Check the repository status:

``` bash
git status
```

Stage the Python file:

``` bash
git add pythagoras.py
```

Commit the first version:

``` bash
git commit -m "Initial Pythagoras program"
```

A **commit** is a saved snapshot of the staged changes, with a message
describing what changed.

If Git asks you to identify yourself, configure a name and email for
your commits:

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Then repeat the commit command.

## Part 3: Improve readability

Edit `pythagoras.py`. Replace the original code with this version, which
uses meaningful variable names and comments:

``` python
import math

# Ask the user for the lengths of the two shorter sides
side_a = int(input("Enter first side: "))
side_b = int(input("Enter second side: "))

# Calculate the square of each side
side_a_squared = side_a ** 2
side_b_squared = side_b ** 2

# Add the squared values and calculate the square root
sum_of_squares = side_a_squared + side_b_squared
hypotenuse = math.sqrt(sum_of_squares)

# Display the result
print("Hypotenuse:", hypotenuse)
```

Save the file. Inspect the changes:

``` bash
git diff
```

Stage and commit the improvement:

``` bash
git add pythagoras.py
git commit -m "Improve code readability"
```

View the commit history:

``` bash
git log --oneline
```

You should now see at least two commits.

## Part 4: Add input validation

Now improve the program so it accepts decimal side lengths and handles
invalid input.

Replace the contents of `pythagoras.py` with:

``` python
import math

try:
    # Ask the user for the lengths of the two shorter sides
    side_a = float(input("Enter first side: "))
    side_b = float(input("Enter second side: "))

    # Check that both side lengths are positive
    if side_a <= 0 or side_b <= 0:
        raise ValueError("Side lengths must be positive")

    # Calculate the hypotenuse
    hypotenuse = math.sqrt(side_a ** 2 + side_b ** 2)

    # Display the result
    print("Hypotenuse:", hypotenuse)

except ValueError as error:
    print("Invalid input:", error)
```

Test the program with: - `3` and `4` --- expected hypotenuse: `5.0`. -
`3.5` and `4.5` --- expected to calculate a decimal result. - `hello` as
one of the inputs --- expected to display an error message. - `-3` as
one of the inputs --- expected to display an error message.

Stage and commit the changes:

``` bash
git add pythagoras.py
git commit -m "Add input validation"
```

## Part 5: Push the repository to GitHub

1.  Sign in to GitHub.
2.  Create a **New repository** called `pythagoras`.
3.  Choose whether the repository should be public or private.
4.  For this exercise, do not initialise the GitHub repository with a
    README, licence, or `.gitignore`; the local repository already
    exists.
5.  Copy the repository URL from GitHub.

In your terminal, replace the example URL below with your own repository
URL:

``` bash
git remote add origin https://github.com/USERNAME/pythagoras.git
git branch -M main
git push -u origin main
```

Refresh the GitHub repository page. You should see `pythagoras.py` and
the commit history.

### What is the difference between Git and GitHub?

-   **Git** is the version control system. It records changes and
    history in your local repository.
-   **GitHub** is an online platform that hosts Git repositories and
    supports sharing and collaboration.

## Part 6: Challenge --- use a branch to add a feature

You will add a feature without making changes directly to the `main`
branch.

The new feature is to calculate the area of the right-angled triangle:

\[ `\text{Area}`{=tex} =
`\frac{\text{side}_a \times \text{side}_b}{2}`{=tex} \]

### Step 1: Create and switch to a feature branch

``` bash
git switch -c add-triangle-area
```

This creates a branch called `add-triangle-area` and switches you to it.

Check which branch you are on:

``` bash
git branch
```

The current branch is marked with an asterisk (`*`).

### Step 2: Add the feature

In `pythagoras.py`, add the following lines after the hypotenuse
calculation and before the output:

``` python
# Calculate the area of the triangle
area = (side_a * side_b) / 2
```

Add an output line alongside the existing hypotenuse output:

``` python
print("Triangle area:", area)
```

Run the program and check that inputs `3` and `4` produce an area of
`6.0`.

### Step 3: Commit the feature on the branch

``` bash
git add pythagoras.py
git commit -m "Add triangle area calculation"
```

### Step 4: Switch back to main

``` bash
git switch main
```

Open `pythagoras.py`. The area calculation should not be present on
`main` yet. The feature exists on the separate branch.

### Step 5: Merge the feature branch into main

``` bash
git merge add-triangle-area
```

Open `pythagoras.py` again. The area calculation should now be present
on `main`.

### Step 6: Push the updated main branch to GitHub

``` bash
git push
```

The GitHub repository should now contain the merged feature.

------------------------------------------------------------------------

## Reflection questions

1.  What does `git init` do?
2.  What is the difference between staging a file and committing it?
3.  Why should commit messages describe the change?
4.  How can `git log --oneline` help you understand a project's history?
5.  What is the difference between Git and GitHub?
6.  Why might a developer create a feature branch instead of changing
    `main` directly?
7.  What does merging a branch do?
8.  How can version control support the long-term sustainability of
    research software?

## Useful commands

  -----------------------------------------------------------------------
  Command                             Purpose
  ----------------------------------- -----------------------------------
  `git init`                          Create a local repository

  `git status`                        Show the current state of the
                                      working directory and staging area

  `git add pythagoras.py`             Stage a file

  `git diff`                          Show unstaged changes

  `git commit -m "message"`           Commit staged changes

  `git log --oneline`                 Show a compact commit history

  `git branch`                        List local branches

  `git switch -c branch-name`         Create and switch to a branch

  `git switch main`                   Switch to the main branch

  `git merge branch-name`             Merge a branch into the current
                                      branch

  `git push`                          Push commits to the remote
                                      repository
  -----------------------------------------------------------------------

## Sustainability connection

Version control helps research software remain understandable and
maintainable. It provides a record of how the software has changed,
makes it easier to recover earlier versions, supports safe
experimentation, and helps collaborators work together.
