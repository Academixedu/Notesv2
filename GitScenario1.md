### Scenario: Working with Git and GitHub (Add, Commit, Revert, and Collaborating via Pull Requests)

#### Context:

You are working on a web application project with a colleague. You both are using Git for version control and GitHub to collaborate by pushing your changes to a remote repository. In this scenario, you will perform basic git operations (`add`, `commit`, `push`), and then handle a pull request (PR) from a colleague. After accepting the PR, you realize the changes are incorrect, so you'll revert those changes and commit again.

---

### Step 1: Initialize a Git Repository Locally and Make Changes

1. First, initialize your project repository locally:

   ```bash
   git init
   ```

2. Create a simple `index.html` file:

   ```html
   <html>
     <body>
       <h1>Welcome to My Web App</h1>
     </body>
   </html>
   ```

3. Add this file to the staging area:

   ```bash
   git add index.html
   ```

4. Commit your changes with a message:

   ```bash
   git commit -m "Initial commit: Added index.html"
   ```

### Step 2: Push the Repository to GitHub

1. Create a new repository on GitHub (let's say it's named `my-web-app`).

2. Add the GitHub remote to your local repository:

   ```bash
   git remote add origin https://github.com/yourusername/my-web-app.git
   ```

3. Push your local repository to GitHub:

   ```bash
   git push -u origin master
   ```

   Now, your repository is live on GitHub.

Yes, when you use `git push -u origin master` to push your code to GitHub, you may be prompted for authentication. GitHub has deprecated the use of username/password authentication for pushing to repositories, and now, you need to use a **Personal Access Token (PAT)** for authentication.

Here’s how you can handle this:

### Steps to Create a Personal Access Token (PAT) for GitHub:

1. **Log in to GitHub** and go to your profile settings.
2. Navigate to **Developer settings**.
3. Click on **Personal access tokens** on the left-hand menu.
4. Select **Generate new token**.
5. Give the token a descriptive name, select the required scopes (usually `repo` for full control over your repositories), and generate the token.
6. Copy the generated token (you will only see it once).

### Using the PAT for Git Push:

```bash
git remote set-url origin https://john-doe:ghp_12345abcde@github.com/john-doe/my-web-app.git

```

### Step 3: Collaborating with a Colleague via Pull Request

1. Your colleague forks your repository and makes a new branch (`feature-header-update`). They make some changes in `index.html` by updating the `<h1>` tag.

   ```html
   <html>
     <body>
       <h1>Welcome to Our Awesome Web App</h1>
       <!-- Updated header -->
     </body>
   </html>
   ```

2. Your colleague commits these changes and pushes them to their forked repository. Then, they open a Pull Request (PR) to your repository.

---

### Step 4: Merging the Pull Request

1. You review your colleague's PR and merge it into the main branch via GitHub.

2. Pull the merged changes into your local repository:

   ```bash
   git pull origin master
   ```

---

### Step 5: Reverting the Changes Made by Your Colleague

1. After testing the merged changes, you realize the header update was incorrect, and you need to revert the changes made by your colleague.

2. Identify the commit made by your colleague by using `git log`:

   ```bash
   git log
   ```

3. Use the `git revert` command to revert their changes. Suppose the commit hash of the PR is `abcd1234`:

   ```bash
   git revert abcd1234
   ```

4. After the revert, check the `index.html` file. It should be back to its original state:

   ```html
   <html>
     <body>
       <h1>Welcome to My Web App</h1>
     </body>
   </html>
   ```

5. Now, commit the reverted changes:

   ```bash
   git commit -m "Reverted colleague's header change"
   ```

---

### Step 6: Push the Reverted Changes to GitHub

1. Push your local changes to the GitHub repository:

   ```bash
   git push origin master
   ```

Now, the reverted version of the project is updated on GitHub, and the original version of the `index.html` is restored.
