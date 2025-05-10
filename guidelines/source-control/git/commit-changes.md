# Git Commit Changes

## Overview

### Deleting Last Pushed Commit for Emergencies

The following pair of commands completes a hard removal of the last commit you've directly pushed to the remote repository. Run it from your local repository.

```bash
git reset --hard HEAD~1
git push --force
```