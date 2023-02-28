gitPush(){
if [ -z "$1" ]
  then
   echo "Please enter a comment before pushing your code changes in"
else
   echo "Pushing Code to GIT"
   gitStatus
   git add --all
   git commit -am "$1"
   git push origin HEAD
fi
}

gitStatus(){
 git status
}

gitGraph(){
 git log --all --decorate --oneline --graph
}

gitCache(){
 git config --global credential.helper 'cache --timeout 36000000'
}

gitFetch(){
 git pull
 git fetch --all && git reset --hard origin/master
 git checkout master
 git fetch && git checkout $1
}

gitBranchHistory(){
 git for-each-ref --format='%(committerdate) %09 %(authorname) %09 %(refname)' | sort
}

gitResetToMaster(){
 git clean -df
 git reset --hard origin/master
}

gitAddChmod(){
  echo "git update-index --chmod=+x $1"
  git update-index --chmod=+x $1
}

gitListStage(){
 git ls-files --stage
}

gitChmodAllFiles(){
for f in *.sh ; do \
#  echo "git update-index --chmod=+x $f" \
  git update-index --chmod=+x $f; \
done
}

tarFile(){
  echo "tar -czvf $1 -C $2 ."
  tar -czvf $1 -C $2 .
}

gitAddToSafeDir(){
 echo "find $1 -name '.git' -type d -exec bash -c 'git config --global --add safe.directory ${0%/.git}' {} \; "
 find $1 -name '.git' -type d -exec bash -c 'git config --global --add safe.directory ${0%/.git}' {} \; 
}

applyDOS2UNIX(){
  echo "Applying dos2unix"
  find $1 -type f -print0 | xargs -0 dos2unix --
}

findCRLF(){
  echo "Applying dos2unix"
  find $1 -not -type d -exec file "{}" ";" | grep CRLF
}

gitBranch(){
  echo "git checkout -b $1"
  git checkout -b $1

  echo "git branch -u origin $1"
  git branch -u origin $1

  echo "git push origin HEAD"
  git push origin HEAD

}

gitDeleteAllButMaster(){
 echo "=============Showing all Branches================"
 git branch
 git branch | grep -v "master" | xargs git branch -D 
}
