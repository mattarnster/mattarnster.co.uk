---
title: "Finding and Replacing Text in a Word Document Using python-docx"
date: 2019-12-15
draft: false
summary: "Tutorial on how to find and replace text in a word document using the python-docx package"
categories: ['Tutorials', 'Automation']
---

Recently I wrote a program using Python to automate the generation of dividend slips for my company.
My requirements were simple:
 1. I needed to read from a CSV file the amounts, dates and addresses of shareholders
 2. Check that a dividend slip file didn't already exist
 3. Open the word template which contained the dividend slip
 4. Replace certain 'trigger words' with the amount, date and addresses
 5. Save the file, not overwriting the original template

In order to accomplish this, I turned to my favorite search-engine and looked for Python packages which would allow me to interface with Word documents.
Straight away, I found [python-docx](https://python-docx.readthedocs.io/en/latest/).

python-docx doesn't have any find/replace features as standard, but I figured out I could just iterate through the paragraphs and then replace text using python's str() functions.

Here's my script:

```python
import docx
import csv
import pathlib

if __name__ == "__main__":
    with open("Dividends.csv") as f:
        reader = csv.DictReader(f)
        for line in reader:
            print("Checking: " + line["Date"] + ".docx")
            if pathlib.Path(line["Date"] + ".docx").exists() == False:
                doc = docx.Document("dividend-template.docx")
                if line["Amount"] != "":
                    print("Generating new Dividend slip for: " + line["Date"])
                    for paragraph in doc.paragraphs:
                        if "DTE" in paragraph.text:
                            orig_text = paragraph.text
                            new_text = str.replace(orig_text, "DTE", line["Date"])
                            paragraph.text = new_text
                        if "AMNT" in paragraph.text:
                            orig_text = paragraph.text
                            new_text = str.replace(orig_text, "AMNT", line["Amount"])
                            paragraph.text = new_text
                        if "ADDR" in paragraph.text:
                            orig_text = paragraph.text
                            new_text = str.replace(orig_text, "ADDR", line["Address"])
                            paragraph.text = new_text
                    print("Saving: " + line["Date"] + ".docx")
                    doc.save(line["Date"] + ".docx")
```