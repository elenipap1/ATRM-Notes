# Lecture 2

## Setup

```
# Installs & Imports

!pip install langchain langchain-mistralai bunkatopics

import pandas as pd
from langchain_mistralai.chat_models import ChatMistralAI
from google.colab import userdata
from langchain_core.messages import HumanMessage, SystemMessage
import matplotlib.pyplot as plt
from wordcloud import WordCloud
from nltk.corpus import stopwords
import nltk
nltk.download('stopwords')
stopwords = set(stopwords.words('english'))
from google.colab import output
output.enable_custom_widget_manager()
import numpy as np
import os

# Model

# ---------------------------------------------
# HOW TO SET YOUR MISTRAL API KEY IN GOOGLE COLAB
# ---------------------------------------------
# 1. Click on the 🔑 "Secrets" icon in the left sidebar
# 2. Add a new secret with the name: MISTRAL_API_KEY
# 3. Paste your group Mistral API key as the value
# 4. Enable access for this notebook
# ---------------------------------------------

MISTRAL_API_KEY = userdata.get('MISTRAL_API_KEY')
os.environ['MISTRAL_API_KEY'] = MISTRAL_API_KEY

# ---------------------------------------------
# IMPORTANT
# ---------------------------------------------
# You must use only the model: "mistral-small-latest"
# Do not change the model name
# =================================================

model = ChatMistralAI(
    api_key=MISTRAL_API_KEY,
    model="mistral-small-latest",
    temperature=1
)
```

## Synths

Foundational code for making a synth. Generate synths for each stakeholder, and then filter by participant type later on.

```
#@title Synths specifications data
participants = [
    {
        "pid": 1,
        "participant_type": "person living with dementia",
        "age": 74,
        "gender": "male",
        "lives_in": "Bristol, urban area",
        "educational_attainment": "Secondary Education",
        "occupation": "Retired firefighter",
        "support": "spouse provides daily assistance",
        "health_conditions": ["diabetes"],
        "hobbies": ["cycling", "gardening"],
        "cognitive_decline_stage": "early"
    },
    {
]
```
### Load synth data

```
#@title Load data
df = pd.DataFrame(participants)
df
```
### Average ages

```
#@title Average ages for participant types

average_ages = df.groupby('participant_type')['age'].mean()
print(average_ages)
```

### Filter rows for a specific participant type

```
#@title Filter rows where participant_type is 'person living with dementia'
plwd_df = df[df['participant_type'] == 'person living with dementia']

# Filter rows where participant_type is 'occupational therapist'
occupational_therapist_df = df[df['participant_type'] == 'occupational therapist']

# Filter rows where participant_type is 'carer'
carer_df = df[df['participant_type'] == 'carer']

# Calculate gender frequency for both
gender_frequency = plwd_df['gender'].value_counts()
print(f"for plwd: {gender_frequency}\n")

gender_frequency = occupational_therapist_df['gender'].value_counts()
print(f"for occupational therapist_df: {gender_frequency}\n")

gender_frequency = carer_df['gender'].value_counts()
print(f"for carers: {gender_frequency}")
```

## Generate interview questions

```
#@title Interview Questions
# Note: Follow the guidelines about "identifying good and bad interviews questions from Foundational module"
# For the people living with dementia
qualitative_questions_for_plwd = [
    "Tell me more about what 'home' means to you",
    "tell me about your daily routines. ",
    "What activities do you enjoy and how does your home make them easier, or harder?",
    "What are the main safety concerns you have in your home?",
    "How comfortable are you with using technology?",
    "How often do family members or caregivers visit?",
    "Does anything in these interactions make you feel particularly good",
]
```

### Read raw data

```
#@title Read raw data
# This is done so you don't have to run the time consuming question answering every time you start the notebook
# If you had data collected from humans, or you already created the CSV file in a previous run, you could start the analysis here.
import pandas as pd
plwd_df = pd.read_csv("/content/RawData.csv")
plwd_df
```

## Descriptive stats

### Length of interviews

```
#@title Graph answer length
#Task: make graph showing average answer length for questions 1 to 7
import matplotlib.pyplot as plt

# Assuming plwd_df is your DataFrame with interview responses
# Calculate the average length of answers for each question (Question_1 to Question_7)
average_answer_lengths = []
for i in range(1, 8):
  question_column = f"Question_{i}"
  if question_column in plwd_df.columns:
    answer_lengths = plwd_df[question_column].str.len()
    average_length = answer_lengths.mean()
    average_answer_lengths.append(average_length)
  else:
    average_answer_lengths.append(0)  # Or handle missing columns differently

# Create a bar chart
plt.figure(figsize=(10, 6))
plt.bar(range(1, 8), average_answer_lengths)
plt.xlabel("Question Number")
plt.ylabel("Average Answer Length")
plt.title("Average Answer Length for Questions 1-7")
plt.show()
```

### Length by age

```
#@title Graph answer length by age
#Task: Make graph showing average answer length for questions 1 to 7 for different ages

import matplotlib.pyplot as plt

# Assuming plwd_df is your DataFrame with interview responses and 'age' column
# Calculate the average length of answers for each question (Question_1 to Question_7) for different age groups
average_answer_lengths_by_age = {}
for age in plwd_df['age'].unique():
  age_df = plwd_df[plwd_df['age'] == age]
  average_answer_lengths = []
  for i in range(1, 8):
    question_column = f"Question_{i}"
    if question_column in age_df.columns:
      answer_lengths = age_df[question_column].str.len()
      average_length = answer_lengths.mean()
      average_answer_lengths.append(average_length)
    else:
      average_answer_lengths.append(0)  # Or handle missing columns differently
  average_answer_lengths_by_age[age] = average_answer_lengths

# Create a bar chart for each age group
plt.figure(figsize=(15, 8))
for age, average_answer_lengths in average_answer_lengths_by_age.items():
  plt.plot(range(1, 8), average_answer_lengths, label=f"Age: {age}")

plt.xlabel("Question Number")
plt.ylabel("Average Answer Length")
plt.title("Average Answer Length for Questions 1-7 by Age")
plt.legend()
plt.show()
```

### Length by gender

```
#@title Graph answer length by gender
#Task: Make graph showing average answer length for questions 1 to 7 for different gender

# Assuming plwd_df is your DataFrame with interview responses and 'gender' column
# Calculate the average length of answers for each question (Question_1 to Question_7) for different gender groups
average_answer_lengths_by_gender = {}
for gender in plwd_df['gender'].unique():
  gender_df = plwd_df[plwd_df['gender'] == gender]
  average_answer_lengths = []
  for i in range(1, 8):
    question_column = f"Question_{i}"
    if question_column in gender_df.columns:
      answer_lengths = gender_df[question_column].str.len()
      average_length = answer_lengths.mean()
      average_answer_lengths.append(average_length)
    else:
      average_answer_lengths.append(0)  # Or handle missing columns differently
  average_answer_lengths_by_gender[gender] = average_answer_lengths

# Create a bar chart for each gender group
plt.figure(figsize=(15, 8))
for gender, average_answer_lengths in average_answer_lengths_by_gender.items():
  plt.plot(range(1, 8), average_answer_lengths, label=f"Gender: {gender}")

plt.xlabel("Question Number")
plt.ylabel("Average Answer Length")
plt.title("Average Answer Length for Questions 1-7 by Gender")
plt.legend()
plt.show()
```

## Word Cloud

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
plot_unigram_wordcloud(plwd_df, 2)
```
