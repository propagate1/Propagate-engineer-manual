# WSL2とUbuntu開発環境構築マニュアル

※macの人は「GitとCursorのインストール方法」まで飛ばしてOK  
（この文書ではそこまでの前提として「Linux環境＝WSL2上のUbuntu」を想定）

---

## はじめに

- **Windowsユーザー**
  - WSL2を入れることで「macやLinuxに近い開発環境」を手に入れる
  - Webサーバーと近いLinux環境をローカルで再現できる
- **macユーザー**
  - もともとUNIX系（Linuxに近い）なのでWSLは不要

> 以降、「Windowsユーザー・Linuxユーザー・WSLユーザー」は  
> **Linux環境を持っている人**として同じ意味で扱います。

---

## WSL2とは ― Web開発における優位性

WSL2（Windows Subsystem for Linux 2）は、**Windows上で本物のLinuxカーネルを動かす仕組み**です。

### ⚙️ 1. 本物のLinux環境

- 実際のLinuxカーネルを使うため、
  - `Python` / `Node.js` / `Ruby` / `Go` などのツールがそのまま動く
  - 本番サーバー（Linux）とほぼ同じ環境で開発できる
- 「ローカルでは動くのに本番でエラー」のズレを減らせる

### ⚡ 2. 高速なパフォーマンス

- WSL1に比べて**ファイルI/Oが大幅に高速**
  - `git clone`, `npm install`, `apt update` などが **2〜5倍（最大20倍）速い**
- 大量ファイルを扱うWeb開発でストレスを減らせる

### 🌍 3. サーバーとの環境差をなくす

- 多くのWebサーバーはLinux
- WSL2上で開発することで、
  - OSによる挙動の違いを最小化
  - ライブラリやコマンドの違いによるトラブルを減らす

### 🔄 4. Windowsとの連携

- WSLからWindowsのドライブにアクセス可能（`/mnt/c` など）
- Linux上で立ち上げたサーバー（例：ポート3000）に  
  Windows側ブラウザから `http://localhost:3000` でアクセスできる

### 🚀 5. 軽量で起動が速い

- 従来の仮想マシンより軽く、数秒で起動
- 必要なときだけ立ち上げればよいので、PCのリソースを圧迫しにくい

---

## Linuxとは

### 🧠 Linux環境を使う意味

- オープンソースのOS
- 多くのサーバー・クラウド・組み込み機器・開発環境で利用されている
- 特徴：
  - 自由に使える（オープンソース）
  - 軽量
  - カスタマイズ性が高い

### ⚙️ 1. 開発・運用の標準環境

- 世界中のWebサーバーの多くがLinux
- Linuxで開発すると**本番環境に近い検証**ができる
- CLI文化が発達しており、自動化・スクリプト化に向いている

### 🔒 2. 安定性と信頼性

- 長期間稼働しても落ちにくい
- サーバー用途を前提にした設計で、メモリ管理も安定
- AWS / GCP / Azure などのクラウドでもLinuxが主流

### 💰 3. 無料で使える

- OSライセンス費用が不要
- ソースコードも公開されており、カスタマイズ・再配布も可能

### 🔧 4. 軽量で古いPCでも動く

- 軽量ディストリビューションも豊富
- 教育・IoT（Raspberry Piなど）でも広く利用

### 主なディストリビューションのざっくり特徴

- **Ubuntu**：汎用的で使いやすい → 本マニュアルではこれを使用
- Debian：安定性重視
- Arch Linux：最新技術志向
- Alpine：超軽量、Docker向き

### 🧩 5. カスタマイズ性

- 不要な機能を削る・セキュリティ強化・開発用に最適化など  
  自分仕様の環境を作りやすい

### 🌍 6. 学習効果

- OS・ネットワーク・サーバー構築などインフラ寄りの知識が身につく
- オープンソースコミュニティへの参加もしやすい

---

## システム要件とバージョン確認（WSL2利用前）

### 最小要件（ざっくり）

- **Windows 10**：Version 2004 / Build 19041 以降（推奨）
- **Windows 11**：すべてのバージョンでWSL2対応
- **ハードウェア**
  - CPU仮想化機能が有効（Intel VT-x / AMD-V など）
  - BIOS/UEFIで仮想化がONになっていること

### Windowsのバージョン確認

- `Win + R` → `winver` と入力
- または 設定 → システム → バージョン情報

```powershell
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
```

---

## WSL2の有効化とインストール

### ✅ 一番簡単なインストール方法（推奨）

1. **PowerShellを管理者として起動**
2. 次のコマンドを実行：

```powershell
wsl --install
```

- これで以下がまとめて実行される：
  - WSLに必要なWindows機能の有効化
  - WSL2用Linuxカーネルのインストール
  - デフォルトLinux（Ubuntu）のインストール
  - WSL2をデフォルトバージョンに設定

3. 再起動後、Ubuntuが初回起動して、**Linuxユーザー名とパスワード**を設定する

### 特定のディストリビューションを指定する場合

```powershell
wsl --list --online      # 利用可能なディストロ一覧
wsl --install -d Ubuntu-24.04
```

---

### 手動セットアップ（古いWindows向け・必要な人だけ）

※詳細は必要なときに公式ドキュメントを参照すればOK。  
要点だけ：

1. WSL機能を有効化
2. Virtual Machine Platformを有効化
3. 再起動
4. Linuxカーネル更新MSIを実行
5. `wsl --set-default-version 2` を実行

---

## Ubuntuのインストールと初期設定

### 初回起動時のポイント

- 起動するとユーザー作成ウィザードが走る：

```text
Enter new UNIX username: developer
New password:
Retype new password:
```

- ユーザー名は**小文字推奨**・スペースや記号なし
- パスワード入力時は何も表示されないが正常
- 作成したユーザーは `sudo` 権限を持つ

### インストール済みディストロの確認

```powershell
wsl -l -v
```

例：

```text
  NAME            STATE    VERSION
* Ubuntu-24.04    Running  2
  Ubuntu-22.04    Stopped  2
```

### WSL1→WSL2への変換

```powershell
wsl --set-version Ubuntu 2
```

---

## 基本的な開発ツールのセットアップ（Ubuntu側）

### 1. まずはシステム更新

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2. Python3 & pip3

```bash
python3 --version
pip3 --version
```

入っていない場合：

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip -y
python3 --version
pip3 --version
```

pipアップデート：

```bash
python3 -m pip install --upgrade pip
```

---

### 3. Gitのインストール & 設定

```bash
sudo apt update
sudo apt install git -y
git --version
```

基本設定（必須）：

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global color.ui auto
```

---

### 4. よく使う開発ツール

```bash
# ビルドツール
sudo apt install build-essential -y

# ダウンロード系
sudo apt install curl wget -y

# エディタ
sudo apt install vim nano -y

# 圧縮
sudo apt install zip unzip -y
```

Node.jsが必要な場合（ざっくり版）：

```bash
sudo apt install nodejs npm -y
node --version
npm --version
```

---

## ファイルシステム連携

### WSLからWindowsのファイルにアクセス

- Windowsドライブは `/mnt/` 以下にマウントされる

```bash
# Cドライブ
cd /mnt/c

# Windowsのユーザーディレクトリ
cd /mnt/c/Users/YourName

# デスクトップの中身を確認
ls /mnt/c/Users/YourName/Desktop
```

- WSL内の今の場所をエクスプローラーで開く：

```bash
explorer.exe .
```

### WindowsからWSLのファイルにアクセス

- エクスプローラーのアドレスバーに：

```text
\\wsl$\Ubuntu\home\username\
```

---

### パフォーマンスの超重要ベストプラクティス

> **「使っているツールと同じOS側にファイルを置く」**

- Linuxツール（`npm`, `python`, `git` など）を使うなら：
  - ✅ `~/projects/...` など **WSL側（Linux）に置く**
  - ❌ `/mnt/c/...`（Windows側）を使うと遅くなる
- Windowsアプリ（VS, Excelなど）をメインで使うなら：
  - ✅ `C:\Users\...` に置き、必要なら `\\wsl$` で参照

---

## ネットワーク統合と localhost

### Linuxで立ち上げたサーバーにWindowsからアクセス

```bash
# WSL内で
python3 -m http.server 8000
```

→ Windowsブラウザから `http://localhost:8000` にアクセス可能

---

## トラブルシューティング（よくあるものだけ）

### 1. 「仮想マシンを起動できませんでした (0x80370102)」

- BIOSで仮想化が無効のパターンが多い
  - BIOS/UEFI設定で「Virtualization」「Intel VT-x」「AMD-V」などを有効化
  - `VirtualMachinePlatform` 機能を有効にして再起動

### 2. WSLがハングした／重い

- 一旦すべて落とす：

```powershell
wsl --shutdown
```

- それでもダメなら「Vmmem」プロセスの負荷をタスクマネージャで確認する

### 3. ネットワークがおかしい

- DNS設定 `/etc/resolv.conf` を確認
- VPN利用時は `.wslconfig` や `dnsTunneling` を活用（必要になった時点で対応）

---

## wsl --shutdown コマンド（よく使う場面）

- 設定変更後に反映したいとき
- WSLが重い／ハングしたとき
- メモリを解放したいとき

```powershell
wsl --shutdown
```

> 注意：実行中のWSL内プロセスは**すべて即時終了**します。

---

## 設定ファイルの概要（必要になったら触る）

### `.wslconfig`（Windows側・全ディストロ共通）

- 場所：`C:\Users\YourName\.wslconfig`
- 主な用途
  - メモリ・CPU・スワップサイズの調整
  - ネットワークモード（NAT / mirrored）
  - GUIアプリON/OFF

※最初からいじる必要はなく、**困ったときに調整するイメージ**でOK。

### `/etc/wsl.conf`（Ubuntu側・そのディストロ専用）

- 主な用途
  - systemdの有効化
  - Windowsドライブのマウントオプション
  - DNS・hostsの自動生成設定
  - デフォルトユーザー設定

---

## Windows Terminalの推奨

- **メリット**
  - タブ・ペイン分割
  - WSL / PowerShell / CMD などを1つのアプリで管理
  - テーマ・フォント・キーバインドなどカスタマイズしやすい

- Microsoft Store から「Windows Terminal」をインストールするだけでOK。

---

## バックアップとリストア（ざっくり）

### バックアップ（エクスポート）

```powershell
wsl --export Ubuntu-20.04 C:\WSL-Backups\ubuntu-2025-01-15.tar
```

### 復元（インポート）

```powershell
wsl --import Ubuntu-20.04 C:\WSL\Ubuntu C:\WSL-Backups\ubuntu-2025-01-15.tar --version 2
```

- 重要な環境は**定期的にtarでエクスポート**しておくと安心

---

## 重要なWSLコマンドのクイックリファレンス

### WSL管理（PowerShellから）

| コマンド | 説明 |
|---------|------|
| `wsl --install` | WSLとデフォルトのUbuntuをインストール |
| `wsl --install -d Ubuntu-24.04` | 指定ディストロをインストール |
| `wsl -l -v` | インストール済みディストロとバージョン一覧 |
| `wsl --list --online` | インストール可能なディストロ一覧 |
| `wsl --set-version Ubuntu 2` | ディストロをWSL2に変換 |
| `wsl --set-default-version 2` | 新規インストールのデフォルトをWSL2に |
| `wsl --set-default Ubuntu` | デフォルトディストロを設定 |
| `wsl --shutdown` | 全WSLを終了 |
| `wsl --terminate Ubuntu` | 特定ディストロを停止 |
| `wsl --unregister Ubuntu` | ディストロを削除 |
| `wsl --status` | WSLの状態を表示 |
| `wsl --update` | WSLを最新に更新 |

---

## Ubuntu必須コマンド（最低これだけ覚えればOK）

| コマンド | 説明 |
|---------|------|
| `pwd` | 現在のディレクトリ表示 |
| `ls` / `ls -la` | ファイル一覧（詳細） |
| `cd directory` | ディレクトリ移動 |
| `cd ~` | ホームに戻る |
| `mkdir name` | ディレクトリ作成 |
| `touch file.txt` | 空ファイル作成 |
| `cat file.txt` | ファイル内容表示 |
| `nano file.txt` | 簡易エディタで編集 |
| `rm file.txt` | ファイル削除 |
| `cp src dest` | ファイルコピー |
| `mv src dest` | 移動／リネーム |
| `sudo apt update` | パッケージリスト更新 |
| `sudo apt upgrade` | パッケージ更新 |
| `sudo apt install package` | パッケージをインストール |
| `exit` | シェルを終了 |

---

## セキュリティ・メンテナンスの最低限

### セキュリティの基本

- `root` で常用しない（通常ユーザー + `sudo`で運用）
- WSL・Ubuntuともにこまめにアップデート：

```bash
sudo apt update && sudo apt upgrade -y
```

```powershell
wsl --update
```

### 定期メンテナンスの目安

- 週1回程度：

```bash
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
```

- WSLが重い/不安定なとき：

```powershell
wsl --shutdown
```

---

## 完全なセットアップワークフロー（要約版）

新しいWSL2 + Ubuntu環境でやることをざっくり順番に並べると：

```bash
# 1. システム更新
sudo apt update && sudo apt upgrade -y

# 2. Python3 & pip3
sudo apt install python3 python3-pip -y

# 3. Git
sudo apt install git -y
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main

# 4. ビルドツール
sudo apt install build-essential -y

# 5. ユーティリティ
sudo apt install curl wget vim nano zip unzip -y

# 6. プロジェクト用ディレクトリ
mkdir -p ~/projects
cd ~/projects

# 7. 簡単なPythonプロジェクトで動作確認
mkdir test-project && cd test-project
python3 -m venv venv
source venv/bin/activate
pip install requests
python3 -c "import requests; print('Success!')"
```

---

## まとめ

- **WSL2 + Ubuntu** で、Windowsでも本番に近いLinux開発環境を構築できる
- 一番重要なポイントは次の2つ：
  1. **使うツールと同じOS側にファイルを置く**（LinuxツールならWSL側）
  2. **定期的なアップデート＆ときどき `wsl --shutdown` でリフレッシュ**

このMarkdownをベースに、  
必要なところだけ詳細を肉付けしていけば、チーム用マニュアルとしても使いやすい構成になります。
