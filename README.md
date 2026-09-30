# Malicious URL Classifier

A binary classifier distinguishing malicious from benign URLs, built on the
UCSD URL Reputation dataset. Developed as part of a machine learning module
during my BSc (Hons) Cyber Security degree.

## What it does

- Loads the UCSD URL dataset (pre-extracted, anonymised numeric features
  in SVM-light format)
- Samples a balanced subset (500 malicious, 500 benign) for training
- Tokenises the feature indices and vectorises them with TF-IDF
  (unigrams and bigrams)
- Trains a logistic regression classifier on an 80/20 stratified split
- Evaluates with a confusion matrix, precision, recall and F1

## Results

**96.5% accuracy** on a balanced holdout set.

That number is the headline, but it's also the part worth being
skeptical of. The dataset here is artificially balanced 50/50. Real
network traffic isn't — malicious URLs are a tiny minority of all
traffic, often by several orders of magnitude. Under that kind of
imbalance, even a low false-positive rate produces a large *absolute*
number of false alarms relative to genuine detections, which is what
actually determines whether a tool is usable in an analyst queue.

Precision at a fixed false-positive rate is the metric that matters
in deployment — accuracy on a balanced sample doesn't tell you that.

## Limitations

- **Class imbalance**: not modelled here; a production system would
  need evaluation on realistic, imbalanced traffic.
- **Lexical-feature evasion**: features are derived from URL structure
  alone, which is trivial for an attacker to manipulate.
- **Concept drift**: attacker URL patterns change over time; a static
  model would need periodic retraining.

## Dataset

[UCSD URL Reputation dataset](https://www.cs.ucsd.edu/~savage/papers/UCSDcselec2009.pdf) —
not included in this repo; download separately.

## Disclosure

AI tools were used to help structure and debug parts of the
Python pipeline during development, alongside lecture material from
weeks 7–8 of the module.
