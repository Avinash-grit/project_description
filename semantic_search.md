## An NLP Semantic Search Pipeline

### Project Summary:
Goal- To find the duplicate issue/ticket in Automative issue management system. where technician describe about issues while vehicle is in launch phase.

### Business Context:

The data sources contain technician comments about issues in a vehicle development phase before the final launch of a vehicle.
Using NLP techniques (like semantic search) to find if there is any duplicate, similar issue arise for any other program in past? 
if yes, then that existing ticket will help the current engineer to get the solution in relatively less time. 

### Technology Stack:
 
![Sentence Transformers](https://img.shields.io/badge/-SentenceTransformers-green?style=for-the-badge=white) 
![HuggingFace Transformers](https://img.shields.io/badge/-Transformers-blue?style=for-the-badge=white) 
![SpaCy](https://img.shields.io/badge/-SpaCy-green?style=for-the-badge=white) 
![Sklearn](https://img.shields.io/badge/-Sklearn-green?style=for-the-badge=white) 
![Docker](https://img.shields.io/badge/-Docker-green?style=for-the-badge=white) 
![Kubernetes](https://img.shields.io/badge/-Kubernetes-blue?style=for-the-badge=white) 
![Streamlit](https://img.shields.io/badge/-Streamlit-yellow?style=for-the-badge=black)  


### Detailed Pipeline:

![SBERT Source](https://raw.githubusercontent.com/UKPLab/sentence-transformers/master/docs/img/InformationRetrieval.png)

- Inspired by the above Retriever - Reader pipeline but strengthened it by ensembling results of 3 pairs of Retriever-Reader models wherein
    - the `Retriever` narrows down the search space and
    - the `Reader` zeros in on the right results

- Ensemble of 3 `Retriever-Reader` Models
