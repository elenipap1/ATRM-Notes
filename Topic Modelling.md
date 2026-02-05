# Topic Modelling

Choosing a question
```
question_number = "2" # @param {"type":"string","placeholder":"1"}
n_topics = 4 # @param {"type":"integer","placeholder":"5"}
question_column = f"Question_{question_number}"
```

```
docs = plwd_df[question_column].dropna().astype(str).tolist()
docs = [d.strip() for d in docs if len(d.strip()) > 20]
k = n_topics

try:
    tv = TopicViz(n_topics=k, random_state=42)
    tv.fit(docs, embedding_model=embedding_model)


    fig = tv.visualize_topics(
            width=800, height=800,
            colorscale="Blues",
            density=True,
            show_text=True,
            repel_labels=True,
            topic_label_mode="embedding",
            label_size_ratio=60
        )
    fig.update_xaxes(autorange=True)
    fig.update_yaxes(autorange=True)
    fig.show()
except Exception as e:
    print("Failed:", e)
```

## Bourdieu Map

Visualise Bourdieu map:
```
fig = tv.visualize_bourdieu(
    embedding_model=embedding_model,
    x_left_words=["This is positive"],
    x_right_words=["This is negative"],
    y_top_words=["This is inside the house"],
    y_bottom_words=["This is outside the house"],
    radius_size=0.2,
    height=800,
    width=800,
    clustering=False,
    density=False,
    convex_hull=True,
)

fig.show()
```
