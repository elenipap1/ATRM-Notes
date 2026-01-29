# Synths

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
```

### Calculate gender frequency for both

```
gender_frequency = plwd_df['gender'].value_counts()
print(f"for plwd: {gender_frequency}\n")
```
