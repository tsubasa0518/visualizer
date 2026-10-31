# 作業ルール

## 変更してよい範囲
- 変更してよいのは、ユーザーが指示したファイルだけです。指示にないファイルは、読むだけにします。
- `git add` は、ファイル名を指定します。`git add -A` と `git add .` は使いません。

## git と GitHub
- 作業は main ではなく、作業用ブランチで行います。ブランチの切り替えは `git switch` を使います。
- push の前に、変更の差分（`git diff`）をユーザーに見せます。
- push は `git push -u origin <ブランチ名>` と、ブランチ名を明示します。
- `git -C` と `git -c` は使いません（権限ルールをすり抜けるためです）。
- main への push と、PR のマージは、毎回ユーザーに確認します。
- 次の操作は禁止です（`.claude/settings.json` の deny でも止めています）。
  - 強制 push
  - `git reset --hard`、`git clean`
  - `gh repo delete` とリポジトリの設定変更
  - リモートのブランチやタグの削除

## 個人情報
- メールアドレス、ローカルのパス、トークン、鍵などを、ファイルにも commit メッセージにも書きません。
- commit の前に、追加する行を次の語で検索し、結果をユーザーに報告します。
  - 語：`/Users/`、`@`、`ghp_`、`github_pat`、`PRIVATE KEY`、`token`、`password`
  - ヒットがあれば、行と理由を示します。意図したものか判断できないときは、commit しません。
- この CLAUDE.md 自身は、上の検索語を含むので、検索でヒットします。それは想定どおりの結果として報告します。

```bash
git diff --cached -U0 | grep -nE '^\+.*(/Users/|@|ghp_|github_pat|PRIVATE KEY|token|password)'
```

## 最初の commit の前
- `git config user.email` が noreply 形式であることを確かめます。メールそのものは表示しません。

```bash
git config user.email | grep -Eq 'noreply\.github\.com$' && echo OK || echo NG
```

- `NG` のときは commit せず、ユーザーに知らせます。
