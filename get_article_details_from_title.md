---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.16.1
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

```python
import crossref.restful
import arxiv
```

# Using crossref and arxiv APIs to get article information based on only the title

A bit hackey, but pretty cool, and should just work as long as your title is unique (and your arxiv title matches the journal title). 

```python
client = arxiv.Client() 
works = crossref.restful.Works()
```

```python
def get_record_from_paper_title(paper_title):
    record = {}
    record["title"] = paper_title
    
    # Grab the arxiv id
    search = arxiv.Search(query = paper_title, max_results = 1)
    results = client.results(search)
    for r in results:
        break
    record["arxiv_link"] = r.entry_id
    
    # Grab the paper info from crossref
    w = works.query(bibliographic=paper_title)
    for paper in w:
        break
    
    author_list = ""
    for a in paper["author"]:
        # print(a)
        author_list += a["given"]
        author_list += " " + a["family"]
        author_list += ", "
    author_list = author_list.rstrip(", ")
    record["author_list"] = author_list
    
    year = paper["created"]['date-parts'][0][0]
    record["year"] = year

    journal_ref = paper["short-container-title"][0]
    journal_ref += " " + paper["volume"]
    if "page" in paper.keys():
        journal_ref += ", " + paper["page"]
    elif "article-number" in paper.keys():
        journal_ref += ", " + paper["article-number"]    
    journal_ref += " (%d)" % year
    record["journal_ref"] = journal_ref
    record["doi"] = paper["DOI"]
    record["journal_link"] = "http://dx.doi.org/" + paper["DOI"]

    return record
```

```python
title = "Imaging Transport Resonances in the Quantum Hall Effect"
get_record_from_paper_title(title)
```

```python
title = "Photon-Pressure Strong-Coupling between two Superconducting Circuits"
get_record_from_paper_title(title)
```

```python
title = "Multi-Tone Microwave Locking via Real-Time Feedback"
get_record_from_paper_title(title)
```

### Todo

* Make it able to handle arxiv entries that have no journal article (yet)
  * Would have to pull instead article info from arxiv api
* Handle (naughty) journal articles that have no arxiv entry
  * easy, would just drop arxiv tag, or set it to None
* Maybe do some fuzzy logic to deal with papers that have slightly different titles on the arxiv and in the journal
