# Example 4: Applying sentence similarity and sentence sentiment analysis on Nused dataset

## Purpose of this example:
This example shows a sentence similarity match score based on comments in the given dataset. We are looking for how similar comments could lead to similar sentiments - similar comments expressing similar opinions. Appreciations and thankfulness to Tomas Bird for providing the data. A note on outputs in this example, the result of this similarity is currently output as text files that contain the comments and their scores. The [Nused_similarity_pairs]() contains roughly 9.9 millions rows of comments and scores, because underlying it is choosing two comments from about 4536 unique comments. The other result file [Nused_sentiment](), however, only contains 4536 rows of comments and their respective sentiment analysis score, as well as a label for their tone. 

## How to this example works: 
### Similarity analysis: 
Model used is: [miniLM-L6-H384-uncased](https://huggingface.co/flax-sentence-embeddings/all_datasets_v4_MiniLM-L6).   

The method first encodes all input sentences. The results are value vectors calculated based on the sentence's own semantic information.  

After encoding, we compute the Cosine similarity between them. In linear algebra, Cosine similarity is used to calculate the angle between vectors, thus finding the similarity in the sentences' values in this case. To interpret the result, a value closer to 1 means the angle between the vectors is small, hence high similarity. A value near 0 means low similarity. These value calculations are pretrained for the model, and cannot be changed.  

After the calculations are completed, we aggregate the similarity score along with the sentences for display at the end. We organized the aggregated results by their similarity score.  

### Sentiment analysis:
Model used is: []

## How to try this for yourself without coding:
### Similarity analysis:
All our model are from `https://huggingface.co`. Copy this link into your browser to try out the models.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Hugging%20face%20landing%20page.png">

Our model that we use is [miniLM-L6-H384-uncased](https://huggingface.co/flax-sentence-embeddings/all_datasets_v4_MiniLM-L6), which calculates the similarity between sentences.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Model%20page.png">

On the right side, you can find an interface API where you can try out any sentences. In the picture below, we tried "That is a happy person" with other sentences such as "That is a happy dog", "That is a very happy person", "Today is a sunny day". By clicking "Compute", we can get the similarity score to check. These scores means how similar the sentences are.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Model%20test.png">

### Sentiment analysis:


## File: [Sentence_similarity_pairs.ipynb](https://github.com/agiga-quanta/Translation-is-fun/blob/main/Examples/Sentence%20similarity%20pairs/Sentence_similarity_pairs.ipynb)

## Instructions and explanation: 
We are using Jupyter notebook here, which allows us to run pip to install `sentence_transformer` from hugging face. This package is necessary for using the model in this example.  
```
pip install sentence-transformers
```

From the package, we import `SentenceTransformer` which is necessary to encode our inputs (in this case, they are sentences).  
```python
from sentence_transformers import SentenceTransformer, util
```

We select the model `flax-sentence-embeddings/all_datasets_v4_MiniLM-L6` from hugging face.

With each step here, we first encode our sentences with `model.encode`. This will encode all inputs and give them a vector of values based on the encoding parameters of the model.   
```python
#Encode all sentences
embeddings = model.encode(sentences)
```

Because our inputs are now vectors of numerical values, we can perform a cosine similarity to find how similar they are. With a value closer to 1, they are highly similar, and closer to 0, they are less similar. This is calculated with `cos_sim` from `util` that we imported from `sentence_transformer` from the beginning.  
```python
#Compute cosine similarity between all pairs
cos_sim = util.cos_sim(embeddings, embeddings)
```

Now after we calculated, we aggregate them together into a list of sentences and similarity score.  
Finally, we sort them with the `sorted` function, based on the first value in the list - in this case is the similarity score - in descending order.  
```python
#Add all pairs to a list with their cosine similarity score
all_sentence_combinations = []
for i in range(len(cos_sim)-1):
    for j in range(i+1, len(cos_sim)):
        all_sentence_combinations.append([cos_sim[i][j], i, j])

#Sort list by the highest cosine similarity score
all_sentence_combinations = sorted(all_sentence_combinations, key=lambda x: x[0], reverse=True)