<p align="right"><a href="README.md">English</a> | <a href="README.ja.md">日本語</a></p>

# Slide–Narration Annotation Data

This package contains five lectures with **192 slides and 1,510 Narration Units (NUs)**, plus a one-slide annotated example with 9 NUs. Annotators compare each NU with the slide image and narration, then assign labels following the manual. ([English](manual/ANNOTATOR_MANUAL_EN.md) | [Japanese](manual/ANNOTATOR_MANUAL_JA.md))

## Files

| Path | Contents |
|---|---|
| `lectures/<name>.json` | One input file per lecture, plus `example.json` |
| `slide_images/<name>/` | Slide images grouped by lecture |
| `manual/` | Annotation manuals and the example image |
| `results/<name>.json` | One annotation file per input file; `example.json` is filled in |

## Data structure

### Lecture input (`lectures/<name>.json`)

The following is a shortened excerpt of `lectures/example.json`. Each slide in the actual file contains its full narration and NU list. `image_path` is relative to the package root.

```json
{
  "lecture_id": "example",
  "source_dataset": "lpm",
  "video_url": "https://www.youtube.com/watch?v=uNrB2uG_9gs",
  "slides": [
    {
      "slide_index": 11,
      "image_path": "slide_images/example/slide_0011.jpg",
      "narration": "and it's more than just simply being too fat but that's what we tend to talk about",
      "narration_units": [
        {
          "unit_id": "example_s011_u03",
          "text": "And it's more than just simply being too fat, but that's what we tend to talk about.",
          "evidence": [
            {
              "quote": "and it's more than just simply being too fat but that's what we tend to talk about"
            }
          ]
        }
      ]
    }
  ]
}
```

### Annotation result (`results/<name>.json`)

`annotations` is keyed by `unit_id`. Each annotation has a primary `label`; `sublabel`, `topic`, and `note` are added when applicable. For an NU whose judgment is pending, leave `label` empty (`""`) and give the reason in `note`. The five lecture result files start with empty `annotations`; `results/example.json` contains completed annotations.

```json
{
  "lecture_id": "example",
  "annotations": {
    "example_s011_u03": {
      "label": "implicit_lecture_content",
      "sublabel": "concept_clarification",
      "topic": "type 2 diabetes involves more than body weight alone"
    }
  }
}
```
