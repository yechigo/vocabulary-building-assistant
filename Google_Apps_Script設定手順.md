# Google Apps Script 設定手順

この設定を1回行えば、アプリが常に最新のスプレッドシートデータを自動取得できるようになります。

## 手順1: スプレッドシートでスクリプトエディタを開く

1. スプレッドシートを開く：
   https://docs.google.com/spreadsheets/d/1YpyunAYU_Ek_p0AQeGXx1tklNl8zi5ZnpQtpZdNYnm4/edit

2. メニューから **「拡張機能」→「Apps Script」** をクリック

## 手順2: スクリプトを貼り付け

1. 開いたエディタに、既存のコード（`function myFunction() {}`）を全て削除

2. 以下のコードを貼り付け：

```javascript
function doGet(e) {
  try {
    // スプレッドシートを取得
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

    // 全データを取得
    const data = sheet.getDataRange().getValues();

    // CSVフォーマットに変換
    const csv = data.map(row => {
      return row.map(cell => {
        // セルに改行やカンマが含まれる場合、ダブルクォートで囲む
        const cellString = String(cell);
        if (cellString.includes(',') || cellString.includes('"') || cellString.includes('\n')) {
          return '"' + cellString.replace(/"/g, '""') + '"';
        }
        return cellString;
      }).join(',');
    }).join('\n');

    // CORS対応のヘッダーを設定してCSVを返す
    return ContentService
      .createTextOutput(csv)
      .setMimeType(ContentService.MimeType.TEXT);

  } catch (error) {
    return ContentService
      .createTextOutput('Error: ' + error.toString())
      .setMimeType(ContentService.MimeType.TEXT);
  }
}
```

3. **「保存」** ボタン（💾アイコン）をクリック
   - プロジェクト名を聞かれたら「単語リストAPI」などの名前を入力

## 手順3: デプロイ（公開）

1. 右上の **「デプロイ」→「新しいデプロイ」** をクリック

2. 「種類の選択」（歯車アイコン）をクリック → **「ウェブアプリ」** を選択

3. 以下のように設定：
   - **説明**: 「CSV API」（任意）
   - **次のユーザーとして実行**: 「自分」
   - **アクセスできるユーザー**: **「全員」** ← 重要！

4. **「デプロイ」** ボタンをクリック

5. 権限の確認が表示される場合：
   - 「アクセスを承認」をクリック
   - Googleアカウントを選択
   - 「詳細」→「（プロジェクト名）に移動（安全ではないページ）」をクリック
   - 「許可」をクリック

## 手順4: URLをコピー

1. デプロイ完了後、**「ウェブアプリのURL」** が表示されます

2. このURLをコピー（例）：
   ```
   https://script.google.com/macros/s/AKfycby.../exec
   ```

3. このURLを**メモ帳などに保存**してください

## 手順5: アプリに設定

1. `english-learning-app.html` を開く

2. ファイルの最初の方（220行目付近）にある以下の部分を見つけて、URLを貼り付け：

```javascript
// Google Apps ScriptのデプロイURL（ここに貼り付け）
const APPS_SCRIPT_URL = 'あなたのURLをここに貼り付け';
```

例：
```javascript
const APPS_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycby.../exec';
```

## 完了！

これで設定完了です。アプリを開くと、自動的に最新のスプレッドシートデータが読み込まれます。

スプレッドシートを更新したら、アプリをリロードするだけで最新データが反映されます。

## トラブルシューティング

### エラーが出る場合

1. Apps Scriptの「アクセスできるユーザー」が **「全員」** になっているか確認
2. デプロイ時に権限を許可したか確認
3. URLが正しくコピーされているか確認（最後の `/exec` まで含む）

### データが更新されない場合

- ブラウザのキャッシュをクリアするか、シークレットモードで開いてみる
