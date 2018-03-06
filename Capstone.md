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

<center><img src="/images/retail.jpg" height="300" width="300"></center>

Apart from textual content, most online retail stores and e-commerce platforms include  vivid videos and images. These graphical media provide exciting frontiers where we can utilize and Machine Learning to enhance reach to the right users. Building data products which are able to recognize uique and vivid images, we are able to further understand the sentiments, insights and preferences of users on e-commerce platforms or within a location. Companies will be able to anticipate incoming demand of potentially profitable items.

<div class="4u"><span class="image fit"><img src="{{ "/images/bag.jpg" | absolute_url }}" alt="" /></span></div>

## Why handbags?
The undeniable competition between different e-commerce giants producing in-house data products such as Asos mobile application and Zalando shows that there is immense commercial value in data product. After considering different components of a data product by online retail stores such as recommender system and image recognition tech, this projects will focus on creating a prototype handbag brand classifier. Luxury handbags will be the focus of this project as they are highly sensationalised objects from the fashion domain. Moreover, from a computer vision perspective, handbags are rather complicated objects within an image. Many luxury brands have design distinct features which differentiates them visually. These features range from the more obvious (e.g., patterns, logos) to the less visible (e.g., textures, pockets, latches, straps). Of course, any fashion IT girl can give an accurate prediction of the handbag's brand without physically inspecting the handbag. How are we suppose to identify which brand is the hottest each season without a classifier on social media?

## Introduction

<center><img src="/images/capstone1.jpg" height="400" width="440"></center>

<p>Before we go into complex concepts such as machine learning and neural network, we must understand how the computer views images. Computers extract images based on the pixels they present. In coloured images as RGB values ( list of numbers which represent the combination of red, green and blue from a nubmer range of 0 to 
255).  Computers could then extract the RGB value of each pixel and put the result in an array for interpretation. </p>


In this project, we will class each brand to the images in the data set. Thus, a new image is added into the computer, it interprets a new image, it will convert the image to an array by using the same technique, which then compares the patterns of numbers against the already-known objects. The computer then allots confidence scores for each class. The class with the highest confidence score is usually the predicted one. This is done by machine learning.


<blockquote>  Machine learning: Algorithms that can make predictions through pattern recognition   </blockquote>

<strong>Why is this a machine learning project? </strong>

There are three main factors that differentiates a machine learning project and a deep learning (DL) algorithm. The first is the size of the data points. Most machine learning (ML) algorithm are in the range of 100-10000 data points while deep learning algorithm require at least a million data points. The second factor is the amount of supervision of the neural network requires. ML algorithm requires much more supervision in terms of selecting optimizers, regularisation and the quality of the data set. On the other hand, DL algorithm develop the neural networks as more information inserted into it. Lastly, DL algorithm requires more computing power than ML algorithm. 

<strong> How do machine learning algorithm process their data? </strong>

They use <u> neural networks </u> 

<center><img src="/images/NN.gif" height="300" width="450"></center>


<blockquote>  Neural Networks: A Neural Network consists of an interconnected group of nodes called neurons. The input feature variables from the data are passed to these neurons as a multi-variable linear combination, where the values multiplied by each feature variable are known as weights. A non-linearity is then applied to this linear combination which gives the neural network the ability to model complex non-linear relationships. </blockquote>

Why is Convolutional Neural Network applicable for image?
 The convolutional neural networks make a conscious tradeoff: if a network is designed for specifically handling the images, some generalizability has to be sacrificed for a much more feasible solution.

If you consider any image, proximity has a strong relation with similarity in it and convolutional neural networks specifically take advantage of this fact. This implies, in a given image, two pixels that are nearer to each other are more likely to be related than the two pixels that are apart from each other. Nevertheless, in a usual neural network, every pixel is linked to every single neuron. The added computational load makes the network less accurate in this case.

By killing a lot of these less significant connections, convolution solves this problem. In technical terms, convolutional neural networks make the image processing computationally manageable through filtering the connections by proximity. In a given layer, rather than linking every input to every neuron, convolutional neural networks restrict the connections intentionally so that any one neuron accepts the inputs only from a small subsection of the layer before it(say like 5*5 or 3*3 pixels). Hence, each neuron is responsible for processing only a certain portion of an image.(Incidentally, this is almost how the individual cortical neurons function in your brain. Each neuron responds to only a small portion of your complete visual field).

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
