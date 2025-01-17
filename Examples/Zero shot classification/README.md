# Example 3: Zero-shot similarity
## Purpose: 
This example shows a more detailed version of sentence similarity. However, we will also check the topic of inputs before filtering inputs with filter topics, and comparing only the relevant inputs to the main input. 
## Method:
Model used is: [bart-large-mnli](https://huggingface.co/facebook/bart-large-mnli).  

In this specific example, we know `'conservation and stewardship', 'salmon enhancement', 'harvest transformation', 'integration and collaboration'` are our main focus from the first input. We are also only interested in `'society', 'association', 'partnership', 'community'` sources only, so anything from `'food', 'news', 'sport', 'culture', 'politics', 'business'` will be ignored.  

The method here used zero-shot-classification pipeline, which in specific uses bart-large-mnli model from Facebook, to calculate the topic similarity to filter topics such as `'society', 'association', 'partnership', 'community', 'food', 'news', 'sport', 'culture', 'politics', 'business'` before comparing them to `'conservation and stewardship', 'salmon enhancement', 'harvest transformation', 'integration and collaboration'`.  
## How to try this for yourself without coding
All our model are from `https://huggingface.co`. Copy this link into your browser to try out the models.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Hugging%20face%20landing%20page.png">

Our model that we use is [miniLM-L6-H384-uncased](https://huggingface.co/flax-sentence-embeddings/all_datasets_v4_MiniLM-L6), which calculates the similarity between sentences.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Bart%20model%20page.png">

On the right side, you can find an interface API where you can try out any sentences. In the picture below, we tried "I have a problem with my iphone that needs to be resolved asap!!", and we add the "class names" - also called topic in this example - "urgent, not urgent, phone, tablet, computer". By clicking "Compute", we can get the topic relativity score to check. These scores means how relavant to the topic a sentence is.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Bart%20model%20test.png">

## File: [Zero_shot_classification.ipynb]([https://github.com/agiga-quanta/Translation-is-fun/blob/main/Examples/Zero%20shot%20classification/zero_shot_classification.ipynb](https://github.com/agiga-quanta/Translation-is-fun/blob/main/Examples/Zero%20shot%20classification/Zero_shot_classification.ipynb))
## Instructions and explanation: 

We are using Jupyter notebook here, which allows us to run pip to install `transformers[pytorch]`. This package is necessary for using the model in this example.  
```
pip install "transformers[pytorch]"
```

We will be installing pandas to set up a tabular format and tabluate to visually show us tables in the results when we print them. Here we set up our `zero-shot-classification` pipeline, as well as the model to be `facebook/bart-large-mnli`.
```python
from tabulate import tabulate
import pandas as pd
from transformers import pipeline

def print_output(model_outputs):
    df = pd.DataFrame(model_outputs)
    prettyprint=tabulate(df[['labels', 'scores']], headers='keys', tablefmt='psql')
    print(prettyprint) 

classifier = pipeline("zero-shot-classification", model="facebook/bart-large-mnli")
```


The following sentences from these sources, same as the previous examples:
1. [Our strategy aims to stem the steep decline of many Pacific salmon populations and to protect and rebuild stocks where possible by implementing a series of immediate and long-term solutions that focus on 4 key areas: conservation and stewardship, salmon enhancement, harvest transformation, integration and collaboration.](https://www.dfo-mpo.gc.ca/campaign-campagne/pss-ssp/index-eng.html)
2. [The Adams River Salmon Society is run by a volunteer board of directors who are elected by the membership at the Annual General Meeting. Our Mission: To promote the conservation of wild salmon and the cultural resources of Tsútswecw Provincial Park through education and interpretation. Our Vision: An interpretive facility has been developed to help visitors to appreciate and understand the ecology of the salmon and the environment. The Interpretive Centre plays an important role in the regional economy by attracting people to the area and providing recreational opportunities that enhance the quality of life for residents and visitors. Additionally, the Society has been working with our partners on the restoration of the former overflow parking lot, which began with the planting of over 1,000 tree seedlings in the spring of 2010. Each year the Society takes on other projects to help with fulfilling our mandate to encourage and facilitate the education and conservation of the natural and cultural resources of Tsútswecw Provincial Park](https://www.salmonsociety.com/salmon-society/about/) 
3. [Watershed Watch Salmon Society is a science-based charity working to defend and rebuild B.C.’s wild salmon. Our History: Watershed Watch Salmon Society was founded two decades ago by a small group of passionate conservationists. Led by ecologist Craig Orr, they noticed that while a lot of work relating to fisheries and habitat restoration was being done on the ground on local waterways, no one was pulling together the pieces to confront policy decisions at the federal and provincial levels. Craig and his associates launched Watershed Watch Salmon Society to fill that gap, offering scientific expertise to policy makers, and highlighting the large scale issues affecting local waterways, from fish farms to climate change. Our Work Today: Today, though Watershed Watch is still small, with a core staff of less than 10 people, we have grown into a highly effective advocacy organization. Our work is still grounded in sound science, and as always, we tackle the complex issues facing wild salmon through collaboration with other organizations and community groups. Over the years, we’ve learned how to use digital tools to expand our reach, but at heart, we are still a bunch of fish enthusiasts. We love nothing more than getting out in the field in our rubber boots, to collect data or maybe to swing some flies. Our Community: Watershed Watch Salmon Society involves many people beyond our paid staff and dedicated board members. We have thousands of supporters and a growing crew of volunteers who are passionate about wild salmon. Whether you read our emails, sign our petitions, donate, get out in your community to talk about our work or join us in the field, you are an important part of Watershed Watch.](https://watershedwatch.ca/about-us/)
4. [Fraser Valley Salmon Society is a non-profit society that ensures and advocates for the rights for all anglers in non-tidal waters and works with DFO as co-managers in conservation and habitat protection. The FVSS is also heavily involved in the training and education of anglers and the public to sport fishing. The following activities take place at the Fraser Valley Salmon Society throughout the year. Ensure angler access on our local waters. Extend salmon fishing opportunities year round. Expand all species opportunities. Maintain a productive, effective working relation with DFO. Provide catch/effort data via our logbook. Using newsletters, mall displays and media; inform the public about responsibilities and opportunities for recreational angling. Represent the resource to everyone's best interest. ](https://www.fraservalleysalmonsociety.ca/)
5. [The Sunshine Coast Salmonid Enhancement Society aims to assist, encourage, and promote salmonid enhancement in the streams of the Sunshine Coast and to provide educational services to the public regarding salmonids. We are fortunate to have the assistance of Howe Sound Pulp Mill as their cooler water is instrumental in providing summer tank facilities for our Coho salmon. The hatchery could not operate on the scale that it does without the tremendous efforts of the community, the Chapman Creek Hatchery volunteers, and our two staff. We are always looking for new volunteers, and more information can be found here. Most of all, we could not sustain our production numbers without the on-going donations of time, gifts-in-kind, and cash from many sources such as our members, the general public, SCRD and the District of Sechelt, SC Credit Union, and many local businesses.](https://scsalmon.org/about-scses)
6. [Alaska Salmon Season Officially Begins Next Week. Alaska’s first fresh salmon of the season always creates extreme interest for restaurant. Alaska’s 2023 salmon season officially gets underway on Monday, May 15 with the arrival of sockeyes and Chinook to the Copper River near Cordova. State fishery managers have announced there will be a 12-hour opener starting at 7am for the drift gillnet fleet. The 2023 Copper River sockeye salmon total run forecast is estimated at 1,646,000 fish, which is 14% below the 10-year average. That would produce a commercial harvest of 987,000 sockeye salmon for the season.](https://www.seafoodnews.com/Story/1252218/Alaska-Salmon-Season-Officially-Begins-Next-Week)
7. [Lobster Fishery Halts as Buyers Stop Purchasing from Harvesters, Says FFAW. The lobster fishery has come to a screeching halt after buyers stopped purchasing product from harvesters. It’s the second closure in the fishery in a week according to FFAW Secretary-Treasurer, Jason Spingle. Spingle says the union’s phones have been ringing off the hook from irate fish harvesters unable to sell their catch, which is a live product. He says the provincial government needs to step in to resolve the matter. The lobster fishery has grown tremendously in recent years and the current price is set at $10.42 a pound. The union wants immediate action to amend legislation allowing harvesters to sell their product outside the province. He says they want to allow buyers with valid licences from Nova Scotia, New Brunswick or Maine to come in and buy lobster from harvesters at the prices set in this province.](https://vocm.com/2023/05/04/lobster-fishery-halts-as-buyers-stop-purchasing-from-harvesters-says-ffaw/)
8. [What You’ll Need For The Best Baked Salmon. When it comes to salmon, don’t overthink it. All you really need is a simple lemon butter sauce and a sprinkle of herbs to make it super delicious. Salmon: You can make this with individual filets or one large piece (aka. half of a salmon). It works great either way! Butter: Keeps the salmon moist and adds that delicious buttery taste. Lemon Juice: A squeeze of lemon adds a bright and zesty touch. Garlic: I’m using 3 garlic cloves, but feel free to add more or less! Herbs: A parsley and dill combo complements the salmon perfectly.](https://downshiftology.com/recipes/best-baked-salmon/)  

These are also shown in the code below. The sentences are enclosed in triple \" because they also have `newline` with them. To work without the triple \" and with a single \' or \", you would have to put everything on one line. 
```python
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
Since we have dataframe and tables set up from above, we can simply use a loop such as above to run our similarity score calculations. We first print the sentence we are checking. Then we define the filter topics: `'society', 'association', 'partnership', 'community', 'food', 'news', 'sport', 'culture', 'politics', 'business'`; before we use the classifier (which is the pipeline and the model). In essence, we are running the sentence through the pipeline, while using the model to classify right here.  
```python
for sentence in sentences:
  print(sentence)
  type_labels = ['society', 'association', 'partnership', 'community', 'food', 'news', 'sport', 'culture', 'politics', 'business']
  outputs = classifier(sentence, type_labels, multi_label=True)
  print_output(outputs)
  if outputs['labels'][0] in  ['society', 'association', 'partnership', 'community']:
    work_labels = ['conservation and stewardship', 'salmon enhancement', 'harvest transformation', 'integration and collaboration']
    outputs = classifier(sentence, work_labels, multi_label=True)
    print_output(outputs)
```

Afterward, we print the outputs and they will be in a table format such as below.   
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Classify%201%20step.png">

After this table, the code then check the outputs. By setting our conditions to `outputs['labels'][0] in ['society', 'association', 'partnership', 'community']:` we are checking if any of these topics has a score closer to 1 than 0, then it will run another classifying process just like before, but this time, the topic is more specific: `'conservation and stewardship', 'salmon enhancement', 'harvest transformation', 'integration and collaboration'`.   
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Classify%202%20step.png">

[Back to mainpage.](https://github.com/agiga-quanta/Translation-is-fun/blob/main/README.md)
