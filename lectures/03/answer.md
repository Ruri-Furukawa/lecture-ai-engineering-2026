# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：第1回の app.ask は、宿題の3レベルを「基礎・応用・発展」という一般的な難易度の階層として説明し、講座の定義とは違う内容をもっともらしく答えていた（「実装」が「応用」になり、「どれか1つで完了」「採点しない」にも触れていない）。エージェントは用語集（data/glossary.json の homework-levels）を検索してから答えたので、「基礎（講義内容の確認）・実装（自分でコードを書く）・発展（調査や試行）、どれか1つで完了、原則採点なし」と定義どおりに答え、最後に「関連する回：第1回」も示した。
- `--trace` で見えたツール呼び出し（ツール名と引数）：「LoRAとQLoRAの違いは？」で search_glossary({'query': 'LoRA'}) と search_glossary({'query': 'QLoRA'}) の2回。docstring に「query には用語を1つだけ」とあるため、用語ごとに分けて検索した
- 1つの質問での LLM 呼び出し回数（`requests`）：2（tool_calls=2、input_tokens=1196、output_tokens=174）
- 追加したツールと、docstring に書いた説明：make_quiz(lecture: int)。「指定した回の用語から1つを選び、確認問題と答えを返す。『第2回の確認問題を出して』のように、復習用の問題を求められたときに使う。」（pytest：11 passed）

## 宿題

- 取り組んだレベル：基礎
- やったこと：
  1. app/agent.py の INSTRUCTIONS から「必ず search_glossary で用語集を確認する」の行を消し、「AIE2026の講座の宿題にある「3レベル」とは何ですか？」を --trace 付きで実行した
  2. （元に戻してから）app/tools.py の search_glossary の docstring の「query には調べたい用語を1つだけ入れる」を「まとめて入れる」に変え、「LoRAとQLoRAの違いは？」を --trace 付きで実行し、演習の結果と比べた
- 結果：
  1. 行を消しても search_glossary({'query': '3レベル'}) が1回呼ばれ、用語集どおりに答えた（requests=2、tool_calls=1）
  2. 検索が search_glossary({'query': 'LoRA QLoRA'}) の1回にまとまった。tool_calls は 2→1、input_tokens は 1196→1035 に減ったが、requests は 2 のままだった。回答は両方の定義を含み、内容は演習とほぼ同じだった
- 気づき・疑問：
  - INSTRUCTIONS の行を消しても検索したのは、docstring にも「答える前に使うこと」と書いてあり、同じ指示がツールの説明文から届いていたためだと考えられる。指示はエージェント全体（INSTRUCTIONS）とツール（docstring）の2か所から効いている
  - requests は LLM の呼び出し回数で、ツールの呼び出し回数とは別。1回の応答で複数のツール呼び出しを依頼できるので、検索を分けても requests は増えなかった
  - まとめて検索してもヒットしたのは、空白を除いた文字列に用語名が含まれるかで判定する実装だったから。返す件数は最大3件なので、用語を多くまとめると取りこぼす可能性がある
