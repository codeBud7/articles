---
title: How to customise your git commit message 🚀
published: true
description: Six steps to defining your git commit workflow
cover_image: 
tags: git,bash
---

**1. Add default template directory**

```bash
git config --global init.templatedir '~/.git-templates'
```

_Files and directories in the default template directory (~/.git-templates) whose name do not start with a dot will be copied to the specific $GitDir after it is created._

_See [git-init](https://git-scm.com/docs/git-init)_

**2. Add default hook directory**

```bash
mkdir -p ~/.git-templates/hooks
```
_The hooks are stored in the hooks subdirectory of the default template directory._

_See [git-hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)_

**3. Add your first hook (e.g. prepare-commit-msg)**

```bash
vi ~/.git-templates/hooks/prepare-commit-msg
```

_Add a custom hook that puts your branch name to the top of your commit message. (Excluding master, develop)_

```bash
#!/bin/sh

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

_See [git-hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)_

**4. Make the hook executable**

```bash
chmod +x ~/.git-templates/hooks/*
```
_Git hooks are not made executable by default._

**5. Re-initialize git in each existing repository**

```bash
git init
```
_To use your new hook you have to re-initialize git._

_See [git-init](https://git-scm.com/docs/git-init)_

**6. Commit 🚀**

```bash
git commit -v
```
_How does it look like?_

![alt text](https://github.com/codeBud7/articles/blob/master/img/how-to-customise-your-git-commit-message.png?raw=true "Sample")


**7. Automation 🤖**
If you'd like to add the same commit message for every git project you could  configure something like this:

First create your template file. (For example `~/.gitmessage`)
```bash
Why:
* 
```

After that you can simply add this file to your `~/.gitconfig`
```bash
[commit]
    template = ~/.gitmessage
```

# Hints 💡
👉 If a hook is already defined in your local git repository the new hook won't overwrite it.
👉 You might have to set the permissions on your new hook. (sudo chmod 775 .git/hooks/prepare-commit-msg)

##### RELATED:
[Chris Beams about git commit messages](https://chris.beams.io/posts/git-commit/)
