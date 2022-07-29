# Linux Show / Display Available Network 

**`ip -c -br  a`**

# How to run the previous command with sudo quickly

**`sudo !!`**

# `Ctrl-L` or clear will not clear the page, you can still scroll backwards and view the buffer data.

**`Ctrl-L`**

# Ctrl+R to search and other terminal history trick

**`Ctrl-R`**  

```(reverse-i-search) :```

# List All Services in Ubuntu

`service --status-all`

# Display only running services in Ubuntu

`service --status-all | grep '\[ + \]'`

# Extract only stopped services in Ubuntu

`service --status-all | grep '\[ - \]'`

# Using the systemctl command

`systemctl --type service --all`