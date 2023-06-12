# ssh

## ssh keys 
$ ssh-keygen -t ed25519 -C "your_email@example.com"


## Adding your SSH key to the ssh-agent
eval "$(ssh-agent -s)"
> Agent pid 59566

 ssh-add --apple-use-keychain ~/.ssh/id_ed25519
 
