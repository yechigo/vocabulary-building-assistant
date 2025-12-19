# 英語学習アプリ - 和訳→英文読み上げ

スプレッドシートの和訳と英文を自動で読み上げて英語学習をサポートするWebアプリケーションです。

## 特徴

- **自動更新**: スプレッドシートを更新すると、アプリを開くだけで最新データを自動取得
- **スマホ・PC対応**: ブラウザで動作（Chrome、Safari、Edgeを推奨）
- **プログラミング不要**: 初回設定（5分程度）のみで使用可能
- **カスタマイズ可能**: 和訳→待機→英文の読み上げ間隔や繰り返し回数を調整可能

## 初回設定（1回のみ、5分程度）

### ステップ1: Google Apps Scriptの設定

詳しくは `Google_Apps_Script設定手順.md` を参照してください。

**簡易手順:**

1. スプレッドシートを開く
2. 「拡張機能」→「Apps Script」
3. 以下のコードを貼り付け：

```javascript
function doGet(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = sheet.getDataRange().getValues();
  const csv = data.map(row => row.map(cell => {
    const cellString = String(cell);
    if (cellString.includes(',') || cellString.includes('"') || cellString.includes('\n')) {
      return '"' + cellString.replace(/"/g, '""') + '"';
    }
    return cellString;
  }).join(',')).join('\n');
  return ContentService.createTextOutput(csv).setMimeType(ContentService.MimeType.TEXT);
}
```

4. 「デプロイ」→「新しいデプロイ」→「ウェブアプリ」
5. 「アクセスできるユーザー」を **「全員」** に設定
6. デプロイして、表示されたURLをコピー

### ステップ2: アプリにURLを設定

1. `english-learning-app.html` をテキストエディタで開く
2. 15行目付近の以下の部分を見つける：

```javascript
const APPS_SCRIPT_URL = '';  // ← デプロイしたURLをここに貼り付け
```

3. コピーしたURLを貼り付け：

```javascript
const APPS_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycby.../exec';
```

4. 保存

## 日常の使い方（設定後）

### 1. アプリを開く

1. `english-learning-app.html` をダブルクリック
2. 自動的に最新のスプレッドシートデータを読み込みます

### 2. 学習開始

1. 読み込みが完了したら「学習を開始」ボタンをクリック
2. 自動で和訳→英文の読み上げが始まります

### 3. スプレッドシートを更新した場合

- スプレッドシートでデータを追加・修正
- アプリをリロード（ページを更新）するだけで最新データが反映されます

**それだけです！毎回ファイルをアップロードする必要はありません。**

## 学習の流れ

```
和訳を表示・読み上げ → 待機（設定した秒数） → 英文を表示・読み上げ → 次の項目へ
```

## スマホで使う方法

### iPhone/iPad:
1. `english-learning-app.html` をiCloudやメールで送信
2. Safariで開く
3. ホーム画面に追加すればアプリのように使用可能

### Android:
1. `english-learning-app.html` をGoogle DriveやOneDriveに保存
2. Chrome、Edgeなどで開く
3. メニューから「ホーム画面に追加」でアプリ化可能

## 設定項目

### 列の設定

デフォルト:
- **英文の列**: B
- **和訳の列**: C

スプレッドシートの列構成が異なる場合は、設定画面で変更できます。

### 待機時間

和訳読み上げ後、英文を読み上げるまでの待機時間（デフォルト: 5秒）

### 繰り返し回数

全データを何周繰り返すか（デフォルト: 1回）

## トラブルシューティング

### 「データの取得に失敗しました」と表示される

**原因**: Google Apps Scriptの設定が正しくない可能性があります。

解決方法:
1. Apps ScriptのURLが正しく設定されているか確認
2. Apps Scriptで「アクセスできるユーザー」が **「全員」** になっているか確認
3. ブラウザで直接Apps ScriptのURLにアクセスして、CSVデータが表示されるか確認

### 音声が再生されない

- ブラウザが音声合成に対応しているか確認（Chrome、Safari、Edgeを推奨）
- デバイスの音量設定を確認
- iOSの場合、サイレントモードを解除

### データが読み込まれない

- 「和訳の列」「英文の列」の設定が正しいか確認
- スプレッドシートの該当列にデータが入っているか確認
- 空白行がある場合、その行はスキップされます

## カスタマイズ例

### 列の配置が異なる場合:

スプレッドシートが以下の構成の場合:

| A列（番号） | B列（和訳）         | C列（英文）                  |
|----------|-------------------|------------------------------|
| 1        | おはようございます   | Good morning                 |

設定:
- 和訳の列: B
- 英文の列: C

### 読み上げ速度を変更したい場合:

HTMLファイルの `speak` 関数内の以下の値を変更:
```javascript
utterance.rate = 0.9; // 0.1〜10（小さいほど遅い）
```

## 対応ブラウザ

- Chrome（推奨）
- Safari（iOS/macOS）
- Microsoft Edge
- Firefox

## 注意事項

- インターネット接続が必要です（スプレッドシート読み込み時）
- 音声合成はブラウザの機能を使用します（オフラインでも動作）
- データ量が多い場合、読み込みに時間がかかる場合があります

## ライセンス

このアプリは自由に使用・改変できます。
