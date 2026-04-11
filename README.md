# 🛡️ Applications of AI in Information Security

> Three machine learning systems tackling real-world cybersecurity problems — malware detection, network intrusion detection, and spam filtering.

---

## 📁 Project Structure

```
COAE/
└── Applications Of AI in Infosec/
    └── Malware Classifier/
        ├── model.ipynb                      # ResNet50 malware image classifier
        ├── preprocessing.ipynb              # Dataset preprocessing pipeline
        ├── preprocessingandloading.ipynb    # Data loading utilities
        ├── split-folders.ipynb              # Train/test split helper
        ├── graph.ipynb                      # Visualization notebook
        ├── newdata/                         # Preprocessed split dataset
        └── malware_classifier.pth           # Saved model weights
```

---

## 🔬 Models Overview

### 1. 🦠 Malware Image Classifier
**Approach:** Transfer Learning with ResNet50  
**Dataset:** [MALIMG Dataset](https://www.kaggle.com/datasets/vishal2430/malware-dataset) — malware binaries visualized as grayscale images

**How it works:**
- Malware binaries are converted into bitmap images and classified by family
- A pretrained ResNet50 backbone (ImageNet weights, frozen) extracts visual features
- A custom classification head (`2048 → 1000 → N classes`) is trained on top
- Images are resized to `75×75` and normalized with ImageNet statistics

**Architecture:**
```
ResNet50 (frozen) → Linear(2048, 1000) → ReLU → Linear(1000, N_classes)
```

**Training Config:**
| Parameter | Value |
|-----------|-------|
| Optimizer | Adam |
| Loss | CrossEntropyLoss |
| Epochs | 10 |
| Train Batch Size | 512 |
| Test Batch Size | 1024 |

---

### 2. 🌐 Network Anomaly Detection
**Approach:** Random Forest Classifier  
**Dataset:** [NSL-KDD](https://www.unb.ca/cic/datasets/nsl.html) (`KDD+.txt`)

**How it works:**
- Classifies network traffic into 5 categories: Normal, DoS, Probe, Privilege Escalation, and Access attacks
- Categorical features (`protocol_type`, `service`) are one-hot encoded
- 34 numeric network traffic features are used alongside encoded categoricals
- An 80/20 train-test split is used, with a further 70/30 validation split within training

**Attack Categories:**
| Label | Category | Examples |
|-------|----------|---------|
| 0 | Normal | — |
| 1 | DoS | neptune, smurf, teardrop |
| 2 | Probe | nmap, portsweep, satan |
| 3 | Privilege Escalation | buffer_overflow, rootkit |
| 4 | Access | ftp_write, guess_passwd |

**Results:**
| Metric | Score |
|--------|-------|
| Accuracy | 99.49% |
| Precision | 99.47% |
| Recall | 99.49% |
| F1-Score | 99.47% |

> ⚠️ **Note:** The `Privilege Escalation` class has low recall (F1: 0.34) due to severe class imbalance — only 21 test samples. Consider oversampling (SMOTE) or class weighting to address this.

---

### 3. 📧 SMS Spam Detection
**Approach:** Multinomial Naive Bayes with NLP Pipeline  
**Dataset:** [SMS Spam Collection](https://archive.ics.uci.edu/dataset/228/sms+spam+collection)

**How it works:**
- Raw SMS text is cleaned, tokenized, stopword-filtered, and stemmed (Porter Stemmer)
- Bag-of-Words features are extracted using `CountVectorizer` with unigram + bigram support
- `GridSearchCV` with 5-fold cross-validation tunes the Laplace smoothing parameter `alpha`
- Best `alpha`: **0.2**

**Example Predictions:**
| Message | Prediction | Spam Prob |
|---------|-----------|-----------|
| "Congratulations! You've won a $1000 gift card..." | 🔴 Spam | 1.00 |
| "Hey, are we still meeting for lunch?" | 🟢 Not Spam | 0.00 |
| "Urgent! Your account has been compromised..." | 🔴 Spam | 0.96 |
| "FREE entry in a weekly competition to win an iPad!" | 🔴 Spam | 1.00 |

---

## ⚙️ Setup & Installation

```bash
# Clone the repo
git clone https://github.com/your-username/COAE.git
cd COAE

# Install dependencies
pip install torch torchvision scikit-learn pandas numpy matplotlib seaborn nltk joblib
```

### For the Malware Classifier
Download and prepare the MALIMG dataset, then run the notebooks in this order:
1. `preprocessing.ipynb`
2. `split-folders.ipynb`
3. `model.ipynb`

### For Network Anomaly Detection
Download `KDD+.txt` from the NSL-KDD dataset and place it in the project root, then run the classifier script.

### For Spam Detection
The SMSSpamCollection dataset should be placed at `Dataset/SMSSpamCollection`.

---

## 🧰 Tech Stack

| Library | Purpose |
|---------|---------|
| PyTorch + torchvision | Malware image classification |
| scikit-learn | Random Forest, Naive Bayes, pipelines |
| pandas / numpy | Data manipulation |
| NLTK | Text preprocessing |
| matplotlib / seaborn | Visualization |
| joblib | Model serialization |

---

## 📄 License

This project is for educational purposes as part of coursework on AI applications in Information Security.
