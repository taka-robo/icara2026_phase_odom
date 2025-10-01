# 論文生成 DevContainer

このリポジトリは、コーディングエージェントを活用して論文や技術文書の草稿を迅速に作成するための VS Code Dev Container 環境です。LaTeX（日本語対応）を中心に、図表生成やテキスト整形に必要なツールがプリインストールされており、エージェントによる自動生成・編集ワークフローをそのまま実行できます。

## ディレクトリ構成

- `project/` — 実際に執筆する LaTeX プロジェクトを配置
- `outline.md` — 原稿の骨子を書き出すための Markdown。こちらを最初に充実させることで、論文執筆作業がより高速になります。
- `template/` — 投稿予定の各ジャーナル・会議向け LaTeX テンプレートを保存。
- `.devcontainer/` — コンテナ設定。VS Code が Dev Container を起動する際に参照します。

## セットアップ

1. VS Code と拡張機能「Dev Containers」または「Remote-Containers」をインストールします。
2. このリポジトリをクローンし、VS Code で開きます。
3. 「Reopen in Container」を実行すると、記載済みのツール一式（LaTeX、PlantUML、Graphviz、日本語フォントなど）が整った状態で開発環境が立ち上がります。

初回ビルドには数分かかることがありますが、コンテナ構築後は同じイメージを再利用します。

## 主なツールと機能

- LaTeX 環境（`latexmk`, `uplatex`, `biber`, `upmendex` など日本語対応パッケージを含む）
- PlantUML / Graphviz によるダイアグラム生成
- Node.js / Python 3 系のランタイム（コード生成補助やデータ処理に活用可能）
- VS Code 推奨拡張機能（GitLens, Draw.io, textlint, autopep8 など）

## 推奨ワークフロー

1. `outline.md` に章立て・論点・引用候補を整理する。
2. コーディングエージェントに `outline.md` と `project/main.tex` を参照させ、必要に応じて `template/` 配下の既存テンプレートも参照しながら原稿ドラフトを生成する。例えば次のようなプロンプトを渡すと、構成に沿ったドラフトを得やすくなります。

   ```text
   あなたは日本語の論文執筆を支援するコーディングエージェントです。
   `outline.md` の骨子に従い、`project/main.tex` や `project/01_introduction/main.tex` などセクションディレクトリ内のファイルを更新する LaTeX コードを作成してください。
   `template/` ディレクトリ内のテンプレート方針を参考に、作業を進めてください。
   章立て・節構成・引用キーを明示し、可能なら図表の挿入位置のコメントも入れてください。
   ```
3. `project/` 配下で必要に応じてテンプレートや画像ファイルを追加し、セクションごとの下書きは `project/01_introduction/main.tex` のようにディレクトリ単位で管理しながら LaTeX を手動またはエージェント経由で編集する。分割運用する場合は `main.tex` から `\input{01_introduction/main}` のように読み込むと管理しやすくなります。
4. テンプレート由来のクラスファイルなど、ビルドに必要なファイルを `project/` に配置しておきます。利用したいクラスファイル名に応じて次のようにコピーしてください（`CLASSFILE` は適切なファイル名に置き換えます）。

   ```bash
   cp template/CLASSFILE.cls project/   # 例: cp template/IEEEtran.cls project/
   ```

5. リポジトリ直下の `latexmkrc` は XeLaTeX を使うよう設定し、生成物を `out/` ディレクトリへ集約します。ルートで次のコマンドを実行すればローカル確認用 PDF が生成されます。

   ```bash
   latexmk -C project/main.tex   # （初回や再ビルド前に）生成物をクリーンアップ
   latexmk project/main.tex      # XeLaTeX で out/main.pdf を生成
   ```

   `project/` に留まって作業したい場合は `latexmk -r ../latexmkrc main.tex` のように設定ファイルのパスを明示してください。生成された PDF や補助ファイルはリポジトリ直下の `out/` 以下に配置され、最終成果物は `out/main.pdf` になります。
