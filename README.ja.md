<p align="right"><a href="README.md">English</a> | <a href="README.ja.md">日本語</a></p>

# スライド・ナレーション関係アノテーションデータ

このパッケージには、講義5本（スライド計192枚、NU（Narration Unit）計1,510件）と、記入例としてスライド1枚分（NU 9件）のアノテーション済みデータ（example）が含まれます。アノテータは各 NU をスライド画像とナレーションに照らして確認し、マニュアルに従ってラベルを付けます。（[English](manual/ANNOTATOR_MANUAL_EN.md) | [日本語](manual/ANNOTATOR_MANUAL_JA.md)）

## ファイル構成

| パス | 内容 |
|---|---|
| `lectures/<name>.json` | 講義ごとの入力ファイル（`example.json` を含む） |
| `slide_images/<name>/` | 講義ごとのスライド画像 |
| `manual/` | 英語・日本語のアノテーションマニュアルと例示用画像 |
| `results/<name>.json` | 入力ファイルに対応するアノテーション結果。`example.json` は記入済み |

## データ構造

### 講義の入力データ（`lectures/<name>.json`）

以下は `lectures/example.json` の抜粋です。このスライドのナレーション全文と、9件の NU から選んだ4件を示します。実際のファイルには9件すべてが含まれています。`image_path` はパッケージのルートからの相対パスです。

```json
{
  "lecture_id": "example",
  "source_dataset": "lpm",
  "video_url": "https://www.youtube.com/watch?v=uNrB2uG_9gs",
  "slides": [
    {
      "slide_index": 11,
      "image_path": "slide_images/example/slide_0011.jpg",
      "narration": "when it comes to diabetes most of us know there are two types type 1 type 2 the way i always remember type 2 is that you're too fat and it's more than just simply being too fat but that's what we tend to talk about and as you can see in the chart there's a lot more type 2 diabetes than there is type 1 now there is a third type of diabetes which is basically women who are pregnant who get elevated glucose levels during pregnancy but for most of us we tend to you've only type 1 and type 2 those of you who are going into the allied health fields you'll might even discuss up to five different types of diabetes but for us for this class type 1 type 2 and remember type 2 is because you're too fat as i always like to do give you a little hints to help you remember things",
      "narration_units": [
        {
          "unit_id": "example_s011_u02",
          "text": "The way I always remember type 2 is that you're too fat.",
          "evidence": [
            {
              "quote": "the way i always remember type 2 is that you're too fat"
            }
          ]
        },
        {
          "unit_id": "example_s011_u03",
          "text": "And it's more than just simply being too fat, but that's what we tend to talk about.",
          "evidence": [
            {
              "quote": "and it's more than just simply being too fat but that's what we tend to talk about"
            }
          ]
        },
        {
          "unit_id": "example_s011_u04",
          "text": "As you can see in the chart, there's a lot more type 2 diabetes than there is type 1.",
          "evidence": [
            {
              "quote": "as you can see in the chart there's a lot more type 2 diabetes than there is type 1"
            }
          ]
        },
        {
          "unit_id": "example_s011_u08",
          "text": "But for us, for this class, type 1, type 2, and remember type 2 is because you're too fat.",
          "evidence": [
            {
              "quote": "but for us for this class type 1 type 2 and remember type 2 is because you're too fat"
            }
          ]
        }
      ]
    }
  ]
}
```

### アノテーション結果（`results/<name>.json`）

`annotations` のキーは `unit_id` です。各項目には主ラベルを `label` に記入し、必要に応じて `sublabel`、`not_on_slide_span`、`note` を追加します。判断を保留する NU は `label` を空欄（`""`）にし、`note` に理由を記入します。講義5本分の結果ファイルは `annotations` が空の状態で配布しており、`results/example.json` には記入済みの結果が入っています。以下は上の入力例と同じ4件の結果です。

```json
{
  "lecture_id": "example",
  "annotations": {
    "example_s011_u02": {
      "label": "on_slide_content"
    },
    "example_s011_u03": {
      "label": "implicit_lecture_content",
      "sublabel": "concept_clarification",
      "not_on_slide_span": "more than just simply being too fat"
    },
    "example_s011_u04": {
      "label": "figure_formula_explanation"
    },
    "example_s011_u08": {
      "label": "metadiscourse",
      "sublabel": "organization_management"
    }
  }
}
```
