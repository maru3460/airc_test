# airc_profiles

**airc_profiles** は、[airc](https://github.com/maru3460/airc) CLI ツールのデフォルトリモートプロファイルストアです。

AIツール設定（Claude Code や他のLLMベースツール）をプロファイルで管理し、異なる環境・用途に応じて素早く切り替えることができます。このリポジトリは、さまざまなワークフローに対応したプロファイルを提供します。

## 🚀 クイックスタート

### 1. airc CLI をインストール

```bash
npm install -g @maru3460/airc
```

### 2. プロファイルをダウンロード

デフォルト設定では、すでに `maru3460/airc_profiles` リポジトリが設定されています。

```bash
# 利用可能なプロファイル一覧を表示
airc remote --list

# プロファイルをダウンロード
airc remote pa-agent

# ローカルに保存されたプロファイルを確認
airc list
```

### 3. プロファイルを切り替え

```bash
airc use pa-agent
```

これで `pa-agent` プロファイルの設定がプロジェクトに適用されます。

### マニフェストシステム (`files.json`)

各プロファイルには `files.json` というマニフェストファイルが含まれています。

- **目的**: プロファイルに含まれる全ファイルのリストを管理
- **自動生成**: `scripts/generate-manifest.js` により自動生成
- **実行タイミング**: pre-commit フック（`simple-git-hooks`を使用）

```json
{
  "version": "1.0",
  "files": [
    ".github/agents/auto.agent.md",
    ".github/instructions/basic.instructions.md",
    ...
  ]
}
```

**仕組み**:
1. プロファイルにファイルを追加・変更
2. `git commit` を実行
3. pre-commit フックが `generate-manifest.js` を実行
4. `files.json` が自動更新され、コミットに含まれる

このシステムにより、プロファイルのダウンロード時に正確なファイルリストを取得できます。

## 👨‍💻 自分用のプロファイルストアを作る

このリポジトリをフォークして、独自のリモートプロファイルストアを作成できます。

### 手順

1. **このリポジトリをフォーク**

GitHub で `maru3460/airc_profiles` をフォークします。

2. **既存のプロファイルを削除**

```bash
git clone https://github.com/YOUR_USERNAME/airc_profiles.git
cd airc_profiles
rm -rf profiles/*
```

3. **自分のプロファイルを追加**

```bash
mkdir -p profiles/my-profile
```

プロファイルに含めたいファイルを追加します。

1. **マニフェストを生成してコミット**

```bash
npm install                      # simple-git-hooks をセットアップ
git add profiles/my-profile
git commit -m "Add my-profile"   # pre-commit フックが files.json を自動生成
git push origin main
```

5. **airc で自分のリポジトリを使用**

```bash
airc remote owner YOUR_USERNAME
airc remote name airc_profiles
airc remote my-profile           # 自分のプロファイルをダウンロード
airc use my-profile
```

## 🔗 リンク

- **airc CLI ツール**: [https://github.com/maru3460/airc](https://github.com/maru3460/airc)
- **Issues**: [https://github.com/maru3460/airc_profiles/issues](https://github.com/maru3460/airc_profiles/issues)
- **ドキュメント**: [airc CLI ドキュメント](https://github.com/maru3460/airc#readme)
