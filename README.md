# ふたりの予定 — セットアップ手順

OAuth不要・無料・PWA対応。所要時間は約20分です。

## ファイル一式

```
futari-schedule/
├── index.html              ← GitHubに上げる
├── manifest.json           ← GitHubに上げる
├── service-worker.js       ← GitHubに上げる
├── icon-192.png            ← GitHubに上げる
├── icon-512.png            ← GitHubに上げる
├── icon-512-maskable.png   ← GitHubに上げる
├── apple-touch-icon.png    ← GitHubに上げる
├── favicon.png             ← GitHubに上げる
└── Code.gs                 ← GASに貼る（GitHubには上げない）
```

GAS URL と TOKEN は **ブラウザで初回入力する方式** なのでファイルに書き込む必要なし。リポジトリは公開してOK。

---

## STEP 1 — GAS バックエンドを作る（10分）

### 1-1. プロジェクト作成

1. <https://script.google.com> を開く
2. 「**新しいプロジェクト**」をクリック
3. 左上「無題のプロジェクト」を「**ふたりの予定**」にリネーム

### 1-2. コード貼り付け

1. デフォルトで開いている `Code.gs` の中身を全消し
2. このフォルダの `Code.gs` の内容を貼り付け

### 1-3. TOKEN を決める

`CONFIG.TOKEN` の値を書き換え。ランダム長文字列を推奨。

たとえばこのページで生成 → <https://www.uuidgenerator.net/>
または ターミナルで：
```bash
openssl rand -hex 24
```

例：
```javascript
const CONFIG = {
  TOKEN: '7a2f9c8e1d4b6...（24文字以上）',
  CALENDAR_ID: 'primary',
  LIST_DAYS: 30,
  TIMEZONE: 'Asia/Tokyo',
};
```

**このTOKENは後でブラウザに入力するので、メモアプリ等に控えておく。**

### 1-4. 権限承認

1. エディタ上部の関数選択で `testCreate` を選ぶ
2. 「**実行**」をクリック
3. 「承認が必要です」→「権限を確認」
4. 自分のGoogleアカウントを選択
5. 「**このアプリは Google で確認されていません**」という警告が出る
   - 「**詳細**」をクリック
   - 「**ふたりの予定（安全ではないページ）に移動**」をクリック
   - これは自作スクリプトなので問題なし。安心してOK
6. Googleカレンダーへのアクセス許可を「許可」
7. Googleカレンダーを開いて、1時間後に「[テスト] 動作確認」が入っていればOK
   - （確認後、このテスト予定は削除してOK）

### 1-5. ウェブアプリとして公開

1. 右上「**デプロイ**」→「**新しいデプロイ**」
2. 歯車アイコン → 種類を「**ウェブアプリ**」に
3. 設定：
   - 説明：「v1」など任意
   - 次のユーザーとして実行：「**自分**」
   - アクセスできるユーザー：「**全員**」
4. 「**デプロイ**」をクリック
5. 「ウェブアプリ」のURLをコピー（`https://script.google.com/macros/s/.../exec`）
   - **このURLも控えておく**

---

## STEP 2 — GitHubに上げる（5分）

### 2-1. リポジトリ作成

1. <https://github.com/new>
2. Repository name：`futari-schedule`（好きな名前でOK）
3. Public か Private はどちらでもOK（TOKENは含まれないため）
4. 「Create repository」

### 2-2. ファイルをアップロード

**コマンドライン派の場合：**

```bash
# このフォルダの中身（Code.gs 以外）をコピーして
cd ~/projects
mkdir futari-schedule && cd futari-schedule

# Code.gs を除いた9ファイルをここに置く
# (index.html, manifest.json, service-worker.js, icon-*.png, apple-touch-icon.png, favicon.png)

git init
git add .
git commit -m "init"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/futari-schedule.git
git push -u origin main
```

**ブラウザだけで完結させたい場合：**

1. リポジトリのページで「**uploading an existing file**」リンク
2. Code.gs 以外の9ファイルをまるごとドラッグ&ドロップ
3. 下の「Commit changes」

### 2-3. GitHub Pages 有効化

1. リポジトリの「**Settings**」タブ
2. 左メニュー「**Pages**」
3. 「Build and deployment」の Source を「**Deploy from a branch**」に
4. Branch を「**main**」「**/(root)**」に → 「Save」
5. ページ上部に `Your site is live at https://YOUR_USERNAME.github.io/futari-schedule/` と表示される（最初の1〜2分はビルド待ち。緑バッジ出るまで待つ）

---

## STEP 3 — ブラウザで初回設定（2分）

1. 上のGitHub PagesのURLをブラウザで開く
2. 「**初回設定**」画面が出る
3. 入力：
   - **GAS URL**：STEP 1-5でコピーしたURL
   - **共有トークン**：STEP 1-3で決めたTOKEN
4. 「**保存して始める**」
5. 自動で接続テストが走り、成功すれば入力画面に進む

---

## STEP 4 — ホーム画面に追加（任意）

**iPhone（Safari）：**
1. URLをSafariで開く
2. 共有ボタン → 「ホーム画面に追加」
3. アプリ風アイコンができる

**Android（Chrome）：**
1. URLを開くと「ホーム画面に追加」の案内
2. または右上メニュー → 「ホーム画面に追加」

---

## STEP 5 — パートナーに共有

LINE等で以下3点を送る：

```
URL: https://YOUR_USERNAME.github.io/futari-schedule/
GAS URL: https://script.google.com/macros/s/.../exec
TOKEN: 7a2f9c8e1d4b6...
```

パートナーは：
1. URLを開く
2. 初回設定でGAS URLとTOKENを入力
3. ホーム画面に追加

これで完成。両端末から同じカレンダーに書き込まれる。

---

## 使い方

| 操作 | 動作 |
|---|---|
| 入力タブで送信 | カレンダーに即時登録（タイトル冒頭に`[そうた]`/`[パートナー]`） |
| 一覧タブ | 今後30日分の予定表示 |
| 一覧の「編集」 | フォームに値が入って編集モードに |
| 一覧の「削除」 | 確認後カレンダーから削除 |
| 右上「⚙」 | 設定変更（URLやTOKENを変えたい場合） |

---

## トラブルシュート

| 症状 | 原因と対処 |
|---|---|
| 設定保存で「テスト失敗：認証エラー」 | TOKENがGAS側と違う。`Code.gs`の値と照合 |
| 設定保存で「通信エラー」 | GAS URLが間違っている／GASが「全員」アクセスになっていない |
| 設定保存できたが一覧が空 | まだ予定を登録していないだけ（30日以内の予定がない） |
| 送信は通るが指定したカレンダーに出ない | GAS側`CALENDAR_ID`を確認。`'primary'`はメインカレンダー |
| Service Workerの更新が反映されない | `service-worker.js`の`CACHE_NAME`を`v2`に上げて再push |
| GASコード変えても反映されない | デプロイ管理 → 鉛筆 → バージョン「新バージョン」→ デプロイ |

---

## カスタマイズ早見表

| やりたいこと | 触る場所 |
|---|---|
| 一覧期間を変更 | `Code.gs` の `LIST_DAYS` |
| 専用カレンダーに分離 | カレンダーを新規作成 → IDコピー → `CALENDAR_ID`に設定 |
| メール通知追加 | `Code.gs`の`create_`関数末尾に`MailApp.sendEmail(...)` |
| 入力者を増やす | `index.html`の`.author-toggle`内に`<button>`追加 |
| 全端末でログアウト相当 | GASで新デプロイ作成して旧URL無効化。各端末で⚙から再設定 |
