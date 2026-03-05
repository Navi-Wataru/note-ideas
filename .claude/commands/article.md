---
description: Issue番号を指定してnote記事を生成・査読・再構築・GitHubに保存する
argument-hint: <issue番号>
---

あなたはNavigate Labのnote記事制作AI編集者です。
CLAUDE.md のルールをすべて適用してください。

Issue #$ARGUMENTS を使って、以下の手順を**すべて順番に**実行してください。

---

## STEP 1: Issue読み取り

https://github.com/Navi-Wataru/note-ideas/issues/$ARGUMENTS にアクセスし、
タイトルと本文をすべて読み取る。

本文が空の場合は、タイトルから記事テーマを推定して進める。

---

## STEP 2: シリーズ文脈の確認

`note-ideas/series.md` を読み、このIssueがどのシリーズに属するかを確認する。
既存の公開済み記事（`articles/`フォルダ）との接続を把握する。

---

## STEP 3: 初稿生成（3ファイル同時）

`templates/generate_article.md` の指示に従い、以下を生成する：

- `articles/<slug>.md` — 記事本文（3000〜5000字）
- `visuals/<slug>_prompts.md` — 画像生成プロンプト5案
- `social/<slug>_sns.md` — X投稿×3 / Instagramリール台本 / note導入文

slugは英小文字+ハイフン。

---

## STEP 4: 全否定査読

生成した記事を、あえて厳しく批評する。以下の8観点を必ず網羅する：

1. フックの強さ
2. 読者の共感ポイント
3. 論理の飛躍・説明不足
4. トーンの一貫性
5. 構成の自然さ
6. バズりやすさ
7. 情報としての新規性・価値
8. 禁止ワード（「整える」）チェック

査読結果を箇条書きで出力してから次に進む。

---

## STEP 5: ゼロベース再構築

査読を踏まえて記事を書き直す。ただの修正ではなく再構築。

構成は以下を基本とする：
1. 強い問い（フック）
2. よくある誤解
3. 身体で起きていること
4. 認知の転換
5. 小さな身体実験
6. 静かな結び

`articles/<slug>.md` を上書き保存する。

---

## STEP 6: GitHubへ保存

```
git checkout -b article/issue-$ARGUMENTS-<slug>
git add articles/<slug>.md visuals/<slug>_prompts.md social/<slug>_sns.md
git commit -m "note article: <タイトル>\n\nCloses #$ARGUMENTS\n\nCo-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
git push -u origin article/issue-$ARGUMENTS-<slug>
```

---

## STEP 7: 完了報告

以下を出力する：

- 生成ファイル一覧
- 記事の要約（3行）
- 査読で指摘した主な問題点と、どう解決したか
- PR作成用のURL（`https://github.com/Navi-Wataru/note-ideas/pull/new/article/issue-$ARGUMENTS-<slug>`）
- PRに貼るべき本文（テンプレート形式で）
- 次に着手すべきIssue番号の提案

---

## 品質チェックリスト（完了前に必ず確認）

- [ ] 文字数 3000〜5000字
- [ ] 「整える」未使用
- [ ] 専門用語に初出説明あり
- [ ] 段落3〜4文以内
- [ ] 行動導線（試せること）あり
- [ ] シリーズ前後の記事への言及あり
