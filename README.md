# Supervised Fine-Tuning (SFT) dengan LoRA
## Studi Kasus: Bahasa Daerah & Budaya Nusantara Indonesia

**Mata Kuliah:** Pemrosesan Bahasa Alami  
**Nama:** Bruri Wijayanto  
**NIM:** 25/568904/PPA/07156  
**Platform:** Google Colab (GPU Tesla T4)

---

## Deskripsi

Repositori ini berisi implementasi **Supervised Fine-Tuning (SFT)** pada small language model menggunakan teknik **LoRA (Low-Rank Adaptation)**. Model dilatih pada dataset instruksi berbasis bahasa daerah dan budaya Nusantara Indonesia yang dibuat secara sintetis.

> ⚠️ Tidak menggunakan Unsloth, OpenAI Fine-tuning API, atau layanan as-a-service lainnya.  
> Stack yang digunakan: `transformers` + `peft` + `trl` + `datasets` (vanilla HuggingFace).

---

## Model & Dataset

| Komponen | Detail |
|----------|--------|
| **Base Model** | `TinyLlama/TinyLlama-1.1B-Chat-v1.0` (1,1B parameter) |
| **Dataset** | Sintetis buatan sendiri — 42 contoh instruksi |
| **Topik dataset** | Adat Jawa, Sunda, Batak, Bali, Bugis, Betawi, kuliner, cerita rakyat, terjemahan bahasa daerah |
| **LoRA config** | r=16, alpha=32, 7 target modules |
| **Training** | 10 epoch, batch efektif=2, lr=2e-4, 180 steps |
| **GPU** | Tesla T4 (15,6 GB VRAM) |

---

## Struktur Repositori

```
sft-lora-bruri/
├── sft_lora_bahasa_daerah.ipynb   ← Notebook utama (dengan output tersimpan)
├── requirements.txt               ← Daftar library
├── data/
│   └── dataset.json               ← Dataset sintetis 42 contoh
└── results/
    ├── training_curve.png         ← Grafik training loss
    └── before_after.md            ← Perbandingan respons base vs fine-tuned
```

> **Catatan:** Adapter LoRA (52,9 MB) tidak di-push ke GitHub karena ukurannya. Tersimpan di Google Drive.

---

## Hasil Training

| Epoch | Train Loss | Eval Loss |
|-------|-----------|-----------|
| 0 | 2,1576 | 1,9310 |
| 2 | 1,3430 | 2,0241 |
| 4 | 0,5960 | 2,4098 |
| 6 | 0,2255 | 2,7681 |
| 8 | 0,0747 | 2,8590 |
| 9 | 0,1061 | 2,8677 |

Train loss turun drastis dari 2,16 → 0,07, namun eval loss naik dari 1,93 → 2,87 — menunjukkan **overfitting klasik** akibat dataset yang kecil (37 contoh training). Fenomena ini dianalisis lengkap di laporan dan jawaban Q4.

---

## Parameter LoRA

| Kategori | Jumlah | Persentase |
|----------|--------|------------|
| Dapat dilatih (LoRA) | 12.615.680 | **1,13%** |
| Frozen (base model) | 1.100.048.384 | 98,87% |
| Total | 1.112.664.064 | 100% |
| Ukuran adapter | 52,9 MB | vs. 2,2 GB full model |

---

## Cara Menjalankan

### 1. Buka di Google Colab
Klik badge di bawah atau upload file `.ipynb` secara manual:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

### 2. Aktifkan GPU
```
Runtime → Change runtime type → T4 GPU
```

### 3. Install dependencies
```bash
pip install transformers==4.44.0 peft==0.12.0 trl==0.10.1 \
    datasets accelerate bitsandbytes matplotlib
```

### 4. Jalankan semua sel
```
Runtime → Run all  (Ctrl+F9)
```

Estimasi waktu total: **~15–20 menit** (termasuk download model 2,2 GB dan training 180 steps).

---

## Load Adapter untuk Inference

```python
from transformers import AutoTokenizer, AutoModelForCausalLM
from peft import PeftModel
import torch

MODEL_ID    = "TinyLlama/TinyLlama-1.1B-Chat-v1.0"
ADAPTER_DIR = "./output/adapter"   # path ke adapter yang disimpan

tokenizer = AutoTokenizer.from_pretrained(MODEL_ID)
base      = AutoModelForCausalLM.from_pretrained(MODEL_ID, torch_dtype=torch.float16)
model     = PeftModel.from_pretrained(base, ADAPTER_DIR)
model.eval()

# Generate
messages = [{"role": "user", "content": "Apa makna filosofi Jawa 'Memayu Hayuning Bawana'?"}]
prompt   = tokenizer.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
inputs   = tokenizer(prompt, return_tensors="pt")

with torch.no_grad():
    output = model.generate(**inputs, max_new_tokens=200, do_sample=True, temperature=0.7)

print(tokenizer.decode(output[0][inputs["input_ids"].shape[1]:], skip_special_tokens=True))
```

---

## Referensi

- Hu et al. (2021). [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685)
- Ouyang et al. (2022). [InstructGPT: Training language models to follow instructions](https://arxiv.org/abs/2203.02155)
- Cahyawijaya et al. (2023). [Cendol: Open Instruction-tuned LLMs for Indonesian Languages](https://aclanthology.org/2024.acl-long.796/)
- [TRL SFTTrainer Docs](https://huggingface.co/docs/trl/sft_trainer)
- [PEFT / LoRA Docs](https://huggingface.co/docs/peft)
- [Dataset bryandts — 9 bahasa daerah](https://huggingface.co/datasets/bryandts/instruction-dataset-indo-java-sunda-bali-gayo-batak-alas-minang-betawi)
