# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：第1回の app.ask は、宿題の3レベルを「基礎・応用・発展」という一般的な難易度の階層として説明し、講座の定義とは違う内容をもっともらしく答えていた（「実装」が「応用」になり、「どれか1つで完了」「採点しない」にも触れていない）。エージェントは用語集（data/glossary.json の homework-levels）を検索してから答えたので、「基礎（講義内容の確認）・実装（自分でコードを書く）・発展（調査や試行）、どれか1つで完了、原則採点なし」と定義どおりに答え、最後に「関連する回：第1回」も示した。
- `--trace` で見えたツール呼び出し（ツール名と引数）：「LoRAとQLoRAの違いは？」で search_glossary({'query': 'LoRA'}) と search_glossary({'query': 'QLoRA'}) の2回。docstring に「query には用語を1つだけ」とあるため、用語ごとに分けて検索した
- 1つの質問での LLM 呼び出し回数（`requests`）：2（tool_calls=2、input_tokens=1196、output_tokens=174）
- 追加したツールと、docstring に書いた説明：make_quiz(lecture: int)。「指定した回の用語から1つを選び、確認問題と答えを返す。『第2回の確認問題を出して』のように、復習用の問題を求められたときに使う。」（pytest：11 passed）git add -A && git commit -m "第3回 演習" && git push -u origin lec03

## 宿題

- 取り組んだレベル：基礎 / 実装 / 発展
- やったこと：
- 結果：
- 気づき・疑問：
