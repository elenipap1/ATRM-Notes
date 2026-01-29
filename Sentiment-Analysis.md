# Sentiment Analysis

AKA opinion mining, is a natural language processing (NLP) technique used to determine the **emotional
tone** behind a body of text. It helps identify whether the sentiment **expressed in the text is positive,
negative, or neutral**. This technique is widely used in **various applications**, such as **analyzing customer
reviews, social media monitoring, and market research**.

## Sentiment Analysis

### Install SpaCy

We use SpaCy (an NLP library) that has small language models you can download. 

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
    "I do not like Rafa's lecture. They are boring. they are the worst lectures ever in the whole world",
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

### Now lets count them!

```
# prompt: count how many have positive polarity, and how many negative

positive_count = 0
negative_count = 0

for sentence, sentiment in results:
    if sentiment.polarity > 0:
        positive_count += 1
    elif sentiment.polarity < 0:
        negative_count += 1

print(f"Number of positive sentences: {positive_count}")
print(f"Number of negative sentences: {negative_count}")
```

### Basic Descriptive Statistics of Sentiment Analysis

```
# prompt: basic descriptive stats of sentiment

import pandas as pd

# Create a DataFrame from the results
df = pd.DataFrame(results, columns=['Sentence', 'Sentiment'])

# Expand the Sentiment column into separate polarity and subjectivity columns
df['Polarity'] = df['Sentiment'].apply(lambda x: x.polarity)
df['Subjectivity'] = df['Sentiment'].apply(lambda x: x.subjectivity)

# Display basic descriptive statistics for polarity and subjectivity
print(df[['Polarity', 'Subjectivity']].describe())
```

With 3 answers, I can say that the sentiment is slightly negative (-0.058) but very subjective (0.675)

## Sentiment Analysis + Word Cloud

### Install SpaCy

We use SpaCy (an NLP library) that has small language models you can download. 

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

### Make the Word Cloud

Generate two clouds for positive and negative sentences each. We use the common Wordcloud library. 

```
# prompt: create a word cloud for positive and another for negative comments

import matplotlib.pyplot as plt
from wordcloud import WordCloud

# Assuming 'results' from the previous code is available

positive_comments = ""
negative_comments = ""

for sentence, sentiment in results:
    if sentiment.polarity > 0:
        positive_comments += sentence + " "
    elif sentiment.polarity < 0:
        negative_comments += sentence + " "

# Generate word clouds
wordcloud_positive = WordCloud(width=800, height=400, background_color="white").generate(positive_comments)
wordcloud_negative = WordCloud(width=800, height=400, background_color="white").generate(negative_comments)

# Display the word clouds
plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1)
plt.imshow(wordcloud_positive, interpolation="bilinear")
plt.axis("off")
plt.title("Positive Comments")

plt.subplot(1, 2, 2)
plt.imshow(wordcloud_negative, interpolation="bilinear")
plt.axis("off")
plt.title("Negative Comments")

plt.show()
```
<img width="557" height="161" alt="Screenshot 2026-01-29 at 17 51 04" src="https://github.com/user-attachments/assets/dceec142-52c7-47bb-82df-0a5ad9953b9f" />

