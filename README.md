# 1. BERT-Based Text Classification for Multi-Class Dataset (43 Categories)

This is an implementation of fine-tuning a BERT model for text classification using a dataset with 43 classes.

---

## Dataset used

- Input file: `train.csv`
- Columns used:
  - `Text`: raw textual input
  - `Category`: target class
- Total classes: **43**
- Preprocessed using lemmatization, stop-word removal, punctuation stripping, and lowercasing.

---

## Model used

- Base model: `bert-base-uncased` from Hugging Face Transformers
- Tokenizer: `BertTokenizer` with max length of 256
- Preprocessing:
  - Lowercasing
  - Removal of punctuation
  - Stop-word removal (`nltk`)
  - Lemmatization (`WordNetLemmatizer`)

---

## Training Details

- Framework: Hugging Face `Trainer`
- Split: 80% train / 20% validation
- Epochs: `3`
- Batch size: `8` (reduced to prevent Colab OOM errors)
- Evaluation: Performed after every epoch
- Metrics: Accuracy, Precision, Recall, F1 Score

---

## Final Results

- **Validation Accuracy:** `0.8707` (87.07%)
- **Model saved** in `bert-finetuned/` directory
- Tokenizer also saved for inference

---

## Common Issues Faced and Fixes

### 1.  Colab RAM Crash (Out of Memory)
**Fix:**
- Reduced `max_len` to `256`
- Reduced `per_device_train_batch_size` to `8`

### 2. Weights & Biases API Key Prompt
**Fix:** disabled it by importing os 

### [Colab Notebook Link](https://colab.research.google.com/drive/1RatBCdqygRkc_bTMfUhVVYHiBpFNVdCo?usp=sharing)

---

# 2. Fashion-MNIST Classification using ResNet50

This project uses **transfer learning** with a pre-trained **ResNet50** convolutional neural network to classify images from the [Fashion-MNIST dataset](https://drive.google.com/drive/folders/1qZNwYOW53GZYZjpmsSpZMBNh1PEQumnb?usp=sharing) into 10 categories of clothing and accessories.
It explores key deep learning concepts including transfer learning, freezing layers, fine-tuning, and data augmentation.

## Dataset

The dataset consists of:
- 60,000 training images
- 10,000 test images
- 10 classes (T-shirt/top, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag, Ankle boot)

Each image is a **28x28 grayscale image**.

---
## Setup steps for local execution

### Environment
- Python 3.8+
- Tensorflow 2.12+
- Numpy, matplotlib

## Workflow Summary

- **Preprocessing**
     - images of the given dataset(28 x 28, grayscale) resized to (224 X 224, RGB)
     - Normalized pixel values to [0,1] (dividing by 255.0)
     - One-hot encoding for labels
- Base Model : **ResNet50**
     - pretrained on ImageNet
     - Top layers removed
     - Custom layers added for clothing classification (Dense, Dropout, Softmax)
- **Training**
     - First trained with frozen ResNet50 layers( only tarined custom head)
     - Later, fine tuned last 40 layers with a lower learning late(1e-4)
- **Evaluation**
     - Achieved **94%** accuracy on validation set

---
## Refrences and External Resources
  - [Tensorflow Transfer Learning Docs](https://www.tensorflow.org/tutorials/images/transfer_learning)
  - [Keras Applications: ResNet50](https://keras.io/api/applications/resnet/#resnet50-function)
  - StackOverflow (error-specific help)
           
---
## Error Handling and Troubleshooting
  - **RAM Crash** in colab session
    Fix : Switched runtime to GPU, Reduced Batch size temporarily to lessen memory load.
  - Model File(.keras) was too large to upload on GitHub directly, so instead uploaded the model and google drive and gave a 
    link in this readme file below.

---
## Files

- `Fashion_MNIST_ResNet50_TransferLearning.ipynb` — Full code notebook
- `resnet50_fashion_mnist.keras` — Trained model file (270 MB)
  - [Download from Google Drive](https://drive.google.com/file/d/1aOiuV_fWZVmxNEgWEJB7kQPeMMe0AXMx/view?usp=sharing)

---

## Run It on Google Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1tK8qkGcEaGuP8MNCsc6Xgkdf7Bl5M-ec?usp=drive_link)

---

# 3. RAG Chatbot with Groq API + Agentic Architecture

This is a context-aware PDF chatbot that uses **retrieval-augmented generation (RAG)** with **open-source LLMs via Groq**.
It uses **FAISS** for chunk retrieval and the **Groq API** to generate responses from open-source LLMs.

Supports:
- FAISS-based semantic search
- Follow-up question rewriting
- Agent-based processing (extract → synthesize → answer)
- History-aware multi-turn conversations

---
**(**IMP**)In case, the ipynb file named RAG_chatbot does not render on github, please open it using this colab link below**
## Demo (Colab)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/10hx93LWFNTWwu1306h5x0Kv2IEYZnUju?usp=sharing)

## PDF used for training and testing this chatbot : PP-001-Space.pdf (uploaded separately in this repository by this name) 

---
## Workflow summary

- **PDF Ingestion**: Extracts and chunks readable content from PDFs.
- **Vector Search**: Uses `sentence-transformers` + `FAISS` for fast retrieval by creating embeddings.
- **Open-Source LLMs**: Uses `llama3-8b-8192` or `mixtral` models via Groq API.
- **Implementing an Agentic Architecture** with:
  - `InfoExtractionAgent` → pulls key facts
  - `SynthesisAgent` → organizes knowledge
  - `QueryAgent` → answers the user’s question
- **Follow-up Question Handling**: Rewrites vague questions using chat history.

---
## References/ External Resources
  - [Groq API](https://console.groq.com)
  - [sentence-transformers](https://www.sbert.net/)
  - [FAISS-Facebook AI Similarity Search](https://github.com/facebookresearch/faiss)
  - [PyMuPDF](https://github.com/pymupdf/PyMuPDF)
  - [Agentic RAG concept](https://docs.llamaindex.ai/en/stable/examples/agentic/)

---
## Error Handling and Troubleshooting
- **FAISS returning just letters like 'e' or 's'(no related text)**
  -Fix : ensured chunk_texts = chunks.copy() was right after chunking, before building the FAISS index. The problem was 
         maybe chunk_texts didnt match embedded chunks.
- **GitHub showing "invalid notebook" or metadata error** : wasnt able to fix this so uploaded the notebook link in the 
                                                            README file
- **Same answer even with history aware chat**
   Fix : Rewriting vague questions using chat history solved this.

---
