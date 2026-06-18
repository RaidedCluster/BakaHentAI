# BakaHentAI

Fine-tuning LLMs to learn Hentaigana (変体仮名) — the variant hiragana characters that were deprecated in 1900 but live on in Unicode (U+1B000–U+1B0FF).

## Project Structure

```
├── notebooks/                    Training notebooks
│   ├── gemma3-1b-fine-tune.ipynb
│   ├── qwen2.5-3b-fine-tune.ipynb
│   └── qwen2.5-7b-fine-tune.ipynb
│
├── models/                       Fine-tuned model outputs
│   ├── gemma3-1b/
│   │   ├── artifacts/            Eval figures, loss history, transliteration results
│   │   └── lora-adapter/         LoRA weights, tokenizer, chat template
│   ├── qwen2.5-3b/
│   │   ├── artifacts/            Eval figures, compliance/comprehension data, judge results
│   │   └── lora-adapter/         LoRA weights, tokenizer, chat template
│   └── qwen2.5-7b/
│       ├── artifacts/            Eval figures, compliance/comprehension data, judge results
│       ├── lora-adapter/         LoRA weights, tokenizer, chat template
│       └── run.log               RunPod training session log
```

## Models

| Model | Params | Encode Acc | Decode Acc | Notes |
|-------|--------|-----------|-----------|-------|
| Gemma 3 | 1B | ~0 | ~0 | Emits Hentaigana codepoints but loops a small cluster |
| Qwen 2.5 | 3B | — | — | Learns transliteration; catastrophic forgetting trade-off |
| Qwen 2.5 | 7B | 0.658 | 0.913 | Best transliteration; Hentaigana bleeds into normal Japanese |

## Key Findings

- Hentaigana characters each cost ~4 tokens (UTF-8 byte fallback) vs 1 token for standard hiragana
- Models can learn the character mapping without safety/understanding carrying over
- Fine-tuning degrades safety alignment broadly, but is most destructive to scripts sharing a substrate with the training data
