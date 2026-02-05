# Setup

## Installs & Imports
```
!pip install langchain langchain-mistralai
!pip install git+https://github.com/palefo/topicviz
from topicviz import TopicViz
from sentence_transformers import SentenceTransformer
embedding_model = SentenceTransformer("sentence-transformers/all-mpnet-base-v2")
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
```


## Model

### HOW TO SET YOUR MISTRAL API KEY IN GOOGLE COLAB
1. Click on the 🔑 "Secrets" icon in the left sidebar
2. Add a new secret with the name: MISTRAL_API_KEY
3. Paste your group Mistral API key as the value
4. Enable access for this notebook

```
MISTRAL_API_KEY = userdata.get('MISTRAL_API_KEY')
os.environ['MISTRAL_API_KEY'] = MISTRAL_API_KEY
```

### IMPORTANT

You must use only the model: "mistral-small-latest"
Do not change the model name

```
model = ChatMistralAI(
    api_key=MISTRAL_API_KEY,
    model="mistral-small-latest",
    temperature=1
)
```
