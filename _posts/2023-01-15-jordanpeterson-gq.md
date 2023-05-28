---
layout: post
title: Interpreting the Comments of Jordan Peterson's Most Viewed YouTube Interview
---

<img src="/public/uploads/jp-gq.gif" width="900">

In the digital age, social media has become a powerful tool for expressing opinions and beliefs. YouTube is one of the most popular video-sharing websites, and researchers have been exploring how people evaluate the views presented on these platforms. Results show that users not only engage with what they see, but also with other people's opinions, creating echo chambers where individuals are exposed only to similar ideas and perspectives.

To gain an understanding of what people think and believe, sentiment analysis can be used as a technique. My article examines the results of such an analysis on the most-watched video of Jordan Peterson from British GQ, which has received over 60 million views. 30,000 comments with the most _likes_ were analyzed with natural language processing (TextBlob) and machine learning algorithms.

![Workflow](/public/uploads/jp-workflow.jpg)

Results showed that most comments were neutral in terms of polarity, indicating neither positive nor negative statements. Additionally, they were not necessarily detected as being subjective. When manually tagged rows were analyzed it revealed that 88 expressed support for Peterson while one supported the interviewer.

This study provides evidence for the existence of echo chambers in online discourse which can lead to people not taking into account essential ideas or actively stifling dissenting voices due to their own self-reinforcing beliefs. Additionally, there are various methods that can be used to improve accuracy when analyzing sentiments such as using the VADER library or SENTIWORDNET which can potentially increase precision when tagging content for valid results. Here I present the primary dataset and other resources used to carry on my analysis.

[Download Dataset](https://docs.google.com/spreadsheets/d/1HKPJJin4aYQzX1ur9wUADYQLi76MPnc2/edit?usp=sharing&ouid=105091277710928109266&rtpof=true&sd=true)

### JPSA — Python SA Tagger

> After passing the final [dataset](https://docs.google.com/spreadsheets/d/1HKPJJin4aYQzX1ur9wUADYQLi76MPnc2/edit?usp=sharing&ouid=105091277710928109266&rtpof=true&sd=true) through JPSA, the average Polarity and Subjectivity of all the comments were calculated.

```python
# Linguistic Perspectives MELL 2021
# Version 0.1.0

import csv
import time
from textblob import TextBlob

def read_dataset(filename):
    with open(filename, "r", encoding="UTF-8") as inputfile:
        dataset_list = inputfile.readlines()
        dataset_list = list(map(str.strip, dataset_list))
    return dataset_list

def perform_sentiment_analysis(dataset_list):
    results_list = []
    for comment in dataset_list:
        tb = TextBlob(comment)
        polarity = round(tb.polarity, ROUNDING_PRECISION)
        subjectivity = round(tb.subjectivity, ROUNDING_PRECISION)
        results_list.append([comment, polarity, subjectivity])
    return results_list

def write_results_to_csv(results_list, filename):
    header_fields = ["Comment", "Polarity", "Subjectivity"]
    with open(filename, "w", encoding="UTF-8") as csvfile:
        csvwriter = csv.writer(csvfile)
        csvwriter.writerow(header_fields)
        csvwriter.writerows(results_list)

def main():
    start_time = time.time()

    dataset_list = read_dataset("dataset.csv")
    results_list = perform_sentiment_analysis(dataset_list)
    write_results_to_csv(results_list, "output.csv")

    print("Process finished in: %s seconds\n" % round((time.time() - start_time), ROUNDING_PRECISION))

if __name__ == "__main__":
    ROUNDING_PRECISION = 3
    main()

```
