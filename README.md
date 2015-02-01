ansibleによるmac環境構築

# 概要
初期状態のmacのセットアップのためのplaybook

# 手順
1. x-codeインストール

```
xcode-select --install
```

2.Homwbrew, python, ansibleインストール

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
brew install python
brew install ansible
```

3.Homebrew, Caskパッケージ管理用のAnsible roleをダウンロード
ここでは~/ansible-workを作業ディレクトリとします。

```
mkdir ~/ansible-works
cd ~/ansible-works
ansible-galaxy install --roles-path=. hnakamur.homebrew-packages
ansible-galaxy install --roles-path=. hnakamur.homebrew-cask-packages
```

4.インベントリファイルの作成
Homebrewでインストールした場合は/usr/local/etc/ansible/hostsがインベントリファイルになります。

```
mkdir -p /usr/local/etc/ansible
echo localhost > /usr/local/etc/ansible/hosts
```

5.playbookの作成
以下参考

```localhost.yml
- hosts: localhost
  connection: local
  gather_facts: no           
  sudo: no
  vars:
    homebrew_packages_taps:
      - homebrew/binary
      - homebrew/dupes
    homebrew_packages_packages:
      - { name: ansible }
      - { name: git }
      - { name: python }
      - { name: vim }
      - { name: wget }
      - { name: bash }
    homebrew_cask_packages_packages:
      - firefox
      - google-chrome
      - heroku-toolbelt
      - kobito
      - macvim
      - coteditor
      - github
      - sourcetree
      - iterm2
      - skype
      - vagrant
      - virtualbox
      - xquartz
  roles:
    - hnakamur.homebrew-packages
    - hnakamur.homebrew-cask-packages
```

6.ansible実行

```
ansible-playbook localhost.yml
```

#補足
* dotfiles(https://github.com/aboutNY/dotfiles.git)
* [任意]bashprofileに以下を挿入

```bash_profile
export HOMEBREW_CASK_OPTS="--appdir=/Applications"
```
