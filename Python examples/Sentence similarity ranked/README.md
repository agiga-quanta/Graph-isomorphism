# Example 2: Sentence similarity ranked
## Purpose:
This example shows a similarity match score based on given inputs. You provide the sentences, and by running the code below, the model will output a similarity score between each inputs. In more details compare to the previous example, here we will compare all other inputs to the first one only.  
## Method: 
Model used is: [miniLM-L6-H384-uncased](https://huggingface.co/flax-sentence-embeddings/all_datasets_v4_MiniLM-L6)

The method first encodes all input sentences. The results are value vectors calculated based on the sentence's own semantic information.   

After encoding, we compute the Cosine similarity between them. In linear algebra, Cosine similarity is used to calculate the angle between vectors, thus finding the similarity in the sentences' values in this case. To interpret the result, a value closer to 1 means the angle between the vectors is small, hence high similarity. A value near 0 means low similarity. These value calculations are pretrained for the model, and cannot be changed.  

After the calculations are completed, we aggregate the similarity score along with the sentences for display at the end. We organized the aggregated results by their similarity score.  

## How to try this for yourself without coding
All our model are from `https://huggingface.co`. Copy this link into your browser to try out the models.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Hugging%20face%20landing%20page.png">

Our model that we use is [miniLM-L6-H384-uncased](https://huggingface.co/flax-sentence-embeddings/all_datasets_v4_MiniLM-L6), which calculates the similarity between sentences.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Model%20page.png">

On the right side, you can find an interface API where you can try out any sentences. In the picture below, we tried "That is a happy person" with other sentences such as "That is a happy dog", "That is a very happy person", "Today is a sunny day". By clicking "Compute", we can get the similarity score to check. These scores means how similar the sentences are.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Model%20test.png">


## File: [Sentence_similarity_ranked.ipynb](https://github.com/agiga-quanta/Translation-is-fun/blob/main/Python%20examples/Sentence%20similarity%20ranked/Sentence_similarity_ranked.ipynb)
## Instructions and explanation: 

We are using Jupyter notebook here, which allows us to run pip to install `sentence_transformer` from hugging face. This package is necessary for using the model in this example.  
```
pip install sentence-transformers
```

From the package, we import `SentenceTransformer` which is necessary to encode our inputs (in this case, they are sentences). 
```python
from sentence_transformers import SentenceTransformer, util
```

We select the model `flax-sentence-embeddings/all_datasets_v4_MiniLM-L6` from hugging face, and select the following sentences from these sources:  
1. [Our strategy aims to stem the steep decline of many Pacific salmon populations and to protect and rebuild stocks where possible by implementing a series of immediate and long-term solutions that focus on 4 key areas: conservation and stewardship, salmon enhancement, harvest transformation, integration and collaboration.](https://www.dfo-mpo.gc.ca/campaign-campagne/pss-ssp/index-eng.html)
2. [The Adams River Salmon Society is run by a volunteer board of directors who are elected by the membership at the Annual General Meeting. Our Mission: To promote the conservation of wild salmon and the cultural resources of Tsútswecw Provincial Park through education and interpretation. Our Vision: An interpretive facility ... and cultural resources of Tsútswecw Provincial Park](https://www.salmonsociety.com/salmon-society/about/) 
3. [Watershed Watch Salmon Society is a science-based charity working to defend and rebuild B.C.’s wild salmon. Our History: Watershed Watch Salmon Society was founded two decades ago by a small group of passionate conservationists. Led by ecologist Craig Orr, they noticed that while a lot of work relating to fisheries ... to talk about our work or join us in the field, you are an important part of Watershed Watch.](https://watershedwatch.ca/about-us/)
4. [Fraser Valley Salmon Society is a non-profit society that ensures and advocates for the rights for all anglers in non-tidal waters and works with DFO as co-managers in conservation and habitat protection. The FVSS is also heavily involved in the training and education of anglers and the public to sport fishing. The following ... Provide catch/effort data via our logbook. Using newsletters, mall displays and media; inform the public about responsibilities and opportunities for recreational angling. Represent the resource to everyone's best interest. ](https://www.fraservalleysalmonsociety.ca/)
5. [The Sunshine Coast Salmonid Enhancement Society aims to assist, encourage, and promote salmonid enhancement in the streams of the Sunshine Coast and to provide educational services to the public regarding salmonids. We are fortunate to have the assistance of Howe Sound Pulp Mill as their cooler water is instrumental in providing summer ... and cash from many sources such as our members, the general public, SCRD and the District of Sechelt, SC Credit Union, and many local businesses.](https://scsalmon.org/about-scses)
6. [Alaska Salmon Season Officially Begins Next Week. Alaska’s first fresh salmon of the season always creates extreme interest for restaurant. Alaska’s 2023 salmon season officially gets underway on Monday, May 15 with the arrival of sockeyes and Chinook to the Copper River near Cordova. State fishery managers have announced there will be a 12-hour opener starting at 7am for the drift gillnet fleet. The 2023 Copper River sockeye salmon total run forecast is estimated at 1,646,000 fish, which is 14% below the 10-year average. That would produce a commercial harvest of 987,000 sockeye salmon for the season.](https://www.seafoodnews.com/Story/1252218/Alaska-Salmon-Season-Officially-Begins-Next-Week)
7. [Lobster Fishery Halts as Buyers Stop Purchasing from Harvesters, Says FFAW. The lobster fishery has come to a screeching halt after buyers stopped purchasing product from harvesters. It’s the second closure in the fishery in a week according to FFAW Secretary-Treasurer, Jason Spingle. Spingle says the union’s phones have been ringing ... He says they want to allow buyers with valid licences from Nova Scotia, New Brunswick or Maine to come in and buy lobster from harvesters at the prices set in this province.](https://vocm.com/2023/05/04/lobster-fishery-halts-as-buyers-stop-purchasing-from-harvesters-says-ffaw/)
8. [What You’ll Need For The Best Baked Salmon. When it comes to salmon, don’t overthink it. All you really need is a simple lemon butter sauce and a sprinkle of herbs to make it super delicious. Salmon: You can make this with individual filets or one large piece (aka. half of a salmon). It works great either way! Butter: Keeps the salmon moist and adds that delicious buttery taste. Lemon Juice: A squeeze of lemon adds a bright and zesty touch. Garlic: I’m using 3 garlic cloves, but feel free to add more or less! Herbs: A parsley and dill combo complements the salmon perfectly.](https://downshiftology.com/recipes/best-baked-salmon/)  

These are also shown in the code below. Copy it so you don't have to type and search the full sentences. The sentences are enclosed in triple \" because they also have `newline` with them. To work without the triple \", you would have to put everything on one line.    
```python
model = SentenceTransformer('flax-sentence-embeddings/all_datasets_v4_MiniLM-L6')

# https://www.dfo-mpo.gc.ca/campaign-campagne/pss-ssp/index-eng.html
# https://www.salmonsociety.com/salmon-society/about/
# https://watershedwatch.ca/about-us/
# https://www.fraservalleysalmonsociety.ca
# https://scsalmon.org/about-scses
# https://www.seafoodnews.com/Story/1252218/Alaska-Salmon-Season-Officially-Begins-Next-Week
# https://vocm.com/2023/05/04/lobster-fishery-halts-as-buyers-stop-purchasing-from-harvesters-says-ffaw/
# https://downshiftology.com/recipes/best-baked-salmon/

sentences = [
  """
  Our strategy aims to stem the steep decline of many Pacific salmon populations and to protect and rebuild stocks where possible by implementing a series of immediate and long-term solutions that focus on 4 key areas: conservation and stewardship, salmon enhancement, harvest transformation, integration and collaboration.
  """,
  """
  The Adams River Salmon Society is run by a volunteer board of directors who are elected by the membership at the Annual General Meeting.
  Our Mission: To promote the conservation of wild salmon and the cultural resources of Tsútswecw Provincial Park through education and interpretation.
  Our Vision: An interpretive facility has been developed to help visitors to appreciate and understand the ecology of the salmon and the environment. The Interpretive Centre plays an important role in the regional economy by attracting people to the area and providing recreational opportunities that enhance the quality of life for residents and visitors. Additionally, the Society has been working with our partners on the restoration of the former overflow parking lot, which began with the planting of over 1,000 tree seedlings in the spring of 2010. Each year the Society takes on other projects to help with fulfilling our mandate to encourage and facilitate the education and conservation of the natural and cultural resources of Tsútswecw Provincial Park
  """,
  """
  Watershed Watch Salmon Society is a science-based charity working to defend and rebuild B.C.’s wild salmon. 
  Our History: Watershed Watch Salmon Society was founded two decades ago by a small group of passionate conservationists. Led by ecologist Craig Orr, they noticed that while a lot of work relating to fisheries and habitat restoration was being done on the ground on local waterways, no one was pulling together the pieces to confront policy decisions at the federal and provincial levels. Craig and his associates launched Watershed Watch Salmon Society to fill that gap, offering scientific expertise to policy makers, and highlighting the large scale issues affecting local waterways, from fish farms to climate change.
  Our Work Today: Today, though Watershed Watch is still small, with a core staff of less than 10 people, we have grown into a highly effective advocacy organization. Our work is still grounded in sound science, and as always, we tackle the complex issues facing wild salmon through collaboration with other organizations and community groups. Over the years, we’ve learned how to use digital tools to expand our reach, but at heart, we are still a bunch of fish enthusiasts. We love nothing more than getting out in the field in our rubber boots, to collect data or maybe to swing some flies.
  Our Community: Watershed Watch Salmon Society involves many people beyond our paid staff and dedicated board members. We have thousands of supporters and a growing crew of volunteers who are passionate about wild salmon. Whether you read our emails, sign our petitions, donate, get out in your community to talk about our work or join us in the field, you are an important part of Watershed Watch.
  """,
  """
  Fraser Valley Salmon Society is a non-profit society that ensures and advocates for the rights for all anglers in non-tidal waters and works with DFO as co-managers in conservation and habitat protection. The FVSS is also heavily involved in the training and education of anglers and the public to sport fishing.
  The following activities take place at the Fraser Valley Salmon Society throughout the year. Ensure angler access on our local waters. Extend salmon fishing opportunities year round. Expand all species opportunities. Maintain a productive, effective working relation with DFO. Provide catch/effort data via our logbook. Using newsletters, mall displays and media; inform the public about responsibilities and opportunities for recreational angling. Represent the resource to everyone's best interest. 
  """,
  """
  The Sunshine Coast Salmonid Enhancement Society aims to assist, encourage, and promote salmonid enhancement in the streams of the Sunshine Coast and to provide educational services to the public regarding salmonids.
  We are fortunate to have the assistance of Howe Sound Pulp Mill as their cooler water is instrumental in providing summer tank facilities for our Coho salmon.
  The hatchery could not operate on the scale that it does without the tremendous efforts of the community, the Chapman Creek Hatchery volunteers, and our two staff. We are always looking for new volunteers, and more information can be found here. Most of all, we could not sustain our production numbers without the on-going donations of time, gifts-in-kind, and cash from many sources such as our members, the general public, SCRD and the District of Sechelt, SC Credit Union, and many local businesses.
  """,
  """
  Alaska Salmon Season Officially Begins Next Week.
  Alaska’s first fresh salmon of the season always creates extreme interest for restaurat
  Alaska’s 2023 salmon season officially gets underway on Monday, May 15 with the arrival of sockeyes and Chinook to the Copper River near Cordova.
  State fishery managers have announced there will be a 12-hour opener starting at 7am for the drift gillnet fleet.
  The 2023 Copper River sockeye salmon total run forecast is estimated at 1,646,000 fish, which is 14% below the 10-year average. That would produce a commercial harvest of 987,000 sockeye salmon for the season.
  """,
  """
  Lobster Fishery Halts as Buyers Stop Purchasing from Harvesters, Says FFAW.
  The lobster fishery has come to a screeching halt after buyers stopped purchasing product from harvesters.
  It’s the second closure in the fishery in a week according to FFAW Secretary-Treasurer, Jason Spingle.
  Spingle says the union’s phones have been ringing off the hook from irate fish harvesters unable to sell their catch, which is a live product. He says the provincial government needs to step in to resolve the matter.
  The lobster fishery has grown tremendously in recent years and the current price is set at $10.42 a pound. The union wants immediate action to amend legislation allowing harvesters to sell their product outside the province.
  He says they want to allow buyers with valid licences from Nova Scotia, New Brunswick or Maine to come in and buy lobster from harvesters at the prices set in this province.
  """,
  """
  What You’ll Need For The Best Baked Salmon.
  When it comes to salmon, don’t overthink it. All you really need is a simple lemon butter sauce and a sprinkle of herbs to make it super delicious.
  Salmon: You can make this with individual filets or one large piece (aka. half of a salmon). It works great either way!
  Butter: Keeps the salmon moist and adds that delicious buttery taste.
  Lemon Juice: A squeeze of lemon adds a bright and zesty touch.
  Garlic: I’m using 3 garlic cloves, but feel free to add more or less!
  Herbs: A parsley and dill combo complements the salmon perfectly.
  """,
  ]
```

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

Now after we calculated, we aggregate them together into a list of sentences and similarity score. Note our difference here compare to the first example is that we are only appending the ones compared with the first sentence only (showed as `cos_sim[0][i]`, instead of `cos_sim[i][i]`). Finally, we sort them with the `sorted` function, based on the first value in the list - in this case is the similarity score - in descending order.  
```python
#Add all pairs to a list with their cosine similarity score
all_sentence_combinations = []
for i in range(1, len(cos_sim)):
    all_sentence_combinations.append([cos_sim[0][i], i])

#Sort list by the highest cosine similarity score
all_sentence_combinations = sorted(all_sentence_combinations, key=lambda x: x[0], reverse=True)

print("Most similar:")
for score, i in all_sentence_combinations:
    print("{:.4f} \t {}".format(cos_sim[0][i], sentences[i]))
```

The final product will look like below,  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Sentence%20ranked.png">

[Next example: We would like to classify our sentences, and then compare the similarity to certain topics we want. How do we do it? - using a zero-shot pipeline, with FB bart model! ](https://github.com/agiga-quanta/Translation-is-fun/blob/main/Python%20examples/Zero%20shot%20classification/README.md)