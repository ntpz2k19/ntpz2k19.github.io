---
layout: post
title: zsh powerlevel10k
---

Zsh adalah shell unix hasil penyempurnaan dari bash. Zsh sendiri memiliki fitur yang lebih luas daripada bash, salah satunya adalah bisa menggunakan auto completition. Untuk saat ini juga shell favorit saya adalah zsh karena tampilanya lebih menarik daripada bash walaupun itu tidak begitu berpengaruh.

Disini saya akan melakukan instalasi zsh dengan tema favorit saya yaitu powerlevel10k dengan plugin git,autosuggestion,syntaxhighlighting, dan fast syntaxhighlighting.

Pertama-tama install zsh sesuai package manager masing-masing:
```bash
# untuk archlinux-dan-turunanya
$ sudo pacman -S zsh
# untuk debian-dan-turunanya
$ sudo apt-get install zsh
# untuk Redhat-dan-turunanya
$ sudo dnf install zsh
```
Setelah instalasi zsh langkah berikutnya adalah menginstall oh-my-zsh dan powerlevel10k.

pilih salah satu langkah instalasi:
```bash
# dengan curl
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# dengan wget
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# dengan fetch
sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Pastikan user terdaftar menggunakan zsh dengan perintah `$ grep $USER /etc/passwd` 

Lalu langkah selanjutnya adalah instalasi powerlevel10k.

Sebelum instalasi, install beberapa font yang diperlukan, untuk pengguna archlinux cukup install package dari aur, cukup dengan perintah `$ yay -S ttf-meslo-nerd-font-powerlevel10k`, namun untuk pengguna distro lain bisa download dan install manual di [sini](https://github.com/romkatv/powerlevel10k#fonts), lalu pindahkan file fonts di /usr/share/fonts/TTF .

Setelah melakukan instalasi fonts, install powerlevel10k dengan perintah `git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`

Lalu coba exit terminal dan buka kembali, maka kita akan disodori setup powerlevel10k, pastikan semua font terbaca, lalu pilih tema mana yang menurut anda suka, kalau untuk saya, saya lebih memilih tema lean dengan encoding ASCII agar bisa terbaca juga di tty.

Setelah installasi powerlevel10k, kita bisa menginstall tema zsh.

```bash
# install tema autosuggestion
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# install tema syntax highlighting
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
# install tema fast syntax highlighting
$ git clone https://github.com/zdharma-continuum/fast-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/fast-syntax-highlighting
```
Lalu edit file .zshrc, tambahkan di bagian plugin sehingga seperti berikut:

`ZSH_THEME="powerlevel10k/powerlevel10k"`

`plugins=(git zsh-autosuggestions zsh-syntax-highlighting fast-syntax-highlighting)`


saus:

[wikipedia.org](https://en.wikipedia.org/wiki/Z_shell)

[github.com oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)

[github.com zsh-fast-syntax](https://github.com/zdharma-continuum/fast-syntax-highlighting#installation)

[github.com zsh-powerlevel10k](https://gist.github.com/minhanhhere/4a124522b2931dd47fa0aed56ad9843e)
