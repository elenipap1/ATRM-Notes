# Bag of Words

1. Mathematical Representation
Converting text into vectors allows for the application of mathematical and statistical techniques. You can
perform operations like similarity calculations, clustering, and classification.
Vectors provide a consistent format for representing documents, which is essential for machine learning
algorithms.
2. Similarity Measurement
how similar or different two documents are based on their content.
3. Dimensionality Reduction
techniques like Principal Component Analysis (PCA) and Singular Value Decomposition (SVD) reduce the
dimensionality of the data, making it more manageable and improving computational efficiency.
4. Machine Learning Applications
Classification: Document vectors can be used as input features for machine learning models to classify
documents into categories (e.g., spam detection, sentiment analysis).
Clustering: Vectors enable clustering algorithms (e.g., K-means) to group similar documents together, which is useful for organizing large datasets.


```
from sklearn.feature_extraction.text import CountVectorizer

# Sample data
sentences = [
    "I love advanced transdisciplinary research methods.",
    "I do not like Rafa's lecture.",
    "Python is cool and fun."
]

# Initialize the CountVectorizer
vectorizer = CountVectorizer()

# Fit and transform the data
X = vectorizer.fit_transform(sentences)

# Convert to array and print the result
print(X.toarray())

# Print the feature names (vocabulary)
print(vectorizer.get_feature_names_out())

```

## Removing Stopwords 

Common words like “and,” “the,” “is,” and “in” that are often filtered out during natural language
processing (NLP) tasks because they do not add much ‘information’.

```
import nltk
nltk.download('stopwords') # Download stopwords if you haven't already
from nltk.corpus import stopwords

# Create a stop word list
stop_words = stopwords.words('english')

#  Initialize CountVectorizer with stop words
vectorizer = CountVectorizer(stop_words=stop_words)


# Fit and transform the data
X = vectorizer.fit_transform(sentences)

# Convert to array and print the result
print(X.toarray())

# Print the feature names (vocabulary)
print(vectorizer.get_feature_names_out())
```
