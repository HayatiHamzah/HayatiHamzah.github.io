---
layout: post
title:  "Handbag Brand detection"
date:   2018-03-02
excerpt: "via Image Classification using Deep Neural Networks"
image: "/images/bag1.jpg"
permalink: /Capstone/
sitemap:
    priority: 0.7
    lastmod: 2017-11-02
    changefreq: weekly
---

## Summary
In order to identify the brand of handbags, a deep neural network was built to recognize images from scratch with a precision of 97% and recall of 96% while explaining the techniques used throughout the process.

## Problem Statement
Machine learning are one of essential data science tools for businesses to understand their content and how their users/consumer interact with it. These efforts have mainly focused on Natural Language Processing (NLP) where we have created tools that can automatically detect topics, entities (e.g., people, organizations, products), and keywords in an article published by any of their products. This information serves as useful building blocks for other tools to improve both the user and editorial experience.

Apart from textual content, most online retail stores and e-commerce platforms include  vivid videos and images. These graphical media provide exciting frontiers where we can utilize and Machine Learning to enhance reach to the right users. Building data products which are able to recognize uique and vivid images, we are able to further understand the sentiments, insights and preferences of users on e-commerce platforms or within a location. Companies will be able to anticipate incoming demand of potentially profitable items.

## Why handbags?
The undeniable competition between different e-commerce giants producing in-house data products such as Asos mobile application and Zalando shows that there is immense commercial value in data product. After considering different components of a data product by online retail stores such as recommender system and image recognition tech, this projects will focus on creating a prototype handbag brand classifier. Luxury handbags will be the focus of this project as they are highly sensationalised objects from the fashion domain. Moreover, from a computer vision perspective, handbags are rather complicated objects within an image. Many luxury brands have design distinct features which differentiates them visually. These features range from the more obvious (e.g., patterns, logos) to the less visible (e.g., textures, pockets, latches, straps). Of course, any fashion IT girl can give an accurate prediction of the handbag's brand without physically inspecting the handbag. How are we suppose to identify which brand is the hottest each season without a classifier on social media?

## Introduction
Before we go into complex concepts such as machine learning and neural network, we must understand how the computer views images. Computers extract images based on the pixels they present. In coloured images as RGB values ( list of numbers which represent the combination of red, green and blue from a nubmer range of 0 to 255).  Computers could then extract the RGB value of each pixel and put the result in an array for interpretation. When the computer interprets a new image, it will convert the image to an array by using the same technique, which then compares the patterns of numbers against the already-known objects. The computer then allots confidence scores for each class. The class with the highest confidence score is usually the predicted one.

<blockquote>  Machine learning:   </blockquote>
<blockquote>  Neural Networks:   </blockquote>
<blockquote>  Convolutional Neural Networks:   </blockquote>


## Dataset
The dataset used in this classifier was collected from Google Images using personalised google search. With Selenium, BeautifulSoup and urllib, the images are collected locally and reviewed manually before being included into the finalised dataset. The data contains selfies, 'outfit of the day' and amateur images. Each image in the dataset can only iclude one image as the prototype does not include object localisation. In total, we collected an evenly-distributed dataset of 7,500 images across 5 brands:

<h3>Brands</h3>
<ul>
  <li>Celine</li>
  <li>Chanel</li>
  <li>Givenchy</li>
  <li>Gucci</li>
  <li>Hermes</li>
</ul>

## Stage 0:Scraping Images
<h2>Preformatted</h2>
<pre><code>
'''The handbag brands are stored in a csv files'''
bags=pd.read_csv("./bag.csv")
handbagnames=bags.values.T.tolist()[0]
typeofbags=bags.values.T.tolist()[1]
url=[]

for handbag in handbagnames:
    url += ["https://www.google.com/search?q=handbag%20purse%20"+handbag+"&source=lnms&tbm=isch"]
    
driver = webdriver.Chrome(executable_path="/Users/hayatibintehamzah/Documents/GitHub/DSIstuff/Capstone/chromedriver")

driver.get(url[4])
WebDriverWait(driver, timeout=100)
time.sleep(20)
page_source = driver.page_source
page_source = page_source[page_source.find("Search Results"):]
soup = BeautifulSoup(page_source, "html.parser")
images = [a.get('src') for a in soup.find_all("img")]
skipped_imgs=0
item = 0
for img in images:
'''    if not os.path.exists("%s" %(img)):'''    
print("adding %s to folder" %(img))
    try:
        with urllib.request.urlopen(img) as response, open("handbag_crossbody_%s_img_%s.jpg" %(handbagnames[4], item), 'wb') as out_file:
            print(img)
            shutil.copyfileobj(response, out_file)
    except:
        pass
    item +=1

</code></pre>

## Stage 1:Pre-processing

### Setup
#### Manual clean-up of dataset
#### Image Augmentation
#### Labelling of brands

## Stage 2: Splitting dataset
## Stage 3: Building a Convolutional Neural Network
With the completion of the pre-processing and spliting of our dataset, we can start building our neural network. The most successful neural network for this project has 3 stacks of doubled-layered convolutional layers with 2x2 maxpooling layer at the end of each stack. Before, we further explore different types of neural networks, we must understand some basic neural network techniques used in this project. 

<blockquote>  Activation:  </blockquote>
<blockquote>  Max-pooling:  </blockquote>
<blockquote>  Dropout:  </blockquote>
<blockquote>  Soft-max:  </blockquote>


## Results 
Once we attain our trained neural network, we can test it out on a brand-new dataset! The model manage to interpret 50 handbag images and got an accuracy of 96% (48/50). 

## Conclusion
There are 3 main things about this project. Quality of data set. Image augmentation and 

## Future project

### Expand dataset

### Web app
