
# WebUI + Qwen3 Setup for Windows + NVIDIA RTX 5060 Ti

> Полноценный запуск Qwen3 (в том числе Qwen3-4B и 8B в int4/int8 и FP16) в `text-generation-webui` на Windows 11 + RTX 5060 Ti (GDDR7, 16 ГБ). Поддержка HF моделей, GGUF, FlashAttention 2, и int4 BitsAndBytes.

---

## ✅ Проверено и работает

- `Qwen3-0.5B`, `Qwen3-1.5B`, `Qwen3-4B`, `Qwen3-8B` (в int4, int8 и FP16)
- `Qwen2.5-Coder-1.5B`, `Qwen2.5-Coder-3B-Instruct`
- `transformers`, `xformers`, `flash-attn` — установлены вручную
- Запуск через `text-generation-webui`
- Windows 10, CUDA 12.8, Python 3.12, torch 2.7.0 + cu128
- WebUI запуск: `python server.py --chat --model Qwen3-4B`

---

## 📦 Установленные компоненты

- `torch==2.7.0+cu128` (официальный build)
- `transformers` с GitHub (`main`, поддержка Qwen3)
- `xformers` с [HF link](https://huggingface.co/lienzzzz/xformers-windows-blackwell2.0-cuda128-RTX50X0)
- `flash-attn` (опционально)
- `bitsandbytes==0.43.1.post4`
- `gradio==4.44.1`, `gradio_client==1.3.0`
- `numba==0.61.0`, `numpy==2.1.3`
- `text-generation-webui` из ветки main

---

## 🛠 Установка

1. Установить [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

2. Создать окружение:

```bash
conda create -n textgen python=3.12 -y
conda activate textgen
```

3. Клонировать WebUI и перейти в папку:

```bash
git clone https://github.com/nan0bug00/text-generation-webui.git
cd text-generation-webui
```

4. Установить зависимости:

```bash
pip install torch==2.7.0+cu128 --index-url https://download.pytorch.org/whl/cu128
pip install bitsandbytes==0.43.1.post4
pip install git+https://github.com/huggingface/transformers
pip install git+https://github.com/huggingface/peft
pip install git+https://github.com/huggingface/accelerate
pip install -r requirements.txt
```

5. Установить xformers (скачать `.whl` с HuggingFace и установить):

```bash
pip install https://huggingface.co/lienzzzz/xformers-windows-blackwell2.0-cuda128-RTX50X0/resolve/main/xformers-0.0.26.dev0-cp312-cp312-win_amd64.whl
```

6. Установить дополнительные библиотеки:

```bash
pip install fastapi==0.112.4 starlette==0.38.6 gradio==4.44.1 gradio_client==1.3.0 Jinja2==3.1.4 filelock==3.16.1 fsspec==2024.10.0
pip install llvmlite==0.44.0 numba==0.61.0 numpy==2.1.3
```

---

## 🚀 Запуск модели

```bash
python server.py --chat --model Qwen_Qwen3-4B
```

Параметры можно задать через `cmd_args.txt`, `settings.yaml` или в интерфейсе WebUI.

---

## 📊 Производительность (5060 Ti)

| Модель        | FP16       | int4       | int8       | RAM VRAM | tok/s |
|---------------|------------|------------|------------|----------|-------|
| Qwen3-0.5B     | ✅         | ✅         | ✅         | ~2–3 ГБ  | 35+   |
| Qwen3-1.5B     | ✅         | ✅         | ✅         | ~4–6 ГБ  | 20+   |
| Qwen3-4B       | ✅         | ✅         | ✅         | ~9–12 ГБ | 10–13 |
| Qwen3-8B       | ⚠️ (частично) | ✅         | ✅         | ~14–15 ГБ | ~6–7 |

---

## 🧠 Советы

- Для ускорения генерации используйте FlashAttention 2 + `bfloat16`
- Ограничьте `max_memory` для безопасного запуска:  
  `--max_memory {0: '15300MiB', 'cpu': '99GiB'}`
- Используйте `low_cpu_mem_usage=True` и `device_map='auto'` для оптимального размещения
- Убедитесь, что драйверы NVIDIA ≥ 550.54 и CUDA ≥ 12.8

---

## 📁 Примеры моделей

```bash
/models
├── Qwen_Qwen3-4B
├── Qwen_Qwen3-8B
└── Qwen2.5-Coder-3B-Instruct
```

GGUF-модели также поддерживаются (через llama.cpp backend).

---

## 📷 Скриншоты

![webui-qwen3](docs/webui_qwen3.jpg)

---

## 👨‍💻 Автор

**@Skyruller** — тестирование и отладка Qwen3 на RTX 5060 Ti  

