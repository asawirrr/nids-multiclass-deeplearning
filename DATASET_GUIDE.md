# 📚 CICIDS2017 Multi-Class Dataset Guide

This guide details the dataset structure, attack distribution, and the 70 active network flow attributes utilized in the **NIDSTask1** Multi-Class NIDS project.

---

## 1. Class Distribution (Before vs After SMOTE)

| Class ID | Label | Raw Count in Subset | After SMOTE Count |
| :---: | :--- | :---: | :---: |
| 0 | BENIGN | 5,000 | 5,000 |
| 1 | Bot | 1,956 | 5,000 |
| 2 | DDoS | 5,000 | 5,000 |
| 3 | DoS GoldenEye | 5,000 | 5,000 |
| 4 | DoS Hulk | 4,982 | 5,000 |
| 5 | DoS Slowhttptest | 5,000 | 5,000 |
| 6 | DoS slowloris | 5,000 | 5,000 |
| 7 | FTP-Patator | 4,998 | 5,000 |
| 8 | PortScan | 4,994 | 5,000 |
| 9 | SSH-Patator | 5,000 | 5,000 |
| **Total**| **10 Classes** | **46,930** | **50,000** |

---

## 2. Dropped Invariant Columns (8 Features with Variance = 0)
The following columns contained single unique values across all network captures and were safely eliminated:
1. `Bwd PSH Flags`
2. `Bwd URG Flags`
3. `Fwd Avg Bytes/Bulk`
4. `Fwd Avg Packets/Bulk`
5. `Fwd Avg Bulk Rate`
6. `Bwd Avg Bytes/Bulk`
7. `Bwd Avg Packets/Bulk`
8. `Bwd Avg Bulk Rate`

Remaining features: **70 statistical flow attributes** (Packet lengths, Flow duration, Inter-arrival times, TCP flags, Window bytes, and Subflow statistics).
