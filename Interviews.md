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

### Simulate Interviews

```
def simulate_interview(row):
    print(f"Interview of participant {row['pid']}")
    person_json = row.to_json()

    persona_prompt = f"""
    The task is to simulate a research interview where you play the role of a human participant.
    The study focuses on dementia and you have to answer questions from a researcher.
    The model should respond creatively, using natural, conversational language that mimics human behavior and thought processes.
    Answers should reflect personality, emotions, and personal experiences (fictional, but believable).
    The model should engage with the interviewer’s questions thoughtfully, offering reflections, anecdotes, and context, while avoiding robotic or overly formal responses.
    It should balance creativity with realism, ensuring that answers seem authentic and relatable.
    At times, the participant may hesitate or elaborate on ideas as a real person might.
    The model should maintain consistency in its fictional character's background and experiences throughout the interview.
    Answer each question with details of what would be expected for a person with the following job and characteristics:
    {person_json}.
    """
    messages = [SystemMessage(persona_prompt)]
    row['persona_prompt'] = persona_prompt

    for i, question in enumerate(qualitative_questions_for_yp):
      question_id = f"Question_{i+1}"
      print(f"\tAsking {question_id}")

      # Researcher turn
      messages.append(HumanMessage(question))
      print(f"\t\tResearcher: {question}")

      # Participant turn
      response = model.invoke(messages)
      messages.append(response)
      print(f"\t\tParticipant: {response.content[:30] + '...'}")

      # Log answer in new column
      row[question_id] = response.content

    return row

yp_df = yp_df.apply(simulate_interview, axis=1)

yp_df.to_csv("RawData.csv", sep=',', index=False, encoding='utf-8')
yp_df
```
### Important!!!!

At this step, before continuing, save the interview data on your computer. 
Check **Files** in the left-hand menu and find **RawData.csv**
If you have already completed the interview code, do not run **Simulate Interview** (this will regenerate the interviews and provide different data!)
Instead, run the next code block **Read raw data**

### Read raw data

This is done so you don't have to run the time consuming question answering every time you start the notebook
If you had data collected from humans, or you already created the CSV file in a previous run, you could start the analysis here.

```
import pandas as pd
plwd_df = pd.read_csv("/content/RawData.csv")
plwd_df
```

