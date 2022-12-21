# Setup your Macbook

Start here to get you mac ready to work on the Cloud. This will help you feel more comfortable and productive when tackling the exercises. Feel free to explore other options and configurations that suits your own working style.

## Get Started

## Shortcut for Visual Studio

A cool feature of visual studio is to be able to open a file from the terminal directly. To install this feature:
- open your `Visual Studio Code` application.
- Press `Command + Shift + P`
- A small text window will open, type in `shell command`
- click on `Shell Command: Install 'code' command in PATH`, press `ok`

- **Step 1 : Homebrew**

Homebrew is a package management system which help in the installation of software on macOS. Basically it is the equivalent of `apt-get` or `yum` on Debian/RHEL systems. [(wiki](https://en.wikipedia.org/wiki/Homebrew_(package_manager)))

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

- **Step 2: xcode**

Xcode is Apple's integrated development environment for macOS, used to develop software for macOS, iOS, iPadOS, watchOS, and tvOS. Good to have as a lot of development tools relies on these libraries. ([doc](https://developer.apple.com/xcode/))

```sh
xcode-select --install
```

- **Step 3: Install git**

Git is the now the most used source-control tool for tracking changes of files from a remote repository. It is essential to master as it is a very powerful collaborative tool to maintain, share and develop source code.

```sh
brew install git
```

## Pimp your shell

Increase your productivity with these tools which allows some cool customizable features. Don't hesitate to explore the plugins that can fit your style and needs. Or create your own ;)

**Be careful as to not break your shell as it can be a pain to restore.**

- **Step 4: ZSH**

Z Shell is an extended `Bourne shell` with some modern improvements including some features coming from features of Bash, ksh and tcsh. ([wiki](https://en.wikipedia.org/wiki/Z_shell))
```sh
brew install zsh
```

- **Step 4: oh-my-zsh**

Oh My ZSH is an open source framework to manage your zsh. Get more info on their [repo here](https://github.com/ohmyzsh/ohmyzsh).

You can install ohmyzsh by running the following command. This will download a script from their repo and execute it.
```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Feel free to explore the different plugins that comes with it and here's a few I recommand. 

To update the plugins for `zsh` you need to open the file `~/.zshrc`.

On your terminal, type:

```sh
code ~/.zshrc
```

```
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
  osx
  ansible
)
```

- **Enabling Plugins (zsh-autosuggestions & zsh-syntax-highlighting)**

 - Download zsh-autosuggestions by
 
 ```
 git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
 ```
 
 - Download zsh-syntax-highlighting by
 
 ```
 git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
 ```

## More utilities

Here are some other utilities we will be using over the course of the journey. I will keep the list updated as we go along the program, feel free to propose some new ones through a pull-request.

```sh
brew install vim    # terminal text-editor
brew install tree   # good to have to list folder hierarchy
brew install jq     # nice tool to output/filter json data
brew install bash   # updated bash version (currently 5.1.8)
```