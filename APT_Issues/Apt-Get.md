# Most of the repositories and PPAs in your sources.list are no longer available and are throwing errors. I'd recommend restoring the default repositories.

**First, restore the default focal repositories using these commands:**

`mkdir ~/solution`

`cd ~/solution/`

```
cat << EOF > ~/solution/sources.list
deb http://archive.ubuntu.com/ubuntu/ focal main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse

deb http://archive.canonical.com/ubuntu focal partner
deb-src http://archive.canonical.com/ubuntu focal partner
EOF
```

`sudo rm /etc/apt/sources.list`
`sudo cp ~/solution/sources.list /etc/apt/sources.list`

**Remove all the PPAs in your system:**

`sudo mv /etc/apt/sources.list.d/* ~/solution`

**Update the repositories:**

`sudo apt update`

**Now there should be no errors.**