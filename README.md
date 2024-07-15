# Web-Scraping-Mars-Project
### Web Scraping Assignment: Mars News and Weather Data
## Project Overview
This project involves scraping and analyzing data from two different websites related to Mars. The goal is to practice web scraping, data processing, and data analysis using Python libraries such as BeautifulSoup, Pandas, and Matplotlib. The project is divided into two parts:

- Scraping the latest news articles about Mars from a NASA news website.
- Scraping weather data from the Mars Temperature Data Site and performing various analyses.

### Part 1: Scrape Mars News
Objective
The objective of Part 1 is to scrape the latest news articles about Mars from a NASA news website, extracting the title and preview text of each article.

- Steps
Import Libraries: Import necessary libraries such as BeautifulSoup and Splinter.
Set Up Splinter: Use Splinter to automate the browser and visit the Mars news site.
Scrape Data: Create a BeautifulSoup object and use it to find and extract the title and preview text of each article.
Store Data: Store the extracted data in a list of dictionaries.
Output: Print the list of dictionaries to verify the scraped data.
Code
python
Copy code

### CODE BELOW:
#Import Splinter and BeautifulSoup
    from splinter import Browser
    from bs4 import BeautifulSoup
    import pandas as pd

#Set up Splinter
    browser = Browser('chrome')

#Visit the Mars news site
    url = 'https://redplanetscience.com/'
    browser.visit(url)

#Create a BeautifulSoup object
    html = browser.html
    soup = BeautifulSoup(html, 'html.parser')

#Extract all news article elements
    article_elements = soup.find_all('div', class_='content_title')

#Create an empty list to store the dictionaries
    articles_list = []

#Loop through the text elements
    for article in article_elements:
        #Extract the title and preview text from the elements
        title = article.text
        preview = article.find_next('div', class_='article_teaser_body').text
        
        #Create a dictionary for the article
        article_dict = {
            'title': title,
            'preview': preview
        }
        
        #Append the dictionary to the list
        articles_list.append(article_dict)

#Close the browser
    browser.quit()

#Output the list of dictionaries
    print(articles_list)
    Output
    The output is a list of dictionaries, each containing the title and preview text of a Mars news article.

### Part 2: Scrape and Analyze Mars Weather Data
Objective
The objective of Part 2 is to scrape weather data from the Mars Temperature Data Site and perform various analyses to answer specific questions about the Martian weather.

- Steps
Import Libraries: Import necessary libraries such as BeautifulSoup, Pandas, and Matplotlib.
Set Up Splinter: Use Splinter to automate the browser and visit the Mars weather data site.
Scrape Data: Create a BeautifulSoup object and use it to find and extract the weather data from the HTML table.
Store Data: Assemble the scraped data into a Pandas DataFrame with appropriate column names.
Analyze Data: Perform various analyses on the data, such as finding the number of months on Mars, the number of Martian days in the dataset, and the average temperature and pressure by month.
Visualize Data: Create bar charts to visualize the average temperature and pressure by month.
Export Data: Write the DataFrame to a CSV file.
Code
python
Copy code

### CODE BELOW:
#Import libraries
    from splinter import Browser
    from bs4 import BeautifulSoup
    import pandas as pd
    import matplotlib.pyplot as plt

#Set up Splinter
    browser = Browser('chrome')

#Visit the Mars Temperature Data Site
    url = 'https://static.bc-edx.com/data/web/mars_facts/temperature.html'
    browser.visit(url)

#Create a BeautifulSoup object
    html = browser.html
    soup = BeautifulSoup(html, 'html.parser')

#Find the table in the HTML
    table = soup.find('table', class_='table')

#Extract all rows of data
    rows = []
    for tr in table.find_all('tr')[1:]:  # Skip the header row
        tds = tr.find_all('td')
        row = [td.text.strip() for td in tds]
        rows.append(row)

#Create a Pandas DataFrame
    columns = ['id', 'terrestrial_date', 'sol', 'ls', 'month', 'min_temp', 'pressure']
    mars_df = pd.DataFrame(rows, columns=columns)
    
#Examine data type of each column
    print(mars_df.dtypes)

#Change data types for data analysis
    mars_df['id'] = mars_df['id'].astype(int)
    mars_df['terrestrial_date'] = pd.to_datetime(mars_df['terrestrial_date'])
    mars_df['sol'] = mars_df['sol'].astype(int)
    mars_df['ls'] = mars_df['ls'].astype(float)
    mars_df['month'] = mars_df['month'].astype(int)
    mars_df['min_temp'] = mars_df['min_temp'].astype(float)
    mars_df['pressure'] = mars_df['pressure'].astype(float)

#Confirm type changes were successful by examining data types again
    print(mars_df.dtypes)

#Analyze the data
    #1. How many months are there on Mars?
    num_months = mars_df['month'].nunique()
    print(f"There are {num_months} months on Mars.")

    #2. How many Martian days' worth of data are there?
    num_sols = mars_df['sol'].nunique()
    print(f"There are {num_sols} Martian days' worth of data in the dataset.")

    #3. What is the average low temperature by month?
    avg_min_temp_by_month = mars_df.groupby('month')['min_temp'].mean()
    print(avg_min_temp_by_month)

#Plot the average temperature by month
    avg_min_temp_by_month.plot(kind='bar', figsize=(10, 6))
    plt.xlabel('Month')
    plt.ylabel('Average Minimum Temperature (°C)')
    plt.title('Average Minimum Temperature by Martian Month')
    plt.show()

#Identify the coldest and hottest months in Curiosity's location
    coldest_month = avg_min_temp_by_month.idxmin()
    hottest_month = avg_min_temp_by_month.idxmax()
    print(f"The coldest month is {coldest_month} and the hottest month is {hottest_month}.")

    #4. Average pressure by Martian month
    avg_pressure_by_month = mars_df.groupby('month')['pressure'].mean()
    print(avg_pressure_by_month)

#Plot the average pressure by month
    avg_pressure_by_month.plot(kind='bar', figsize=(10, 6), color='orange')
    plt.xlabel('Month')
    plt.ylabel('Average Atmospheric Pressure')
    plt.title('Average Atmospheric Pressure by Martian Month')
    plt.show()

    #5. How many terrestrial (earth) days are there in a Martian year?
    mars_df.plot(x='terrestrial_date', y='min_temp', figsize=(12, 6))
    plt.xlabel('Terrestrial Date')
    plt.ylabel('Minimum Temperature (°C)')
    plt.title('Minimum Daily Temperature Over Time')
    plt.show()

#Write the data to a CSV
    mars_df.to_csv('mars_weather_data.csv', index=False)
    print("Data has been written to mars_weather_data.csv")
    Output
    The output includes various analyses and visualizations of the Mars weather data, as well as a CSV file containing the scraped data.

### Instructions
To run the project, follow these steps:

Clone the repository to your local machine.
Ensure you have the required dependencies installed (BeautifulSoup, Splinter, Pandas, Matplotlib).
Launch the Jupyter notebook files (part_1_mars_news.ipynb and part_2_mars_weather.ipynb).
Import the required dependencies.
Extract data using the provided code.
Clean and transform the data.
Perform the analyses as described.
Generate visualizations to display the results.
Verify the results by examining the printed outputs and plots.

Project Files:
part_1_mars_news.ipynb: The main analysis code for Part 1.
part_2_mars_weather.ipynb: The main analysis code for Part 2.
mars_weather_data.csv: The CSV file containing the scraped Mars weather data.

### Credits
Class Notes
Peer Discussions
XPert Learning Assistant
Study Materials
