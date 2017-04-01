# fix-git-commits.sh

Execute script:
```
sh ./fix-git-commits.sh <repo-name>
```

**fix-git-commits.sh**
```
#!/bin/sh

GIT_REPO_NAME=$1

git clone --bare https://github.com/benbarth/$GIT_REPO_NAME.git
cd $GIT_REPO_NAME.git

git filter-branch --env-filter '
CORRECT_NAME="Ben Barth"
OLD_EMAIL="ben@example.com"
CORRECT_EMAIL="benbarth@users.noreply.github.com"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

Push the corrected history:
```
git push --force --tags origin 'refs/heads/*'
```

https://help.github.com/articles/changing-author-info/
