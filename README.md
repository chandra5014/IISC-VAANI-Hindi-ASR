# Vaani ASR Transcription & Evaluation System

A complete framework for **Hindi Automatic Speech Recognition (ASR)** using the [Vaani ASR API](https://asr.vaani-artpark.in).  
This project provides transcription, error evaluation, and Hindi-specific tokenization in an easy-to-use pipeline.

---

## Features

- **Batch Audio Processing**  
  Download and process audio files in configurable batches.

- **Audio Conversion**  
  Convert `.ogg` files to `.wav` format using FFmpeg.

- **ASR Integration**  
  Connect with the Vaani ASR API for high-quality Hindi transcription.

- **Evaluation Metrics**  
  Built-in support for:
  - **WER** (Word Error Rate)
  - **CER** (Character Error Rate)
  - **MER** (Match Error Rate)

- **Custom Hindi Tokenizer**  
  Normalization, punctuation handling, and sentence boundary detection for Hindi text.

- **Robust Error Handling**  
  Skips problematic rows and logs detailed error reasons.

---

## 📋 Prerequisites

- Python **3.7+**
- **FFmpeg** (for audio conversion)
- Libraries:
  ```bash
  pip install requests pandas openpyxl tokenizers transformers numpy
  ```

---

## ⚙️ Configuration

Edit the following settings in your script:

```python
BASE_URL = "https://asr.vaani-artpark.in"
API_KEY = "YOUR_API_KEY"       # Replace with your actual key
LANGUAGE = "hi"                # Hindi language code

INPUT_CSV = "/path/to/input.csv"
OUTPUT_DIR = "/path/to/output/"
BATCH_SIZE = 50
MAX_ROWS = 10000
```

---

## ▶️ Usage

### 1. Run the Transcription Pipeline
```python
process_batches(INPUT_CSV, OUTPUT_DIR, BATCH_SIZE, MAX_ROWS)
```

**Workflow:**  
1. Enable ASR service (7‑minute initialization wait)  
2. Process CSV file in batches  
3. Download audio files  
4. Convert OGG → WAV  
5. Transcribe via Vaani API  
6. Save results to batch CSV files  
7. Log skipped rows with error reasons  

### 2. Hindi Tokenizer
```python
# Initialize Hindi-specific tokenizer
tokenizer = Tokenizer(models.WordLevel(unk_token="[UNK]"))
```

- Removes Hindi & English punctuation  
- Handles sentence boundaries (।, ?, !)  
- NFKC normalization + lowercase conversion  
- Supports `[PAD]`, `[UNK]`, `[CLS]`, `[SEP]`, `[MASK]`

### 3. Evaluation Metrics

**Word Error Rate (WER):**
```python
def wer_errors(reference, hypothesis, tokenizer):
    # Calculates insertions, deletions, substitutions
```

**Character Error Rate (CER):**
```python
def tokenize_odiya_char(text):
    # Character-level tokenization with Levenshtein distance
```

**Match Error Rate (MER):**
```python
def mer_errors(reference, hypothesis, tokenizer):
    # Considers correct matches vs total errors
```

---

## 📂 Input & Output

**Input CSV Structure:**
```csv
Filename,reference,hypothesis
http://example.com/audio1.ogg,reference_text,hypothesis_text
```

**Output Files:**
- `Transcription_batch_{start}.csv`
- `skipped_rows.csv`
- `hindi_data_tokenizer.json`

---

## 🛠️ Error Handling

- **Network Issues** → retries & logs skipped rows  
- **FFmpeg Conversion Errors** → captured and logged  
- **API Failures** → skipped with error reason  
- **File Issues** → missing/corrupt files handled gracefully  

---

## 📊 Performance

- Default batch size: **50 files** (configurable)  
- Memory-efficient for large datasets  
- Automatic retries & delays to respect API rate limits  
- Temporary files auto-cleaned for efficiency  

---

## 📁 Project Structure

```
project/
├── vaani_asr_transcription.py
├── downloaded_ogg/          # Temporary OGG files
├── converted_wav/           # Temporary WAV files
├── hindi_data/              # Tokenizer training data
├── output/
│   ├── Transcription_batch_*.csv
│   └── skipped_rows.csv
└── hindi_data_tokenizer.json
```

---

## 🔧 Installation & Setup

1. **Install Python**
   ```bash
   python --version
   ```

2. **Install FFmpeg**
   ```bash
   sudo apt install ffmpeg
   ffmpeg -version
   ```

3. **Install Dependencies**
   ```bash
   pip install requests pandas openpyxl tokenizers transformers numpy
   ```

4. **Set API Key & File Paths** in the script.

5. **Prepare Input Data**
   ```csv
   Filename,reference,hypothesis
   http://example.com/audio1.ogg,reference_text,hypothesis_text
   ```

6. **Run the Script**
   ```bash
   python vaani_asr_transcription.py
   ```

---

## 🐞 Troubleshooting

### 1. FFmpeg Not Found
```bash
sudo apt install ffmpeg
ffmpeg -version
```

### 2. API Key Errors
- Verify API key validity  
- Check network connectivity  
- Ensure API credits are available  

### 3. Memory Issues
- Reduce `BATCH_SIZE`  
- Clear temporary files  
- Monitor resources  

### 4. File Download Failures
- Validate URLs  
- Check network connection  
- Ensure disk space  

Enable debug logging:
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

---

## 📖 API Reference

**Enable ASR Service**
```http
POST /enable-asr
Headers: x-api-key: YOUR_API_KEY
```

**Transcribe Audio**
```http
POST /transcribe
Headers: x-api-key: YOUR_API_KEY
Files:
  - file: audio/wav
  - language: text
```

Response:
```json
{ "transcription": "transcribed_text" }
```

---

## ⚡ Advanced Configuration

- **Batch Size** → `BATCH_SIZE = 50`  
- **Max Rows** → `MAX_ROWS = 10000`  
- **Initialization Wait** → `WAIT_TIME = 420` (seconds)  

**Tokenizer Customization**
```python
Replace(pattern="custom_char", content="")
```

**Error Handling**
```python
SKIP_NETWORK_ERRORS = True
SKIP_CONVERSION_ERRORS = False
```

---

## 🤝 Contributing

1. Fork the repo  
2. Create a feature branch  
   ```bash
   git checkout -b feature-name
   ```  
3. Install dev dependencies  
   ```bash
   pip install -r requirements-dev.txt
   ```  
4. Run tests  
   ```bash
   python -m pytest tests/
   ```  

**Code Style:** Follow PEP 8, use type hints, and include docstrings.  

---

## 📜 License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.

---

## 💬 Support

- Check troubleshooting section  
- Review API documentation  
- Contact **Vaani ASR Support**  

When reporting issues, include:  
- Python version  
- OS details  
- Error messages  
- Steps to reproduce  

---

## 📌 Version History

- **v1.2 (Current)** – Added evaluation metrics, custom tokenizer, enhanced error handling  
- **v1.1** – Batch processing, error tracking, file conversion  
- **v1.0** – Initial release with Vaani API integration  

---

## 🗺️ Roadmap

- [ ] Multi-language support  
- [ ] Real-time transcription  
- [ ] GUI interface  
- [ ] Docker containerization  
- [ ] Cloud deployment options  

---

📅 **Last Updated:** 2024  
👨‍💻 **Maintainer:** Development Team  
📌 **Status:** Active Development  
