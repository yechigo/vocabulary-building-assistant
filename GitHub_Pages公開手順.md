# GitHub Pagesで英語学習アプリを公開する手順

このアプリをインターネット上に公開し、どこからでもアクセスできるようにします。

## 前提条件

- GitHubアカウント（持っていない場合は https://github.com で無料作成）
- 完成したアプリ: `english-learning-app.html`

## 手順1: GitHubで新しいリポジトリを作成

1. **GitHub.com にログイン** https://github.com

2. 右上の **「+」ボタン** → **「New repository」** をクリック

3. 以下のように入力：
   - **Repository name**: `vocabulary-learning-app`（または好きな名前）
   - **Description**: （任意）「English learning app with text-to-speech」など
   - **Public** を選択 ← 重要！（無料でGitHub Pagesを使うため）
   - **「Add a README file」のチェックは外す** ← 重要！
   - **「Add .gitignore」は「None」のまま**
   - **「Choose a license」は「None」のまま**

4. **「Create repository」** をクリック

5. 作成後、以下のような画面が表示されます：
   ```
   Quick setup — if you've done this kind of thing before
   https://github.com/あなたのユーザー名/vocabulary-learning-app.git
   ```
   このURLをメモしておく（後で使います）

## 手順2: ファイルをGitHubにアップロード

### 方法A: ブラウザから直接アップロード（簡単・推奨）

1. リポジトリのページで **「uploading an existing file」** のリンクをクリック

2. 以下のファイルをドラッグ&ドロップ：
   - `english-learning-app.html`
   - `index.html`

3. 下部の「Commit changes」で：
   - Commit message: `Add English learning app`
   - **「Commit changes」** ボタンをクリック

### 方法B: コマンドライン（Git経験者向け）

ターミナルで以下を実行：

```bash
cd "/Users/yosukeechigo/Library/Mobile Documents/com~apple~CloudDocs/ws/vocabulary-building-assistant"

# リモートリポジトリを追加（URLは自分のものに変更）
git remote add origin https://github.com/あなたのユーザー名/vocabulary-learning-app.git

# index.htmlを追加してコミット
git add index.html
git commit -m "Add index.html for GitHub Pages"

# GitHubにプッシュ
git branch -M main
git push -u origin main
```

## 手順3: GitHub Pagesを有効化

1. リポジトリのページで **「Settings」** タブをクリック

2. 左側のメニューから **「Pages」** をクリック

3. 「Source」セクションで：
   - **Branch**: `main` を選択
   - フォルダは **`/ (root)`** を選択
   - **「Save」** をクリック

4. 数分待つと、ページ上部に以下のようなメッセージが表示されます：
   ```
   Your site is live at https://あなたのユーザー名.github.io/vocabulary-learning-app/
   ```

5. このURLをクリックして、アプリが表示されるか確認

## 完成！

これで、以下のURLでどこからでもアクセスできます：

```
https://あなたのユーザー名.github.io/vocabulary-learning-app/
```

### スマホでの使い方

1. **iPhoneのSafariで上記URLを開く**

2. 画面下部の **共有ボタン**（□に↑）をタップ

3. **「ホーム画面に追加」** を選択

4. 名前を「英語学習」などに変更して **「追加」**

5. ホーム画面にアプリアイコンが追加され、アプリのように使えます

## トラブルシューティング

### ページが表示されない

- GitHub Pagesの反映には数分かかります（最大10分程度）
- ブラウザのキャッシュをクリアして再読み込み

### 404エラーが出る

- `index.html` がリポジトリのルートにあるか確認
- GitHub Pagesの設定で Branch が `main` になっているか確認

### アプリは開くがデータが読み込めない

- Google Apps ScriptのURLが正しく設定されているか確認
- Apps Scriptで「アクセスできるユーザー」が「全員」になっているか確認

## データの更新

スプレッドシートのデータを更新した場合：

1. アプリをリロード（ブラウザの更新ボタン）
2. 自動的に最新データが読み込まれます

## アプリの更新

`english-learning-app.html` を修正した場合：

1. GitHubのリポジトリページを開く
2. `english-learning-app.html` をクリック
3. 鉛筆アイコン（Edit）をクリック
4. 内容を更新
5. 「Commit changes」をクリック
6. 数分後に変更が反映されます

---

## 参考情報

- 公開URL形式: `https://ユーザー名.github.io/リポジトリ名/`
- GitHub Pages ドキュメント: https://docs.github.com/ja/pages
- 費用: 完全無料（パブリックリポジトリの場合）

## 注意事項

- **Google Apps ScriptのURLが含まれているため、リポジトリは必ずPublicにしてください**
- セキュリティ上問題がある情報（パスワードなど）は含めないでください
- スプレッドシート自体は公開する必要はありません（Apps Script経由でのみアクセス）
