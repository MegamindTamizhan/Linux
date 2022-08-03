# How to fix apt-get update error on Ubuntu or Debian

Ubuntu or Debian Linux system maintains a local database (so-called APT package index) of packages that are available from the repositories defined in `/etc/apt/sources.list`, or in `/etc/apt/sources.list.d/`. The APT package index can often become outdated as the packages contained in the repositories change, or new repositories are added to the system.

In order to update the local apt package index, you run:

`$ sudo apt-get update`

However, for various reasons you may encounter errors while updating the APT package index. In the following I describe several ways to fix common `apt-get` update errors.

# Hash Sum Mismatch Error

* `Failed to fetch http://in.archive.ubuntu.com/ubuntu/dists/updates/main/iyy/Translation-en  Hash Sum mismatch`

* `Some index files failed to download. They have been ignored, or old ones used instead`

This error can happen when fetching the latest repositories during `"apt-get update"` was interrupted, and a subsequent `"apt-get update"` is not able to resume the interrupted fetch. In this case, remove the content in `/var/lib/apt/lists` before retrying `"apt-get update"`.

`$ sudo rm -rf /var/lib/apt/lists/*`

`$ sudo apt-get update`

If the above does not solve the problem, use the following commands instead.

`$ sudo rm -R /var/lib/apt/lists/partial/*`

`$ sudo apt-get update`

# 404 Not Found Error

If you are getting `"404 Not Found"` error, and you are using a rather old Ubuntu release, there is a chance that the 404 error occurs because your Ubuntu installation is no longer supported.

To find out whether this is the case, first check which Ubuntu release you are using, by running:

`$ cat /etc/lsb-release`

Then check out the `"End of Life"` date of your Ubuntu release by referring to https://wiki.ubuntu.com/Releases

If your release has reached end of life (EOL), you need to modify `/etc/apt/sources.list` as follows, in order to avoid 404 errors during `"apt-get update"`. Replace `CODENAME` with the codename of Ubuntu release that you are using.