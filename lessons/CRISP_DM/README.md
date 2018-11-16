# The CRISP-DM (Cross-Industry Process for Data Mining)

### All Data Science Problems Involve

1. Curiosity.
2. The **right** data.
3. A tool of some kind (Python, Tableau, Excel, R, etc.) used to find a solution (You could use your head, but that would be inefficient with the massive amounts of data being generated in the world today).
4. Well communicated or deployed solution.

---

CRISP-DM steps:

`1.` Business Understanding

`2.` Data Understanding

`3.` Prepare Data

`4.` Data Modeling

`5.` Evaluate the Results

`6.` Deploy

---

`1.` **Business Understanding** - this means understanding the problem and questions you are interested in tackling in the context of whatever domain you're working in. Examples include

- How do we acquire new customers?
- Does a new treatment perform better than an existing treatment?
- How can improve communication?
- How can we improve travel?
- How can we better retain information?

`2.` **Data Understanding** - at this step, you need to move the questions from **Business Understanding** to data. You might already have data that could be used to answer the questions, or you might have to collect data to get at your questions of interest.

`3.` **Prepare Data**

We need to wrangle the data in a way for us to answer our questions. The wrangling and cleaning process is said to take 80% of the time of the data analysis process. 

`4.` **Data Modeling**

We do not always need to do predictive modeling. We can use **descriptive** and a **inferential** statistics to retrieve some useful results, too.

A lot of the hype in data science, artificial intelligence, and deep learning is integrated into step `4`, but there are still plenty of questions to be answered not using machine learning, artificial intelligence, and deep learning.

Extra Useful Tools to Know But That Are NOT Necessary for ALL Projects:

- Deep Learning
- Fancy machine learning algorithms

 There are two main 'pain' points for passing data to machine learning models in `sklearn`:

1. Missing Values
2. Categorical Values

`4.1.`Three strategies for working with missing values include:

1. We can remove (or “drop”) the rows or columns holding the missing values.
2. We can impute the missing values.
3. We can build models that work around them, and only use the information provided.

`4.1.1.` The missing values hold information. A quick removal of the rows or columns associated with these missing values would remove missing data that could be used to better inform models.

Instead of removing these values, we might keep track of the missing values using indicator values, or counts associated with how many questions an individual skipped.

A few instances in which dropping a row might be okay are:

1. Dropping missing data associated with mechanical failures.
2. The missing data is in a column that you are interested in predicting.
3. Dropping columns with no variability in the data.
4. Dropping data associated with information that you know is not correct.
5. If a large proportion of a column is missing data, this is a reason to consider dropping it.

`4.1.2.`Imputation is likely the most common method for working with missing values for any data science team. The methods shown here included the frequently used methods of imputing the mean, median, or mode of a column into the missing values for the column.

Regardless your imputation approach, you should be very cautious of the **BIAS** you are imputing into any model that uses these imputed values. Though imputing values is very common, and often leads to better predictive power in machine learning models, it can lead to over generalizations. Machines can only 'learn' from the data they are provided. If you provide biased data (due to imputation, poor data collection, etc.), it should be no surprise, you will achieve results that are biased.

`5.` **Results**

Results are the findings from our wrangling and modeling.

`6.` **Deploy**

Deploying can occur by moving your approach into production or by using your results to persuade others within a company to act on the results. Communication is such an important part of the role of a data scientist.

Two techniques for deploying your results include:

1. Automated techniques built into computer systems or across the web. 
2. Communicate results with text, images, slides, dashboards, or other presentation methods to company stakeholders.

`6.2.1.`README

Some of the qualities of good **README** files:

1. Installations

2. Project Motivation

3. File Descriptions

4. How to Interact with your project

5. Licensing, Authors, Acknowledgements, etc.

`6.2.2` Create a blog post: Post on _Medium_

You should think of captivating your audience with:

1.  A great image
2. A corresponding, catchy title.
3. Start with an engaging question or current event that your audience is likely thinking about.
4. Write in small blocks of text.
5. Break up white space.
6. Try to relate to a problem the reader is facing.
7. Provide recap or call to action at the end of the article.

Related to Keeping Yourself and Your Reader Motivated:

1. Keep your post short and sweet. 1-2 pages per post will increase audience engagement, as well as the likelihood you complete the writing of your post.
2. Create an outline. A useful outline for your post could just be: introduction, each of your questions, and then a conclusion. This will help you stay focused when writing your post.
3. Review. Review. Review. When you think you have reviewed enough, find someone else to review. The more review you have of your work, the better it will become.

