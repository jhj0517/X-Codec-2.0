# X-Codec-2.0
Paper: LLaSA: Scaling Train-time and Test-time Compute for LLaMA-based Speech Synthesis (Comming Soon!)


## Directly used on Hugging Face

**Codec**: [xcodec2](https://huggingface.co/HKUST-Audio/xcodec2) (Please install new version xcodec2==0.1.3)
 
**LLaMA based TTS 3b version**: [Llasa-3B](https://huggingface.co/HKUST-Audio/Llasa-3B)

**LLaMA based TTS 1b version**: [Llasa-1B](https://huggingface.co/HKUST-Audio/Llasa-1B)

**LLaMA based TTS 8b version**: [Llasa-8B](https://huggingface.co/HKUST-Audio/Llasa-8B) (Comming Soon!)


## Features

- **Single Vector Quantization**
  - 65536 Codebook Size using Finite Scalar Quantization achieving 99% codebook usage. ( comparable to text tokenizers, LLaMA3 128256)
  - 50x1 Tokens per Second

- **Multilingual Speech Semantic Support**
  - Uses Wav2Vec2-BERT, a semantic encoder pre-trained on 4.5M hours of unlabeled audio data covering more than 143 languages.
  - Codec trained on 150k hours of multilingual speech data, including Emilia (En/Zh/De/Fr/Ja/Ko) and MLS (En/Fr/De/Nl/Es/It/Pt/Pl).

- **High-Quality Speech Reconstruction**
  - Transformer + Vocos Decoder
  - BigCodec encoder
  - Spec discriminator with FFT sizes {78, 126, 206, 334, 542, 876, 1418, 2296} tailored for transformer decoder. [Details here](https://openreview.net/pdf?id=4YpMrGfldX)
  - Achieving UTMOS 4.13 WER 2.47 (hubert-large-ls960-ft)  sim 0.82 (wavlm_large_finetune) stoi 0.92  pesq-nb 3.05  pesq-wb 2.44 on librispeech-test-clean reconstruction (gt: WER 2.09 UTMOS 4.09)
  - Only for 16kHz speech


##  Commandline Usage
## Setup
Code is tested on `python3.9`

Please follow the following steps to setup your environment
1. Clone this repo
2. conda create --name xcodec2 python=3.9 
3. conda activate xcodec2  
2. `pip install -r requirements.txt`
3. [Download the pretrained checkpoint here](https://huggingface.co/HKUST-Audio/xcodec2/blob/main/ckpt/epoch%3D4-step%3D1400000.ckpt)


## Inference
```bash
python inference.py  
```
 
## Train
To train a XCodec2, firstly you have to prepare your data 

1. Make a file list by:
```bash
python get_tsv.py
```

2. Train a X-Codec-2.0 with the default setting by:

```bash
python train.py log_dir=/path/to/log_dir
```

## Large-scale training, Batch inference and large-scale code extracting:

Batch inference
```bash
python inference_save_code.py
```
Training
```bash
Sbatch train_slurm.sh
```

Code extracting
```bash
Sbatch large_scale_save_code.sh
```

Code will save in output folder with the same subfolder structure for audio file.


 
## Acknowledgement
I would like to extend a special thanks to authors of BigCodec, since our code base is mainly borrowed from  [BigCodec](https://github.com/Aria-K-Alethia/BigCodec).
