# How to install and use Homebrew on Mac

I use Homebrew to install dev related CLI and GUI apps on my Mac machine.

To install Homebrew; run
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

To install packages (CLI apps):
```shell
brew install [package name]
```

To install GUI apps
```shell
brew install --cask [package name]
```
To upgrade a package
```shell
brew update
brew doctor
brew upgrade [package]
brew cleanup
```

To view info about a package
```shell
brew info [package name]
```

To remove package
```shell
brew remove [package name]
```

To view package dependencies
```shell
brew deps [package name]
```

To force delete all versions of a package
```shell
brew uninstall [package name] -f
```

To ignore deletion of package dependencies
```
brew uninstall --ignore-dependencies [package name]
```
