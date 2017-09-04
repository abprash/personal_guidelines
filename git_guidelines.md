#git guidelines#

##for prashanth

#initial cloning
*git clone <GIT URL>

#always pull before you push
#git pull is equivalent of git fetch + git merge
git pull origin master

#to add files (to stage files for commiting)
	git add .

	OR

	git add <filename>

#to commit 
git commit -m <commit message>

#push - to commit your changes to the cloud
git push origin master

#to checkout a particular commit 
	git checkout <commit-SHA>

	OR

	git checkout origin <commit-SHA>
	git checkout FETCH_HEAD

#to view list of commits
git log

#to view general status
git status

#to create branch
#branches in git are very cheap (really cheap!) so branch away
git checkout -b <new_branch_name>

#ignoring changes
	git checkout <file name>
	OR

	#to ignore multiple file changes
	git checkout .


#merging

#to delete a previous push to GitHub
#basically undoes a push to github
#but the commit stays in the local system
git push -f origin HEAD^:master


#to remove the most recent local
git reset --hard HEAD~


