<p align="right"><a href="README.md">English</a> | <a href="README.ja.md">日本語</a></p>

# Slide–Narration Annotation Data

This package contains the data for the main annotation: six lectures with **178 slides (164 to be annotated) and 1,032 NUs**, plus a one-slide annotated example. A Narration Unit (NU) is a unit of speech obtained by dividing the narration by explanation point or discourse act (such as a question or a change of topic); it is the unit to which labels are assigned. For each slide, annotators refer to the lecture video, the slide image, the narration and the NUs, and label each NU following the manual ([English](manual/ANNOTATOR_MANUAL_EN.md) | [Japanese](manual/ANNOTATOR_MANUAL_JA.md)).

## Data statistics

Statistics for the slides to be annotated in the six lectures (`example` excluded). Duration is the total length of the slides' time ranges.

| Lecture | `lecture_id` | Slides to be annotated | Narration words | NUs | Avg. NUs/slide | Avg. words/NU | Duration (min) |
|---|---|---:|---:|---:|---:|---:|---:|
| Essay Writing and Presentation Skills | `EssayWritingandPresentationskills/23` | 28\* | 5,927 | 202 | 7.2 | 29.3 | 38.2 |
| Developmental Psychology | `LectureSeriesforIntrotoDevelopmentalPsy/13` | 19 | 6,094 | 161 | 8.5 | 37.9 | 36.4 |
| Respiratory System | `bio-4/K9i7s33lM6Q` | 26 | 3,666 | 136 | 5.2 | 27.0 | 28.4 |
| CPR and Choking | `OralMedicineINBDE/5` | 10 | 2,087 | 86 | 8.6 | 24.3 | 14.7 |
| Global Climate Change | `Biol1020/05` | 64 | 5,681 | 209 | 3.3 | 27.2 | 50.0 |
| Introduction to Psychology | `LectureSeriesForIntrotoPsy-PSY101/1` | 17 | 10,070 | 238 | 14.0 | 42.3 | 51.6 |
| **Total** | | **164** | **33,525** | **1,032** | **6.3** | **32.5** | **219.4** |

\* In Essay Writing and Presentation Skills, slides with `slide_index` 1–28 are to be annotated (the opening recap and the later example essay, exercise, summary and references are not).

## Files

| Path | Contents |
|---|---|
| `lectures/<name>.json` | One input file per lecture, plus `example.json` |
| `slide_images/<name>/` | Slide images grouped by lecture |
| `manual/` | Annotation manuals (English and Japanese) and their example images |
| `results/<name>.json` | One annotation file per input file; `example.json` is filled in |

`<name>` is the same in all three folders. It is the `lecture_id` with `/` replaced by `_`.

## Data structure

### Lecture input (`lectures/<name>.json`)

The following is an example from `lectures/example.json` (`...` marks omitted parts).

- `start_time` and `end_time` are the start and end of the narration of a slide or an NU, in seconds from the start of the video.
- `unit_id` is `<lecture_id>_s<slide_index, 3 digits>_u<NU number, 2 digits>` (for example, `Biol1020/05_s006_u02`).
- `image_path` is relative to the package root; the four-digit number in the file name is the `slide_index`.
- Slides with an empty `narration_units` list are not to be annotated (`slide_index` 0 and 29–41 of Essay Writing and Presentation Skills).

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

### Annotation result (`results/<name>.json`)

`annotations` is keyed by `unit_id`. `results/example.json` contains the completed annotations of the worked example in section 4.1 of the manual.

| Field | When to fill in | Values |
|---|---|---|
| `label` | Required | Main label: `on_slide_textual_content`, `on_slide_visual_content`, `implicit_lecture_content`, `metadiscourse`, `unrelated_content`, `invalid_nu`, or `""` (blank; see below) |
| `sublabel` | When `label` is `implicit_lecture_content` or `metadiscourse` | Sublabel. For `implicit_lecture_content`: `concept_clarification`, `example`, `explanation_reasoning`, `contextual_knowledge`, `qualification_relation`, `method_action`, `prior_content_recall`, `other`<br>For `metadiscourse`: `organization_management`, `speaker_stance`, `audience_interaction`, `other` |
| `not_on_slide_span` | When `label` is `implicit_lecture_content` | Supplementary part: copy it exactly as it appears in the NU `text` |
| `remainder_label` | When `label` is `implicit_lecture_content` | Main label of the remaining part: `on_slide_textual_content`, `on_slide_visual_content`, `metadiscourse`, `unrelated_content`, or `""` (blank, when the whole NU is a supplement or no main label can be assigned to the remaining part) |
| `note` | Optional; required when `label` is `invalid_nu` or blank, or when `sublabel` is `other` | Free description |

If the slide image is unclear or incomplete and this affects your judgment of the relationship between the NU and the slide, leave `label` blank (`""`) and write the relevant part and the reason in `note` (manual 2.3).

The following is an example from `results/example.json` (`...` marks omitted parts).

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
