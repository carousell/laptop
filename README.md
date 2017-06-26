Laptop
======

Laptop is a script to set up an macOS laptop for development at Carousell. This
is a fork of thoughtbot's [laptop](https://github.com/thoughtbot/laptop)
project with local customization for Carousell's environment. It is less
opinioniated and can be customized to support platform defaults (eg. Android
Studio for software engineers working on the Android platform).

It can be run multiple times on the same machine safely.  It installs,
upgrades, or skips packages based on what is already installed on the machine.

Requirements
------------

We support:

* macOS Yosemite (10.10)
* macOS El Capitan (10.11)
* macOS Sierra (10.12)

Older versions may work but aren't regularly tested. Bug reports for older
versions are welcome.

Install
-------

Install Homebrew (Cask), Slack, Chrome and Encrypt the hard disk:

```sh
curl --remote-name https://raw.githubusercontent.com/carousell/laptop/master/mac
sh mac -encrypt
```

### Engineering Setup

```
sh mac -dev 2>&1 | tee ~/laptop.log
```

### For Android Development

You can pass the `-android` parameter to the `mac` script
to install Android Studio and other libraries required
for Android development.

```
sh mac -dev -android 2>&1 | tee ~/laptop.log
```

### For iOS Development

```sh
sh mac -dev -ios 2>&1 | tee ~/laptop.log
```

Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/carousell/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

What it sets up
---------------

### General Install for Everyone:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Apps:

* Google Chrome
* Slack

### Engineering Setup

Unix tools:

* [Git] for version control
* [OpenSSL] for Transport Layer Security (TLS)
* [Tmux] for saving project state and switching between projects

[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[Tmux]: http://tmux.github.io/

Programming languages and configuration:

* [Node.js] and [NPM], for running apps and installing JavaScript packages
* [Rbenv] for managing versions of Ruby
* [Ruby Build] for installing Rubies
* [Ruby] stable for writing general-purpose code
* [Java]
* [Python] for most of our tools

[Node.js]: http://nodejs.org/
[NPM]: https://www.npmjs.org/
[Rbenv]: https://github.com/sstephenson/rbenv
[Ruby Build]: https://github.com/sstephenson/ruby-build
[Ruby]: https://www.ruby-lang.org/en/
[Java]: https://java.com/en/download/
[Python]: https://www.python.org/downloads/

Apps:
* [Tunnelblick] for VPN access
* [iTerm2] for a better Terminal

[Tunnelblick]: https://tunnelblick.net/
[iTerm2]: https://www.iterm2.com/


Android specific:

* [Android Studio]

[Android Studio]: https://developer.android.com/studio/index.html

iOS Specific:

* [Carthage]
* [Cocoapods]
* [Fastlane]
* XCode

[Carthage]: https://github.com/Carthage/Carthage
[Cocoapods]: https://cocoapods.org/
[Fastlane]: https://fastlane.tools/

It should take less than 15 minutes to install (depends on your machine).

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.

Contributing
------------

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

[ShellCheck]: http://www.shellcheck.net/about.html
[Syntastic]: https://github.com/scrooloose/syntastic

License
-------

Copyright for portions of project laptop are held by thoughtbot, inc 2011-2017
as part of thoughtbot/laptop. All other copyright for project carousell/laptop
are held by Carousell, 2017. 

It is free software, and may be redistributed under the terms
specified in the [LICENSE] file.

[LICENSE]: LICENSE

