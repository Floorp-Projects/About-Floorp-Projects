## Floorp for Linux のインストールはこちら。
**ターミナル（端末）に貼り付けてください。**

**(y/n)の選択が出てきた場合、y を選択してください**\
*flatpak*, *curl* が**必要**ですので最初にインストールします。

#### 前提パッケージのインストール
##### Debian / Ubuntu の場合
```sh
sudo apt install -y flatpak curl
```
##### Red Hat 系(AlmaLinuxなど)の場合
```sh
sudo dnf install -y flatpak curl
```
#### Floorp for Linux のインストール
```
curl https://sda1.net/storage/floorp/floorp_install.sh | sudo bash && curl https://sda1.net/storage/floorp/floorp_install.sh | sudo bash
```
**完了後、再起動をすることをお勧めします。**\
アプリ一覧に表示されない場合があるからです。

**また、既定のブラウザへの設定が動作しない場合があります。**\
Linux の設定(ex.お気に入りのアプリケーション)から、既定のブラウザを変更してみてください。\
設定から既定のブラウザの確認をオフにできます。
