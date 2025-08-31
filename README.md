# forgejo-to-github

Forgejo から GitHub へリポジトリを自動同期する Forgejo Actions ワークフローです。

## 使い方

1) Forgejo のこのリポジトリに Secrets を設定

- `GH_TOKEN`: GitHub の個人用アクセストークン（repo 権限）
- `GH_REPO`: 同期先の GitHub リポジトリの `owner/repo` 形式（例: `your-org/your-repo`）

2) ランナーラベルの確認（必要に応じて修正）

- デフォルトでは `.forgejo/workflows/sync-to-github.yml` の `runs-on: docker` を使用しています。
- ご利用の Forgejo ランナーのラベルに合わせて `docker` を `self-hosted` や `ubuntu-latest` などへ変更してください。

3) トリガー

- ブランチ/タグの push、削除、手動実行（workflow_dispatch）で起動します。

## 仕組み（概要）

- `actions/checkout@v4` で履歴を全取得（fetch-depth: 0）。
- GitHub リモートをトークン付き URL で追加し、`git push --mirror` でブランチ/タグ/削除をミラーリングします。

## 注意事項

- `--mirror` は削除も含め「Forgejo 側の状態を GitHub 側に反映」します。意図しない上書きにご注意ください。
- LFS を使用している場合、ランナーに `git-lfs` のインストールが必要です（必要ならワークフローに追加してください）。

## ファイル

- `.forgejo/workflows/sync-to-github.yml`: 同期用ワークフロー本体。
