# "Attention Is All You Need" — Reading-Group Cheat Sheet

**Paper:** *Attention Is All You Need* — Vaswani, Shazeer, Parmar, Uszkoreit, Jones, Gomez, Kaiser, Polosukhin — **arXiv:1706.03762**

**Provenance note (please read):** every factual claim in sections 1–4 below was pulled from the live sources listed in the *Sources* section at the bottom — nothing is from memory. Where a lookup failed (see §3), it is explicitly marked as **NOT VERIFIED** rather than filled in from recollection. Fix the flagged items before the talk.

---

## 1. The paper (arXiv)

**Identification.** Searched for the paper and confirmed the target via its live arXiv record: **"Attention Is All You Need"**, arXiv ID **1706.03762**. The paper's own author block (from the arXiv LaTeX source) lists eight authors:

| Author | Affiliation (as printed in the paper) |
| --- | --- |
| Ashish Vaswani | Google Brain |
| Noam Shazeer | Google Brain |
| Niki Parmar | Google Research |
| Jakob Uszkoreit | Google Research |
| Llion Jones | Google Research |
| Aidan N. Gomez | University of Toronto (work performed while at Google Brain) |
| Łukasz Kaiser | Google Brain |
| Illia Polosukhin | (work performed while at Google Research) |

The team therefore spanned **Google Brain / Google Research** and the **University of Toronto**.

### 1.1 BLEU scores claimed on the two WMT 2014 translation tasks

From the paper's **abstract** (verbatim):

> "Our model achieves 28.4 BLEU on the WMT 2014 English-to-German translation task, improving over the existing best results, including ensembles, by over 2 BLEU. On the WMT 2014 English-to-French translation task, our model establishes a new single-model state-of-the-art BLEU score of 41.8 after training for 3.5 days on eight GPUs, a small fraction of the training costs of the best models from the literature."

From the paper's **Results** section (Table 1, "The Transformer achieves better BLEU scores than previous state-of-the-art models on the English-to-German and English-to-French newstest2014 tests at a fraction of the training cost."):

| Model | BLEU EN-DE | BLEU EN-FR |
| --- | --- | --- |
| Transformer (base model) | 27.3 | 38.1 |
| Transformer (big) | **28.4** | **41.8** |

From the **Results** section prose (verbatim):

> "On the WMT 2014 English-to-German translation task, the big transformer model (Transformer (big) in Table 1) outperforms the best previously reported models (including ensembles) by more than 2.0 BLEU, establishing a new state-of-the-art BLEU score of 28.4. The configuration of this model is listed in the bottom line of Table 2. Training took 3.5 days on 8 P100 GPUs."

> "On the WMT 2014 English-to-French translation task, our big model achieves a BLEU score of 41.0, outperforming all of the previously published single models, at less than 1/4 the training cost of the previous state-of-the-art model."

**⚠️ Caveat worth knowing for Q&A:** the paper is not internally consistent on EN-FR. The **abstract and Table 1 say 41.8 BLEU**, while the **Results prose says 41.0 BLEU**. If someone asks, the headline number the paper leads with (abstract + table) is **41.8**.

### 1.2 Training setup (days and GPUs)

From the **Training** section, "Hardware and Schedule" (verbatim):

> "We trained our models on one machine with 8 NVIDIA P100 GPUs. For our base models using the hyperparameters described throughout the paper, each training step took about 0.4 seconds. We trained the base models for a total of 100,000 steps or 12 hours. For our big models,(described on the bottom line of table 2), step time was 1.0 seconds. The big models were trained for 300,000 steps (3.5 days)."

**Cheat-sheet version:**

- **8 NVIDIA P100 GPUs on one machine**
- **Base model:** 100,000 steps ≈ **12 hours** (~0.4 s/step)
- **Big model:** 300,000 steps = **3.5 days** (~1.0 s/step)
- The headline claim (abstract) is **"3.5 days on eight GPUs"**

---

## 2. Where it happened (author institutions)

The paper's affiliations are Google Brain / Google Research (Mountain View, California) and the University of Toronto (Aidan N. Gomez, `aidan@cs.toronto.edu`).

### ❗ NOT VERIFIED — Google Maps lookup unavailable

The lookups for both locations were attempted but **every Google Maps API endpoint used returned an error**:

```
"You must enable Billing on the Google Cloud Project at
 https://console.cloud.google.com/project/_/billing/enable
 Learn more at https://developers.google.com/maps/gmp-get-started"
```

This affected: place search, geocoding, reverse geocoding, place details, directions, distance matrix, and elevation — i.e. **no address, coordinates, place ID, phone number, or user rating could be retrieved from the live source**. Per the ground rule for this cheat sheet (*no facts from memory*), those fields are left blank below rather than half-remembered.

**To complete before the presentation:** enable billing on the Google Cloud project (or swap in an API key with the Maps Platform enabled) and re-run these two queries, then fill in:

| Field | Google HQ, Mountain View, CA | University of Toronto, St. George campus |
| --- | --- | --- |
| Query used | "Google headquarters, Mountain View, California" | "University of Toronto, St. George campus" |
| Street address | ⬜ *to be filled from API* | ⬜ *to be filled from API* |
| Coordinates | ⬜ *to be filled from API* | ⬜ *to be filled from API* |
| Place ID | ⬜ *to be filled from API* | ⬜ *to be filled from API* |
| Phone | ⬜ *to be filled from API* | ⬜ *to be filled from API* |
| User rating | ⬜ *to be filled from API* | ⬜ *to be filled from API* |

(During the session, reverse-geocoding was also attempted with approximate coordinates as a fallback and failed with the same billing error.)

---

## 3. Notable follow-up papers that build directly on the Transformer architecture

All three verified against their live arXiv sources (titles as printed in the papers' own LaTeX):

1. **"BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding"** — Devlin, Chang, Lee, Toutanova (Google AI Language). **arXiv:1810.04805**. Pre-trains a deep *bidirectional* Transformer encoder with a masked-language-model objective; new state-of-the-art on eleven NLP tasks, including GLUE 80.5%, MultiNLI 86.7%, SQuAD v1.1 Test F1 93.2, SQuAD v2.0 Test F1 83.1.

2. **"Universal Transformers"** — Dehghani et al. **arXiv:1807.03819**. Casts itself as "a generalization of the Transformer model": a parallel-in-time recurrent self-attentive sequence model combining the Transformer's parallelizability and global receptive field with an RNN's recurrent inductive bias, plus a dynamic per-position halting mechanism; outperforms the standard Transformer on WMT14 En-De by 0.9 BLEU and sets a new state of the art on LAMBADA.

3. **"Language Models are Few-Shot Learners"** (GPT-3) — Brown et al. (OpenAI). **arXiv:2005.14165**. Scales up the Transformer language-model recipe to 175B parameters and studies task-agnostic few-shot ("in-context") learning without gradient updates or fine-tuning.

> Method note: the arXiv search API (HTTP 429) and Google Scholar fetch were unavailable during this session, so these were confirmed by pulling each paper's live arXiv source directly rather than via keyword search; two of the three titles↔IDs were additionally corroborated by independent third-party listings.

---

## 4. Sources

- **Paper (abstract, full text, Results section, Training section):** arXiv:1706.03762 live LaTeX source — retrieved via the arXiv LaTeX tooling for `1706.03762` (paper prompt, section list, Results section, Training section).
- **Follow-up papers:** live arXiv LaTeX sources for `1810.04805`, `1807.03819`, `2005.14165` (paper prompts / abstracts).
- **Google Maps lookups (Google HQ & U of T St. George):** attempted via Maps place search / geocoding / reverse geocoding / place details — **all failed with a billing-enabled error**; see §2.
- **arXiv keyword search & Google Scholar search:** attempted multiple times; arXiv search API returned HTTP 429 rate-limit and Google Scholar fetch failed, so paper identification and follow-up verification were done via direct live-source pulls instead.
