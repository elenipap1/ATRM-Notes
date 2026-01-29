# Descriptive Stats

## Graph - interview answer by length

```
import matplotlib.pyplot as plt
```
Assuming plwd_df is your DataFrame with interview responses

Calculate the average length of answers for each question (Question_1 to Question_7)

```
average_answer_lengths = []
for i in range(1, 8):
  question_column = f"Question_{i}"
  if question_column in yp_df.columns:
    answer_lengths = yp_df[question_column].str.len()
    average_length = answer_lengths.mean()
    average_answer_lengths.append(average_length)
  else:
    average_answer_lengths.append(0)  # Or handle missing columns differently
```

### Create a graph

```
plt.figure(figsize=(10, 6))
plt.bar(range(1, 8), average_answer_lengths)
plt.xlabel("Question Number")
plt.ylabel("Average Answer Length")
plt.title("Average Answer Length for Questions 1-7")
plt.show()
```
<img width="877" height="544" alt="Screenshot 2026-01-29 at 17 14 44" src="https://github.com/user-attachments/assets/2e4e7db2-78b9-4575-86a4-16353ca666da" />


## Graph - interview answer by age

**Task:** Make graph showing average answer length for questions 1 to 7 for different ages

```
import matplotlib.pyplot as plt
```
Assuming plwd_df is your DataFrame with interview responses and 'age' column

Calculate the average length of answers for each question (Question_1 to Question_7) for different age groups

```
average_answer_lengths_by_age = {}
for age in yp_df['age'].unique():
  age_df = yp_df[yp_df['age'] == age]
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
```

### Create a graph for each age group

```
plt.figure(figsize=(15, 8))
for age, average_answer_lengths in average_answer_lengths_by_age.items():
  plt.plot(range(1, 8), average_answer_lengths, label=f"Age: {age}")

plt.xlabel("Question Number")
plt.ylabel("Average Answer Length")
plt.title("Average Answer Length for Questions 1-7 by Age")
plt.legend()
plt.show()
```

<img width="891" height="493" alt="Screenshot 2026-01-29 at 17 14 23" src="https://github.com/user-attachments/assets/7d9aa7d9-eba3-4d1a-ae7f-a8ab4d51291a" />

## Graph - interview answer by gender

**Task:** Make graph showing average answer length for questions 1 to 7 for different gender

Assuming yp_df is your DataFrame with interview responses and 'gender' column

Calculate the average length of answers for each question (Question_1 to Question_7) for different gender groups

```
average_answer_lengths_by_gender = {}
for gender in yp_df['gender'].unique():
  gender_df = yp_df[yp_df['gender'] == gender]
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
```

### Create a bar chart for each gender group

```
plt.figure(figsize=(15, 8))
for gender, average_answer_lengths in average_answer_lengths_by_gender.items():
  plt.plot(range(1, 8), average_answer_lengths, label=f"Gender: {gender}")

plt.xlabel("Question Number")
plt.ylabel("Average Answer Length")
plt.title("Average Answer Length for Questions 1-7 by Gender")
plt.legend()
plt.show()
```
<img width="894" height="495" alt="Screenshot 2026-01-29 at 17 13 13" src="https://github.com/user-attachments/assets/3938c307-15e5-4c5c-b1d8-194ebd7f45fb" />
