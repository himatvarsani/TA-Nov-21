# Refresh on Linux Commands

Here's a list of basic linux commands you might or might not be familiar with but which are going to be very helpful during your learning journey.

- Know the machine you are connected:

```sh
pwd         # print the name your current directory
ls          # list files in the current directory
uptime      # uptime of your mac
df -h       # disk and partitions
uname -a    # name of your mac
```

- Remote access:

```sh
# connect with username and password
ssh ec2-user@ip_address

# using private-key
ssh -i ~/.ssh/private_key ec2-user@ip_address
```

- Files and volumes:

 ```sh
 cd
 mkdir
 rm
 mv
 cp
 ls
 find
 grep
 cat
 less
 head
 tail
 vim
 du
 df
 which
 watch
 lsof
 md5sum
 tar
 locate
 tree
 chmod
 chown
 mount
 ln -s
 sort
 touch
 pbcopy
 mdfind
 ```

- System: 
```sh
sudo
man
env
adduser
addgroup
```

- Services:
```sh
ps
kill
service
defaults
```