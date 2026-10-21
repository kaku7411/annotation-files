<p align="right"><a href="README.md">English</a> | <a href="README.ja.md">日本語</a></p>

# Slide–Narration Annotation Data

This package contains five lectures with **192 slides and 1,506 Narration Units (NUs)**, plus a one-slide annotated example with 9 NUs. Annotators compare each NU with the slide image and narration, then assign labels following the manual. ([English](manual/ANNOTATOR_MANUAL_EN.md) | [Japanese](manual/ANNOTATOR_MANUAL_JA.md))

## Data statistics

These figures are recalculated from the five lecture files; the one-slide `example` is excluded. English words are counted by splitting on whitespace. Averages are calculated from the totals and rounded to one decimal place.

| Field and lecture | Slides | Avg. narration words/slide | NUs | Avg. NUs/slide | Avg. words/NU |
|---|---:|---:|---:|---:|---:|
| Introduction to Psychology (PSY101/1) | 16 | 625.3 | 258 | 16.1 | 38.5 |
| Developmental Psychology (/13) | 18 | 336.8 | 190 | 10.6 | 31.8 |
| Essay Writing and Presentation Skills (/21) | 33 | 251.0 | 284 | 8.6 | 28.5 |
| Essay Writing and Presentation Skills (/23) | 44 | 208.0 | 333 | 7.6 | 27.0 |
| Questionnaire Design (IPP-9EF18) | 81 | 100.1 | 441 | 5.4 | 18.3 |
| **Total** | **192** | **216.7** | **1,506** | **7.8** | **27.3** |

## Files

| Path | Contents |
|---|---|
| `lectures/<name>.json` | One input file per lecture, plus `example.json` |
| `slide_images/<name>/` | Slide images grouped by lecture |
| `manual/` | Annotation manuals and the example image |
| `results/<name>.json` | One annotation file per input file; `example.json` is filled in |

## Data structure

### Lecture input (`lectures/<name>.json`)

The following excerpt of `lectures/example.json` shows the complete narration for the example slide and four selected NUs. The actual file contains all nine NUs for this slide. `image_path` is relative to the package root.

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

### Annotation result (`results/<name>.json`)

`annotations` is keyed by `unit_id`. Each annotation has a primary `label`; `sublabel`, `not_on_slide_span`, and `note` are added when applicable. For an NU whose judgment is pending, leave `label` empty (`""`) and give the reason in `note`. The five lecture result files start with empty `annotations`; `results/example.json` contains completed annotations. The excerpt below shows the results for the same four NUs shown above.

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
