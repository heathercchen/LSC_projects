# How people’s attitude towards COVID-19 has changed?
### Heather Chen

#### 1. Research questions
My research project is basically a content analysis program that focuses on the changes in people’s attitudes during the period of covid-19. To be specific, using Global Database of Events, Language and Tone (GDELT), my research devotes to answering the following three questions: <br />
**1)	How the volume of news about covid-19 has changed? Are there any breakpoints?**<br />
**2)	Which countries report covid-19 news most often? Can we see a pattern here?**<br />
**3)	Do countries differ in their focus about covid-19?**<br />
In the sections below, I will first introduce the dataset that I utilize in this project. In the third section, I will discuss the large-scale computing strategies that I employ and why they are necessary for the implementation of the project. In the fourth section, I will present general results that answer the research questions. In the last section, I will discuss shortly the limitations of my project and how it can be improved if possible. <br />

#### 2. Data and project description
The corpus that I am using in this project is a sub-database called “covid19.onlinenewsgeo” from GDELT. It extracts and updates news about covid-19 across the world every 15 minutes. The database contains the title, url, origin country, contextual content, and other important information about one individual news. I will only focus on several aspects in the following analysis. <br /> <br/>
Also, given the huge volume of covid-19 news, I only select news from January 7th to March 31st. On January 7th, the Chinese government officially reported (or acknowledged) the disease for the first time, so it is a reasonable start point. The length of three months gives us enough time to track changes in attitudes. Therefore, data from January 7th to March 31st is the optimal choice given the limited computing power (which I will discuss in detail later in the fifth section). <br />

#### 3. Parallelism or large-scale computing strategy
I mainly utilize two large-scale computing strategies in this project: Amazon S3 for data storage and PySpark on EMR cluster for data analysis. <br />
**1)	Amazon S3 bucket** <br />
I use Amazon S3 bucket for data storage because the daily news data (in csv format) is too large to save and process on local machine. The 85-days-corpus takes over 50 GBs of storage. Uploading them onto S3 bucket also makes them available for future processing on EMR cluster. <br />
**2)	PySpark on EMR cluster**<br />
To do exploratory analysis and topic modeling on the corpus, I employ PySpark on EMR cluster. Exploratory data analysis involves selecting and filtering data of given criterion. I create an EMR cluster of 2 instances since this part does not require much computing power. However, topic modeling requires more complex machine learning methods, including Tokenizer, StopwordRemover, TF-IDF, CountVectorizer, as well as Latent Dirichlet Allocation. Therefore, an 8-instances cluster is created especially for this part.  <br />

#### 4. Results
Here in this section, I will present the results of my project in the order of research questions. final_project.ipynb contains most of the information in exploratory analysis. final_project2.ipynb does all of topic modeling. <br />

**1)	How the volume of news about covid-19 has changed?** <br />
![Figure 1.1](https://github.com/heathercchen/LSC_projects/graphs/1.1.jpg) <br />
![Figure 1.2](https://github.com/heathercchen/LSC_projects/graphs/1.2.jpg) <br />
To answer this question, I make a graph using data size against time and another graph using the amount of news against time. These two graphs show very similar trends. The news of covid-19 surged at around January 17th for the first time. The second outbreak happens at around March 5th. From then on, there are more than 600,000 of news every day about covid-19. <br />

**2)	Which countries report covid-19 news most often? Can we see a pattern here?**<br />
![Figure 2.1](https://github.com/lsc4ss-a20/final-project-heathercchen/blob/master/graphs/2.1.jpg)  <br />
From the graph above we can see that, different countries have different trend when reporting the news about covid-19. Here I only select 5 countries, China, America, the United Kingdom, Brazil, and India. The peak of news from China appears around the end of January, which is different from any other countries here. The U.S. reports few news about the pandemic at the beginning, just like other western countries. However, people’s attention tremendously increases at around the beginning of March. (From my experience, it is when the cases in the U.S. begin to spread across the states.) <br />

**3)	Do countries differ in their focus about covid-19?** <br />
In topic modeling part, I only select China and the U.S. as two countries of example. In each of the three months, I extract 5 topics and top 10 words from these topics. Below are the results from China in January, Febuary, and March. <br />
![Figure 3.1.1](https://github.com/heathercchen/LSC_projects/graphs/3.1.1.jpg)  <br />
![Figure 3.1.2](https://github.com/heathercchen/LSC_projects/graphs/3.1.2.jpg)  <br />
![Figure 3.1.3](https://github.com/heathercchen/LSC_projectsgraphs/3.1.3.jpg)  <br />
Results in January reveal no clear patterns. While in Febuary, the second topic contains words like “li”, “wenliang”, and “media”, which clearly illustrates the story of China’s whistle-blower Wenliang Li. His warnings against the virus were suppressed by officials. His death brought nationwide sorrow, respectfulness, and anger. Topics in March contain more information on people’s daily lives, such as lockdown, quarantine, and restrictions. The names of political leaders also appear in topic 3.  <br />
![Figure 3.2.1](https://github.com/heathercchen/LSC_projects/graphs/3.2.1.jpg)  <br />
![Figure 3.2.2](https://github.com/heathercchen/LSC_projectsgraphs/3.2.2.jpg)  <br />
![Figure 3.2.3](https://github.com/heathercchen/LSC_projectsgraphs/3.2.3.jpg)  <br />
At the same time in the U.S., the media’s focus is somewhat different. As we can see in January, there are many names of locations such as “alaska”, “chicago”, “san francisco”, and “los angeles”. States in the U.S. begin to report their first cases. In Febuary, we can see similar names of places, while there are more political terms such as “trump”, “white house”, “president”, and “cdc”. In March, besides reporting new cases in several states, we can see in topic 1 words like “economic” and “package”. They are not found in any of the topics in China. <br />

Summarized from the above, from this simple topic modeling, we can clearly get a sense of what these two countries are looking at during the three months. There are some similarities between their focuses, while we cannot conclude for sure about their preferences on certain well-structured topics (E.g., Americans focus more on economic topics regarding covid).<br />

#### 5. Discussions (Further advancements for better results) 
Having all the results, to some degree, what we get in content analysis captures what happened in real lives. <br /> <br />
 However, my project has a lot of space for future improvements. Due to the speed of using VPN to access AWS (I am in China right now), I am not able to run on larger datasets and employ more complicated machine learning techniques. If time and computing power permits, I would include more enhanced techniques on the corpus, including lemmatizing and normalizing the tokens. <br />
