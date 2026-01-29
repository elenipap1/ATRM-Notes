# Interviews 

### Generate interview questions

Note: Follow the guidelines about "identifying good and bad interviews questions from Foundational module"
For the people living with dementia (plwd):

```
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

This is done so you don't have to run the time consuming question answering every time you start the notebook
If you had data collected from humans, or you already created the CSV file in a previous run, you could start the analysis here.

```
import pandas as pd
plwd_df = pd.read_csv("/content/RawData.csv")
plwd_df
```

