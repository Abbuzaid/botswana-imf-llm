# Botswana IMF LLM pipeline

This repository turns the IMF's 2025 Article IV report on Botswana (Country Report No. 25/325, December 2025) into checked summaries, structured indicators, a retrieval system and a dashboard. Everything is in one Colab notebook, `Botswana_IMF_LLM_Pipeline.ipynb`.

## How to run

Open the notebook in Google Colab with `data/` and `cache/` beside it, then choose *Runtime > Run all*. Every model reply is stored in `cache/ledger.jsonl`, so a rerun reads the stored replies, makes no model calls and needs no key.

New model calls need a Gemini API key saved as a Colab secret named `GEMINI_FREE_KEY`. No key is written in the notebook.

## What is in the folder

- `data/` holds the IMF report as a PDF.
- `cache/ledger.jsonl` holds the stored model replies, `cache/ocr_pages.json` the stored OCR text of the picture pages, and `cache/vectors_*.npy` the embeddings.
- `results/` holds the JSON outputs, `dashboard.png` and `extension.png`.
- `requirements.txt` lists the versions used in the saved run.

## Notes on the method

Five pages of the report are pictures, and Table 5 (the only source of the Tier 1 capital ratio) is one of them. The notebook reads three of the five with Tesseract OCR and restores decimal points that OCR loses in Table 5. The OCR text used by the saved run came from Tesseract 5.3.4 on Colab and is stored in `cache/ocr_pages.json`. It is looked up by a hash of the image bytes, so use the versions in `requirements.txt`: a different Pillow or Tesseract version could change the text, and the stored replies would then no longer match.

The extension compares a single query with multi-query retrieval from three policy perspectives, merged by CombMNZ.

## Licence

MIT, see `LICENSE`.
