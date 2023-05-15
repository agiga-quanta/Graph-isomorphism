# Special: Using ChatGPT to get sentence similarity and sentence classification
Everyone knows about ChatGPT and since it is popular, we will try to get `ChatGPT` to replicate what we did in example 1-3. We will want it to perform a sentence similarity comparison or calculations if it can. After that, we will ask `ChatGPT` to perform sentence classifications and also rank them from most similar in classifications to the least. 

## Step 1: Access ChatGPT
This is the link to chat with `ChatGPT`. This is the `ChatGPT` version 3.5, with further updates to be able to help users communicate easier. 
`https://chat.openai.com`  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Log%20in%20sign%20up.png">

At this point, you can log in or sign up with you Microsoft account or a Google acocunt. Once you are in you will be greeted with this page below. You can directly begin your conversations here.   
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/ChatGPT%20welcome%20page.png">

Or you can review and continue an old conversations by selecting the conversations on the left side
(ChatGPT with red circles)

## Step 2: Getting ChatGPT to compare sentences to fine sentence similarity.
The image below shows a response from `ChatGPT` about getting a sentence similarity score. 
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/ChatGPT%20unable%20to%20rank%20them%20in%20scores.png">

As you have seen, `ChatGPT` will not score the similarity between the sentences. However, we can get ChatGPT to maybe rank our sentences from the most similar to the least. This might be possible if we overcome the limit of the model input.We cannot pass all 8 sentences into the chat and make it rank for us at once, so we have switched to call them sentence #1 to #8, with #1 being the one to compare to the rest:  

> ### Sentence #1
> Our strategy aims to stem the steep decline of many Pacific salmon populations and to protect and rebuild stocks where possible by implementing a series of immediate and long-term solutions that focus on 4 key areas: conservation and stewardship, salmon enhancement, harvest transformation, integration and collaboration.
> ### Sentence #2
> The Adams River Salmon Society is run by a volunteer board of directors who are elected by the membership at the Annual General Meeting. Our Mission: To promote the conservation of wild salmon and the cultural resources of Tsútswecw Provincial Park through education and interpretation. Our Vision: An interpretive facility has been developed to help visitors to appreciate and understand the ecology of the salmon and the environment. The Interpretive Centre plays an important role in the regional economy by attracting people to the area and providing recreational opportunities that enhance the quality of life for residents and visitors. Additionally, the Society has been working with our partners on the restoration of the former overflow parking lot, which began with the planting of over 1,000 tree seedlings in the spring of 2010. Each year the Society takes on other projects to help with fulfilling our mandate to encourage and facilitate the education and conservation of the natural and cultural resources of Tsútswecw Provincial Park.
> ### Sentence #3
> Watershed Watch Salmon Society is a science-based charity working to defend and rebuild B.C.’s wild salmon. Our History: Watershed Watch Salmon Society was founded two decades ago by a small group of passionate conservationists. Led by ecologist Craig Orr, they noticed that while a lot of work relating to fisheries and habitat restoration was being done on the ground on local waterways, no one was pulling together the pieces to confront policy decisions at the federal and provincial levels. Craig and his associates launched Watershed Watch Salmon Society to fill that gap, offering scientific expertise to policy makers, and highlighting the large scale issues affecting local waterways, from fish farms to climate change. Our Work Today: Today, though Watershed Watch is still small, with a core staff of less than 10 people, we have grown into a highly effective advocacy organization. Our work is still grounded in sound science, and as always, we tackle the complex issues facing wild salmon through collaboration with other organizations and community groups. Over the years, we’ve learned how to use digital tools to expand our reach, but at heart, we are still a bunch of fish enthusiasts. We love nothing more than getting out in the field in our rubber boots, to collect data or maybe to swing some flies. Our Community: Watershed Watch Salmon Society involves many people beyond our paid staff and dedicated board members. We have thousands of supporters and a growing crew of volunteers who are passionate about wild salmon. Whether you read our emails, sign our petitions, donate, get out in your community to talk about our work or join us in the field, you are an important part of Watershed Watch.
> ### Sentence #4
> Fraser Valley Salmon Society is a non-profit society that ensures and advocates for the rights for all anglers in non-tidal waters and works with DFO as co-managers in conservation and habitat protection. The FVSS is also heavily involved in the training and education of anglers and the public to sport fishing. The following activities take place at the Fraser Valley Salmon Society throughout the year. Ensure angler access on our local waters. Extend salmon fishing opportunities year round. Expand all species opportunities. Maintain a productive, effective working relation with DFO. Provide catch/effort data via our logbook. Using newsletters, mall displays and media; inform the public about responsibilities and opportunities for recreational angling. Represent the resource to everyone's best interest. 
> ### Sentence #5
> The Sunshine Coast Salmonid Enhancement Society aims to assist, encourage, and promote salmonid enhancement in the streams of the Sunshine Coast and to provide educational services to the public regarding salmonids. We are fortunate to have the assistance of Howe Sound Pulp Mill as their cooler water is instrumental in providing summer tank facilities for our Coho salmon. The hatchery could not operate on the scale that it does without the tremendous efforts of the community, the Chapman Creek Hatchery volunteers, and our two staff. We are always looking for new volunteers, and more information can be found here. Most of all, we could not sustain our production numbers without the on-going donations of time, gifts-in-kind, and cash from many sources such as our members, the general public, SCRD and the District of Sechelt, SC Credit Union, and many local businesses.
> ### Sentence #6
> Alaska Salmon Season Officially Begins Next Week. Alaska’s first fresh salmon of the season always creates extreme interest for restaurant Alaska’s 2023 salmon season officially gets underway on Monday, May 15 with the arrival of sockeyes and Chinook to the Copper River near Cordova. State fishery managers have announced there will be a 12-hour opener starting at 7am for the drift gillnet fleet. The 2023 Copper River sockeye salmon total run forecast is estimated at 1,646,000 fish, which is 14% below the 10-year average. That would produce a commercial harvest of 987,000 sockeye salmon for the season.
> ### Sentence #7
> Lobster Fishery Halts as Buyers Stop Purchasing from Harvesters, Says FFAW. The lobster fishery has come to a screeching halt after buyers stopped purchasing product from harvesters. It’s the second closure in the fishery in a week according to FFAW Secretary-Treasurer, Jason Spingle. Spingle says the union’s phones have been ringing off the hook from irate fish harvesters unable to sell their catch, which is a live product. He says the provincial government needs to step in to resolve the matter. The lobster fishery has grown tremendously in recent years and the current price is set at $10.42 a pound. The union wants immediate action to amend legislation allowing harvesters to sell their product outside the province. He says they want to allow buyers with valid licences from Nova Scotia, New Brunswick or Maine to come in and buy lobster from harvesters at the prices set in this province.
> ### Sentence #8
> What You’ll Need For The Best Baked Salmon. When it comes to salmon, don’t overthink it. All you really need is a simple lemon butter sauce and a sprinkle of herbs to make it super delicious. Salmon: You can make this with individual filets or one large piece (aka. half of a salmon). It works great either way! Butter: Keeps the salmon moist and adds that delicious buttery taste. Lemon Juice: A squeeze of lemon adds a bright and zesty touch. Garlic: I’m using 3 garlic cloves, but feel free to add more or less! Herbs: A parsley and dill combo complements the salmon perfectly.

However, even with this approach, ChatGPT cannot rank the sentences we gave, and then proceed to produce its own sentences and rank those, seen below.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/ChatGPT%20can%20rank.png">

## Step 3: Classifying topics with ChatGPT

In this step, `ChatGPT` shows problems again with its limited memory and being inconsistent. The below image shows `ChatGPT` problems with details:
### Step 3.1: ChatGPT cannot remember inputs
`ChatGPT` is unable to recall the content of inputs and thus recreate a different sentence to compile with request. At first, we thought it was the limit of input, and we might have exceeded it. However, Open AI did not reveal this information to public, and online searches ranges from 2000 to 4000 characters, so this would mean we have to split the sentences into different inputs. Even after splitting them into chunks, `ChatGPT` forgets it.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/ChatGPT%20forgets%20again.png">

We then defined the inputs, naming them as "Sentence" with numbers, `ChatGPT` still forgets about it after one query.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Chat%20GPT%20forgets.png">

This means we would have to ask it to classify each sentence individually. However, we would encounter the next problem

### Step 3.2: ChatGPT cannot do one thing consistently
From the prompt below, where we asked it to classify only one sentence against the relevancy to the topics and rank them, `ChatGPT` cannot give consistent replies.  
Classifying Sentence #1  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Sentence%20%231.png">

`ChatGPT` only gave 2 types of outputs, "Relevant" and "Not Relevant".

Classifying Sentence #2  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Sentence%20%232.png">

`ChatGPT` only gave 5 outputs, from 0 to 4. This is a lot more helpful.

Classifying Sentence #3  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Sentence%20%233.png">

`ChatGPT` only gave 2 outputs, Relevant" and "Not Relevant".

It would have helped had the answers at least has the same number of rankings, but for "Sentence #1" it stuck to "Relevant" and "Not Relevant", and then for "Sentence #2", it gave us numbers from 0 to 4. However, if you ask it again, it will give us 1/5 to 4/5, such as the picture below.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Sentence%20%232%20again.png">

Additionally, if we ask `ChatGPT` to repeat what it did to "Sentence #2" to "Sentence #3", we get this.  
<img width="640" alt="image" src="https://github.com/agiga-quanta/Translation-is-fun/blob/main/images/Sentence%202%20and%203.png">

It is because of these inconsistent, we cannot reliably use `ChatGPT` to perform our tasks. 

[Back to mainpage.](https://github.com/agiga-quanta/Translation-is-fun/blob/main/README.md)