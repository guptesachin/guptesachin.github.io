local repo
```
git --git-dir=./.mysync --work-tree=./ init
git commit #if local files found
git --git-dir=./.mysync --work-tree=./ fetch-pack ssh://sachin@127.0.0.1/tmp/syncTest/.mysync
```

remote-repo
```
git --git-dir=./.mysync init
git --git-dir=./.mysync config core.preloadindex true
git --git-dir=./.mysync config user.email ""
git --git-dir=./.mysync config user.name ""
git --git-dir=./.mysync --work-tree=./ commit --allow-empty -m "test"
git --git-dir=./.mysync --work-tree=./ ls-files -X ./.mysync/info/exclude -o -m | git --git-dir=./.mysync --work-tree=./ update-index --add --remove --stdin
```

post-commit local repo
```
#! /bin/sh

git push myauto

echo "--done push to remote--"
```
post-receive remote repo
```
!# /usr/bin/env bash

while read oldrev newrev refname
do
	BRANCH=$(git rev-parse --symbolic --abbrev-ref $refname)
	if [ "$BRANCH" == "syncLocal" ]
	then
		GIT_WORK_TREE=/tmp/syncRemote git merge syncLocal
	fi
	echo "merge to master"
done
```
