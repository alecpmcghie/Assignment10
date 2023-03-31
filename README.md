# 310 D Assignment 10 
### Hypothesis 
I believe that when the model is tasked with testing the toxic labeled smaller text examples it will return a higher output than the toxic labeled text with longer text examples.
### Testing Procedure 
To test the hypothesis I will first filter out the non toxic rows from the dataset. After sorting I will use the `get_toxicity_score()` function to test for the toxicity of the statement. I will then add these values to the dataframe as well as adding a word counter to see how many words are in the string and add that as well. To test I plan to calculate the linear regression of the points on a graph and see if I’m correct based on a negative sloping line.
### Experimentation
For this experiment I plan to filter out the toxic examples and create a new dataframe using the code below.
``` python
toxic_df = df[df[“toxic”] == “yes”]
```
Initally I applied `get_toxicity_score` to all of the rows in `toxic_df` After running the code I found that the API had a maximum amount of requests.
To limit the testing I decided to create a smaller dataframe to help limit the amount of requests I made to the API.
```python 
#Changing the size of the dataframe to 30 rows
toxic_comments = toxic_comments.head(30)
```


By creating this dataframe we will have a dataframe with only the first 30 toxic rows and will help with our testing.
After making our data frame the next step will be to test for the size of the text and append this data to the dataframe to group the different lengths. To do so we will use the code below.
```python
def count_words(text):
    return len(re.findall(r"\S+", text))
#Function that when given a text returns the amount of words

#Creating a new column in the dataframe with the word count
toxic_comments["word_count"] = toxic_comments["comment_text"].apply(count_words)

```

With this code in place we can now make another column that contains the toxicity score of each of the statements

```python
toxic_comments[“score”] = toxic_comments[“comment_text”].apply(get_toxicity_score)
```
Now we have the data that we want to analyze prepared we can start testing.

### Testing
In the initial test I ran into some issues, but used the information from earlier classes and what I could find online and in my book on pandas to clean the data.
To clean the data and remove rows with above 50 words I used the code below
```python 
plot_test_df = plot_test_df.loc[plot_test_df["word_count"]<= 50]
```
After cleaning the data I ran a linear regression test and plotted the outcome on the plot of data.

The code that I used for this is below.
```python
import numpy as np
import scipy.stats as stats

x = plot_test_df["word_count"]
y = plot_test_df["score"]
#Setting x and y to the dataframe columns

slope, intercept, r, p, std_err = stats.linregress(x, y)
#Linear regression calculation
line = slope * x + intercept
#Creating a line from calculation

#Creating the plot
fig, ax = plt.subplots()
#fig used from pyplot, short for figure, ax is the subplot
ax.scatter(x, y, label="Datapoints")
#Scatter plot for the datapoints
ax.plot(x, line, color="red", label="Regression Line")
#Plotting the regression line
ax.set_xlabel("Num of Words")
ax.set_ylabel("Toxic Score")
ax.legend()
plt.show()
```
![image](https://user-images.githubusercontent.com/123593094/229182207-20096016-8e51-4a5b-b096-95c063b90be2.png)


Above is a graphical representaion of the datapoints and the linear regression line that I made. As you can see my initial hypothesis was correct that the lower word counts had higher toxic scores due to the regression line being sloped down. Although we can make some inference from this regression line being negitive it is important to remember that it was only calculated with 29 datapoints so I can't say with confidence that these conclusions are accurate. If I were to do this test again I would want to increase the amount of requests that I can make to the API because if I tested the original dataset with all 3,843 toxic statements it would be enough to make solid conclusions on the analysis. 
<br>Throughout this project I got alot of meaningful expirience into the world of API's and API integration within python. Although this was a small project I found that the issues that I ran into helped with my problem solving skills and the skills assosiated with data science. I really enjoyed doing this assignment and it was satisfying once I got the final data and could draw conclusions from it.
