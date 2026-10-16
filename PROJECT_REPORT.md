# 📑 Technical Project Report: Multi-Class Network Intrusion Detection
**Project Title:** Multi-Class Network Attack Detection with SMOTE, Deep Learning, and Statistical Hypothesis Testing  
**Dataset:** CICIDS2017 (Canadian Institute for Cybersecurity)  
**Classification Mode:** Multi-Class (10 Classes)  

---

## 1. Executive Summary
Modern enterprise networks face diverse cyber threats spanning volumetric DDoS floods, application-layer DoS (Hulk, GoldenEye, Slowloris), stealth reconnaissance (PortScan), credential brute-forcing (FTP/SSH Patator), and botnet infiltration. Traditional intrusion detection systems reliant on static signatures frequently miss polymorphic and zero-day variants.

This project delivers a multi-class Network Intrusion Detection System (NIDS) trained on 50,000 synthetic and real network flow instances derived from the CICIDS2017 dataset. By combining **SMOTE oversampling**, **StandardScaler normalization**, and **5-Fold Cross-Validation**, we evaluate three diverse learning paradigms:
1. **Random Forest Classifier (RF)**
2. **Support Vector Machine (SVM with RBF kernel)**
3. **Deep Feedforward Neural Network (Keras / TensorFlow)**

Empirical results prove that Random Forest achieves state-of-the-art performance (**99.74% Accuracy, 0.9974 F1-score, 0.9996 ROC-AUC**), outperforming both SVM and Deep Neural Networks at a statistically significant level ($p < 0.001$).

---

## 2. Threat Analysis: The 10 Target Classes
1. **BENIGN (Class 0):** Baseline normal network behavior.
2. **Bot (Class 1):** Compromised nodes executing command-and-control payloads (Ares botnet).
3. **DDoS (Class 2):** High-volume distributed packet floods exhausting bandwidth.
4. **DoS GoldenEye (Class 3):** Layer-7 HTTP Keep-Alive connection starvation.
5. **DoS Hulk (Class 4):** Obfuscated HTTP request floods bypassing caches.
6. **DoS Slowhttptest (Class 5):** Slow HTTP request body uploads holding sockets open.
7. **DoS slowloris (Class 6):** Partial HTTP header transmissions keeping server threads occupied.
8. **FTP-Patator (Class 7):** Automated multi-threaded dictionary attacks against FTP port 21.
9. **PortScan (Class 8):** TCP/UDP reconnaissance scans probing network topology.
10. **SSH-Patator (Class 9):** Automated dictionary attacks against SSH port 22.

---

## 3. Data Preprocessing & Class Balancing (SMOTE)
In raw network traffic, Benign flows account for over 80% of samples, while minority attacks (such as Botnet or DoS Slowhttptest) contain fewer than 2,000 flows. Direct training induces severe bias toward majority classes.

### The SMOTE Solution:
- **Majority Classes Capped:** Classes with abundant traffic are capped at 5,000 flows each.
- **SMOTE Oversampling:** Synthetic Minority Over-sampling Technique (`sampling_strategy='auto'`, $k=5$ nearest neighbors) synthesizes minority samples along feature space line segments:
  $$x_{new} = x_i + \lambda \cdot (x_{zi} - x_i), \quad \lambda \sim U(0, 1)$$
- **Balanced Corpus:** Exactly 5,000 flows per class across all 10 classes, generating a perfectly balanced dataset of **50,000 total flows**.

---

## 4. Deep Neural Network Architecture (Keras / TensorFlow)
- **Input Dimension:** 70 numeric flow attributes
- **Layer 1:** Dense (128 neurons, ReLU activation) + Dropout (0.3)
- **Layer 2:** Dense (64 neurons, ReLU activation) + Dropout (0.3)
- **Output Layer:** Dense (10 neurons, Softmax activation)
- **Loss Function:** `sparse_categorical_crossentropy`
- **Optimizer:** Adam (learning rate = 0.001)
- **Epochs:** 10 epochs (Validation Accuracy: 98.99%, Test Accuracy: 99.09%)

---

## 5. Statistical Significance Testing (Hypothesis Testing)
To eliminate random sampling bias, 5-Fold Stratified Cross-Validation was conducted, and paired Student's t-tests were computed across all 5 folds:

$$\Delta = 	ext{Score}_{M_1} - 	ext{Score}_{M_2}, \quad t = rac{ar{\Delta}}{s_{\Delta} / \sqrt{n}}$$

### Results Summary:
* **RF vs SVM:** $t = 24.68, p = 1.6 	imes 10^{-5}$ $ightarrow$ **Significant ($p < 0.05$)**
* **RF vs Neural Network:** $t = 10.47, p = 4.7 	imes 10^{-4}$ $ightarrow$ **Significant ($p < 0.05$)**
* **Neural Network vs SVM:** $t = -14.15, p = 1.4 	imes 10^{-4}$ $ightarrow$ **Significant ($p < 0.05$)**

---

## 6. Conclusion
Random Forest is the unequivocally superior classifier for multi-class flow-based intrusion detection on CICIDS2017, achieving near-perfect detection without requiring GPU acceleration.
