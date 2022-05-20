# Floorp ブラウザードキュメント

---
## Floorp のリポジトリ構造について

### 一般公開用リポジトリについて

公開用リポジトリは三つの種類があります


1.　 <a href="https://github.com/floorp-projects/floorp-dev">Floorp-dev</a>：Floorp の一般公衆公開用の Firefox のフォークリポジトリ

2.　 <a href="https://github.com/floorp-projects/Floorp-l10-central"> Floorp-l10-central</a>：Floorp の多言語化をするためのリポジトリ。ただし、例外として`en-US`は含まれていません

3.　 その他のソースコードが含まれるリポジトリ。ツール等

---

重要なのは上の二つです。Floorp-dev は開発者の作業用のリポジトリであり、Floorp 開発者も実際にパッチ作成のために使用します。以下では、<a href="https://github.com/floorp-projects/floorp-dev">Floorp-dev</a> のリポジトリ・ブランチについて解説します。

`Floorp-dev` には3つ（4つ）のブランチがあります。```master```・```release```・```beta```が存在しています。

Linux 版の作成のために一時的に作成されたブランチが含まれることがあります。デフォルトは```beta```に現在なっています

---

```master``` は Firefox でいう Nighly のバージョンであり、Floorp 開発者が今後提供する機能のイメージ構想に使用します。その為、全てのソースコードが```release```に降ってくる訳ではありません。

```beta``` は Floorp の次回リリースされる予定のバージョンです。Firefox のバージョンは Beta・aurora であり、実際にリリースされるコードが含まれていますが、全ての機能が```release``` に適用されるとは限りません。

```release``` は現在リリースされているブラウザーのソースコードであり、Firefox の安定版と同じバージョンです。

---

## Floorp の日本語版・グローバル版を作成する

<strong>重要：最初に、Firefox Nightly のソースコードをビルドし、インストーラー化までをする必要があります。これにより、ビルド環境を整えます。</strong>

Firefox の Windows のビルド方法（英語）：https://firefox-source-docs.mozilla.org/setup/index.html をご覧ください。`./mach run` が終わったら、`./mach package` を実行する必要があります。

---

Floorp の一般公開用ドキュメントには、英語のインストーラー化までのみ書いています。ここでは、日本語版・多言語化の解説が追加されています。ビルドしたいバージョンに合わせてブランチ等を変更してください。


1.　まず最初に、<a href="https://github.com/floorp-projects/floorp">Floorp</a> をクローンし、ソースコードを `bootsrap` します。ターミナルは Mozilla Build Shell を使用してください。

```
git clone https://github.com/floorp-projects/floorp.git

cd floorp

./mach bootstrap

```
Floorp ではアーティファクトビルドは使用しません。常にテストではない限り、`2` を選択するべきです。その他の選択はすべて n(No) を選択してください。

その後、`mozconfig`ファイルをソースコードの一番のディレクトリに追加します。`mozconfig` は Firefox のビルド設定を決める設定ファイルです。作成後、中身に OS に合わせて以下を書き込みます。
```
Windows の場合

ac_add_options --with-branding=browser/branding/official
ac_add_options --with-l10n-base={Floorp の l10n-Central のディレクトリ}
ac_add_options --disable-maintenance-service
ac_add_options --disable-crashreporter
ac_add_options --disable-update-agent
ac_add_options --disable-updater
ac_add_options --enable-bootstrap
ac_add_options --disable-tests
ac_add_options --disable-debug
ac_add_options --disable-verify-mar
ac_add_options --enable-optimize="-O2"
ac_add_options --enable-proxy-bypass-protection
ac_add_options --with-google-safebrowsing-api-keyfile={Floorp 用の Google API のディレクトリ}
MOZ_REQUIRE_SIGNING=
MOZ_TELEMETRY_REPORTING=
MOZ_DATA_REPORTING=

Linux の場合、

ac_add_options --with-branding=browser/branding/official
ac_add_options --with-l10n-base={Floorp の l10n-Central のディレクトリ}
ac_add_options --disable-crashreporter
ac_add_options --enable-bootstrap
ac_add_options --disable-tests
ac_add_options --disable-debug
ac_add_options --disable-verify-mar
ac_add_options --enable-optimize="-O2"
ac_add_options --enable-proxy-bypass-protection
ac_add_options --with-google-safebrowsing-api-keyfile={Floorp 用の Google API のディレクトリ}
MOZ_REQUIRE_SIGNING=
MOZ_DATA_REPORTING=

```

これを書き込んだら、ビルド・テストを実行します。ビルドをする前にウイルス対策アプリの除外リストに Floorp のソースコードのディレクトリと `C:\mozilla-build\` を追加します。

```
cd floorp

./mach build

./mach run

```
### インストーラー化

テストを実行して問題が無い場合、インストーラー化しましょう！！


```
cd floorp-dev

英語版インストーラー作成・インストーラー化

./mach package  または ./mach build installers-en-US


日本語版インストーラー作成

./mach build installers-ja


多言語化

export MOZ_CHROME_MULTILOCALE="ar cs da de el en-GB en-US es-ES es-MX fr hu id it ja ko lt nl nn-NO pl pt-BR pt-PT ru sv-SE th vi zh-CN zh-TW"

AB_CD=multi ./mach package

```

日本語版をビルドすると、再度、`./mach build` した場合の時のみ、`./mach run ` した時インターフェースの文字が消失します。以下の方法で復元可能です。

```
./mach build installers-en-US

./mach build

```

これで完了です。パッチを作成してソースコードを以下の方法で送り付けましょう！

---

## Floorp 向けに書いたパッチを寄稿するには

<strong>重要：テスト・ベータ版用のパッチを寄稿する際には、絶対 `release` にプッシュしないで下さい</strong>

まず最初に、<a href="https://github.com/floorp-projects/floorp">Floorp</a> をクローンします。

```
git clone https://github.com/floorp-projects/floorp.git
```

その後、ブランチが`beta`であることを確認してください。`release` は使用しません。

```
git branch
```

Floorp のソースコードを改造しましょう！以下は Floorp のソースコード形態についてのドキュメントです。

Floorp はアドオン・UI 部分・アドオンの実装部分・その他の Firefox の実装部分を改造して作成されています。改変場所についての解説です。また、 Firefox はよくソースコードの構造変えたがるのであてにならない場合あることに注意。

```

browser/branding
Floorp のブランディング。このソースコードは MPL2.0 でライセンスされていないので、認めていないリリースの場合での使用は認められません。

browser/extensions
組み込みのシステムアドオンが入っています。Floorp アップデーター等もここにあります。

browser/locales
en-US のローカライズが入っています。上記の通り、その他の言語は別にあります。preferences サイト等の改変時にはここの改変も必要です。

browser/themes
Floorp ブラウザの外観のソースコードが含まれています。ソースコードは Materiafox に基づきますが、browser.shared.css や tab.css などにソースコードが分けられています。

toolkit/modules
GTKテーマの実装等 etc...

toolkit/themes
ブラウザー内部サイトのデザインの実装。about:support から about:config まで結構グローバル。

toolkit/mozapps/extensions/default-theme
システムテーマの実装。

```
