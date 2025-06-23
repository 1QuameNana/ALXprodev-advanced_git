pre-commit hooks

#!/bin/bash

# Pre-commit hook: Ensure every directory contains a README file
directories=$(git ls-files | awk -F/ 'NF>1 {print $1}' | sort -u)

missing_readmes=()

for dir in $directories; do
    if ! ls "$dir"/README* >/dev/null 2>&1; then
        missing_readmes+=("$dir")
    fi
done

if [ ${#missing_readmes[@]} -ne 0 ]; then
    echo "Commit blocked. The following directories are missing a README:"
    for dir in "${missing_readmes[@]}"; do
        echo " - $dir"
    done
    exit 1
fi

exit 0



Post-merge hooks
#!/bin/bash
# Post-merge hook: Log merges into main branch
branch=$(git symbolic-ref --short HEAD)
if [ "$branch" = "main" ]; then
    echo " Merge completed on $(date) by $(git config user.name)" >> .git/merge-log.txt
fi
