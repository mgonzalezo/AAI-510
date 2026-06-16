# Discussion 2.1: Is Feature Engineering Still Relevant?

## Initial Post

I strongly disagree with the statement that feature engineering is no longer relevant. While deep learning has reduced the need for *manual* feature engineering in certain domains, particularly computer vision and natural language processing, feature engineering remains not only relevant but **essential** in the majority of real-world machine learning applications. I argue this from both a theoretical perspective and from my professional experience as a principal software engineer working on AI-driven solutions in the telecommunications industry at Red Hat.

### Deep Learning Excels in Unstructured Data, But Most Business Data Is Structured

Deep learning's ability to learn hierarchical representations automatically is genuinely transformative for unstructured data. Convolutional neural networks learn edge detectors, texture patterns, and object-part relationships directly from raw pixels (LeCun et al., 2015). Transformer architectures learn contextual word embeddings from raw text (Vaswani et al., 2017). In these domains, hand-crafted features like SIFT descriptors or TF-IDF vectors have indeed been superseded.

However, the vast majority of enterprise and industrial ML applications operate on **structured, tabular data**: customer records, financial transactions, sensor telemetry, network performance metrics, where deep learning has not demonstrated consistent superiority. Gradient-boosted tree ensembles (XGBoost, LightGBM) continue to dominate tabular data benchmarks and Kaggle competitions, and these models depend heavily on well-engineered features (Shwartz-Ziv & Armon, 2022; Grinsztajn et al., 2022). A comprehensive benchmark study by Grinsztajn et al. (2022) found that tree-based models outperformed deep learning on medium-sized tabular datasets, precisely because domain-informed feature engineering combined with tree ensembles captures the irregular, heterogeneous patterns typical of business data more effectively than end-to-end neural approaches.

### Telecom AI: Where Feature Engineering Is Mission-Critical

In my work on telco network AI, specifically around virtualized Radio Access Network (vRAN) deployments on OpenShift, feature engineering is not optional; it is the difference between a useful model and a useless one. Consider network anomaly detection for a 5G RAN deployment: raw telemetry from a Centralized Unit (CU) might include thousands of time-series counters: packet counts, latency samples, CPU utilization, scheduling metrics, arriving at sub-second intervals. A deep learning model cannot simply ingest this raw stream and produce actionable predictions. Instead, the critical step is engineering features that encode **domain knowledge**:

- **Rolling window aggregations** (5-min, 15-min, 1-hour) that capture temporal trends in handover failure rates
- **Ratio features** like PRB (Physical Resource Block) utilization relative to connected UE count, which normalizes load
- **Delta features** comparing current performance against a site-specific baseline, since absolute values vary across cell configurations
- **Cross-layer features** combining RAN-level KPIs with infrastructure metrics (container CPU throttling events correlated with DL throughput degradation)

These engineered features encode years of telecom engineering knowledge that no neural network could discover from raw data alone, especially given the relatively small labeled datasets available in production telecom environments where failures are rare events.

### Feature Engineering as a Form of Inductive Bias

From a learning theory perspective, feature engineering is a mechanism for injecting **inductive bias** into the model. The No Free Lunch theorem (Wolpert & Macready, 1997) reminds us that no learning algorithm is universally superior: performance depends on how well the algorithm's assumptions match the problem structure. Feature engineering is how practitioners encode problem structure explicitly, reducing the hypothesis space and enabling models to learn from smaller datasets with fewer parameters.

Deep learning substitutes manual feature engineering with **learned feature engineering** via its hierarchical architecture, but this requires massive datasets to discover the same structure that domain experts can encode directly. In data-scarce domains like telco fault prediction, medical diagnosis, or credit risk (as we explored in Module 1), the cost of collecting enough labeled data for end-to-end deep learning is prohibitive. Feature engineering bridges this gap.

### The Pragmatic Argument

Even in domains where deep learning works well, feature engineering has not disappeared; it has **evolved**. Practitioners still engineer features for data preprocessing, create embeddings for categorical variables, design attention masks, and structure input representations. What has changed is the *granularity* of feature engineering, not its relevance. The modern ML pipeline is best understood as a spectrum where domain knowledge and automated representation learning complement each other rather than compete (Zheng & Casari, 2018).

### Conclusion

Feature engineering remains a cornerstone of applied machine learning. Deep learning has expanded the toolkit by automating representation learning for unstructured data, but it has not eliminated the need for domain-informed feature construction in structured data problems, which constitute the majority of industrial ML applications. The most effective systems combine both: domain expertise encoded through features and the representational power of modern architectures.

## References

Grinsztajn, L., Oyallon, E., & Varoquaux, G. (2022). Why do tree-based models still outperform deep learning on typical tabular data? *Advances in Neural Information Processing Systems, 35*, 507–520.

LeCun, Y., Bengio, Y., & Hinton, G. (2015). Deep learning. *Nature, 521*(7553), 436–444. https://doi.org/10.1038/nature14539

Shwartz-Ziv, R., & Armon, A. (2022). Tabular data: Deep learning is not all you need. *Information Fusion, 81*, 84–90. https://doi.org/10.1016/j.inffus.2021.11.011

Vaswani, A., Shakhnardzev, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, Ł., & Polosukhin, I. (2017). Attention is all you need. *Advances in Neural Information Processing Systems, 30*.

Wolpert, D. H., & Macready, W. G. (1997). No free lunch theorems for optimization. *IEEE Transactions on Evolutionary Computation, 1*(1), 67–82. https://doi.org/10.1109/4235.585893

Zheng, A., & Casari, A. (2018). *Feature engineering for machine learning: Principles and techniques for data scientists*. O'Reilly Media.

---

*AI Disclosure: Claude Code (Anthropic) was used to assist with structuring this discussion post, locating references, and refining the prose. The arguments, domain examples from telco vRAN experience, and analytical reasoning reflect the author's professional knowledge and were reviewed for accuracy.*
