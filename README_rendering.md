
# Completing Win-Loss Analyses with Python



## Getting Started

1. Export the report you want to use from Salesforce as an Excel spreadsheet.
2. Delete any extraneous columns or duplicates.  Clean up the data as necessary.
    1. When you export a report from Salesforce to Excel, it will create many extra columns labeled as currency.            Delete these columns.
    2. When creating a report with opportunities, use Excel's "Delete Duplicates" function and only select the      opportunities column.
    3. Move the Account column (or whichever column is appropriate for your use case) to the very left.
    4. Identify which column is the key variable.  Generally, this would be the sales stage column to determine whether the opportunity was a "win" or a "loss".  Utilize find and replace to change the names from "11- Closed Lost" or "10- Closed Won" to "Lost" and "Won" to make it easier to read later.
3. Click "Save As" and change the format of your file to ".csv" before you save it.  Rename your file with no spaces.


## Downloading the Right Software


1. Download the latest version of Python
    1. https://www.python.org/downloads/
2. Download Anaconda for the latest version of Python
    1. https://www.anaconda.com/distribution/
3. Go to your terminal and type in "jupyter notebook".
4. A screen saying "Home" with files should appear in your browser.
    


## Finding this Notebook

1. Go to the Win-Loss Quantitative Analysis Guide folder in the Product Management Drive
2. Download the ".ipynb" file named "Win-Loss Quantitative Analysis Guide" and save it somewhere you can easily access it.



## Opening the Notebook

1. Go to your terminal and type in "jupyter notebook".
2. Wait for a tab in your browser to appear.
3. Press upload in the top right corner and add the ".ipynb" file named "Win-Loss Quantitative Analysis Guide" that you just downloaded.
4. Press upload again in the top right corner and add your ".csv" file containing the data you would like to analyze
5. In the interface with all the files listed, click on the ".ipynb" file named "Win-Loss Quantitative Analysis Guide"



## Using the Notebook

1. Below these instructions, look at the first box (cell).  Where it says **file_name = 'ip360z2_WL.csv'**, enter your file name with the '.csv' included.  If your file name is "win_loss_TE", then change the text inside the quotes to **win_loss_TE.csv**.
2. Click inside the box (cell).
3. Hold down shift and enter at the same time to run the code.  You will see a table with your spreadsheet data appear below the first box (cell).
4. Scroll past your table and click inside the next cell with code.
5. Hold down shift and enter at the same time to run the code.  You will see many tables appear.  The code has comments beginning with a **#** to help you understand what is happening and what output you are receiving.
    1. You can click on the left side of the output box with all the tables to expand it and easily scroll through it, and you can also click on the left side again to make the box smaller again.
6. Continue to scroll down.  Click on cells with code, read through the comments, and hold down shift + enter to run the code.
7. Copy paste the tables you would like to use, making sure to highlight the top left corner along with the rest of the table to preserve the formatting.



```python
# Put the name of your Excel file in place of the red text below (do not remove the quotes)

file_name = 'ip360z2_WL.csv' # name of your excel file
win_loss = pd.read_csv(file_name)

win_loss
```


```python
# Frequency tables for each categorical feature
for column in win_loss.select_dtypes(include=['object']).columns:
    display(pd.crosstab(index=win_loss[column], columns='Percent', normalize='columns'))
    
    #'object' will include all strings
    #change 'object' to 'category' to only include categorical dtypes
    #change 'object' to 'datetime' to only select datetimes
    #change 'object' to 'number' to select all numeric types only
    
# Frequency tables for each numerical feature
for column in win_loss.select_dtypes(include=['number']).columns:
    display(pd.crosstab(index=win_loss[column], columns='Percent', normalize='columns'))
    
    
# Summary statistics of numeric columns
    # Includes count, mean, standard deviation, minimum, 25%, 50%, 75%, and maximum
display(win_loss.describe())

# Summary statistics of character/categorical columns
display(win_loss.describe(include=['object']))

# Summary statistics of all columns
display(win_loss.describe(include='all'))



# Histograms

    #Edit number of bins in historgrams by changing bins = '40' (change 40 to any value you would like)
    #Change figure size of histograms by changing the x and y dimensions after figsize =

%matplotlib inline
histogram =  win_loss.hist(bins=40, figsize=(40, 40))
```


```python
# Frequency tables with row titles (e.g: Website Marketing, Social Media Marketing) describing the factor in 
# question (e.g: Lead Source) and column titles of Win & Loss


# Histograms showing correlation between numerical data types and sales stage

for column in win_loss.select_dtypes(exclude=['object']).columns:
    print(column)
    histogram = win_loss[[column, 'Sales Stage']].hist(by='Sales Stage', bins=40)
    plt.show()
    
# Histograms showing correlation between categorical data types and sales stage    

for column in win_loss.select_dtypes(include=['object']).columns: 
    if column is not 'Sales Stage':
        display(pd.crosstab(index=win_loss[column], columns=win_loss['Sales Stage'], normalize='columns'))
```


```python
# View correlations between factors in your data in a table (and graphically depending on your dataset)

display(win_loss.corr())
pd.plotting.scatter_matrix(churn, figsize=(12, 12))
plt.show()
```
