# Example 4: Applying sentence similarity and sentence sentiment analysis on Nused dataset

## Purpose of this example:
This example shows a sentence similarity match score based on comments in the given dataset. We are looking for how similar comments could lead to similar sentiments - similar comments expressing similar opinions. Appreciations and thankfulness to Tomas Bird for providing the data. A note on outputs in this example, the result of this similarity is currently output as text files that contain the comments and their scores. The [Nused_similarity_pairs.txt]() contains roughly 9.9 millions rows of comments and scores, because underlying it is choosing two comments from about 4536 unique comments. The other result file [Nused_sentiment.txt](), however, only contains 4536 rows of comments and their respective sentiment analysis score, as well as a label for their tone. 

## How this example works: 
### Similarity analysis: 
Model used is: [miniLM-L6-H384-uncased](https://huggingface.co/flax-sentence-embeddings/all_datasets_v4_MiniLM-L6).   

The method first encodes all input sentences. The results are value vectors calculated based on the sentence's own semantic information. After encoding, we compute the Cosine similarity between them. In linear algebra, Cosine similarity is used to calculate the angle between vectors, thus finding the similarity in the sentences' values in this case. To interpret the result, a value closer to 1 means the angle between the vectors is small, hence high similarity. A value near 0 means low similarity. These value calculations are pretrained for the model, and cannot be changed. After the calculations are completed, we aggregate the similarity score along with the sentences for display at the end. We organized the aggregated results by their similarity score and due to the size of the output, written the output to [Nused_similarity_pairs.txt]().  

### Sentiment analysis:
Model used is: [cardiffnlp/twitter-roberta-base-sentiment-latest]()

The model can be used directly on an input. In our case, we will sort our inputs into a list, where we can then iteratively run every inputs through the model. After running them through, the model will give a label (`positive`, `neutral`, or `negative`), as well as a score to how close it is to the label. After running the calculation, the result list of comments, labels and scores is written to [Nused_sentiment.txt]().

## How to try this for yourself without coding:
### Similarity analysis:
All our model are from `https://huggingface.co`. Copy this link into your browser to try out the models.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Hugging%20face%20landing%20page.png">

Our model that we use is [miniLM-L6-H384-uncased](https://huggingface.co/flax-sentence-embeddings/all_datasets_v4_MiniLM-L6), which calculates the similarity between sentences.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Model%20page.png">

On the right side, you can find an interface API where you can try out any sentences. In the picture below, we tried "That is a happy person" with other sentences such as "That is a happy dog", "That is a very happy person", "Today is a sunny day". By clicking "Compute", we can get the similarity score to check. These scores means how similar the sentences are.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Model%20test.png">

### Sentiment analysis:
All our model are from `https://huggingface.co`. Copy this link into your browser to try out the models.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Hugging%20face%20landing%20page.png">

Our model that we use is [twitter-roberta-base-sentiment-latest](https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment-latest), which calculates the sentiment of inputs.  
<img width="640" alt="image" src="">

On the right side, you can find an interface API where you can try out any sentences. The example here is "Covid cases are increasing fast!" and the model gives the sentiment score below.
<img width="640" alt="image" src="">

## File: [Sentence_similarity_pairs.ipynb](https://github.com/agiga-quanta/Translation-is-fun/blob/main/Examples/Sentence%20similarity%20pairs/Sentence_similarity_pairs.ipynb)

## Instructions and explanation: 
### Similarity analysis:
We are using Jupyter notebook here, which allows us to run pip to install `sentence_transformer` from hugging face. This package is necessary for using the model in this example.  
```
pip install sentence-transformers
```

After we install the package, we need to change the working directory to link to Google Drive.  
```python
# Allowing colab to access your google drive, as well as reading and writing in it.
from google.colab import drive
drive.mount("/content/drive")
```

And since our data is not at the homepage of our google drive, we will have to change the directory.
```
cd drive/MyDrive/TIS
```

From the package, we import `SentenceTransformer` which is necessary to encode our inputs. The `pandas` module is also needed to organize and read the raw data. We select the model `flax-sentence-embeddings/all_datasets_v4_MiniLM-L6` from hugging face. The data here, however, is not taken directly from the excel file. To get `NuSeds test.tsv`, you will have to do the following step:

1. Open the `Nuseds comments area 25.xlsx`.
2. Navigate to `Data` sheet.
3. Copy the column `Comments` and paste it into a new empty sheet.
4. Export that single sheet as a Tabular Seperated Value (.tsv) file, and name it `NuSeds test.tsv`.

```python
# Using the same model as example 1 and 2. Our data is saved in a tsv file
# The tsv file is made by copying the "Comments" column into an excel sheet and export it as a tsv file
from sentence_transformers import SentenceTransformer, util;
import pandas as pd;
model = SentenceTransformer('all-MiniLM-L6-v2')
data_df = pd.read_csv('NuSeds test.tsv', sep='\t')
```

However, before we encode the comments similar to how we do it before, we have to clean the data of repetitive comments, and comments with needless space in it.  We first extract the data into a list for easier iterating. We then import `re` module, that can helps us remove the empty space in a comment from `This  is  a     comment` to `This is a comment`. Afterward, we then take out the unique comments again. The final result is that from about 19 thousand comments, we have extracted 4 thousand unique comments.  
```python
#Getting comments list without empty space and identical comments
comments_list = [*set(data_df.Comments.tolist())]

#Removing comments with multiple empty spaces together
import re
comments_range = range(len(comments_list))
for i in comments_range:
  comments_list[i] = re.sub(' +', ' ', comments_list[i])

#Getting comments list without empty space and identical comments
comments_list_unique = [*set(comments_list)]
```

With each step here, we first encode our sentences with `model.encode`. This will encode all inputs and give them a vector of values based on the encoding parameters of the model.   
```python
#Encode all sentences
embeddings = model.encode(comments_list_unique)
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
all_comments_combinations = []
for i in range(len(cos_sim)-1):
    for j in range(i+1, len(cos_sim)):
        all_comments_combinations.append([cos_sim[i][j], i, j])

#Sort list by the highest cosine similarity score
all_comments_combinations = sorted(all_comments_combinations, key=lambda x: x[0], reverse=True)
```

Last but not least, our result will be written out for further usage.
```python
# Writing our results in txt file due to the massive size.
with open('nused_pair_result.txt', 'w') as f:
    for score, i, j in all_comments_combinations:
        f.write("{} | {} | {:.4f}\n".format(comments_list_unique[i], comments_list_unique[j], cos_sim[i][j]))
```


### Sentiment analysis:
We are using Jupyter notebook here, which allows us to run pip to install `sentence_transformer` from hugging face. This package is necessary for using the model in this example.  
```
pip install sentence-transformers;
```

After we install the package, we need to change the working directory to link to Google Drive.  
```python
# Allowing colab to access your google drive, as well as reading and writing in it.
from google.colab import drive
drive.mount("/content/drive")
```

And since our data is not at the homepage of our google drive, we will have to change the directory.
```
cd drive/MyDrive/TIS
```

Since Huggingface already has a pipeline specialized for sentiment analysis, we can use that one and the model [cardiffnlp/twitter-roberta-base-sentiment-latest](). However, this model does have a limitation of maximum input length of 3825 characters, as well as allowing the model to truncate our inputs when it can, so we have to set it here.  
```python
# Using a model for sentence classification. The model cardiffnlp/twitter-roberta-base-sentiment-latest will give the highest scoring sentiment and its label. The maximum input is only 1064 character, so anything longer has been replaced with label 'na' and score 'na' points. 
from transformers import pipeline
sentiment_task = pipeline("sentiment-analysis", model='cardiffnlp/twitter-roberta-base-sentiment-latest', tokenizer='cardiffnlp/twitter-roberta-base-sentiment-latest', max_length = 3825, truncation = True)
```

From the package, we import `SentenceTransformer` which is necessary to encode our inputs. The `pandas` module is also needed to organize and read the raw data. We select the model `flax-sentence-embeddings/all_datasets_v4_MiniLM-L6` from hugging face. The data here, however, is not taken directly from the excel file. To get `NuSeds test.tsv`, you will have to do the following step:

1. Open the `Nuseds comments area 25.xlsx`.
2. Navigate to `Data` sheet.
3. Copy the column `Comments` and paste it into a new empty sheet.
4. Export that single sheet as a Tabular Seperated Value (.tsv) file, and name it `NuSeds test.tsv`.
```python
# Our data is saved in a tsv file
# The tsv file is made by copying the "Comments" column into an excel sheet and export it as a tsv file
import pandas as pd;
data_df = pd.read_csv('NuSeds test.tsv', sep='\t')
```
However, before we encode the comments similar to how we do it before, we have to clean the data of repetitive comments, and comments with needless space in it.  We first extract the data into a list for easier iterating. We then import `re` module, that can helps us remove the empty space in a comment from `This  is  a     comment` to `This is a comment`. Afterward, we then take out the unique comments again. The final result is that from about 19 thousand comments, we have extracted 4 thousand unique comments.  
```python
#Getting comments list without empty space and identical comments
comments_list = [*set(data_df.Comments.tolist())]

#Removing comments with multiple empty spaces together
import re
comments_range = range(len(comments_list))
for i in comments_range:
  comments_list[i] = re.sub(' +', ' ', comments_list[i])

#Getting comments list without empty space and identical comments
comments_list_unique = [*set(comments_list)]
```

For the sentiment analysis, we create an iterative loop where we run the model through each of the comments and only keep the label with the highest score, and the score itself. 
```python
comment_range = range(len(comments_list_unique))

# Running the sentiment analysis after checking input length. 
comments_list_sentiment = []
for i in comment_range:
  if len(comments_list_unique[i]) > 1064:
    comments_list_sentiment.append(dict({'comment': comments_list_unique[i], 'label': 'na', 'score': 'nan'}))
    continue
  #print(i, '\t', len(comments_list_unique[i]))
  sentiment_temp = sentiment_task(comments_list_unique[i])[0]
  comments_list_sentiment.append(dict({'comment': comments_list_unique[i], 'label': sentiment_temp['label'], 'score': sentiment_temp['score']}))
```

Last but not least, our result will be written out for further usage.
```python
# Writing our results in txt file due to the size.
with open('nused_senti_result.txt', 'w') as f:
    for i in comment_range:
        f.write("{} | {} | {}\n".format(comments_list_sentiment[i]['comment'], comments_list_sentiment[i]['label'], comments_list_sentiment[i]['score']))
```