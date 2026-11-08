# Can an LLM Guess the Star Rating? Prompt Engineering with GPT-4o-mini

Does giving a large language model examples in the prompt help it predict how many stars a customer gave?

This is a group project for **Large Language Models / Text Analytics for Marketing** at Erasmus University Rotterdam. We asked **GPT-4o-mini** to predict the star rating (1–5) of Sephora skincare reviews, using four prompting strategies.

## Key findings

| Prompt strategy | Examples in the prompt | Accuracy | Mean absolute error |
|---|---|---|---|
| **Zero-shot** | none | 65.3% | 0.357 |
| **One-shot, positive** | one 5★ review | **67.0%** | **0.340** |
| **One-shot, negative** | one 1★ review | 63.7% | 0.377 |
| **Few-shot** | five reviews (one per star) | 63.3% | 0.380 |

*300 test reviews (60 per star rating), 1,200 predictions in total.*

- **More examples did not help.** The few-shot prompt was the least accurate. The model already understands star ratings well without examples.
- **Extremes are easy, the middle is hard.** With few-shot prompting, the model got 5★ reviews right 92% of the time but only 53–60% of 1–4★ reviews.
- **The example barely biased the model.** A positive vs. negative example shifted the average prediction by only 0.04 stars.
- **Errors are small.** When the model is wrong, it is usually off by one star (MAE ≈ 0.35).

## Method

1. **Data.** A balanced test set of 300 Sephora reviews (60 per star rating), sampled with a fixed seed so every group member worked on the same reviews.
2. **Model.** `gpt-4o-mini` via the OpenAI API, `temperature = 0`, answer limited to a single number.
3. **Four prompt conditions:**
   - *Zero-shot:* only the review.
   - *One-shot positive / negative:* one 5★ or one 1★ example review first.
   - *Few-shot:* five example reviews, one for each star rating.
4. **Evaluation.** Accuracy, mean absolute error, average predicted rating (to check bias) and accuracy per star rating.

## Repository structure

```
└── llm_rating_prediction.ipynb   full experiment with outputs (Google Colab)
```

## Reproduce

1. Download the **[Sephora Products and Skincare Reviews](https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews)** dataset by Nady Inky on Kaggle (CC BY 4.0) and create the 10,000-review sample (see the notebook).
2. Add your own OpenAI API key in the notebook. No key is stored in this repository.
3. Run all cells. The 1,200 API calls take about 10–20 minutes.

## Tools

Python · OpenAI API (GPT-4o-mini) · prompt engineering · pandas · scikit-learn
