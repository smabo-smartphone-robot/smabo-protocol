# smabo-protocol

smabo のリポジトリどうしの決めごとの正本です。ここだけを直し、各リポジトリの写しは `gen.py` で作ります。

| ファイル | 中身 |
|----------|------|
| `protocol.json` | プロトコル番号、送信元 prefix、誰がどのトピックを送り・受け取るか、型、説明 |
| `face_schema.json` | 表情の設定の形（smabo-app が配り、smabo-sim が同じものを真似る） |
| `TOPICS.md` | トピックの一覧表（生成） |
| `gen.py` | 写しを作る・ずれを確かめる |

## 使い方

smabo の各リポジトリを同じフォルダに並べてチェックアウトします（`smabo-repos/smabo-brain`、
`smabo-repos/smabo-protocol` …）。

```bash
python3 gen.py           # 写しを書き出す（チェックアウトしていないリポジトリは飛ばす）
python3 gen.py --check   # ずれていれば一覧を出して終了コード 1
```

書き出す先:

| リポジトリ | ファイル |
|------------|----------|
| smabo-brain | `brain/protocol_gen.py`（smabo-brain-ros も brain.topics 経由で使う） |
| smabo-web | `src/ws/protocol.gen.ts` |
| smabo-app | `lib/core/wire/protocol.g.dart`、`test/face_schema.json` |
| smabo-esp32 | `protocol_gen.py` |
| smabo-sim | `smabo_sim/protocol_gen.py`、`tests/face_schema.json` |

## 変えるとき

1. `protocol.json` を直す。トピック名を変える・消す、メッセージの型や意味を変えるときは `protocol` を 1 つ上げる
2. `python3 gen.py` で写しを作り、各リポジトリでテストを通してコミットする
3. 番号を上げたら、smabo-brain の README の「バージョンと互換性」の表に行を足す

smabo-brain のテスト（`tests/test_protocol.py`）は、隣にこのリポジトリがあれば `gen.py --check` を実行します。
