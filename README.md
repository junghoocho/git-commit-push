# git_commit_push

I am an old dog. I learned version control system with CVS and subversion (SVN). Under CVS or SVN, a single "commit" is all I need when I want to commit my changes to the central repository. Unfortunately, things are different with git. Under git, I first have to "commit" my changes to the local repository and then "push" all my commits to the remote origin repository. Two commands to run, not one! But since it is difficult to teach new tricks to an old dog, I always forget to "push" after "commit". The consequence of this small negligence is often enormous. If I forget to "push", my changes are not reflected in the origin repository, which leads to many unresolved conflicts down the roads... So much unnecessary work just because I "forgot"!

This short bash function is for people like me. Using this function, we can run a single-command `git_commit_push` to commit and push with one command, not two. When called, this function (1) commits all changes in the working tree to the local repository and then (2) pushes the commited changes to the remote origin repository. No more unresolved conflicts because I "forgot!". 

Of course, "git_commit_push" is too long to type for me, so I add "alias gcp=git_commit_push" as well, so that I can just type "gcp" when I want to commit and push. 

So if you want to use this funciton, place the following code (together with your favorite alias) in .bashrc (or .profile) and enjoy using `git_commit_push`! No more suffering from "I forgot again!"

```bash
function git_commmit_push() {
    COMMIT_MSG="Update"
    ERROR=0
    while getopts ":a:m:" opt; do
      case $opt in
        a)
          git add "$OPTARG"
          ;;
        m)
          COMMIT_MSG="$OPTARG"
          ;;
        h | *)
          echo "Usage: git_commit_push [-a file] [-m commit_message]"
          echo "  Commit all changes in the working tree to the local repo and push them to remote origin repo"
          echo '  -a "git add" the file before commit'
          echo '  -m use the provided commit message instead of the default message "Update"'
          ERROR=1
          ;;
      esac
    done
    shift $((OPTIND -1))
    OPTIND=1

    if [ $ERROR == 0 ]; then
        git commit -a -m "$COMMIT_MSG"
        ORIGIN=$(git remote)
        if [ $ORIGIN ]; then
            git push
        fi
    fi
}
```
