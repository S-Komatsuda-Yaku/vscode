# VSCode (Code - OSS) フォーク後 運用・便利コマンド集

本ドキュメントは、`microsoft/vscode` をフォークした後の日常的な開発・運用で使用する便利コマンドをまとめたチートシートです。

---

## 1. リモート設定の確認と構成

フォーク環境では、リモートが以下のように設定されています。

- **`origin`**: あなたのフォークリポジトリ (`https://github.com/S-Komatsuda-Yaku/vscode.git`)
- **`upstream`**: 本家リポジトリ (`https://github.com/microsoft/vscode.git`)

### 設定確認
```bash
git remote -v
```

---

## 2. 日常の開発フロー

### 2.1 ブランチの作成と切り替え
特定のタグ（例: `1.138.0`）や既存ブランチから新しい機能ブランチを作成する場合:

```bash
# 1.138.0 タグから新規ブランチを作成
git checkout -b feature/my-custom-ui 1.138.0

# 既存ブランチから新規ブランチを作成
git checkout -b feature/new-feature
```

### 2.2 変更の確認・コミット・プッシュ
```bash
# 変更状態の確認
git status

# 変更差分の確認
git diff

# 変更をステージングしてコミット
git add .
git commit -m "feat: 独自のUIコンポーネントを追加"

# 自分のフォークリポジトリ (origin) にプッシュ
git push -u origin feature/my-custom-ui
```

---

## 3. 本家 (upstream) の更新を取り込む

本家の最新修正や新しいリリースを取り込みたい場合の手順です。

### 3.1 本家の最新情報を取得
```bash
git fetch upstream
```

### 3.2 本家のタグ一覧・ブランチ確認
```bash
# 本家のタグ一覧を確認
git tag -l "1.*" | sort -V | tail -n 20

# 本家の特定タグから新しいブランチを作る場合
git checkout -b custom-ide-1.139.0 tags/1.139.0
```

### 3.3 現在のブランチに本家の変更をマージする
```bash
# 本家の release ブランチまたは main をマージ
git merge upstream/main
# または
git merge upstream/release/1.138
```

---

## 4. ビルド・実行・デバッグコマンド

### 4.1 開発ビルド (Watchモード)
バックグラウンドでコードの変更を監視・コンパイルします。

```bash
# ターミナル1: TypeScript コンパイルの常時監視
npm run watch
```

### 4.2 Code - OSS の起動
```bash
# ターミナル2: ビルドされたカスタムVSCodeを起動
./scripts/code.sh
```

### 4.3 キャッシュのクリーン・再ビルド
ビルドがおかしくなった場合の復旧手順:

```bash
# ビルド成果物の削除
npm run clean

# 再度依存関係を整合
npm install
```

---

## 5. トラブルシューティング & 便利コマンド

### 5.1 作業中の一時退避 (Stash)
ブランチを切り替えたいが、まだコミットしたくない場合:

```bash
# 一時退避
git stash -u

# 退避した変更を復元
git stash pop
```

### 5.2 変更の破棄
```bash
# 特定ファイルの変更を元に戻す
git checkout -- <file-path>

# すべての未コミット変更を破棄して直前コミットに戻す (注意)
git reset --hard HEAD
```

### 5.3 コミットログの確認
```bash
# グラフ形式で見やすく表示
git log --oneline --graph --decorate -n 20
```
