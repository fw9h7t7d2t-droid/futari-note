# ふたりノート

ふたりの予定と家計簿を共有するWebアプリ。

- アプリ本体：GitHub Pages（無料）
- データの共有・ログイン：Firebase（Firestore＋Googleログイン。ふたりで使う分には無料枠で足ります）

## ファイル

| ファイル | 中身 |
|---|---|
| `index.html` | アプリ本体 |
| `config.js` | Firebaseの設定（**ここだけ書き換える**） |
| `firestore.rules` | データベースのルール（Firebaseに貼り付ける） |
| `manifest.webmanifest`, `icon-*.png` | ホーム画面用のアイコン |

---

## セットアップ手順

### 1. GitHub Pages で公開する
1. GitHubで新しいリポジトリ `futari-note` を作り、このフォルダのファイルを全部アップロード
2. リポジトリの Settings → Pages → Branch を `main` / `(root)` にして保存
3. 数分後に `https://<ユーザー名>.github.io/futari-note/` で開けるようになる（まだログインはできません）

### 2. Firebase を用意する
1. https://console.firebase.google.com で「プロジェクトを追加」（名前例：futari-note、Googleアナリティクスはオフで可）
2. **Webアプリを追加**：プロジェクトの概要 → `</>` → アプリ名を入れて登録 → 表示される `firebaseConfig` をコピー
3. **ログイン**：構築 → Authentication → 始める → ログイン方法で「Google」を有効にする
4. **承認済みドメイン**：Authentication → 設定 → 承認済みドメイン → `<ユーザー名>.github.io` を追加
5. **データベース**：構築 → Firestore Database → データベースを作成 → ロケーション `asia-northeast1（東京）` → 本番環境モードで開始
6. **ルール**：Firestore → ルール タブに `firestore.rules` の中身を貼り付け、メールアドレス2つをけいやと凪のGoogleアカウントに書き換えて「公開」

### 3. config.js を書き換える
GitHub上で `config.js` を編集して、`firebaseConfig` を手順2でコピーしたものに置き換え、保存（コミット）。1〜2分待ってアプリを開き直す。

### 4. ふたりで使う
- けいや・凪それぞれ、アプリのURLを開いて「Googleでログイン」
- iPhoneは Safari の共有ボタン →「ホーム画面に追加」でアプリのように使える

---

## うまくいかないとき
- **「登録されていません」と出る** → `firestore.rules` のメールアドレスと、ログインしたGoogleアカウントが一致しているか確認
- **ログイン画面がすぐ閉じる／エラーになる** → Firebaseの承認済みドメインに `<ユーザー名>.github.io` があるか確認
- **「config.js にFirebaseの設定がまだ入っていません」** → 手順3を確認

## 安全のための仕組み
- データはFirestoreのルールで、登録した2つのGoogleアカウント以外は読み書きできない
- `config.js` の Firebase の値は公開されても問題ない種類のもの（守りはルール側で行う）
