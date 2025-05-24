# CiteArXiv: A Citation-enriched Dataset for Scientific Articles Summarization
This repository accompanies our paper accepted at **WWW Companion ’25, April 28-May 2, 2025, Sydney, NSW, Australia**

[Link to dataset here](https://drive.google.com/drive/folders/1A8HCo4yTZuHn6Tz3fbna5L1F72Ej3vCt?usp=sharing)

## Motivation

Citation-based summarization approach has shown promise and gained attention from the research community.
However, this approach remains underexplored at scale due to the absence of large datasets enriched with citation information, 
creating significant barriers to training robust models and conducting comprehensive evaluations.
We present CiteArXiv, a large-scale citation-enriched extension of the ArXiv dataset ([link](https://aclanthology.org/N18-2097/)), 
where each paper is paired with its citation sentences. 


## Dataset Construction

Some automatically collected datasets are larger but do not include citation information.
The CiteArXiv dataset is constructed through a two-phase process:

### Phase 1: ArXiv Training Set Sampling

Due to the large size and presence of noisy samples in the ArXiv training set (203,038 samples), a representative subset is selected to enhance diversity and generalization:
1. **Topic Clustering**: Latent Dirichlet Allocation (LDA) is used to cluster training documents into topics
2. **Topic Coherence Estimation**: Topic coherence is employed to estimate the optimal number of topics k
3. **Document Selection**: Documents are clustered by topic, and top documents with the highest probability per topic are selected

### Phase 2: Citation Augmentation
The citation augmentation process provides citation information for each input d

![image](https://github.com/user-attachments/assets/c8452252-0e0c-4b5b-a682-585649503deb)

# CiteArXiv Dataset

### Each sample in the final CiteArXiv dataset consists of:
- A document
- Relevant citations
- A summary

## Data Structure
Includes multiple samples in dictionary format:
```
sample_ID: {
    ID
    label  # summary label
    document: {
        section_list  # List of section IDs in units
        sent_list     # List of sentence IDs in units
        units: {
            unit_ID: {
                ID        # ID of unit encoded as x|y|z|t: where x = 0 if in document, x=1 if in citation, 
                         # y is the order of section containing it, z is the order of sentence containing it,
                         # and t is the order of edu (ordering starts at 1, default order is 0)
                unit_type # unit type belonging to EDU, sentence, section
                is_citation = False
                parent    # Parent of unit 
                children  # Children of unit
                cite      # Cited by citation ID
                text      # text content
            }
        }
    }
    citation: {
        section_list  # List of section IDs in units
        sent_list     # List of sentence IDs in units
        units: {
            unit_ID: {
                ID        # ID of unit encoded as x|y|z|t: where x = 0 if in document, x=0 if in citation,
                         # y is the order of section containing it, z is the order of sentence containing it, 
                         # and t is the order of edu (ordering starts at 1, default order is 0)
                unit_type # unit type: sentence, section
                is_citation = True
                parent    # Parent of unit 
                children  # Children of unit
                cite      # Cites document ID if it's a section unit, stores similarity with document section if it's a sentence unit
                text      # text content
            }
        }
    }
}
```

