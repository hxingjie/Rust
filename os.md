# OS

## config for Ubuntu

### config ssh

```shell
sudo apt-get install openssh-server // 安装ssh
sudo service ssh start // 启动ssh
sudo ufw disable // 关闭防火墙
sudo service ssh status // 查看状态
sudo apt install net-tools
ifcong // 查看ip

windows -> cmd: ping ip // 检查是否能ping通

windows -> vscode -> install remote-ssh
remote-ssh: config
  Host ubuntu-os
    HostName 192.168.236.134
    User ubuntu-os
    ForwardAgent yes
```

### config github

```shell
使用ssh-keygen -t rsa -b 4096 -C "你的邮箱" 命令，创建ssh key，下面的选项全部直接敲回车即可。
ssh-keygen -t rsa -b 4096 -C wy15219110558@163.com

cat ~/.ssh/id_rsa.pub 命令查看生成的公钥，并完整的复制下来。
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC9uNjMS7k5R4Be8mQfpbo9ImVHP/Bptz2i5DSGeebhR5fNFxG2Zl1GtS+Iyeetpk5u7x9b77bNrLIgvrX4wu/VS7/DoWdSfF9hzXLWIONoAHNkoRU5hog70TC+5HfC2oXL+MX+wNhqu4i3Do4WzMvICTgjCjhzahH7iHpblYQA8yTBynWPZtUVUgeO0n63bCq/PzEAawIR/kSDlaYnnU6S39+m227JaPc6WBK+Tivb9VRHHL1YexXwfTpscglmfG4K/s4BOtSZDQ9MUv7VQ0e6eLg7jizIcx986SMVZjEPASM9fnbH9S0a4q6YwvgQzEFr9JQ2se5QPWOprY6OILOrCp9cLnh0aLpnrkEnx3Kayp2jF8t85jPoaP/ZmkoncaOetIugI3jSqpQ9QCQESyro/qawUgaXktq59ZeADG/mtCJsq0+n7QJtMKrN68HD7ZiNsvtCzipmdCnPdCnAgZ2u1JJooq91ia0Vri0XB471zZWJVx6Zwhv2E0kbXaM/zBGk/k70uoD4sma9Xcn5Y1XiG8hCSj5GqukL/IG+xaVjJcdwNHemi9rupXqKuE19dSD/17s8+pWvey2F2KpuSasi5+XpLBZwRNRFLnzxsal8aDKvTx+oI6zD1byOiDylzhyOzk1DMT6c/n0ZlKwYjFllq40RWMZIzlwKTFGW9Opsdw== wy15219110558@163.com

在github仓库界面点击自己的头像，选择settings。进入到设置页面后，点击左侧的SSH and GPG keys选项。点击New SSH key选项，并将复制下来的内容粘贴上去，添加该ssh key的描述。随后点击Add SSH key，并一路点击确认即可。
```

### config rust

```shell
sudo apt update
sudo apt install curl
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

source ~/.bashrc
rustc -V
cargo -V
```

### git clone

```shell
git clone git@github.com:LearningOS/rust-rustlings-2024-autumn-hxingjie.git
cd rust-rustlings-2024-autumn-hxingjie
cargo install --force --path .
```

### run first code

```shell
cd rust-rustlings-2024-autumn-hxingjie
rustlings run variables1

rustlings watch # 检查是否完成
删除 // I AM NOT DONE, 做下一题
```

### push

```shell
git config user.email wy15219110558@163.com
git config user.name hxingjie
git add .; git commit -m "update"; git push
```



## config for MacOs

```shell
$ ssh-keygen -t rsa -b 4096 -C wy15219110558@163.com
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/hexingjie/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/hexingjie/.ssh/id_rsa
Your public key has been saved in /Users/hexingjie/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:JXTnard6/sBGgF9XLzOzulPMiU+M71c4G6XUUzkKZnU wy15219110558@163.com
The key's randomart image is:
+---[RSA 4096]----+
|        . . o. Eo|
|       . o *  .+o|
|        o = + B.+|
|         + + o.Bo|
|        S + oB.=.|
|         . +oo% .|
|            *= +.|
|           ooo+ .|
|          .oo+o. |
+----[SHA256]-----+

$ cat /Users/hexingjie/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDHkp0kQON2tCIqqGe1EM2+vaOlfg5qVryIJCfLzRsOcXXg3+LemDbl33RjEB5EtMkNabqocogqkY5Vk3JAjoG57ZE2P7gZNEN+aRiacSjJapI6oMSwjvXO8PSorBOLvPDIVvgih3mC+p5vQCEDpoRVczDiZrz62xvfBKfQQ5WCH6QwqiYxmooZ0KmKnxKg0XgeVnRQzLrkK6kC+LBjHvjSm82yB3ayQuUNDyc4wx2I34S7h8EKeGx9pwMlAEt4W0+hVJQXUs3U9m2LkN9j62mKI83djm8j//2/0tMVzUpHHrQ5b5bbZp/6Ts8dE+6gdKmFhgovgLOMRkBD2hy00ruewF4OnAK5WyCQDIWPMQwWRcDrEILp7q7UX7WrJxzRxW74CKkwJKWv1rS+fUdzNOGgxXfYAVHe6nBgOyO/w4e7dyXpTRbRuNUSYyK50ceu9KAmeMKhtnK9o0lGOYWiO++en6E+fKUkr/98AZ4pj+J47FoX9dNZ/kDa5Pht51VYLVCiOGFHSLHAK/jiB0N9IDXM44tPukmik4Qm6KzWntBAVDLvTwAzkCMZ8/P/3U2hHhNUF1/6jwacrFNC0caK9o5mXAUxolmUb4yKqn26jdgW6ARTxYTWdLr3AHnt0J8oyY1FGZcE9Rxp+nW7gxck2bS4LAAhQ3gs4UIpRRmf0+pkTQ== wy15219110558@163.com

$ git clone git@github.com:LearningOS/rust-rustlings-2024-autumn-hxingjie.git
$ cd rust-rustlings-2024-autumn-hxingjie
$ cargo install --force --path .

$ rustlings watch # 检查是否完成

$ git config user.email wy15219110558@163.com
$ git config user.name hxingjie
$ git add .; git commit -m "update"; git push
```

## Ubuntu

```shell
$ sudo apt install update
$ sudo apt install gcc

// 镜像: https://rsproxy.cn

sudo apt install curl
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.bashrc
rustc -V
cargo -V
```

