# DramaVQA

**DramaVQA: Probing Detailed Understanding of Ultra-Long TV Dramas**

[Project](https://github.com/xinyuehuo/DramaVQA) | [Videos & Metadata](https://huggingface.co/datasets/198808xc/drama_bench_data) | [Question Data](DramaVQA/) | [Evaluation](evaluation/) | [License](LICENSE)

DramaVQA is a video question answering benchmark for evaluating detailed understanding of TV dramas over extended temporal contexts. It challenges multimodal models to locate visual evidence, recognize characters and events, and reason over narratives that span many episodes.

## Overview

The full single-point evaluation set contains **20,000 multiple-choice questions across 12 TV dramas**, covering approximately **466 hours** of video. Each question has one correct answer among 2–8 options. Questions are checked for uniqueness within the entire drama, allowing the same question to be evaluated with progressively longer video inputs. The project also provides a **2,000-question single-point Mini subset**, multi-point questions, multi-hop questions, and a training set covering **three additional TV dramas**.

Key features include:

- **Four temporal tracks:** evaluate video understanding from minutes to tens of hours.
- **Detailed question coverage:** seven major categories and 28 subcategories cover visual details, actions, events, and reasoning.
- **LLM-assisted annotation with human verification:** questions undergo generation, filtering, human checking and editing, language-prior removal, and global uniqueness verification.
- **Long-context research:** support the study of video retrieval, persistent memory, and reasoning across distant events.

### Evaluation tracks

| Track | Temporal level | Average input duration |
| --- | --- | --- |
| Medium | L3 | 7.6 minutes |
| Long | L4 | 43.1 minutes |
| Extra-Long | L5 | 9.3 hours |
| Ultra-Long | L6, full drama | 38.9 hours |

The tracks vary the amount of video context available for answering a question; they do not represent four separate collections of 20,000 questions.

### Question categories

| Category | Examples of evaluated abilities |
| --- | --- |
| Visual Perception | Character appearance, objects, scenes, and on-screen text |
| Spatial Understanding | Spatial locations, relative positions, and viewpoints |
| Action and Interaction | Body movements, action participants, and object motion |
| Event Understanding | Event summaries, background, outcomes, and motivations |
| Temporal Understanding | Changes, transitions, ordering, and temporal comparisons |
| Relation & Logical Reasoning | Character relations, human-object relations, and causal inference |
| Counting | Object counts and quantity comparisons |

## Data

The dataset is organized into two parts:

- **Videos and metadata:** source videos and supporting metadata are hosted at **[198808xc/drama_bench_data](https://huggingface.co/datasets/198808xc/drama_bench_data)** on Hugging Face.
- **Question data:** question annotations are stored in the **[`DramaVQA/`](DramaVQA/) subdirectory of this repository**.

The broader data collection described in the paper contains 15 TV series totaling approximately 606 hours; 12 series form the evaluation benchmark, and three additional series are reserved for training.

### Videos and metadata

The Hugging Face data repository is organized into drama or season directories, with the following contents:

| Path within a drama or season | Contents |
| --- | --- |
| `video_data/` | Episode videos in MP4 format |
| `face_video_data/` | Videos with face annotations |
| `face_data/` | Face metadata in JSON format |
| `subtitle_data/` | Subtitle metadata in JSON format |
| `timestamp_data/` | Timestamp metadata in CSV format |
| `SHA256SUMS` | File checksums |

### Question data

All question-data paths below are relative to the project root:

| Collection | Directory | Contents |
| --- | --- | --- |
| Single-point Full | [`DramaVQA/Bench_Data/`](DramaVQA/Bench_Data/) | Full single-point evaluation set with **20,000 questions** |
| Single-point Mini | [`DramaVQA/Bench_Data_mini_2000/`](DramaVQA/Bench_Data_mini_2000/) | Single-point Mini subset with **2,000 questions** |
| Multi-point | [`DramaVQA/Multi-Point/`](DramaVQA/Multi-Point/) | Multi-point questions |
| Multi-hop | [`DramaVQA/Multi-Hop/`](DramaVQA/Multi-Hop/) | Multi-hop questions |
| Training | [`DramaVQA/Training_Set/`](DramaVQA/Training_Set/) | Training questions from **three additional dramas**, separate from the 12 evaluation dramas |

The training set contains `downton_abbey.json` (*Downton Abbey*), `ren_shi_jian.json` (*A Lifelong Journey*), and `zhen_huan_zhuan.json` (*Empresses in the Palace*).

Use the question annotations from this repository together with the corresponding videos and metadata from Hugging Face for evaluation or training.

### Download

Install the Hugging Face CLI:

```bash
python -m pip install --upgrade huggingface_hub
```

From the project root, download the videos and metadata you need. For example, download the subtitle metadata for *The Long Night*:

```bash
hf download 198808xc/drama_bench_data --repo-type dataset --include "chen_mo_de_zhen_xiang/subtitle_data/*" --local-dir data/drama_bench_data
```

Replace the include pattern with another drama or data directory as needed. You can append `--dry-run` to preview matching files and download sizes.

To download all files:

```bash
hf download 198808xc/drama_bench_data --repo-type dataset --local-dir data/drama_bench_data
```

The full repository includes video files and requires substantial disk space; selective downloads are recommended. See the [Hugging Face CLI documentation](https://huggingface.co/docs/huggingface_hub/guides/cli#hf-download) for download options.

## Evaluation

The [`evaluation/`](evaluation/) directory is reserved for the DramaVQA evaluation code. **This repository currently includes evaluation documentation; executable evaluation scripts are not yet included.** See [`evaluation/README.md`](evaluation/README.md) for the protocol and intended scope.

The protocol described in the paper evaluates single-choice QA accuracy separately on the Medium, Long, Extra-Long, and Ultra-Long tracks. Models return one option label, such as `[A]`. Inference time is reported alongside accuracy, including video captioning costs for retrieval-based and agentic methods.

## File Structure

```text
DramaVQA/
├── README.md                 # Project overview, dataset access, and evaluation information
├── LICENSE                   # MIT License for repository code and documentation
├── .gitignore                # Excludes local data and generated outputs
├── DramaVQA/                 # Question annotations stored in this repository
│   ├── Bench_Data/           # Single-point Full set (20K questions)
│   ├── Bench_Data_mini_2000/  # Single-point Mini subset (2K questions)
│   ├── Multi-Point/          # Multi-point questions
│   ├── Multi-Hop/            # Multi-hop questions
│   └── Training_Set/         # Training questions from three additional dramas
└── evaluation/               # Directory for evaluation code
    └── README.md             # Evaluation protocol and implementation status
```

The download command creates `data/drama_bench_data/` locally for Hugging Face videos and metadata. This download directory is excluded from version control; question annotations are stored in the separate `DramaVQA/` directory included in the project.

## Citation

If you use DramaVQA in your research, please cite:

```bibtex
@misc{huo2026dramavqa,
  title  = {DramaVQA: Probing Detailed Understanding of Ultra-Long TV Dramas},
  author = {Xinyue Huo and Pengfei Chen and Jiannan Ge and Kaiwen Duan and Yunning Cao and Jiacheng Shao and Lingxi Xie and Qi Tian},
  year   = {2026},
  url    = {https://github.com/xinyuehuo/DramaVQA}
}
```

## License

The code and documentation in this repository are released under the **[MIT License](LICENSE)**.
