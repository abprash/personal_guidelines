# git guidelines

## for prashanth

## Some points to remember
+ Always pull before you push.
+ Usual cycle should be like - Pull - Merge - Commit - Push. Repeat.
+ Commit frequently. (Seriously I am not kidding, even a change spanning 10 lines of code can be committed. Git takes care of it.) The frequent commit practice would aid you in case you break the system.
+ Branch in case you want to experiment. One need not take risks on the master.
+ As far as possible always try to compile before push. Better to push only if the compilation succeeds.


#### initial cloning
* `git clone <GIT URL>`

#### always pull before you push. git pull is equivalent of git fetch + git merge

* `git pull origin master`

#### to add files (to stage files for commiting)
* `git add .`

	OR

* `git add <filename>`

#### to commit
* `git commit -m <commit message>`

#### push - to commit your changes to the cloud
* `git push origin master`

#### to checkout a particular commit
* `git checkout <commit-SHA>`

	OR

*	`git checkout origin <commit-SHA>`
*	`git checkout FETCH_HEAD`

#### to view list of commits
* `git log`

#### to view general status
* `git status`

#### to create branch
branches in git are very cheap (really cheap!) so branch away
* `git checkout -b <new_branch_name>`

#### ignoring changes for a single file
*	`git checkout <file name>`

OR

#### to ignore multiple file changes
*	`git checkout .`


#### merging
<>

#### to delete a previous push to GitHub
basically undoes a push to github
but the commit stays in the local system
* `git push -f origin HEAD^:master`


#### to remove the most recent local
* `git reset --hard HEAD~`

&ltscript src="https://gist.github.com/&ltgist_id&gt.js"&gt &lt/script&gt
