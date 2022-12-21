# The Incremental Ring Challenge

For this challenge you will need to work together to complete the following objective:
> 5 instances talking to each other by receiving a file with a number in it, increment it and send it to another server

You can work within a single private subnet to deploy 5 instances.

Write a bash script to:
* Open the file received from another instance
* Increment the number present in the file
* Send the file on to the next instance

Rules:
* This is a collaborative work, so coordinate your actions well
* On the server, only use bash script (use any utilities you like)
* Send the file securely

> Tips:
> * Think about synchronising the execution of the script. Look up a utility called `cron`.
> * It'll be easier to automate if the file location is the same across the servers
