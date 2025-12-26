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

## 4. SSHキーの作成（Windows）

Windowsの場合、GitHubとの連携のためにSSHキーを作成します。

Git Bashを開いて以下のコマンドを実行：

```bash
ssh-keygen
```

以下のように表示されたら、何も入力せずに **Enter** を3回押してください（デフォルト設定でOK）：

```
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/ユーザー名/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

---

## 5. Gitインストール確認

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

