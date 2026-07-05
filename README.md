<!-- This is the markdown template for the final project of the Building AI course, 
created by Reaktor Innovations and University of Helsinki. -->

# SpamGuard: A Simple Spam/Text Classifier

Building AI course project

## Summary

SpamGuard is a machine learning classifier that automatically detects whether a text message or email is spam or legitimate ("ham"). It uses word-frequency features and a Naive Bayes / logistic regression model to flag unwanted messages before they reach the user's inbox.

## Background

Spam messages are more than just annoying — they waste time, clutter inboxes, and are often used as a vector for phishing, scams, and malware. Despite decades of filtering technology, spam still makes up a significant share of global email traffic, and unwanted text messages ("SMS spam") are an increasing problem as well.

Problems this project addresses:
* Users waste time manually sorting through unwanted messages
* Spam and phishing messages can lead to fraud or security breaches
* Existing filters are often either too strict (blocking real messages) or too lax (letting spam through)

My personal motivation for this project comes from noticing how much spam still slips through default filters on personal email and SMS, and wanting to understand how a classifier like this actually works under the hood rather than treating it as a "black box."

## How is it used?

The solution is intended to run automatically in the background of an email or messaging platform:

1. A new message arrives.
2. The message text is converted into a numerical representation (e.g. bag-of-words or TF-IDF).
3. The trained model calculates the probability that the message is spam.
4. If the probability exceeds a set threshold, the message is flagged or moved to a spam folder; otherwise, it's delivered normally.

**Users:** anyone receiving emails or text messages — from individuals to businesses managing customer inboxes.
**Environment:** this should run continuously and in real time, ideally with a low false-positive rate, since incorrectly flagging a legitimate message as spam can cause a user to miss something important.

Example of a simplified scoring loop:

```
def classify_message(text, model):
    features = extract_features(text)          # e.g. word counts or TF-IDF
    spam_probability = model.predict(features)  # value between 0 and 1
    return "spam" if spam_probability > 0.5 else "ham"
```

## Data sources and AI methods

The model can be trained on publicly available, labeled spam datasets, such as:
* [SMS Spam Collection Dataset (UCI Machine Learning Repository)](https://archive.ics.uci.edu/ml/datasets/sms+spam+collection)
* [Enron Spam Dataset](https://www.cs.cmu.edu/~enron/)

**AI methods used:**

| Technique | Purpose |
| --------- | ------- |
| Bag-of-words / TF-IDF | Convert raw text into numerical features |
| Naive Bayes | Simple, fast probabilistic classifier well-suited to text data |
| Logistic Regression | Alternative classifier for comparison, outputs a probability score |
| Train/test split | Evaluate how well the model generalizes to unseen messages |

The core idea is that certain words (e.g. "free," "winner," "urgent," "click here") appear disproportionately often in spam messages. By calculating the likelihood ratio of these words appearing in spam vs. legitimate messages (similar to how tf-idf and Bayes' rule work), the model can assign a spam probability to any new, unseen message.

## Challenges

This project does **not** solve every spam-related problem, and has some important limitations:

* **Evolving spam tactics:** spammers constantly change their language and tactics to evade filters, so a static model can become outdated.
* **False positives:** an overly aggressive filter might block legitimate messages, which can have real consequences (e.g. missing an important email).
* **Bias in training data:** if the training data isn't representative of the population using the tool, the model may perform unevenly across different writing styles, languages, or dialects.
* **Privacy:** any system that scans private messages raises legitimate privacy questions, and users should have transparency and control over how their messages are processed.
* **Bag-of-words limitations:** because bag-of-words ignores word order, the model may miss context-dependent spam that uses seemingly innocent words in a suspicious order.

## What next?

To grow this project further, I would want to:
* Experiment with more advanced NLP techniques (e.g. word embeddings, transformer-based models) to better capture context and nuance
* Continuously retrain the model on new data to keep up with evolving spam patterns
* Build a simple web interface or browser extension so people could test the classifier on their own messages
* Explore techniques to reduce false positives, since the cost of blocking a real message is often higher than letting a spam message through

To move forward, I'd benefit from more hands-on experience with NLP libraries (e.g. scikit-learn, spaCy) and feedback from real users on how well the classifier performs on their own inboxes.

## Acknowledgments

* Built as a final project for the [Building AI course](https://buildingai.elementsofai.com/) by Reaktor Innovations and University of Helsinki
* Dataset references: UCI Machine Learning Repository, Enron Spam Dataset
* Course concepts on Bayes' rule, TF-IDF, and Naive Bayes classifiers directly informed the design of this project
