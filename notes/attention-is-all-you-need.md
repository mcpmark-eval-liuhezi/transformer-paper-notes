# Attention Is All You Need — Reading Notes

**Paper:** *Attention Is All You Need* — Vaswani, Shazeer, Parmar, Uszkoreit, Jones, Gomez, Kaiser, Polosukhin
**arXiv:** [1706.03762](https://arxiv.org/abs/1706.03762)
**Journal club session:** next week

> All facts below are taken directly from the paper's abstract and Model Architecture section (arXiv:1706.03762), not from memory.

## Key numbers

| Metric | Value |
| --- | --- |
| WMT 2014 English-to-German BLEU | **28.4** (improving over the existing best results, including ensembles, by over 2 BLEU) |
| WMT 2014 English-to-French BLEU | **41.8** (new single-model state of the art) |
| Training cost for the En→Fr result | **3.5 days on eight GPUs** — a small fraction of the training costs of the best models from the literature |

## Architecture summary

The Transformer is a sequence transduction architecture that is **based solely on attention mechanisms, dispensing with recurrence and convolutions entirely**. It keeps the classic encoder–decoder structure: the encoder maps an input sequence of symbol representations to a sequence of continuous representations, and the decoder autoregressively generates the output sequence one element at a time, consuming the previously generated symbols as additional input.

- **Encoder stack:** $N = 6$ identical layers, each with two sub-layers — a multi-head self-attention mechanism and a simple, position-wise fully connected feed-forward network.
- **Decoder stack:** $N = 6$ identical layers, each with the encoder's two sub-layers plus a **third sub-layer that performs multi-head attention over the output of the encoder stack**. The decoder's self-attention sub-layer is masked so that positions cannot attend to subsequent positions, which (combined with output embeddings offset by one position) ensures predictions for position $i$ depend only on known outputs at positions less than $i$.
- **Residual + layer norm:** each sub-layer's output is $\mathrm{LayerNorm}(x + \mathrm{Sublayer}(x))$; all sub-layers and embeddings produce outputs of dimension $d_{model} = 512$ to facilitate the residual connections.
- **Scaled Dot-Product Attention:** $\mathrm{Attention}(Q, K, V) = \mathrm{softmax}\!\left(\frac{QK^T}{\sqrt{d_k}}\right)V$ — dot products of the query with all keys, divided by $\sqrt{d_k}$, then softmax to get the weights on the values.
- **Multi-Head Attention:** instead of a single attention function with $d_{model}$-dimensional keys, values and queries, the queries, keys and values are linearly projected $h$ times with different, learned linear projections to $d_k$, $d_k$ and $d_v$ dimensions; attention is performed in parallel on each projected version, giving $d_v$-dimensional outputs that are concatenated and projected again:
  $$\mathrm{MultiHead}(Q, K, V) = \mathrm{Concat}(\mathrm{head}_1, \ldots, \mathrm{head}_h)W^O \quad \text{where } \mathrm{head}_i = \mathrm{Attention}(QW_i^Q, KW_i^K, VW_i^V)$$
  with $W_i^Q \in \mathbb{R}^{d_{model} \times d_k}$, $W_i^K \in \mathbb{R}^{d_{model} \times d_k}$, $W_i^V \in \mathbb{R}^{d_{model} \times d_v}$ and $W^O \in \mathbb{R}^{h d_v \times d_{model}}$.
  Multi-head attention lets the model jointly attend to information from different representation subspaces at different positions; with a single attention head, averaging inhibits this.
- **Dimensions:** the paper employs $h = 8$ parallel attention heads with $d_k = d_v = d_{model}/h = 64$; due to the reduced per-head dimension, the total computational cost is similar to that of single-head attention with full dimensionality.

## Background: Bahdanau et al. (arXiv:1409.0473)

Bahdanau et al. framed attention as a fix for the **fixed-length-vector bottleneck** in encoder–decoder NMT: they let the model automatically (soft-)search for the parts of a source sentence relevant to predicting a target word, rather than encoding it into a single fixed-length vector.

## Section map of the paper

Introduction · Background · Model Architecture · Why Self-Attention · Training · Results · Conclusion · Attention Visualizations

## Discussion points for the session

- Why removing recurrence/convolutions improves parallelism and shortens training time
- Trade-offs of the $1/\sqrt{d_k}$ scaling (vs. additive attention, for large $d_k$)
- What multi-head attention buys over a single head
- Generalization beyond MT: English constituency parsing with large and limited training data
