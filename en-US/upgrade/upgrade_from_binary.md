---
name: From binary
---

# Upgrade from binary

**Downloads are available at [Install from binary](/docs/installation/install_from_binary).**

**Note** If you plan to perform a copy-paste upgrade, make sure you delete the old `templates` directory first!

Find the location where the current installation is:

```bash
# On a default environment this is generally in the Git home folder
$ sudo su - git
$ cd ~
$ pwd
/home/git
$ ls
gogs gogs-repositories
```

Move or rename the existing Gogs folder (but absolutely do not remove it).

```bash
$ mv gogs gogs_old
```

Download and unzip the new binary:

```bash
# Check for the latest release to download based on the OS and ARCH you are running
$ wget https://dl.gogs.io/$VERSION/gogs_${VERSION}_${OS}_${ARCH}.tar.gz
$ tar -zxvf gogs_${VERSION}_${OS}_${ARCH}.tar.gz
$ ls
gogs gogs_old  gogs-repositories gogs_${VERSION}_${OS}_${ARCH}.tar.gz
```

Copy `custom`, `data`, and `log` directories to the unzipped directory:

```bash
$ cp -R gogs_old/{custom,data,logs} gogs
```

Then, run and test in your browser:

```bash
$ cd gogs
$ ./gogs web
```

Well done!

**Note** If you use the bundled `scripts`, make sure they are executable:

```bash
grep -rl '^#!' scripts | xargs chmod +x
```
