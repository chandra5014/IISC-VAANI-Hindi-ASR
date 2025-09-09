# Vaani ASR Transcription and Evaluation System

Overview
This Python script provides a comprehensive Automatic Speech Recognition (ASR) evaluation framework specifically designed for Hindi language processing. The system integrates with the Vaani ASR API to perform batch audio transcription and quality assessment through a multi-stage pipeline.

Features
Core Functionality
Batch Audio Processing: Downloads and processes audio files in configurable batches

Format Conversion: Converts OGG audio files to WAV format using FFmpeg

ASR Integration: Interfaces with Vaani ASR API for Hindi transcription

Quality Metrics: Calculates WER, CER, and MER for transcription accuracy

Custom Tokenization: Hindi-specific tokenizer with punctuation handling

Error Handling: Robust error management with skipped row tracking

Supported Metrics
WER (Word Error Rate): Measures word-level transcription accuracy

CER (Character Error Rate): Evaluates character-level precision

MER (Match Error Rate): Alternative error measurement methodology

Prerequisites
System Requirements
Python 3.7+

FFmpeg (for audio conversion)

Google Colab environment (as configured)

Required Libraries
pip install requests pandas openpyxl tokenizers transformers numpy
Configuration
API Settings
BASE_URL = "https://asr.vaani-artpark.in"
API_KEY = "XXXXXXXXXXXXXXXX"  # Replace with your API key
LANGUAGE = "hi"  # Hindi language code
File Paths
INPUT_CSV = "/path/to/your/input.csv"
OUTPUT_DIR = "/path/to/output/directory"
BATCH_SIZE = 50  # Configurable batch size
MAX_ROWS = 10000  # Maximum rows to process
Usage
1. Audio Transcription Pipeline
The main processing function handles the complete workflow:

process_batches(INPUT_CSV, OUTPUT_DIR, BATCH_SIZE, MAX_ROWS)
Workflow Steps:

Enables ASR service with 7-minute initialization wait

Processes CSV file in configurable batches

Downloads audio files from URLs

Converts OGG to WAV format

Sends audio to Vaani ASR API

Saves transcriptions to batch CSV files

Tracks and saves skipped rows with error reasons

2. Custom Hindi Tokenizer
Creates a specialized tokenizer for Hindi text processing:

# Initialize tokenizer with Hindi-specific normalization
tokenizer = Tokenizer(models.WordLevel(unk_token="[UNK]"))
Features:

Removes Hindi and English punctuation

Handles sentence endings (।, ?, !, etc.)

NFKC normalization and lowercase conversion

Special tokens: [PAD], [UNK], [CLS], [SEP], [MASK]

3. Evaluation Metrics
Word Error Rate (WER)
def wer_errors(reference, hypothesis, tokenizer):
    # Calculates insertions, deletions, and substitutions
    # Returns total errors and reference length
Character Error Rate (CER)
def tokenize_odiya_char(text):
    # Character-level tokenization
    # Uses Levenshtein distance for error calculation
Match Error Rate (MER)
def mer_errors(reference, hypothesis, tokenizer):
    # Alternative error measurement
    # Considers correct matches vs total errors
Input/Output Format
Input CSV Structure
Filename,reference,hypothesis
http://example.com/audio1.ogg,reference_text,hypothesis_text
Output Files
Batch Transcriptions: Transcription_batch_{start}.csv

Skipped Rows: skipped_rows.csv

Tokenizer: hindi_data_tokenizer.json

Error Handling
The system includes comprehensive error management:

Network Issues: Handles download failures

Conversion Errors: Manages FFmpeg conversion problems

API Failures: Catches transcription service errors

File Issues: Manages missing or corrupted files

All errors are logged with specific reasons in the skipped rows file.

Performance Considerations
Batch Processing
Default batch size: 50 files

Configurable based on system resources

Memory-efficient processing for large datasets

API Rate Limiting
7-minute initialization wait for ASR service

Built-in delays between requests

Automatic retry mechanisms

# File Structure
project/
├── vaani_asr_transcription.py
├── downloaded_ogg/          # Temporary OGG files
├── converted_wav/           # Temporary WAV files
├── hindi_data/              # Tokenizer training data
├── output/
│   ├── Transcription_batch_*.csv
│   └── skipped_rows.csv
└── hindi_data_tokenizer.json
Troubleshooting
Common Issues
FFmpeg Not Found

Install FFmpeg: sudo apt install ffmpeg

Verify installation: ffmpeg -version

API Key Issues

Verify API key validity

Check network connectivity

Ensure sufficient API credits

Memory Issues

Reduce batch size

Clear temporary directories

Monitor system resources

Debug Mode
Enable verbose logging by modifying print statements or adding logging configuration.

API Reference
Vaani ASR Endpoints
Enable ASR Service
POST /enable-asr
Headers: x-api-key: YOUR_API_KEY
Transcribe Audio
POST /transcribe
Headers: x-api-key: YOUR_API_KEY
Files: file (audio/wav), language (text)
Contributing
Fork the repository

Create feature branch

Implement changes with tests

Submit pull request

License
This project is licensed under the MIT License - see the LICENSE file for details.

Support
For issues and questions:

Check troubleshooting section

Review API documentation

Contact Vaani ASR support

Version History
v1.0: Initial release with basic transcription

v1.1: Added batch processing and error handling

v1.2: Implemented evaluation metrics and custom tokenizer

Note: This script is designed for research and evaluation purposes. Ensure compliance with audio data usage policies and API terms of service.

I've created a comprehensive README.md file for your Vaani ASR transcription script. Here's the content:

