# ゼロから始めるAIサイト制作の初期セットアップ

※ここでは**非エンジニア向け**に、AIサイト制作を行うための**最初の環境構築**だけを簡潔に解説します。

---

## 1. Git インストール

### 1-1. インストール手順

- 次のQiita記事に従って、**「手順3 Gitの設定」まで**実施してください。  
  - https://qiita.com/takeru-hirai/items/4fbe6593d42f9a844b1c

### 1-2. WSLユーザーの注意（WindowsでWSLを使う人）

- プロジェクトは **Linux側（Ubuntu）のディレクトリ** で扱うのが安全です。
  - エクスプローラーのサイドバーで：
    - 「Linux」 → 「Ubuntu」 → 「home」 → 自分のユーザー名  
  - アドレス欄の先頭に **`Linux` / `Ubuntu`** と表示されていることを確認してください。

### 1-3. Gitのユーザー設定

Qiita記事の「手順3: Gitの設定」まで来たら、ターミナルで次を実行します。

```bash
git config --global user.name "propagate1"
git config --global user.email "info@propagateinc.com"
```

> パスワード入力時に画面に何も表示されないのは**正常**で、ちゃんと入力されています。

### 1-4. Gitインストール確認

以下のコマンドを実行し、バージョンが表示されればOKです。

```bash
git --version
```

---

## 2. Cursor のインストール概要

**流れ**

1. パッケージ管理・パフォーマンス向上のため、**Node.js と npm** をインストール
2. **Cursor エディタ本体**をインストール

---

## 3. Node.js と npm のインストール

### 3-1. mac の人

1. Node.js公式サイトへアクセス  
   - https://nodejs.org/ja
2. **LTS版（長期サポート版）の「macOS Installer (.pkg)」** をダウンロード
3. ダウンロードした `.pkg` をダブルクリックしてインストール

### 3-2. WSL の人（Ubuntu）

1. スタートメニューから「WSL」または「Ubuntu」を開く
2. 以下のコマンドを1行ずつ実行

```bash
sudo apt update
sudo apt install nodejs npm
```

### 3-3. インストール確認（共通）

Node.js と npm が入っているか、バージョン表示で確認します。

```bash
# Node.js のインストール確認
node -v

# npm のインストール確認
npm -v
```

どちらも **バージョンが表示されれば成功** です。

---

## 4. Cursor のインストール

1. 公式サイトから Cursor をダウンロード  
   - https://cursor.com/ja/download
   - Windows → **Windows(x64) (ユーザー)** を選択（Windowsの人）
   - Mac → Mac → **macOS（Apple Silicon / Intel）用インストーラー** を選択してダウンロードする
（ほとんどのMacはApple Siliconなので，通常は「macOS（Apple Silicon）」を選べばよい）
2. インストーラーを **WSL内のファイルシステムに保存して実行** してインストールすることを推奨
3. Git のリポジトリ（プロジェクトフォルダ）は、**Windows内のフォルダ** で運用する方針であれば、その場所に作成する

> 運用方針はチームルールに合わせてください。  
> （一般的には「WSL内にプロジェクトを置き、Linux側で開発」が高速でトラブルも少ないです）

#### Cursorインストール後に確認すること

Cursor のターミナル（内蔵ターミナル）を開いて、次の2点を確認します。

- `wsl` コマンドまたは Ubuntu シェルが使えること
- `git --version` が実行できること

どちらも問題なく動けば、Cursor からWSLとGitが使える状態です。
