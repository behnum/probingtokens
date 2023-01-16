---
layout: post
title: Interpreting the Comments on Jordan Peterson's Most Viewed YouTube Interview
---

<video src="/public/uploads/jp-gq.mov" autoplay>
</video>

In the digital age, social media has become a powerful tool for expressing opinions and beliefs. YouTube is one of the most popular video-sharing websites, and researchers have been exploring how people evaluate the views presented on these platforms. Results show that users not only engage with what they see, but also with other people's opinions, creating echo chambers where individuals are exposed only to similar ideas and perspectives.

To gain an understanding of what people think and believe, sentiment analysis can be used as a technique. My article examines the results of such an analysis on the most-watched video of Jordan Peterson from British GQ, which has received over 60 million views. 30,000 comments with the most _likes_ were analyzed with natural language processing (TextBlob) and machine learning algorithms.

![Workflow](/public/uploads/jp-workflow.jpg)

Results showed that most comments were neutral in terms of polarity, indicating neither positive nor negative statements. Additionally, they were not necessarily detected as being subjective. When manually tagged rows were analyzed it revealed that 88 expressed support for Peterson while one supported the interviewer.

This study provides evidence for the existence of echo chambers in online discourse which can lead to people not taking into account essential ideas or actively stifling dissenting voices due to their own self-reinforcing beliefs. Additionally, there are various methods that can be used to improve accuracy when analyzing sentiments such as using the VADER library or SENTIWORDNET which can potentially increase precision when tagging content for valid results. Here I present the primary dataset and other resources used to carry on my analysis.

[Download Dataset](https://docs.google.com/spreadsheets/d/1HKPJJin4aYQzX1ur9wUADYQLi76MPnc2/edit?usp=sharing&ouid=105091277710928109266&rtpof=true&sd=true)

### JPSA — Python SA Tagger

> After passing the final [dataset](https://docs.google.com/spreadsheets/d/1HKPJJin4aYQzX1ur9wUADYQLi76MPnc2/edit?usp=sharing&ouid=105091277710928109266&rtpof=true&sd=true) through JPSA, the average Polarity and Subjectivity of all the comments were calculated.

```python
# Behnam Baharmand (bb222nm@student.lnu.se)
# Linguistic Perspectives MELL 2021
# Version 0.1.0

import csv
import time
from textblob import TextBlob

# Takes in dataset.csv which is one comment per line.
# Saves output.csv which is original_comment | polarity | sentiment

start_time = time.time()  # Let's see how long it takes to run the code

results_list = []

with open("dataset.csv", "r", encoding="UTF-8") as inputfile:
    dataset_list = inputfile.readlines()  # list of lines
    dataset_list = list(map(str.strip, dataset_list))  # Remove line breaks

    for item in dataset_list:
        tb = TextBlob(item)
        polarity = round(tb.polarity, 3)
        subjectivity = round(tb.subjectivity, 3)
        results_list.append([item, polarity, subjectivity])

print(len(results_list))

export_filename = "jspa-output.csv"
header_fields = ["Comment", "Polarity", "Subjectivity"]
rows = results_list

with open(export_filename, "w", encoding="UTF-8") as csvfile:
    csvwriter = csv.writer(csvfile)  # Createa csv writer obj
    csvwriter.writerow(header_fields)  # writing the field

    csvwriter.writerows(results_list)  # writing the data rows

print("▐░░ Processs finished in: %s seconds\n" % round((time.time() - start_time), 3))

```
