
# Connecting to Remote Repository

1. Create New Directory
2. Initialize Directory `git init`
3. Connect to remote git repository `git remote add origin https://github.com/USERNAME/REPO_NAME.git` `git remote -v`
4. Pull commits and current files in the repository `git fetch origin`

# General Git Workflow

1. Pull Repository Updates: `git pull origin`
2. Stage files from current directory: `git add .` \
   _Note: Cannot stage new empty directory; must have files inside_
3. Commit added files and changes: `git commit -m "COMMIT MESSAGE"` \
   _Note: Changes now made to local repo, need to push to remote repo_
4. Push committed changes to repo: `git push origin`

# More Git Commands

Switching Branches: `git switch BRANCH`

List Current Branch: `git branch --show-current`

Pull Repository Logs: `git log`

Display all pulled Repository Files: `ls -la`&#x20;

* `-l`: Long list format&#x20;
* `-a`: Display all

Change remote URL: `git remote set-url origin git://new.url.here`

Remove remote name: `git remote remove origin`

Remove file from repository: `git rm --cached <file-path>`

Remove directory from repository: `git rm -r --cached <directory-path>`

## Creating and adding files to .gitignore

1. Create `.gitignore` if it does not exist `touch .gitignore`
2. Update `.gitignore`: You will need to edit and add patterns representing the files or directories you wish to ignore. Each line in the `.gitignore` file specifies a pattern. Here are some examples:
   * Ignore a specific file: `secret.txt`
   * Ignore all `.log` files: `*.log`
   * Ignore a specific directory: `bin/` (ignoring the directory called bin)
   * Ignore all `.txt` files in a specific directory: `docs/*.txt`
   * **Using a Text Editor:** Open your `.gitignore` file in any text editor. Add the patterns for the files or directories you want to ignore, each on a new line. Save the file after making your changes.
   * **Using Command Line:** You can also append entries to your `.gitignore` file directly from the command line.&#x20;
     * For example, to ignore all `.log` files, you could use: `echo "*.log" >> .gitignore`
3. Add and commit the `.gitignore` file: \
   `git add .gitignore` \
   `git commit -m "Add/update .gitignore"`

#### **Resources:**

Setting Up personal access token: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens
