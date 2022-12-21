# Git setup for github

Generate a new keypair file and don't forget to define the path and name of the
generated key.

```sh
ssh-keygen -t rsa -C "your@email.address" -f ~/.ssh/test_ssh_key
```

# Configure your ssh config

```sh
vi ~/.ssh/config
```

In the file, use the code below
```sh
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/test_ssh_key
  IdentitiesOnly yes
```

# Here's the basics of git

Clone your existing repository:
```sh
git clone git@github.com:cloudreach/talent-academy.git
cd talent-academy
```

Example with adding a new file:
```sh
touch new_file.txt
```

Check the status of your repository:
```sh
git status
```

It should give you something like:
```sh
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new_file.txt

nothing added to commit but untracked files present (use "git add" to track)
```


Add the file that you want to commit (save):
```sh
git add new_file.txt
```

Commit your changes, which will save the state of your current work locally:
```sh
git commit -m "ADD - saving my new file"
```


You are now ready to push your changes:
```sh
git push
```
