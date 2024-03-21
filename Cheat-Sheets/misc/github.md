# Connecting to Remote Repository

1. Create New Directory 
2.  Initialize Directory
	`git init`
	`git config --global user.name "USERNAME"`
	`git config --global user.email "EMAIL_ADDR"`
3. Connect to remote git repository
	`git remote add origin https://github.com/USERNAME/REPO_NAME.git`
	`git remote -v`
4. Pull commits and current files in the repository
	`git fetch origin`
# General Git Workflow 

1. Pull Repository Updates:
	`git pull origin`
	
2. Stage files from current directory:
	`git add .`
	*Note: Cannot stage new empty directory; must have files inside*
	
3. Commit added files and changes:
	`git commit -m "COMMIT MESSAGE"`
	*Note: Changes now made to local repo, need to push to remote repo*

4. Push committed changes to repo:
	`git push origin`
	
## More Git Commands
Switching Branches:
	`git switch BRANCH`
	
List Current Branch:
	`git branch --show-current`

Pull Repository Logs:
	`git log`
	
Display all pulled Repository Files:
`ls -la`
	`-l`: Long list format
	`-a`: Display all
	
Change remote URL: 
`git remote set-url origin git://new.url.here`

Remove remote name: 
`git remote remove origin`

### **Resources:**

Setting Up personal access token: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens