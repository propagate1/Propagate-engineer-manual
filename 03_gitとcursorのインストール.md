# 03. Git と Cursor のインストール

※ここでは**非エンジニア向け**に、開発環境の**インストール手順**を簡潔に解説します。

---

# Part 1: Git のインストール

## 1. Git とは？

- **Git**：ファイルやフォルダの「変更履歴」を管理するためのツール
- 「いつ」「誰が」「何を」変えたかを記録し、過去の状態に戻すことができる
- チームでの共同作業には必須のツール

---

## 2. Git のインストール手順

### 2-1. macOS の場合

1. **Homebrewがインストールされている場合**
   - ターミナルを開いて以下のコマンドを実行：
   ```bash
   brew install git
   ```

2. **Homebrewがインストールされていない場合**
   - Xcode Command Line Toolsをインストール：
   ```bash
   xcode-select --install
   ```
   - これによりGitも一緒にインストールされます

### 2-2. Windows の場合

※参考：[Gitのインストール方法(Windows版) - Qiita](https://qiita.com/takeru-hirai/items/4fbe6593d42f9a844b1c)

#### Step 1: インストーラのダウンロード

1. Git公式サイトにアクセス
   - https://git-scm.com/
2. 「**Download**」ボタンをクリック
3. `Git-X.XX.X-64-bit.exe` がダウンロードされます（数分かかる場合があります）

#### Step 2: Gitのインストール

1. ダウンロードしたインストーラ（`Git-X.XX.X-64-bit.exe`）を実行
2. 以下の画面が表示されたら、基本的に「**Next**」をクリックして進めます

**主な設定項目（デフォルトでOK）：**

| 設定項目 | 推奨選択 |
| :--- | :--- |
| デフォルトエディタ | **Use Visual Studio Code as Git's default editor**（または任意） |
| ブランチ名 | **Let Git decide**（デフォルト） |
| PATH設定 | **Git from the command line and also from 3rd-party software** |
| SSH設定 | **Use bundled OpenSSH** |
| HTTPS設定 | **Use the OpenSSL library** |
| 改行コード | **Checkout Windows-style, commit Unix-style line endings** |
| ターミナル | **Use MinTTY (the default terminal of MSYS2)** |
| git pull動作 | **Default (fast-forward or merge)** |
| 認証情報 | **Git Credential Manager** |

3. 最後に「**Install**」をクリックしてインストール完了
4. スタートメニューに「**Git Bash**」が表示されていればインストール成功です

---

## 3. Gitのユーザー設定

インストールが完了したら、ターミナル（Mac）または Git Bash（Windows）で次を実行します。

```bash
git config --global user.name "propagate1"
git config --global user.email "info@propagateinc.com"
```

---

## 4. SSHキーの作成とGitHubへの登録

GitHubとの連携のためにSSHキーを作成し、GitHubに登録します。
これにより、パスワードなしでGitHubからプロジェクトをcloneできるようになります。

### Step 1: SSHキーの作成

ターミナル（Mac）または Git Bash（Windows）を開いて以下のコマンドを実行：

```bash
ssh-keygen
```

以下のように表示されたら、何も入力せずに **Enter** を3回押してください（デフォルト設定でOK）：

```
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/ユーザー名/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

> ※Windowsの場合は `/c/Users/ユーザー名/.ssh/id_rsa` と表示されます

### Step 2: 公開鍵をコピーする

作成したSSHキー（公開鍵）をクリップボードにコピーします。

**macOS の場合：**
```bash
cat ~/.ssh/id_rsa.pub | pbcopy
```

**Windows（Git Bash）の場合：**
```bash
cat ~/.ssh/id_rsa.pub | clip
```

> これで公開鍵がクリップボードにコピーされました。

### Step 3: GitHubにSSHキーを登録する

1. GitHubにログイン
   - https://github.com/
   - アカウント：`info@propagateinc.com`（グーグルでログイン）

2. 右上のプロフィールアイコンをクリック → **Settings** を選択

3. 左メニューから **SSH and GPG keys** をクリック

4. **New SSH key** ボタンをクリック

5. 以下を入力：
   - **Title**: 自分のPC名など分かりやすい名前（例：`Takeru`）
   - **Key type**: Authentication Key（デフォルト）
   - **Key**: 先ほどコピーした公開鍵を貼り付け（`ssh-rsa` で始まる長い文字列）

6. **Add SSH key** をクリック

7. GitHubのパスワードを求められたら入力して完了

### Step 4: SSH接続の確認

SSHキーが正しく登録されたか確認します。

```bash
ssh -T git@github.com
```

初回は以下のように表示されます：
```
The authenticity of host 'github.com (xxx.xxx.xxx.xxx)' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

`yes` と入力してEnterを押してください。

以下のメッセージが表示されれば成功です：
```
Hi propagate1! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 5. GitHubからプロジェクトをcloneする

SSHキーの登録が完了したら、GitHubからプロジェクトをcloneできます。
ここでは、エンジニアマニュアルのリポジトリをcloneします。

### Step 1: Propagateフォルダを作成

ターミナル（Mac）または Git Bash（Windows）で以下を実行：

```bash
# ホームディレクトリに移動
cd ~

# Propagateフォルダを作成
mkdir Propagate

# 作成したフォルダに移動
cd Propagate
```

### Step 2: エンジニアマニュアルをclone

Propagateフォルダ内で以下のコマンドを実行：

```bash
git clone git@github.com:propagate1/Propagate-engineer-manual.git
```

> リポジトリURL: https://github.com/propagate1/Propagate-engineer-manual

### Step 3: cloneの確認

```bash
# cloneしたディレクトリに移動
cd Propagate-engineer-manual

# ファイル一覧を確認
ls
```

以下のようなファイルが表示されればclone成功です：
```
01_slackとアカウントのログイン.md
02_会社説明.md
03_gitとcursorのインストール.md
README.md
```

### フォルダ構成

cloneが完了すると、以下のようなフォルダ構成になります：

```
~/
└── Propagate/                          ← 作成したフォルダ
    └── Propagate-engineer-manual/      ← cloneしたリポジトリ
        ├── 01_slackとアカウントのログイン.md
        ├── 02_会社説明.md
        ├── 03_gitとcursorのインストール.md
        └── README.md
```

> 今後、他のプロジェクトもこの `Propagate` フォルダ内にcloneしていきます。

---

## 6. Gitインストール確認

以下のコマンドを実行し、バージョンが表示されればOKです。

```bash
git --version
```

---

# Part 2: Cursor のインストール

## 1. Cursor とは？

- **コード編集用エディタ（VS Code系）＋ AIアシスタントが一体化したアプリ**
- プログラミングのコードを編集しながら、
  - エラーの原因を聞く
  - 修正案を出してもらう
  - 新しいコードを書いてもらう  
  といったことを **同じ画面で完結できる** のが特徴

---

## 2. インストールの流れ

1. パッケージ管理・パフォーマンス向上のため、**Node.js と npm** をインストール
2. **Cursor エディタ本体**をインストール

---

## 3. Node.js と npm のインストール

### 3-1. macOS の場合

1. Node.js公式サイトへアクセス  
   - https://nodejs.org/ja
2. **LTS版（長期サポート版）の「macOS Installer (.pkg)」** をダウンロード
3. ダウンロードした `.pkg` をダブルクリックしてインストール

### 3-2. Windows（WSL）の場合

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
   - Mac → **macOS（Apple Silicon / Intel）用インストーラー** を選択してダウンロードする
   （ほとんどのMacはApple Siliconなので，通常は「macOS（Apple Silicon）」を選べばよい）
2. インストーラーを実行してインストール

> 運用方針はチームルールに合わせてください。  
> （一般的には「WSL内にプロジェクトを置き、Linux側で開発」が高速でトラブルも少ないです）

---

## 5. Cursorインストール後に確認すること

Cursor のターミナル（内蔵ターミナル）を開いて、次の2点を確認します。

- `git --version` が実行できること
- `node -v` と `npm -v` が実行できること

すべて問題なく動けば、開発環境のセットアップは完了です！

