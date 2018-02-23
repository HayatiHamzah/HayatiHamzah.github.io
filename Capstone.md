---
layout: post
title:  "Handbag Brand detection"
date:   2018-03-02
excerpt: "via Image Classification using Deep Neural Networks"
image: "/images/bag1.jpg"
permalink: /Capstone/
---

## Summary
In order to identify the brand of handbags, a deep neural network was built to recognize images from scratch with a precision of 97% and recall of 96% while explaining the techniques used throughout the process.

## Problem Statement
Machine learning are one of essential data science tools for businesses to understand their content and how their users/consumer interact with it. These efforts have mainly focused on Natural Language Processing (NLP) where we have created tools that can automatically detect topics, entities (e.g., people, organizations, products), and keywords in an article published by any of their products. This information serves as useful building blocks for other tools to improve both the user and editorial experience.

Apart from textual content, most online retail stores and e-commerce platforms include  vivid videos and images. These graphical media provide exciting frontiers where we can utilize and Machine Learning to enhance our experiences.

After considering different components of a data product by online retail store, this projects will focus on creating a prototype handbag brand classifier. We decided to focus on handbags because they are objects from the fashion domain, where Condé Nast already has a significant presence. Furthermore, from a computer vision perspective, handbags are rather complex objects. Many brands have features that distinguish them visually. These features range from the more obvious (e.g., patterns, logos) to the less visible (e.g., textures, pockets, latches, straps). Indeed, a “human expert” can make a reasonably good prediction of the handbag's brand without having seen that exact model.


## Introduction
<blockquote>  Machine learning  </blockquote>

## Dataset
The dataset used in this classifier was collected from Google Images using personalised google search. With Selenium, BeautifulSoup and urllib, the images are collected locally and reviewed manually before being included into the finalised dataset. The data contains selfies, 'outfit of the day' and amateur images. Each image in the dataset can only iclude one image as the prototype does not include object localisation. In total, we collected an evenly-distributed dataset of 7,500 images across 5 brands:

<h3>Unordered</h3>
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
### Manual clean-up of dataset
### Image Augmentation

## Stage 2: Splitting dataset
## Stage 3: Building a Convolutional Neural Network
## Results 

## Conclusion

## Future project

### Expand dataset
### Web app

