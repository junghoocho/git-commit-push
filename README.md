# gitpush
You can place the following bash function in .bashrc (or .bash_profile) to use "gitpush" as a single command that performs "git commit" and "git push" in one shot.

```bash
function gitpush() {
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
          echo "Usage: gitpush [-a file] [-m commit_message]"
          echo "  -a add the file or directory to the respository"
          echo '  -m use the provided commit message. Default is "Update"'
          ERROR=1
          ;;
      esac
    done
    shift $((OPTIND -1))
    OPTIND=1

    if [ $ERROR == 0 ]; then
        git commit -a -m "$COMMIT_MSG"
        ORIGIN=`git remote`
        if [ $ORIGIN ]; then
            git push $ORIGIN
        fi
    fi
}
```
