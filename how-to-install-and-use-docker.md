# How to install Docker on a Mac

- Will be using Homebrew to install Docker instead of downloading package from the internet
- Make sure Homebrew is installed, if not refer to [this guide](https://github.com/t0mclaudio/starting-software-project/blob/master/how-to-install-and-use-homebrew.md) for notes on how to use Homebrew

  To install Docker on Mac; run this command
  ```shell
  brew install --cask docker
  ```
  Refer to Homebrew guide to differentiate between using `brew install` and `brew cask install`; but the short answer is the former installs CLI apps while the latter installs GUI apps

  ## How to use docker
  ```shell
  docker --version
  ```

  
