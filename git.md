#### Git Help

##### create a new repository
* create a new directory, open it 
* create a new git repository
    * git init

##### checkout a repository
* create a working copy of a local repository
    * git clone /path/to/repository
* create a working copy of a remote repository
    * git clone username@host:/path/to/repository

##### add & commit
* local repository consists of three "trees" maintained by git
    1. Working Directory which holds the actual files
    2. Index which acts as a staging area
    3. HEAD which points to the last commit made
* propose changes (add it to the Index)
    * git add <filename>
    * git add *
* commit these changes to the HEAD
    * git commit -m "Commit message"
    
##### pushing changes
* changes are now in the HEAD of your local working copy
* send those changes to your remote repository
* change master to whatever branch you want to push your changes to
    * git push origin master
 
* If you have not cloned an existing repository and want to connect your repository to a remote server
    * git remote add origin <server>
* Now you are able to push your changes to the selected remote server
