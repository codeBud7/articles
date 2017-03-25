---
title: How to customise your git commit messages
published: false
description: 
cover_image: 
tags: git
---

1. Enable git templates
```
git config --global init.templatedir '~/.git-templates'
```

_This tells git to copy everything in ~/.git-templates to your per-project .git/ directory when you run git init._

2. Create a directory to hold the global hooks
```
mkdir -p ~/.git-templates/hooks
```

3. Write your hooks in ~/.git-templates/hooks
```
#!/bin/bash

# This way you can customize which branches should be skipped when
# prepending commit message. 
if [ -z "$BRANCHES_TO_SKIP" ]; then
  BRANCHES_TO_SKIP=(master develop test)
fi

BRANCH_NAME=$(git symbolic-ref --short HEAD)
BRANCH_NAME="${BRANCH_NAME##*/}"

BRANCH_EXCLUDED=$(printf "%s\n" "${BRANCHES_TO_SKIP[@]}" | grep -c "^$BRANCH_NAME$")
BRANCH_IN_COMMIT=$(grep -c "\[$BRANCH_NAME\]" $1)

if [ -n "$BRANCH_NAME" ] && ! [[ $BRANCH_EXCLUDED -eq 1 ]] && ! [[ $BRANCH_IN_COMMIT -ge 1 ]]; then 
  sed -i.bak -e "1s/^/$BRANCH_NAME /" $1
fi
```

4. Make sure the hook is executable
```
chmod a+x ~/.git-templates/hooks/*
```

5. Re-initialize git in each existing repo you'd like to use this in
```
git init
```

NOTE:
* If you already have a hook defined in your local git repo, this will not overwrite it.

Related:
* https://chris.beams.io/posts/git-commit/
* https://help.github.com/articles/changing-a-commit-message/
* https://coderwall.com/p/jp7d5q/create-a-global-git-commit-hook
* https://gist.github.com/bartoszmajsak/1396344
