---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が利用できるようにします。[指示ファイル][instruction-files]には、作成するコードだけでなく、その構成方法も記述できます。フィルター機能を構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリとパス指定の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- コメントではコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載します。該当する場合は注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` 契約を記述し、関連コードが変わった場合はコメントも更新します。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示は、Copilot がコーディング スタイルや要件を理解できるようにコンテキストと設定を提供します。規約、ライブラリ、コードに含めるコメントの形式などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの形式やサポート状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが含まれています。変更を加える前に内容を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングに関する指針を確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存プロジェクト ファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、参照すべき例を Copilot に伝えられます。

## チームの方針に合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ改善が必要です。新しく生成する TypeScript に TSDoc コメントを追加するよう、中心となる `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを見つけます (35 行目付近)。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新を保存した後、フィルターの会話を終了して再開すると、機能のコンテキストを保ったまま新しい指示を読み込めます。

1. フィルターの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチ上でリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで更新した指針を適用するよう Copilot に依頼します。

   ```plaintext
   先ほど指示とコードのガイダンスを更新しました。生成したコードをそのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取り込む方法を確認し、新しい標準を機能に適用しました。このレッスンでは次のことを行いました。

- リポジトリとパス指定の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行][next-lesson]し、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
