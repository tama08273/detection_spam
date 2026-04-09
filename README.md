# Robust Phishing Detection under Adversarial Attacks
### 〜 AIセキュリティにおけるロバスト性評価 PoC 〜

## Overview

本リポジトリは、機械学習ベースのフィッシングURL検知モデルに対し、

- 通常性能評価（Baseline）
- ハイパーパラメータ最適化（Optuna）
- 説明可能性分析（SHAP）
- Evasion Attack（回避攻撃）
- Data Poisoning（汚染攻撃）
- ロバスト性比較評価

を実施し、**AIセキュリティ観点での耐攻撃性（Robustness）を検証するPoC** である。

単なる精度向上ではなく、  
**「攻撃下でも壊れない検知器設計」** を主眼としている。

---

## Design Perspective

本実装は、単なるアルゴリズム比較ではなく、  
**機械学習システムの設計思想と評価構造** に基づいて構築している。

特に以下の観点を重視している：

- クロスバリデーションの楽観性バイアス
- 不均衡データ下での閾値設計
- モデル複雑性と汎化のトレードオフ
- 実運用を想定した評価設計

これらの理論的背景は以下のリポジトリに整理している：

👉 [ml-design-principles](https://github.com/tama08273/ml-design-principles)

---

## Background

近年、フィッシング対策としてMLベース検知が広く利用されているが、

- Adversarial Evasion
- Training Data Poisoning
- Data Drift

などの影響により、実運用環境では性能劣化が発生する可能性がある。

そのため、

> Accuracy最大化 ＝ 安全

ではなく、

> 攻撃下での性能維持（Robustness）

の評価が重要となる。

本検証ではこの観点からモデルの耐攻撃性を体系的に評価した。

---

## Objectives

本PoCの目的：

1. フィッシング検知モデルのベースライン性能確認
2. Optunaによる最適化
3. SHAPによる重要特徴の可視化（説明可能性）
4. 攻撃シミュレーションによる性能劣化測定
5. モデル間ロバスト性比較（Logistic vs Decision Tree）
6. 実運用（SOC/CSIRT）視点での示唆抽出

---

## Dataset

UCI Phishing Websites Dataset を使用。

30個のURL/HTML特徴量から構成される。

例：

- URL長
- SSL状態
- サブドメイン数
- iframe利用
- リダイレクト
- Google Index
- PageRank

ラベル：
- 1 : phishing
- -1 : legitimate

---

## Methodology

### 1. Baseline Modeling
- Logistic Regression
- Decision Tree

### 2. Hyperparameter Optimization
- Optuna による自動最適化

### 3. Explainability
- SHAP による特徴重要度分析
- 検知ロジックの可視化

### 4. Adversarial Evaluation

#### Evasion Attack
重要特徴を摂動し、検知回避を模擬

#### Data Poisoning
学習ラベルの一部反転による訓練データ汚染

### 5. Robustness Comparison
攻撃前後で攻撃下での Recall 低下率を指標に、モデルの堅牢性を比較

---

## Results (example)

| Model | Recall Clean | Recall Attack | Recall Drop |
|---------|-------------|--------------|--------------|
| Logistic | 0.88 | 0.68 | -0.20 |
| Decision Tree | 0.96 | 0.59 | -0.37 |

観測結果：

- 高精度モデルでも攻撃下で大幅劣化
- Logistic Regressionの方が相対的に頑健（低下率が小さい）
- 決定木は攻撃に対して極度に脆弱
- モデル選択時は精度だけでなくロバスト性の考慮が必須

---

## Key Findings

### 1. 精度最大化のみでは不十分
Accuracyが高くても攻撃下で破綻する可能性がある。

### 2. Recall重視設計が重要
False Negative は直接的なセキュリティリスクとなる。

### 3. 説明可能性は運用上必須
SHAPにより誤検知/見逃しの原因分析が可能。

### 4. 継続的評価が不可欠
MLモデルは「作って終わり」ではなく、
再学習・ドリフト監視を含む MLOps が前提。

---

## Practical Implications

SOC/CSIRT運用において：

- Alert fatigue の回避
- 閾値チューニング
- 継続再学習
- Adversarial training

など、**運用設計と一体化したAIセキュリティ戦略** が重要であることが示唆された。

---

## Repository Structure
notebook/
└ robust_phishing_detection_poc.ipynb

README.md


---

## Technologies

- Python
- scikit-learn
- Optuna
- SHAP
- pandas / numpy / matplotlib

---

## About this Project

本リポジトリは **AIセキュリティ分野での自主研究プロジェクト** です。

フィッシング検知モデルの実運用ロバスト性を、脅威モデリングに基づいて体系的に検証しています。

---