# Word Cloud

A word cloud is a visual representation of the most frequent terms in a collection of texts, where the size of each word reflects how often it appears.

In this analysis, we generate word clouds using either single words (*unigrams*) or multi-word expressions, such as two-word phrases (*bigrams*) or three-word phrases (*trigrams*), to better capture recurring themes in the interviews.

You can choose which interview questions to analyze when generating the word clouds.

To improve interpretability, common function words and conversational fillers are removed using stop words. These stop words can be customized based on the observed output; we provide an initial set of suggested stop words, which can be further refined iteratively as new uninformative terms appear in the word clouds.

## Unigram Word Cloud

```
from wordcloud import WordCloud, STOPWORDS
import matplotlib.pyplot as plt
import re

# Custom stopwords (interview fillers)
STOPWORDS_CUSTOM = set(STOPWORDS).union({
    "know", "mean", "think", "like", "just", "really",
    "basically", "actually", "well", "so", "right", "okay",
    "kind", "sort", "maybe", "probably", "home", "thing",
    "make", "used", "always", "sometimes", "usually", "still",
    "keep"
})

# Basic text normalization
def preprocess(text):
    return re.sub(r"[^\w\s]", "", text.lower())

# Normalize stopwords to match preprocessing
STOPWORDS_PROCESSED = {preprocess(w) for w in STOPWORDS_CUSTOM}

def plot_unigram_wordcloud(df, question):
    """Plot a unigram word cloud for a given question column."""

    # Load and preprocess text
    text = (
        df[f"Question_{question}"]
        .fillna("")
        .astype(str)
        .apply(preprocess)
        .str.cat(sep=" ")
    )

    # Generate word cloud
    wc = WordCloud(
        width=800,
        height=400,
        background_color="white",
        stopwords=STOPWORDS_PROCESSED,
        min_word_length=4,
        collocations=False
    ).generate(text)

    # Plot
    plt.figure(figsize=(12, 6))
    plt.imshow(wc, interpolation="bilinear")
    plt.axis("off")
    plt.title(f"Unigram Word Cloud – Question {question}")
    plt.show()

# CHOOSE QUESTION HERE
plot_unigram_wordcloud(yp_df, 2)
```
<img width="881" height="481" alt="Screenshot 2026-01-29 at 17 34 27" src="https://github.com/user-attachments/assets/0bf1a171-875c-49f4-ac1a-65a6562c9087" />

## N-Gram Word Cloud

```
from sklearn.feature_extraction.text import CountVectorizer
from wordcloud import WordCloud, STOPWORDS
import matplotlib.pyplot as plt
import re

# Custom stopwords (interview-style fillers)
STOPWORDS_CUSTOM = set(STOPWORDS).union({
    "know", "mean", "think", "like", "just", "really",
    "basically", "actually", "well", "so", "right", "okay",
    "kind", "sort", "maybe", "probably", "home", "thing",
    "make", "used", "always", "sometimes", "usually", "still",
    "keep", "oh", "ah", "bit"
})

# Basic text normalization
def preprocess(text):
    return re.sub(r"[^\w\s]", "", text.lower())

# Normalize stopwords to match preprocessing
STOPWORDS_PROCESSED = {preprocess(w) for w in STOPWORDS_CUSTOM}

def plot_ngram_wordcloud(df, question, ngram_range=(2, 2), max_features=100):
    """Plot an n-gram word cloud for a given question column."""

    # Load and preprocess text
    texts = (
        df[f"Question_{question}"]
        .fillna("")
        .astype(str)
        .apply(preprocess)
        .tolist()
    )

    # Extract n-grams
    vectorizer = CountVectorizer(
        ngram_range=ngram_range,
        stop_words=list(STOPWORDS_PROCESSED),
        max_features=max_features
    )

    X = vectorizer.fit_transform(texts)
    freqs = X.sum(axis=0).A1
    ngrams = vectorizer.get_feature_names_out()

    # Generate word cloud
    wc = WordCloud(
        width=800,
        height=400,
        background_color="white",
        colormap="viridis"
    ).generate_from_frequencies(dict(zip(ngrams, freqs)))

    # Plot
    plt.figure(figsize=(12, 6))
    plt.imshow(wc, interpolation="bilinear")
    plt.axis("off")
    plt.title(f"N-gram Word Cloud ({ngram_range[0]}–{ngram_range[1]}) – Question {question}")
    plt.show()
```


### CHOOSE N HERE

```
(2,2) = bigrams | (3,3) = trigrams | (2,3) = both
plot_ngram_wordcloud(yp_df, 2, ngram_range=(2, 3))
```

<img width="890" height="479" alt="Screenshot 2026-01-29 at 17 34 51" src="https://github.com/user-attachments/assets/a35dc8e1-626a-4924-af77-0c4472f7beb9" />
