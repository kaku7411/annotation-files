<p align="right"><a href="README.md">English</a> | <a href="README.ja.md">日本語</a></p>

# スライド・ナレーション関係アノテーションデータ

このパッケージには、本番アノテーション用のデータとして、講義6本（スライド計178枚、うちアノテーション対象164枚、NU 計1,032件）と、記入例としてスライド1枚分のアノテーション済みデータ（example）が含まれます。Narration Unit（NU）は、ナレーションを、説明の要点や談話行為（質問、話題の切り替えなど）ごとに区切った発話の単位であり、ラベルを付ける単位です。アノテータはスライドごとに、講義動画、スライド画像、ナレーション、NU を参照し、マニュアルに従って各 NU にラベルを付けます。（[English](manual/ANNOTATOR_MANUAL_EN.md) | [日本語](manual/ANNOTATOR_MANUAL_JA.md)）

## データ統計

以下は6講義のアノテーション対象スライドの集計です（`example` を除く）。時間はスライドの区間の長さの合計です。

| 講義 | `lecture_id` | 対象スライド数 | ナレーション語数 | NU 数 | 平均 NU 数／枚 | NU 平均語数／件 | 時間（分） |
|---|---|---:|---:|---:|---:|---:|---:|
| 学術的文章作成 | `EssayWritingandPresentationskills/23` | 28※ | 5,927 | 202 | 7.2 | 29.3 | 38.2 |
| 発達心理学 | `LectureSeriesforIntrotoDevelopmentalPsy/13` | 19 | 6,094 | 161 | 8.5 | 37.9 | 36.4 |
| 呼吸器系 | `bio-4/K9i7s33lM6Q` | 26 | 3,666 | 136 | 5.2 | 27.0 | 28.4 |
| 心肺蘇生と窒息 | `OralMedicineINBDE/5` | 10 | 2,087 | 86 | 8.6 | 24.3 | 14.7 |
| 地球規模の気候変動 | `Biol1020/05` | 64 | 5,681 | 209 | 3.3 | 27.2 | 50.0 |
| 心理学入門 | `LectureSeriesForIntrotoPsy-PSY101/1` | 17 | 10,070 | 238 | 14.0 | 42.3 | 51.6 |
| **合計** | | **164** | **33,525** | **1,032** | **6.3** | **32.5** | **219.4** |

※ 学術的文章作成のアノテーション対象は `slide_index` 1〜28 です（冒頭の Recap と、後半の作文例・練習・まとめ・参考文献は対象外）。

## ファイル構成

| パス | 内容 |
|---|---|
| `lectures/<name>.json` | 講義ごとの入力ファイル（`example.json` を含む） |
| `slide_images/<name>/` | 講義ごとのスライド画像 |
| `manual/` | 英語・日本語のアノテーションマニュアルと例示用画像 |
| `results/<name>.json` | 入力ファイルに対応するアノテーション結果。`example.json` は記入済み |

`<name>` は3つのフォルダで共通で、`lecture_id` の `/` を `_` に置き換えたものです。

## データ構造

### 講義の入力データ（`lectures/<name>.json`）

以下は `lectures/example.json` の例です（`...` は省略を表します）。

- `start_time`／`end_time` は、スライドまたは NU のナレーションの開始・終了時刻で、動画の先頭からの秒数です。
- `unit_id` は `<lecture_id>_s<slide_index（3桁）>_u<NU 番号（2桁）>` です（例：`Biol1020/05_s006_u02`）。
- `image_path` はパッケージのルートからの相対パスで、ファイル名の4桁の数字は `slide_index` と一致します。
- `narration_units` が空のスライドはアノテーション対象外です（学術的文章作成の `slide_index` 0、29〜41）。

```json
{
  "lecture_id": "example",
  "source_dataset": "lpm",
  "video_url": "https://www.youtube.com/watch?v=uNrB2uG_9gs",
  "slides": [
    {
      "slide_index": 11,
      "image_path": "slide_images/example/slide_0011.jpg",
      "start_time": 1291.0,
      "end_time": 1343.0,
      "narration": "When it comes to diabetes, most of us know there are two types: type 1, type 2. The way I always remember type 2 is that you're too fat. And it's more than just simply being too fat, but that's what we tend to talk about. As you can see in the chart, there's a lot more type 2 diabetes than there is type 1. Now, there is a third type of diabetes, which is basically women who are pregnant who get elevated glucose levels during pregnancy. But for most of us, we tend to think of only type 1 and type 2. Those of you who are going into the allied health fields, you'll might even discuss up to five different types of diabetes. But for us, for this class, type 1, type 2, and remember type 2 is because you're too fat. As I always like to do, give you a little hints to help you remember things.",
      "narration_units": [
        ...,
        {
          "unit_id": "example_s011_u03",
          "text": "And it's more than just simply being too fat, but that's what we tend to talk about.",
          "start_time": 1302.55,
          "end_time": 1307.19
        },
        {
          "unit_id": "example_s011_u04",
          "text": "As you can see in the chart, there's a lot more type 2 diabetes than there is type 1.",
          "start_time": 1307.83,
          "end_time": 1311.99
        },
        ...,
        {
          "unit_id": "example_s011_u06",
          "text": "But for most of us, we tend to think of only type 1 and type 2.",
          "start_time": 1321.11,
          "end_time": 1324.47
        },
        ...,
        {
          "unit_id": "example_s011_u08",
          "text": "But for us, for this class, type 1, type 2, and remember type 2 is because you're too fat.",
          "start_time": 1332.71,
          "end_time": 1338.71
        },
        ...
      ]
    },
    ...
  ]
}
```

### アノテーション結果（`results/<name>.json`）

`annotations` のキーは `unit_id` です。`results/example.json` には、マニュアル 4.1 の作業例の記入済みの結果が入っています。

| フィールド | 記入する場合 | 値 |
|---|---|---|
| `label` | 必須 | 主ラベル：`on_slide_textual_content`、`on_slide_visual_content`、`implicit_lecture_content`、`metadiscourse`、`unrelated_content`、`invalid_nu`、または `""`（空欄。下記参照） |
| `sublabel` | `label` が `implicit_lecture_content` または `metadiscourse` の場合 | サブラベル。`implicit_lecture_content` の場合：`concept_clarification`、`example`、`explanation_reasoning`、`contextual_knowledge`、`qualification_relation`、`method_action`、`prior_content_recall`、`other`<br>`metadiscourse` の場合：`organization_management`、`speaker_stance`、`audience_interaction`、`other` |
| `not_on_slide_span` | `label` が `implicit_lecture_content` の場合 | 補足部分：NU の `text` からそのままコピーする |
| `remainder_label` | `label` が `implicit_lecture_content` の場合 | 残りの部分の主ラベル：`on_slide_textual_content`、`on_slide_visual_content`、`metadiscourse`、`unrelated_content`、または `""`（空欄。NU 全体が補足である場合、または残りの部分に主ラベルを付けられない場合） |
| `note` | 任意。`label` が `invalid_nu` または空欄の場合、`sublabel` が `other` の場合は必須 | 自由記述 |

スライド画像が不鮮明・不完全で、NU とスライドの関係の判断に影響する場合は、`label` を空欄（`""`）のままにし、`note` に該当箇所と理由を記入します（マニュアル 2.3）。

以下は `results/example.json` の例です（`...` は省略を表します）。

```json
{
  "lecture_id": "example",
  "annotations": {
    ...,
    "example_s011_u03": {
      "label": "implicit_lecture_content",
      "sublabel": "concept_clarification",
      "not_on_slide_span": "more than just simply being too fat",
      "remainder_label": "metadiscourse"
    },
    "example_s011_u04": {
      "label": "on_slide_visual_content"
    },
    ...,
    "example_s011_u06": {
      "label": "metadiscourse",
      "sublabel": "audience_interaction"
    },
    ...,
    "example_s011_u08": {
      "label": "on_slide_textual_content"
    },
    ...
  }
}
```
