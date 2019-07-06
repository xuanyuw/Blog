---
layout: post
title: "Accent Classifier pt1: Data Cleaning"
author: "Xuanyu Wu"
categories: data
tags: [data, audio]
image: you-want-data-heres-your-data.jpg
---

<sub> Image source: https://memegenerator.net/img/instances/75147366/you-want-data-heres-your-data.jpg</sub>

In this series, I will attempt to build my own accent classifier from the scratch for the very first time. Ideally, this classifier should tell if the native language of the speaker is mandarin or english. Is it doable? Absolutely. Will I success? I don't know, but we'll see how it goes.

Since accent is mainly expressed via certain pronunciations, the most intuitive method I can think of is comparing the audio of mandarin speakers and native english speakers saying the same thing. So I found this data base called "The speech accent archive" from GMU (http://accent.gmu.edu/index.php). The transcript of all the audios is (at least they are supposed to be):


>Please call Stella.  Ask her to bring these things with her from the store:  Six spoons of fresh snow peas, five thick slabs of blue cheese, and maybe a snack for her brother Bob.  We also need a small plastic snake and a big toy frog for the kids.  She can scoop these things into three red bags, and we will go meet her Wednesday at the train station.

I found 132 mandarin speakers' recordings and 627 native english speaker's recordings. I know this dataset is extremely small, but just for practice's sake, we are gonna go with it.

### Get speaker bios

First thing first, we need to get speakers' info. So I scraped the data directly from their website and put them into a DataFrame.

~~~~python
import requests
import pandas as pd
from bs4 import BeautifulSoup
import urllib
import os

GENERAL_URL = 'http://accent.gmu.edu/browse_language.php?function=find&language='

def getBioDf(language):
    """
    get all the speaker bio from "the speech accent archive" and put them into a pandas DataFrame
    """
    # get html file
    url = GENERAL_URL + language.lower()
    raw = requests.get(url)
    bs = BeautifulSoup(raw.text, 'html.parser')
    # extract bio info from each link
    hrefs = []
    for link in bs.findAll('a'):
        hrefs.append(link.get('href'))
    # hrefs[0] is the first half of each url, and hrefs[1:8] are not speaker pages
    hrefs = [hrefs[0] + i for i in hrefs[9:]] 
    df = pd.DataFrame(columns = ['speaker id','birth place', 'native language','other language(s) ',
                                 'age, sex','age of english onset', 'english learning method',
                                 'english residence', 'length of english residence'])
    for i in range(len(hrefs)):
        sLink = hrefs[i]
        speakerRaw = requests.get(sLink)
        speakerBs = BeautifulSoup(speakerRaw.text, 'html.parser')
        bio = BeautifulSoup(str(speakerBs.findAll("ul", {"class": "bio"})))
        elems = bio.findAll('em')
        speakerID = BeautifulSoup(str(speakerBs.findAll("div", {"id": "translation"}))).find('h5').next_element.text
        row = [speakerID] + [i.next_element.next_element for i in elems]
        row[2] = language.lower()
        df.loc[i] = row
    return df
~~~~

### Clean, clean, clean

To control our sample, we only consider the native english speakers in usa, and the L2 speaker whose english residence is blank (potentially little native english exposure) or the ones that include usa.

~~~~python
englishBio = englishBio.loc[englishBio['english residence'] == ' usa']
englishBio = englishBio.drop('Unnamed: 0', axis = 1).reset_index(drop = True)
englishBio.head()
~~~~

Then we get:
![englishBio](https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/englishbio.png)

Now do the same thing for mandarin speakers, except for this time, we include the ones who had never lived abroad.

~~~~python
mandarinBio = mandarinBio[mandarinBio['english residence'].str.contains('usa') 
            | mandarinBio['length of english residence'].str.contains('0 years')]
mandarinBio = mandarinBio.drop('Unnamed: 0', axis = 1).reset_index(drop = True)
mandarinBio.head()
~~~~

<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/mandarinbio.png'>

Now the data is still quite messy to work with, becuase age and sex were put togeter, and there are useless 'years' in length of english residence. So let's get rid of those.

~~~~python
def clearYears(df):
    """
    remove 'years' in every entry and convert the string into digits
    """
    df['length of english residence'] = df['length of english residence'].str.replace(' years', '', regex = True)
    df['length of english residence'] = pd.to_numeric(df['length of english residence'])

clearYears(mandarinBio)
clearYears(englishBio)
~~~~

The length of english residence may not tell the whole story, we need another variable to indicate the amount of english exposure of a speaker. The amount of years they've learned english might be a good idea. 

~~~~python
def addNumYears(df):
    """
    split the 'age,sex' column in the given dataframe, and calculate the english learning period
    """
    ages = []
    sexes = []
    for i in df['age, sex']:
        split = i.split(', ')
        ages.append(float(split[0]))
        sexes.append(split[1])
    df['age'] = ages
    df['sex'] = sexes
    df.drop(columns = 'age, sex', inplace = True)
    df['length of english learning'] = df['age'] - df['age of english onset']

addNumYears(mandarinBio)
addNumYears(englishBio)
~~~~

Now the DataFrames look like this:
Mandarin:
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/mandarinafter.png'>
English:
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/englishafter.png'>

And for english speakers, the ideal is 'native' native speaker (monolingual), so we pick them out.

~~~~python
englishBio = englishBio[englishBio['other language(s) '] == 'none']

mandarinBio = mandarinBio.reset_index(drop = True)
englishBio = englishBio.reset_index(drop = True)
~~~~

Now let's look at the distribution of some possible significant variables that would affect accent development.

~~~~python
import seaborn as sns
import matplotlib.pyplot as plt

def overlayDistr(variable):
    fig, ax = plt.subplots()
    sns.distplot(mandarinBio[variable], ax = ax, label = "L2 Speaker")
    sns.distplot(englishBio[variable], ax = ax, label = "Native Speaker")
    leg = ax.legend()
~~~~

English learning time:

~~~~python
overlayDistr('length of english learning')
~~~~
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/englishlearning.png'>

English-speaking country resident time:
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/resident.png'>

English onset age:
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/onset.png'>

I can't help noticing that in the first two distribution, the plots of native speaker have very long tails, so let's see the age distribution
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/age.png'>

So it looks like we do have more elderly in native speaker group. I heard that voice could change as people age. Though I'm not sure it will affect our result or not, I'll keep the age range roughly the same just to be safe. (We have way too many english samples anyways)

~~~~python
englishBio = englishBio[englishBio['age'] < max(mandarinBio['age'])]
~~~~

Let's see the distributions now.
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/learningafter.png'>
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/residenceafter.png'>
<img src = 'https://raw.githubusercontent.com/xuanyuw/Blog/gh-pages/_posts/20190706_data_cleanning_img/ageafter.png'>

Looks a teeny-tiny bit better. But now we only have 116 mandarin speaker samples and 133 native speaker samples. (I cry.)

### Get Audio

Finally, we can get some audio!

~~~~python
def getAudio(path, language, speakerDf):
    """
    Download the audio in wav format into local directory. It's gonna take a while. be patient!
    """
    folderName = language + '_audio'
    if not os.path.exists(folderName):
        os.makedirs(path + folderName)
    sound_url = 'http://chnm.gmu.edu/accent/soundtracks/{}.mp3'
    for speaker in speakerDf['speaker id']:
        (filename, hearder) = urllib.request.urlretrieve(sound_url.format(speaker))
        sound = AudioSegment.from_mp3(filename)
        sound.export(path + folderName + '/{}.wav'.format(speaker), format="wav")
~~~~

Now we have them! Obviously I cannot just use around 200 audio files. So I plan to split them into words and train the neural network on single utterances. The next step would be split the audio into utterance. And that's another long story.
