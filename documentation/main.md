# Sample change for testing

Main page for content

Additional content


Now a nested list:

First, get these ingredients:

carrots
celery
lentils
Boil some water.

Dump everything in the pot and follow this algorithm:

find wooden spoon
uncover pot
stir
cover pot
balance wooden spoon precariously on pot handle
wait 10 minutes
goto first step (or shut off burner when done)
Do not bump wooden spoon or it will fall.

Notice again how text always lines up on 4-space indents (including that last line which continues item 3 above).

Here's a link to a website, to a local doc, and to a section heading in the current doc. Here's a footnote 1.

Tables can look like this:

size material color

---
title: Tutorial title
shortTitle: <subject> # Max 31 characters
intro: 'Article intro. See tips for a great intro below'
product: "{{ optional product callout }}"
type: tutorial
topics:
  - <topic> # One or more from list of allowed topics: https://github.com/github/docs/blob/main/data/allowed-topics.js
versions:
  - <version>
---

{% comment %}
Follow the instructions in https://docs.github.com/contributing/writing-for-github-docs/content-model#quickstart to write this article.
Great intros clarify who the tutorial is intended for, state what the user will accomplish, and state the technologies that will be used.
For product callout info, see https://github.com/github/docs/tree/main/content#product
For product version instructions, see https://github.com/github/docs/tree/main/content#versioning
Remove these comments from your article file when you're done writing
{% endcomment %}

## Introduction

{% comment %}
The tutorial introduction should include the following in a short paragraph -

- Clarify audience
- State prerequisites and prior knowledge needed
- State what the user will accomplish or build and the user problem it solves
- Link to an example of the project the user will complete
{% endcomment %}

## Step 1: Action the user will take

{% comment %}
In one sentence, describe what the user will do in this step
Steps should break down the tasks the user will complete in sequential order
Avoid replicating conceptual information that is covered elsewhere, provide inline links instead. Only include conceptual information unique to this use case.
{% endcomment %}

### Task chunk

{% comment %}
A step may require the user to perform several tasks - break those tasks down into chunks, allowing the user to scan quickly to find their place if they navigated away from this screen to perform the task.
An example might be creating a personal access token for the action to use and then storing it in secrets
For UI based tasks, include the button or options the users should click
If the task adds code, include the code in context (don't just show `needs: setup` show the entire `setup` and `dependent` jobs)
{% endcomment %}

### Another task chunk

## Step 2: Do the next thing

{% comment %}
Rinse and repeat, adding steps and tasks until the tutorial is complete
Remember to show code snippets in context
{% endcomment %}

## Further reading

{% comment %}
Include a bulleted list of tutorials or articles the user can reference to extend the concepts taught in this tutorial
{% endcomment %}

- [Article title](article-URL)
