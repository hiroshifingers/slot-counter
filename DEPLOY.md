# スロットカウンター 独立サイト（公開手順）

このディレクトリ単体で完結する PWA です。ビルド不要・すべて相対パスなので、
静的ホスティングにそのまま置けば動きます。将来のアプリ化（Capacitor 等でラップ）の
たたき台でもあります。

- 同期: Supabase（`js/cloud.js` の anon キーは公開前提・RLS 保護）。
  別URLに置き換えても同じアカウントでログインすればクラウドから全データが降ってきます。
- データ本体はブラウザの IndexedDB（`practice_counter` DB）とクラウド。ここのファイルには入りません。

## 初回公開（GitHub Pages・新リポジトリ `slot-counter`）

1. GitHub で **Public** リポジトリ `slot-counter` を作成（空でよい）。
2. このディレクトリで：

   ```bash
   cd "practice_counter"
   git init
   git add -A
   git commit -m "init: スロットカウンター独立サイト"
   git branch -M main
   git remote add origin https://github.com/<ユーザー名>/slot-counter.git
   git push -u origin main
   ```

3. リポジトリの Settings → Pages → Source = `main` / `/ (root)` を選択して保存。
4. 数十秒後 `https://<ユーザー名>.github.io/slot-counter/` で公開。
   スマホで開き、ログイン → 記録タブの 🔄 でクラウドから同期 → ホーム画面に追加。

## 更新のたび

コードを変えたら `sw.js` の `CACHE` 版番号と `index.html`／`sw.js` の `?v=N` を揃えて上げ、
commit & push。network-first SW なのでオンラインで開き直せば最新化されます。

## ローカル確認

```bash
cd "practice_counter"
python3 -S -m http.server 8765     # http://localhost:8765
```

（Supabase ログインを挟むので、オフライン確認だけなら localStorage の
`pc_supabase_session` にダミーを入れればゲートを回避できます。）
