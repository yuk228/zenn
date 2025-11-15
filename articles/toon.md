---
title: 'TOONってなんぞや'
published: false
type: tech
emoji: '🤖'
topics: ['toon', 'llm', 'token']
---

何気なく X を見ていたら、`TOON`というフォーマットを見つけました。
気になって調べてみたので共有します。

## TOON ってなに？

"**T**oken-**O**riented **O**bject **N**otation" (トークン指向オブジェクト表記)という名称。

LLM に JSON のような構造化データを渡す際、TOON に変換することでトークン使用量を 30% ~ 60%削減できるとのこと。

拡張子は`.toon`で、以下のような形式で記述します。

```toon:users.toon
[2,]{id,name,email,age}:
  1,山田太郎,taro.yamada@example.com,30
  2,佐藤花子,hanako.sato@example.com,25
```

人力で書くのに適していないので、エンコーダーなり使って変換するのが良いかもです。

toon の Github を置いておきます。
https://github.com/toon-format/toon

## どれくらい節約できるの？

```json:users.json
[
  {
    "id": 1,
    "name": "山田太郎",
    "email": "taro.yamada@example.com",
    "age": 30
  },
  {
    "id": 2,
    "name": "佐藤花子",
    "email": "hanako.sato@example.com",
    "age": 25
  }
]
```

この JSON データを TOON に変換しました。

```toon:users.toon
[2,]{id,name,email,age}:
  1,山田太郎,taro.yamada@example.com,30
  2,佐藤花子,hanako.sato@example.com,25
```

かなりトークン節約できてそうですね。見ただけで分かるレベルです。
| |JSON|TOON|節約率|
|:-:|:-:|:-:|:-:|
|GPT 5|82 Token|45 Token|45%
|Claude Sonnet 4.5 |88 Token|54 Token|39%

## 自分でやってみよう

### 環境構築

`python-toon`という Python のライブラリを使用して変換を行います。

https://pypi.org/project/python-toon/

パッケージマネージャーには高速な`uv`を用います。

```bash
uv init toon-converter
cd toon-converter
uv add python-toon
```

### エンコード

```python:main.py
import json
from toon import encode


def main():
    with open("test.json", "r", encoding="utf-8") as f:
        users = json.load(f)

    print(encode(users))


if __name__ == "__main__":
    main()
```

`test.json`に json データを入れてください。

### デコード

```python:main.py
import json
from toon import decode


def main():
    with open("test.toon", "r", encoding="utf-8") as f:
        users = f.read()

    print(json.dumps(decode(users), ensure_ascii=False, indent=2))


if __name__ == "__main__":
    main()
```

エンコードと同じ要領です。
`test.toon`に toon データを入れてください。

## じゃあ全部 TOON でいいやん！

そうでもないようです。

公式によると、TOON には以下のような欠点があるようです。

- 深くネストされた、あるいは不均一な構造のとき
-
