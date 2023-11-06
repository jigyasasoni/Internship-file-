# Web Scraping Assignment 2 

Q1: Write a python program to scrape data for “Data Analyst” Job position in “Bangalore” location. You
have to scrape the job-title, job-location, company_name, experience_required. You have to scrape first 10
jobs data.
This task will be done in following steps:
1. First get the webpage https://www.shine.com/
2. Enter “Data Analyst” in “Job title, Skills” field and enter “Bangalore” in “enter the location” field.
3. Then click the searchbutton.
4. Then scrape the data for the first 10 jobs results you get.
5. Finally create a dataframe of the scraped data.
Note: All of the above steps have to be done in code. No step is to be done manually.


```python
!pip install selenium

```

    Requirement already satisfied: selenium in c:\anaconda3\lib\site-packages (4.15.1)
    Requirement already satisfied: urllib3[socks]<3,>=1.26 in c:\anaconda3\lib\site-packages (from selenium) (1.26.14)
    Requirement already satisfied: trio~=0.17 in c:\anaconda3\lib\site-packages (from selenium) (0.23.0)
    Requirement already satisfied: trio-websocket~=0.9 in c:\anaconda3\lib\site-packages (from selenium) (0.11.1)
    Requirement already satisfied: certifi>=2021.10.8 in c:\anaconda3\lib\site-packages (from selenium) (2022.12.7)
    Requirement already satisfied: sortedcontainers in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (2.4.0)
    Requirement already satisfied: cffi>=1.14 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.15.1)
    Requirement already satisfied: idna in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (3.4)
    Requirement already satisfied: sniffio>=1.3.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.3.0)
    Requirement already satisfied: exceptiongroup>=1.0.0rc9 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.1.3)
    Requirement already satisfied: attrs>=20.1.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (22.1.0)
    Requirement already satisfied: outcome in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.3.0.post0)
    Requirement already satisfied: wsproto>=0.14 in c:\anaconda3\lib\site-packages (from trio-websocket~=0.9->selenium) (1.2.0)
    Requirement already satisfied: PySocks!=1.5.7,<2.0,>=1.5.6 in c:\anaconda3\lib\site-packages (from urllib3[socks]<3,>=1.26->selenium) (1.7.1)
    Requirement already satisfied: pycparser in c:\anaconda3\lib\site-packages (from cffi>=1.14->trio~=0.17->selenium) (2.21)
    Requirement already satisfied: h11<1,>=0.9.0 in c:\anaconda3\lib\site-packages (from wsproto>=0.14->trio-websocket~=0.9->selenium) (0.14.0)
    


```python
import selenium
from selenium import webdriver
import pandas as pd 
from selenium.webdriver.common.by import By
import warnings
warnings.filterwarnings("ignore")
import time
```


```python
driver = webdriver.Chrome ()
```


```python
driver.get('https://www.shine.com/')
```


    ---------------------------------------------------------------------------

    NoSuchWindowException                     Traceback (most recent call last)

    Cell In[41], line 1
    ----> 1 driver.get("https://www.shine.com/")
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py:355, in WebDriver.get(self, url)
        353 def get(self, url: str) -> None:
        354     """Loads a web page in the current browser session."""
    --> 355     self.execute(Command.GET, {"url": url})
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py:346, in WebDriver.execute(self, driver_command, params)
        344 response = self.command_executor.execute(driver_command, params)
        345 if response:
    --> 346     self.error_handler.check_response(response)
        347     response["value"] = self._unwrap_value(response.get("value", None))
        348     return response
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\errorhandler.py:229, in ErrorHandler.check_response(self, response)
        227         alert_text = value["alert"].get("text")
        228     raise exception_class(message, screen, stacktrace, alert_text)  # type: ignore[call-arg]  # mypy is not smart enough here
    --> 229 raise exception_class(message, screen, stacktrace)
    

    NoSuchWindowException: Message: no such window: target window already closed
    from unknown error: web view not found
      (Session info: chrome=119.0.6045.106)
    Stacktrace:
    	GetHandleVerifier [0x00007FF63A5882B2+55298]
    	(No symbol) [0x00007FF63A4F5E02]
    	(No symbol) [0x00007FF63A3B05AB]
    	(No symbol) [0x00007FF63A390038]
    	(No symbol) [0x00007FF63A416BC7]
    	(No symbol) [0x00007FF63A42A15F]
    	(No symbol) [0x00007FF63A411E83]
    	(No symbol) [0x00007FF63A3E670A]
    	(No symbol) [0x00007FF63A3E7964]
    	GetHandleVerifier [0x00007FF63A900AAB+3694587]
    	GetHandleVerifier [0x00007FF63A95728E+4048862]
    	GetHandleVerifier [0x00007FF63A94F173+4015811]
    	GetHandleVerifier [0x00007FF63A6247D6+695590]
    	(No symbol) [0x00007FF63A500CE8]
    	(No symbol) [0x00007FF63A4FCF34]
    	(No symbol) [0x00007FF63A4FD062]
    	(No symbol) [0x00007FF63A4ED3A3]
    	BaseThreadInitThunk [0x00007FFC499B257D+29]
    	RtlUserThreadStart [0x00007FFC49FCAA58+40]
    



```python
job_title = driver.find_element_by_id('id_q')
job_title.send_keys('Data Analyst')

location = driver.find_element_by_id('id_l')
location.send_keys('Bangalore')
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Cell In[17], line 1
    ----> 1 job_title = driver.find_element_by_id('id_q')
          2 job_title.send_keys('Data Analyst')
          4 location = driver.find_element_by_id('id_l')
    

    AttributeError: 'WebDriver' object has no attribute 'find_element_by_id'



```python
search_button = driver.find_element_by_xpath('//button[@type="submit"]')
search_button.click()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Cell In[18], line 1
    ----> 1 search_button = driver.find_element_by_xpath('//button[@type="submit"]')
          2 search_button.click()
    

    AttributeError: 'WebDriver' object has no attribute 'find_element_by_xpath'



```python
job_titles = driver.find_elements_by_xpath('//a[@class="job_title_anchor"]')
job_locations = driver.find_elements_by_xpath('//li[@class="w-30 mr-10 result-display-location"]/span')
company_names = driver.find_elements_by_xpath('//a[@class="result-display-company-name"]')
experience_required = driver.find_elements_by_xpath('//li[@class="w-30 mr-10 result-display-exp"]/span')

data = []
for i in range(10):
  job = {
  'Job Title': job_titles[i].text,
  'Job Location': job_locations[i].text,
  'Company Name': company_names[i].text,
  'Experience Required': experience_required[i].text
  }
  data.append(job)
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Cell In[19], line 1
    ----> 1 job_titles = driver.find_elements_by_xpath('//a[@class="job_title_anchor"]')
          2 job_locations = driver.find_elements_by_xpath('//li[@class="w-30 mr-10 result-display-location"]/span')
          3 company_names = driver.find_elements_by_xpath('//a[@class="result-display-company-name"]')
    

    AttributeError: 'WebDriver' object has no attribute 'find_elements_by_xpath'



```python
df = pd.DataFrame(data)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Cell In[20], line 1
    ----> 1 df = pd.DataFrame(data)
    

    NameError: name 'data' is not defined



```python
driver.quit()
```
# Q2:Write a python program to scrape data for “Data Scientist” Job position in“Bangalore” location. You
have to scrape the job-title, job-location, company_name. You have to scrape first 10 jobs data.
This task will be done in following steps:
1. First get the webpage https://www.shine.com/
2. Enter “Data Scientist” in “Job title, Skills” field and enter “Bangalore” in “enter thelocation” field.
3. Then click the search button.
4. Then scrape the data for the first 10 jobs results you get.
5. Finally create a dataframe of the scraped data.
Note: All of the above steps have to be done in code. No step is to be done manually

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://www.shine.com"
response = requests.get(url)

job_title = "Data Scientist"
location = "Bangalore"

search_url = f"{url}/job-search/{job_title}-jobs-in-{location}"
response = requests.get(search_url)

soup = BeautifulSoup(response.content, "html.parser")
job_results = soup.find_all("li", class_="search_listing")

data = []
for job in job_results[:10]:
title = job.find("h2").text.strip()
company = job.find("span", class_="company_name").text.strip()
location = job.find("span", class_="location").text.strip()
experience = job.find("span", class_="exp").text.strip()

data.append({
  "Job Title": title,
  "Company Name": company,
  "Job Location": location,
  "Experience Required": experience
  })

df = pd.DataFrame(data)
print(df)
```

    Empty DataFrame
    Columns: []
    Index: []
    
# Q3: In this question you have to scrape data using the filters available on the webpage
 You have to use the location and salary filter.
You have to scrape data for “Data Scientist” designation for first 10 job results.
You have to scrape the job-title, job-location, company name, experience required.
The location filter to be used is “Delhi/NCR”. The salary filter to be used is “3-6” lakhs
The task will be done as shown in the below steps:
1. first get the web page https://www.shine.com/
2. Enter “Data Scientist” in “Skill, Designations, and Companies” field.
3. Then click the search button.
4. Then apply the location filter and salary filter by checking the respective boxes
5. Then scrape the data for the first 10 jobs results you get.
6. Finally create a dataframe of the scrapeddata.
Note: All of the above steps have to be done in code. No step is to be done manually.

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
```


```python
try:
    soup = BeautifulSoup('html', 'html.parser')
except TypeError:
    print('TypeError occurred')
```


```python
try:
    soup.title
except AttributeError:
    print('AttributeError occurred')
```


```python
url = "https://www.shine.com/"
response = requests.get(url)
```


```python
soup = BeautifulSoup(response.content, 'html.parser')
```


```python
search_input = soup.find('input', {'id': 'txt_search'})
search_input['value'] = 'Data Scientist'

```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[55], line 2
          1 search_input = soup.find('input', {'id': 'txt_search'})
    ----> 2 search_input['value'] = 'Data Scientist'
    

    TypeError: 'NoneType' object does not support item assignment



```python
search_button = soup.find('button', {'id': 'btn_search'})
response = requests.post(url, data={'txt_search': 'Data Scientist'})
```


```python
soup = BeautifulSoup(response.content, 'html.parser')
location_filter = soup.find('input', {'id': 'chk_location_1'})
location_filter['checked'] = True
salary_filter = soup.find('input', {'id': 'chk_salary_1'})
salary_filter['checked'] = True



```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[60], line 3
          1 soup = BeautifulSoup(response.content, 'html.parser')
          2 location_filter = soup.find('input', {'id': 'chk_location_1'})
    ----> 3 location_filter['checked'] = True
          4 salary_filter = soup.find('input', {'id': 'chk_salary_1'})
          5 salary_filter['checked'] = True
    

    TypeError: 'NoneType' object does not support item assignment



```python
job_listings = soup.find_all('div', {'class': 'w-100'})
data = []
for job in job_listings[:10]:
  title = job.find('h3').text.strip()
  location = job.find('span', {'class': 'location'}).text.strip()
  company = job.find('span', {'class': 'company-name'}).text.strip()
  experience = job.find('span', {'class': 'exp'}).text.strip()
  data.append([title, location, company, experience])
```


```python
df = pd.DataFrame(data, columns=['Job Title', 'Job Location', 'Company Name', 'Experience Required'])
```
Q4: Scrape data of first 100 sunglasses listings on flipkart.com. You have to scrape four attributes:
6. Brand
7. ProductDescription
8. Price
The attributes which you have to scrape is ticked marked in the below image.
To scrape the data you have to go through following steps:
1. Go to Flipkart webpage by url :https://www.flipkart.com/
2. Enter “sunglasses” in the search fieldwhere “search for products, brands and more” is written and
click the search icon
3. After that you will reach to the page having a lot of sunglasses. From this page you can scrap the
required data as usual.

```python
import requests
from bs4 import BeautifulSoup

url = "https://www.flipkart.com/"
search_query = "sunglasses"
max_listings = 100
scraped_data = []

while len(scraped_data) < max_listings:
response = requests.get(url)
soup = BeautifulSoup(response.content, "html.parser")
  
search_field = soup.find("input", attrs={"title": "Search for products, brands and more"})
search_field["value"] = search_query
  
search_icon = soup.find("button", attrs={"type": "submit"})
response = requests.post(url, data=search_icon.form)
soup = BeautifulSoup(response.content, "html.parser")
  
listings_container = soup.find("div", attrs={"class": "_1AtVbE"})
  
for listing in listings_container.find_all("div", attrs={"class": "_2kHMtA"}):
brand = listing.find("div", attrs={"class": "_2WkVRV"}).text
description = listing.find("a", attrs={"class": "IRpwTa"}).text
price = listing.find("div", attrs={"class": "_30jeq3 _1_WHN1"}).text
  
scraped_data.append({"Brand": brand, "ProductDescription": description, "Price": price})
  
if len(scraped_data) == max_listings:
break
  
  
  next_button = soup.find("a", attrs={"class": "_1LKTO3"})
if next_button:
url = "https://www.flipkart.com" + next_button["href"]
else:
break

for data in scraped_data:
print(data)
```


      Cell In[61], line 10
        response = requests.get(url)
        ^
    IndentationError: expected an indented block after 'while' statement on line 9
    

Q5: Scrape 100 reviews data from flipkart.com for iphone11 phone. You have to go the link:
https://www.flipkart.com/apple-iphone-11-black-64-gb/productreviews/itm4e5041ba101fd?pid=MOBFWQ6BXGJCEYNY&lid=LSTMOBFWQ6BXGJCEYNYZXSHRJ&market
place=FLIPKART As shown in the above page you have to scrape the tick marked attributes. These are:
1. Rating
2. Review summary
3. Full review
4. You have to scrape this data for first 100reviews.
Note: All the steps required during scraping should be done through code only and not manually.

```python
import time
from bs4 import BeautifulSoup
from selenium import webdriver

driver = webdriver.Chrome()

url = "https://www.flipkart.com/apple-iphone-11-black-64-gb/productreviews/itm4e5041ba101fd?pid=MOBFWQ6BXGJCEYNY&lid=LSTMOBFWQ6BXGJCEYNYZXSHRJ&marketplace=FLIPKART"

driver.get(url)

scroll_count = 5
for _ in range(scroll_count):
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight)")
    time.sleep(2)  

reviews_data = []

while len(reviews_data) < 100:
    soup = BeautifulSoup(driver.page_source, 'html.parser')
    review_cards = soup.find_all('div', class_='t-ZTKy')

    for card in review_cards:
        rating = card.find('div', class_='_3LWZlK')
        review_summary = card.find('p', class_='_2-N8zT')
        full_review = card.find('div', class_='t-ZTKy')
        
        if rating and review_summary and full_review:
            reviews_data.append({
                'Rating': rating.text,
                'Review Summary': review_summary.text,
                'Full Review': full_review.text
            })

    try:
        next_button = driver.find_element_by_partial_link_text("Next")
        next_button.click()
        time.sleep(2)  
    except Exception as e:
        break  

driver.quit()

for i, review in enumerate(reviews_data, start=1):
    print(f"Review {i}:")
    print(f"Rating: {review['Rating']}")
    print(f"Review Summary: {review['Review Summary']}")
    print(f"Full Review: {review['Full Review']}\n")

```


    ---------------------------------------------------------------------------

    NoSuchWindowException                     Traceback (most recent call last)

    Cell In[70], line 13
         11 scroll_count = 5
         12 for _ in range(scroll_count):
    ---> 13     driver.execute_script("window.scrollTo(0, document.body.scrollHeight)")
         14     time.sleep(2)  
         16 reviews_data = []
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py:406, in WebDriver.execute_script(self, script, *args)
        403 converted_args = list(args)
        404 command = Command.W3C_EXECUTE_SCRIPT
    --> 406 return self.execute(command, {"script": script, "args": converted_args})["value"]
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py:346, in WebDriver.execute(self, driver_command, params)
        344 response = self.command_executor.execute(driver_command, params)
        345 if response:
    --> 346     self.error_handler.check_response(response)
        347     response["value"] = self._unwrap_value(response.get("value", None))
        348     return response
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\errorhandler.py:229, in ErrorHandler.check_response(self, response)
        227         alert_text = value["alert"].get("text")
        228     raise exception_class(message, screen, stacktrace, alert_text)  # type: ignore[call-arg]  # mypy is not smart enough here
    --> 229 raise exception_class(message, screen, stacktrace)
    

    NoSuchWindowException: Message: no such window: target window already closed
    from unknown error: web view not found
      (Session info: chrome=119.0.6045.106)
    Stacktrace:
    	GetHandleVerifier [0x00007FF63A5882B2+55298]
    	(No symbol) [0x00007FF63A4F5E02]
    	(No symbol) [0x00007FF63A3B05AB]
    	(No symbol) [0x00007FF63A390038]
    	(No symbol) [0x00007FF63A416BC7]
    	(No symbol) [0x00007FF63A42A15F]
    	(No symbol) [0x00007FF63A411E83]
    	(No symbol) [0x00007FF63A3E670A]
    	(No symbol) [0x00007FF63A3E7964]
    	GetHandleVerifier [0x00007FF63A900AAB+3694587]
    	GetHandleVerifier [0x00007FF63A95728E+4048862]
    	GetHandleVerifier [0x00007FF63A94F173+4015811]
    	GetHandleVerifier [0x00007FF63A6247D6+695590]
    	(No symbol) [0x00007FF63A500CE8]
    	(No symbol) [0x00007FF63A4FCF34]
    	(No symbol) [0x00007FF63A4FD062]
    	(No symbol) [0x00007FF63A4ED3A3]
    	BaseThreadInitThunk [0x00007FFC499B257D+29]
    	RtlUserThreadStart [0x00007FFC49FCAA58+40]
    

Q6: Scrape data forfirst 100 sneakers you find whenyou visit flipkart.com and search for “sneakers” inthe
search field.
You have to scrape 3 attributes of each sneaker:
1. Brand
2. ProductDescription
3. Price
As shown in the below image, you have to scrape the above attributes.

```python
import requests
from bs4 import BeautifulSoup

sneakers_data = []

url = "https://www.flipkart.com/search?q=sneakers"

response = requests.get(url)

if response.status_code == 200:
    page = response.text
    soup = BeautifulSoup(page, 'html.parser')

    product_cards = soup.find_all('div', class_='_2kHMtA')

    for card in product_cards[:100]:  
        brand = card.find('div', class_='_2WkVRV').text
        product_description = card.find('a', class_='IRpwTa').text
        price = card.find('div', class_='_30jeq3').text

        sneakers_data.append({
            'Brand': brand,
            'Product Description': product_description,
            'Price': price
        })

for i, sneaker in enumerate(sneakers_data, start=1):
    print(f"Sneaker {i}:")
    print(f"Brand: {sneaker['Brand']}")
    print(f"Product Description: {sneaker['Product Description']}")
    print(f"Price: {sneaker['Price']}\n")

```
Q7: Go to webpage https://www.amazon.in/ Enter “Laptop” in the search field and then click the search icon. Then
set CPU Type filter to “Intel Core i7” as shown in the below image:
After setting the filters scrape first 10 laptops data. You have to scrape 3 attributes for each laptop:
1. Title
2. Ratings
3. Price

```python
import requests
from bs4 import BeautifulSoup

url = "https://www.amazon.in/s?field-keywords=Laptop"
user_agent = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"

headers = {"User-Agent": user_agent}
response = requests.get(url, headers=headers)

soup = BeautifulSoup(response.content, "html.parser")

laptops = soup.find_all("div", class_="s-result-item")

for laptop in laptops[:10]:
    title = laptop.find("span", class_="a-text-normal").text
    rating = laptop.find("span", class_="a-icon-alt")
    price = laptop.find("span", class_="a-price")

    if rating:
        rating = rating.text
    else:
        rating = "Not available"

    if price:
        price = price.find("span", class_="a-offscreen").text
    else:
        price = "Price not available"

    print("Title: ", title)
    print("Rating: ", rating)
    print("Price: ", price)
    print("\n")

```
Q8: Write a python program to scrape data for Top 1000 Quotes of All Time.
The above task will be done in following steps:
1. First get the webpagehttps://www.azquotes.com/
2. Click on TopQuotes
3. Than scrap a) Quote b) Author c) Type Of Quotes

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()

driver.get("https://www.azquotes.com/")

top_quotes_button = driver.find_element(By.XPATH, '//*[@id="header-bar"]/div/div/div[2]/ul[1]/li[2]/a')
top_quotes_button.click()

quotes = []
authors = []
types = []

for page_number in range(1, 11): 
    quote_elements = driver.find_elements(By.CLASS_NAME, "title")
    author_elements = driver.find_elements(By.CLASS_NAME, "author")
    type_elements = driver.find_elements(By.CLASS_NAME, "kw")

for quote_element, author_element, type_element in zip(quote_elements, author_elements, type_elements):
    quotes.append(quote_element.text)
    authors.append(author_element.text)
    type.append(type_element.text)

    if page_number < 10:
        next_page_button = driver.find_element(By.CLASS_NAME, "pagination__next")
        next_page_button.click()

for i in range(len(quotes)):
    print(f"Quote: {quotes[i]}\nAuthor: {authors[i]}\nType of Quote: {types[i]}\n")

driver.quit()

```


    ---------------------------------------------------------------------------

    NoSuchElementException                    Traceback (most recent call last)

    Cell In[7], line 11
          8 driver.get("https://www.azquotes.com/")
         10 # Step 2: Click on "Top Quotes" using XPath
    ---> 11 top_quotes_button = driver.find_element(By.XPATH, '//*[@id="header-bar"]/div/div/div[2]/ul[1]/li[2]/a')
         12 top_quotes_button.click()
         14 # Step 3: Scrape the data for the top 1000 quotes
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py:740, in WebDriver.find_element(self, by, value)
        737     by = By.CSS_SELECTOR
        738     value = f'[name="{value}"]'
    --> 740 return self.execute(Command.FIND_ELEMENT, {"using": by, "value": value})["value"]
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\webdriver.py:346, in WebDriver.execute(self, driver_command, params)
        344 response = self.command_executor.execute(driver_command, params)
        345 if response:
    --> 346     self.error_handler.check_response(response)
        347     response["value"] = self._unwrap_value(response.get("value", None))
        348     return response
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\remote\errorhandler.py:229, in ErrorHandler.check_response(self, response)
        227         alert_text = value["alert"].get("text")
        228     raise exception_class(message, screen, stacktrace, alert_text)  # type: ignore[call-arg]  # mypy is not smart enough here
    --> 229 raise exception_class(message, screen, stacktrace)
    

    NoSuchElementException: Message: no such element: Unable to locate element: {"method":"xpath","selector":"//*[@id="header-bar"]/div/div/div[2]/ul[1]/li[2]/a"}
      (Session info: chrome=119.0.6045.106); For documentation on this error, please visit: https://www.selenium.dev/documentation/webdriver/troubleshooting/errors#no-such-element-exception
    Stacktrace:
    	GetHandleVerifier [0x00007FF6B49182B2+55298]
    	(No symbol) [0x00007FF6B4885E02]
    	(No symbol) [0x00007FF6B47405AB]
    	(No symbol) [0x00007FF6B478175C]
    	(No symbol) [0x00007FF6B47818DC]
    	(No symbol) [0x00007FF6B47BCBC7]
    	(No symbol) [0x00007FF6B47A20EF]
    	(No symbol) [0x00007FF6B47BAAA4]
    	(No symbol) [0x00007FF6B47A1E83]
    	(No symbol) [0x00007FF6B477670A]
    	(No symbol) [0x00007FF6B4777964]
    	GetHandleVerifier [0x00007FF6B4C90AAB+3694587]
    	GetHandleVerifier [0x00007FF6B4CE728E+4048862]
    	GetHandleVerifier [0x00007FF6B4CDF173+4015811]
    	GetHandleVerifier [0x00007FF6B49B47D6+695590]
    	(No symbol) [0x00007FF6B4890CE8]
    	(No symbol) [0x00007FF6B488CF34]
    	(No symbol) [0x00007FF6B488D062]
    	(No symbol) [0x00007FF6B487D3A3]
    	BaseThreadInitThunk [0x00007FFC499B257D+29]
    	RtlUserThreadStart [0x00007FFC49FCAA58+40]
    


Q9: Write a python program to display list of respected former Prime Ministers of India(i.e. Name, Born-Dead,
Term of office, Remarks) from https://www.jagranjosh.com/.
This task will be done in following steps:
1. First get the webpagehttps://www.jagranjosh.com/
2. Then You have to click on the GK option
3. Then click on the List of all Prime Ministers of India
4. Then scrap the mentioned data and make theDataFrame

```python
import pandas as pd
import requests
from bs4 import BeautifulSoup

url = "https://www.jagranjosh.com/"
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

gk_link = soup.find('a', text='GK')
gk_url = gk_link['href']

gk_page = requests.get(gk_url)
gk_soup = BeautifulSoup(gk_page.text, 'html.parser')

pm_list_link = gk_soup.find('a', text='List of all Prime Ministers of India')
pm_list_url = pm_list_link['href']

pm_data = []
pm_page = requests.get(pm_list_url)
pm_soup = BeautifulSoup(pm_page.text, 'html.parser')

table = pm_soup.find('table', {'class': 'table'})
rows = table.find_all('tr')[1:] 

for row in rows:
    columns = row.find_all('td')
    name = columns[0].text.strip()
    born_dead = columns[1].text.strip()
    term_of_office = columns[2].text.strip()
    remarks = columns[3].text.strip()

    pm_data.append([name, born_dead, term_of_office, remarks])

df = pd.DataFrame(pm_data, columns=['Name', 'Born-Dead', 'Term of Office', 'Remarks'])

print(df)

```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Cell In[9], line 23
         20 pm_soup = BeautifulSoup(pm_page.text, 'html.parser')
         22 table = pm_soup.find('table', {'class': 'table'})
    ---> 23 rows = table.find_all('tr')[1:] 
         25 for row in rows:
         26     columns = row.find_all('td')
    

    AttributeError: 'NoneType' object has no attribute 'find_all'

Q10: Write a python program to display list of 50 Most expensive cars in the world (i.e.
Car name and Price) from https://www.motor1.com/
This task will be done in following steps:
1. First get the webpage https://www.motor1.com/
2. Then You have to type in the search bar ’50 most expensive cars’
3. Then click on 50 most expensive carsin the world..
4. Then scrap the mentioned data and make the dataframe.

```python
import pandas as pd
import requests
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()

url = "https://www.motor1.com/"
driver.get(url)

search_query = "50 most expensive cars"
search_box = driver.find_element(By.NAME, "q")
search_box.send_keys(search_query)
search_button = driver.find_element(By.CLASS_NAME, "search-icon")
search_button.click()

expensive_cars_link = driver.find_element(By.PARTIAL_LINK_TEXT, "50 Most Expensive Cars in the World")
expensive_cars_link.click()

cars_data = []

for _ in range(5): 
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

soup = BeautifulSoup(driver.page_source, 'html.parser')

car_name_elements = soup.find_all("h2", class_="listing-title")
car_price_elements = soup.find_all("div", class_="price")

for car_name, car_price in zip(car_name_elements, car_price_elements):
    car_data = {
        "Car Name": car_name.text,
        "Price": car_price.text
    }
    cars_data.append(car_data)

df = pd.DataFrame(cars_data)

print(df)

driver.quit()

```


```python

```
