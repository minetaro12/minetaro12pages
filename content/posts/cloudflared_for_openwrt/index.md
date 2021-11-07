+++
date = 2021-11-07T17:31:00+09:00
tags = [linux", "cloudflare", "openwrt"]
title = "OpenWrtでCloudflaredを動かしてみる"

+++
ルーター上でCloudflaredを動かしてみたかったのでやってみた

## ビルド方法

1\.  OpenWrtのSDKをダウンロードして解凍

[ここ](https://downloads.openwrt.org/)からターゲットのSDKをダウンロードして解凍する

自分はGL-MT300N-V2で動かしたいので[https://archive.openwrt.org/releases/21.02.0/targets/ramips/mt76x8/openwrt-sdk-21.02.0-ramips-mt76x8_gcc-8.4.0_musl.Linux-x86_64.tar.xz](https://archive.openwrt.org/releases/21.02.0/targets/ramips/mt76x8/openwrt-sdk-21.02.0-ramips-mt76x8_gcc-8.4.0_musl.Linux-x86_64.tar.xz)をダウンロードした

2\. リポジトリをクローン

`packages`内に[これ](https://github.com/minetaro12/openwrt-cloudflared)を`git clone`

3\. ビルドの準備

次のコマンドを実行してパッケージをインストールする

```
./scripts/feeds update -a
./scripts/feeds install -a
```

`make menuconfig`を実行して`Extra packages`内の`openwrt-cloudflared`にスペースで*を入れる

4\. ビルド

次のコマンドでビルドする

`make ./package/openwrt-cloudflared/compile`

終わると`bin`内にパッケージができる

## 動かす

バイナリが20MBくらいあるので、exrootで拡張するかパッケージを解凍してtmp等別の場所に入れてうごかすことをおすすめします。

自分は`/etc/init.d/cloudflared`を作成して、起動時にバイナリをGithubからダウンロードするようにしました

デーモン化する場合は`/root/.cloudflared`の中身を`/etc/cloudflared`にコピーする必要があります。

```
#!/bin/sh /etc/rc.common

START=99
STOP=15

start() {
        # commands to launch application
        if [ ! -e /tmp/cloudflared ];then
                wget https://github.com/minetaro12/openwrt-cloudflared/releases/download/2021.11.0/cloudflared-linux-ramips -O /tmp/cloudflared
        fi
        chmod +x /tmp/cloudflared
        tmux new-session -s cloudflared-ssh -d "/tmp/cloudflared --hostname hogehoge.com --url ssh://localhost:22"
}

stop() {
        # commands to kill application
        tmux send-keys -t cloudflared-ssh C-c
        sleep 5
}
```