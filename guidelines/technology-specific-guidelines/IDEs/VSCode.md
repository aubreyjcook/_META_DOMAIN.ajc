# VS Code overview

Visual Studio Code (VS Code) is a popular code editor developed by Microsoft. It is widely used by developers for its lightweight design, powerful features, and extensive customization options. VS Code supports a wide range of programming languages and provides a rich set of tools for code editing, debugging, and version control.

## Table of Contents

### Github Multi Account Config Note

Configuring multiple accounts to use with Github in VS Code is rather annoying to work with repos.

Here's the process:

1. Config SSH keys for each account locally. (You do this via terminal and have the option of various encryption methods.) It's an option to set a passphrase here but I have found it to be tedious, as you will be prompted for it with every commit.
2. Config the SSH config file within system environment under `~/.ssh/config` to use the correct SSH key for each account. This was done on Windows, in Linux or MacOS it may differ. It looks something like
```
# Main GitHub account
Host github.com
  User git
  HostName github.com
  IdentityFile ~/.ssh/id_rsa

# Secondary GitHub account
Host github-secondary
  User git
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_secondary
```

Notably you do not redefine the user, it's meant to be `git` nor the hostname, although that would be obvious. The main thing that needs to be defined is the `IdentityFile` which must be defined with an accurate path to the SSH key you generated in step 1. I've found it's better to include the ssh key within the same directory as the config file.

3. Add the SSH key to the Github accounts. You needed to grab the public key from the terminal from what you made in step 1 to do this. You normally do this with `cat ~/.ssh/id_rsa.pub` and then copy the output to the Github account settings. This is a one-time setup and doesn't need to be repeated for each repo or commit.
4. Open VS Code and go into a repo, either cloned from one of the accounts or whatever.
5. In terminal define `git config --local user.name "username"` and `git config --local user.email "useremail"` for the repo. Note that defining it in local instead of global is important. Once this is defined in the local repo, it's persistent and does not need to be redefined for each commit or instance of VS Code. It does not override your global setting so you main account doesn't typically need to be added to each local repo.
6. In terminal define the `git remote set-url origin git@github-secondary:username/reponame.git` to set the remote origin to the correct account. This is also persistent and does not need to be redefined for each commit or instance of VS Code. The main command doesn't change much. The 'url' that the command points to is the main concern. It's comprised of `git@` then `github-secondary:` or main or primary or whatever, this is defined in the SSH config file above. Then following is the `username/reponame.git` which is the account and repo name you're working with. You can verify this after setting it by running `git remote -v` in the terminal. Notably, this variable is often defined by default, but when using SSH keys it tends to be wrong in the sense that it attempts to use HTTPS typically by default which will not work. The main error you see when failing to set this when everything else is correct is that the repo doesn't exist, but you can also get permission denied when it's combined with not setting the ssh/user config correctly.
7. One more check to run for SSH which can be done optionally now or earlier is to define the ssh connection with `ssh -T git@github-secondary` or main/primary/etc. This is just a verification step to ensure that the SSH keys are working correctly and that the connection is established. Verifying this doesn't mean you won't get an error especially in the previous step, but it tends to rule out the SSH keys as the issue if it's working correctly. If you defined a passphrase in step 1, you will be prompted for it here.
8. The last step is to make sure you are logged in VS Code with the git integration with the account you intend to push with, especially when using the integrated 'source-control' tab in the sidebar. This is a common oversight and can lead to confusion when trying to push commits. In my experience you typically need to log in once to make the commit change and push with the correct account you want to associate with the repo before it will remain persistent, but you may need to check this. This is particularly true when using an account that is not the same as the one you have defined as global in git config. This issue mainly stems from VS Code itself, which has a poor way of handling multiple account switching. Regardless, once you have commited once with all correct settings, it tends to remain persistent, meaning changing from a secondary back to a main will maintain this setting until you log out or change it again.
9. Make commits/pushes as normal. A way to verify you are getting the right account is to commit but not push, then verify the author of the commit which shows in the source control timeline or in the terminal with `git log`. If it's the wrong account, you need to go back and check the steps above.