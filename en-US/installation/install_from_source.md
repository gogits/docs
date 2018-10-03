---
name: From source
---

# Install from source

## Dependencies

### Overall

- [Go Programming Language](http://golang.org): Version >= 1.7

We are going to create a new user called `git` and install/setup everything under that user:

```sh
$ sudo adduser --disabled-login --gecos 'Gogs' git
```

## Installing Go

If your system's Go matches the requirements, please skip this section.

### Download

Install Go in `/home/git/local/go` so it wouldn't interfere with future updates of your system's package manager:

```sh
$ sudo su - git
$ cd ~
# create a folder to install 'go'
$ mkdir local
# Download go (change go$VERSION.$OS-$ARCH.tar.gz to the latest release)
$ wget https://storage.googleapis.com/golang/go$VERSION.$OS-$ARCH.tar.gz
# expand it to ~/local
$ tar -C /home/git/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```

### Set Up the Environment

Set the paths that correspond to your system:

```sh
$ sudo su - git
$ cd ~
$ echo 'export GOROOT=$HOME/local/go' >> $HOME/.bashrc
$ echo 'export GOPATH=$HOME/go' >> $HOME/.bashrc
$ echo 'export PATH=$PATH:$GOROOT/bin:$GOPATH/bin' >> $HOME/.bashrc
$ source $HOME/.bashrc
```

## Install Gogs

The general way to install Gogs:

```sh
# Download and install dependencies
$ go get -u github.com/gogs/gogs

# Build main program
$ cd $GOPATH/src/github.com/gogs/gogs
$ go build
```

### Build from `develop` branch

In case you want to try `develop` (or any other) branch:

```sh
$ mkdir -p $GOPATH/src/github.com/gogs
$ cd $GOPATH/src/github.com/gogs

# Make sure you don't use "https://github.com/gogs/gogs.git"
$ git clone --depth=1 -b develop https://github.com/gogs/gogs
$ cd gogs
$ go build
```

### Test Installation

To make sure Gogs is working:

```sh
$ cd $GOPATH/src/github.com/gogs/gogs
$ ./gogs web
```

If you do not see any error messages, hit `Ctrl-C` to stop Gogs.

### Build with Tags

A couple of things do not come with Gogs automatically, you need to compile Gogs with corresponding [build tags](https://golang.org/pkg/go/build/#hdr-Build_Constraints).

Available build tags are:

- `sqlite`: SQLite3 database support
- `pam`: PAM authentication support
- `cert`: Generate self-signed certificates support
- `miniwinsvc`: Builtin windows service support (or you can use NSSM to create a service)

For example, you want to support all of them, first delete directory `$GOPATH/pkg/${GOOS}_${GOARCH}/github.com/gogs/gogs` and then do:

```sh
$ go get -u -tags "sqlite pam cert" github.com/gogs/gogs
$ cd $GOPATH/src/github.com/gogs/gogs
$ go build -tags "sqlite pam cert"
```

If you get error: `fatal error: security/pam_appl.h: No such file or directory`, then install missing package via `sudo apt-get install libpam0g-dev`.

## Next steps

- See [Configuration and run](/docs/installation/configuration_and_run) to go further.
- For more detailed instructions including setting webserver and database see [the detailed steps](/docs/advanced/configuration_for_source_builds).
