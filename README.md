# Lecture 3 - Sentiment Analysis

This is how to perform sentiment analysis:

```
!pip install spacytextblob
import spacy
from spacytextblob.spacytextblob import SpacyTextBlob

# Load the spaCy model
nlp = spacy.load("en_core_web_sm")

# Add the SpacyTextBlob component to the pipeline
nlp.add_pipe("spacytextblob")

# Sample sentences
sentences = [
    "I love behavioural research.",
    "I do not like Rafa's lecture. They are boring.",
    "Python is cool and fun."
]

# Perform sentiment analysis
results = []
for sentence in sentences:
    doc = nlp(sentence)
    results.append((sentence, doc._.blob.sentiment))

# Print the results
for sentence, sentiment in results:
    print(f"Sentence: '{sentence}' - Sentiment: {sentiment}")
```
