# Web-Scraping-Assignment 3

# Write a python program which searches all the product under a particular product from www.amazon.in. The product to be searched will be taken as input from user. For e.g. If user input is ‘guitar’. Then search for guitars.


```python
!pip install openpyxl

```

    Requirement already satisfied: openpyxl in c:\anaconda3\lib\site-packages (3.0.10)
    Requirement already satisfied: et_xmlfile in c:\anaconda3\lib\site-packages (from openpyxl) (1.1.0)
    


```python
import openpyxl
from bs4 import BeautifulSoup

def parse_html_and_write_to_excel(html_file, excel_file):
    with open(html_file, 'r', encoding='utf-8') as file:
        html_content = file.read()

    soup = BeautifulSoup(html_content, 'html.parser')

    result_info_bars = soup.find_all('div', class_='s-widget-container s-spacing-small s-widget-container-height-small celwidget slot=UPPER template=RESULT_INFO_BAR widgetId=result-info-bar')

    workbook = openpyxl.Workbook()
    worksheet = workbook.active

    worksheet.append(['Product_Name', 'Product_Price', 'Product_Reviews'])

    for result_info_bar in result_info_bars:
        product_name = ""
        product_price = ""
        product_reviews = ""
        
        product_name_tag = result_info_bar.find('span', class_='a-size-medium a-color-base a-text-normal')
        if product_name_tag:
            product_name = product_name_tag.get_text(strip=True)

        product_price_tag = result_info_bar.find('span', class_='a-price-whole')
        if product_price_tag:
            product_price = product_price_tag.get_text(strip=True)
            
        product_reviews_tag = result_info_bar.find('span', class_='a-icon-alt')
        if product_reviews_tag:
            product_reviews = product_reviews_tag.get_text(strip=True)

        worksheet.append([product_name, product_price, product_reviews])

    workbook.save(excel_file)

if __name__ == "__main__":
    html_file = 'Amazon.in.html' 
    excel_file = 'amazon_products.xlsx' 

    parse_html_and_write_to_excel(html_file, excel_file)

```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    Cell In[14], line 42
         39 html_file = 'Amazon.in.html' 
         40 excel_file = 'amazon_products.xlsx' 
    ---> 42 parse_html_and_write_to_excel(html_file, excel_file)
    

    Cell In[14], line 5, in parse_html_and_write_to_excel(html_file, excel_file)
          4 def parse_html_and_write_to_excel(html_file, excel_file):
    ----> 5     with open(html_file, 'r', encoding='utf-8') as file:
          6         html_content = file.read()
          8     soup = BeautifulSoup(html_content, 'html.parser')
    

    File C:\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py:282, in _modified_open(file, *args, **kwargs)
        275 if file in {0, 1, 2}:
        276     raise ValueError(
        277         f"IPython won't let you open fd={file} by default "
        278         "as it is likely to crash IPython. If you know what you are doing, "
        279         "you can use builtins' open."
        280     )
    --> 282 return io_open(file, *args, **kwargs)
    

    FileNotFoundError: [Errno 2] No such file or directory: 'Amazon.in.html'


# In the above question, now scrape the following details of each product listed in first 3 pages of your search results and save it in a data frame and csv. In case if any product has less than 3 pages in search results then scrape all the products available under that product name. Details to be scraped are: "Brand
Name", "Name of the Product", "Price", "Return/Exchange", "Expected Delivery", "Availability" and
“Product URL”. In case, if any of the details are missing for any of the product then replace it by “-“.


```python
import pandas as pd
import requests
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.common.keys import Keys

def scrape_product_details(product):
    details = {
        'Brand Name': '-',
        'Name of the Product': '-',
        'Price': '-',
        'Return/Exchange': '-',
        'Expected Delivery': '-',
        'Availability': '-',
        'Product URL': '-'
    }

    try:
        details['Brand Name'] = product.find('span', class_='a-size-base-plus').get_text(strip=True)
    except AttributeError:
        pass

    try:
        details['Name of the Product'] = product.find('span', class_='a-text-normal').get_text(strip=True)
    except AttributeError:
        pass

    try:
        details['Price'] = product.find('span', class_='a-offscreen').get_text(strip=True)
    except AttributeError:
        pass

    try:
        details['Return/Exchange'] = product.find('div', class_='a-row a-size-base a-color-secondary').get_text(strip=True)
    except AttributeError:
        pass

    try:
        details['Expected Delivery'] = product.find('span', class_='a-text-bold').get_text(strip=True)
    except AttributeError:
        pass

    try:
        details['Availability'] = product.find('span', class_='a-size-medium a-color-success').get_text(strip=True)
    except AttributeError:
        pass

    try:
        details['Product URL'] = 'https://www.amazon.in' + product.find('a', class_='a-link-normal')['href']
    except AttributeError:
        pass

    return details

def search_amazon_and_scrape(product_name, num_pages=3):
    driver = webdriver.Chrome(executable_path='/path/to/chromedriver')

    product_details_list = []
    
if __name__ == '__main__':
    
    try:
        product_name = input('Enter the product you want to search on Amazon.in: ')
        search_amazon_and_scrape(product_name)
    except KeyboardInterrupt:
        print("\nOperation interrupted by the user.")
        
    for page in range(1, num_pages + 1):
        driver.get(f'https://www.amazon.in/s?k={product_name}&page={page}')
        driver.implicitly_wait(10)
        
        page_source = driver.page_source
        soup = BeautifulSoup(page_source, 'html.parser')

        products = soup.find_all('div', class_='s-result-item')

        for product in products:
            details = scrape_product_details(product)
            product_details_list.append(details)

    driver.quit()

    df = pd.DataFrame(product_details_list)

    df.to_csv('amazon_product_details.csv', index=False)

if __name__ == '__main__':
    product_name = input('Enter the product you want to search on Amazon.in: ')
    search_amazon_and_scrape(product_name)

```

    
    Operation interrupted by the user.
    


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Cell In[6], line 68
         65 except KeyboardInterrupt:
         66     print("\nOperation interrupted by the user.")
    ---> 68 for page in range(1, num_pages + 1):
         69     driver.get(f'https://www.amazon.in/s?k={product_name}&page={page}')
         70     driver.implicitly_wait(10)
    

    NameError: name 'num_pages' is not defined



```python
!pip install google_images_download
```

    Requirement already satisfied: google_images_download in c:\anaconda3\lib\site-packages (2.8.0)
    Requirement already satisfied: selenium in c:\anaconda3\lib\site-packages (from google_images_download) (4.15.2)
    Requirement already satisfied: trio~=0.17 in c:\anaconda3\lib\site-packages (from selenium->google_images_download) (0.23.0)
    Requirement already satisfied: trio-websocket~=0.9 in c:\anaconda3\lib\site-packages (from selenium->google_images_download) (0.11.1)
    Requirement already satisfied: certifi>=2021.10.8 in c:\anaconda3\lib\site-packages (from selenium->google_images_download) (2022.12.7)
    Requirement already satisfied: urllib3[socks]<3,>=1.26 in c:\anaconda3\lib\site-packages (from selenium->google_images_download) (1.26.14)
    Requirement already satisfied: outcome in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium->google_images_download) (1.3.0.post0)
    Requirement already satisfied: sniffio>=1.3.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium->google_images_download) (1.3.0)
    Requirement already satisfied: exceptiongroup>=1.0.0rc9 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium->google_images_download) (1.1.3)
    Requirement already satisfied: idna in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium->google_images_download) (3.4)
    Requirement already satisfied: sortedcontainers in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium->google_images_download) (2.4.0)
    Requirement already satisfied: cffi>=1.14 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium->google_images_download) (1.15.1)
    Requirement already satisfied: attrs>=20.1.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium->google_images_download) (22.1.0)
    Requirement already satisfied: wsproto>=0.14 in c:\anaconda3\lib\site-packages (from trio-websocket~=0.9->selenium->google_images_download) (1.2.0)
    Requirement already satisfied: PySocks!=1.5.7,<2.0,>=1.5.6 in c:\anaconda3\lib\site-packages (from urllib3[socks]<3,>=1.26->selenium->google_images_download) (1.7.1)
    Requirement already satisfied: pycparser in c:\anaconda3\lib\site-packages (from cffi>=1.14->trio~=0.17->selenium->google_images_download) (2.21)
    Requirement already satisfied: h11<1,>=0.9.0 in c:\anaconda3\lib\site-packages (from wsproto>=0.14->trio-websocket~=0.9->selenium->google_images_download) (0.14.0)
    

# Write a python program to access the search bar and search button on images.google.com and scrape 10 images each for keywords ‘fruits’, ‘cars’ and ‘Machine Learning’, ‘Guitar’, ‘Cakes’.


```python
!pip install selenium beautifulsoup4
!pip install --upgrade selenium


```

    Requirement already satisfied: selenium in c:\anaconda3\lib\site-packages (4.15.1)
    Requirement already satisfied: beautifulsoup4 in c:\anaconda3\lib\site-packages (4.11.1)
    Requirement already satisfied: trio~=0.17 in c:\anaconda3\lib\site-packages (from selenium) (0.23.0)
    Requirement already satisfied: urllib3[socks]<3,>=1.26 in c:\anaconda3\lib\site-packages (from selenium) (1.26.14)
    Requirement already satisfied: certifi>=2021.10.8 in c:\anaconda3\lib\site-packages (from selenium) (2022.12.7)
    Requirement already satisfied: trio-websocket~=0.9 in c:\anaconda3\lib\site-packages (from selenium) (0.11.1)
    Requirement already satisfied: soupsieve>1.2 in c:\anaconda3\lib\site-packages (from beautifulsoup4) (2.3.2.post1)
    Requirement already satisfied: cffi>=1.14 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.15.1)
    Requirement already satisfied: outcome in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.3.0.post0)
    Requirement already satisfied: sniffio>=1.3.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.3.0)
    Requirement already satisfied: exceptiongroup>=1.0.0rc9 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.1.3)
    Requirement already satisfied: idna in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (3.4)
    Requirement already satisfied: attrs>=20.1.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (22.1.0)
    Requirement already satisfied: sortedcontainers in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (2.4.0)
    Requirement already satisfied: wsproto>=0.14 in c:\anaconda3\lib\site-packages (from trio-websocket~=0.9->selenium) (1.2.0)
    Requirement already satisfied: PySocks!=1.5.7,<2.0,>=1.5.6 in c:\anaconda3\lib\site-packages (from urllib3[socks]<3,>=1.26->selenium) (1.7.1)
    Requirement already satisfied: pycparser in c:\anaconda3\lib\site-packages (from cffi>=1.14->trio~=0.17->selenium) (2.21)
    Requirement already satisfied: h11<1,>=0.9.0 in c:\anaconda3\lib\site-packages (from wsproto>=0.14->trio-websocket~=0.9->selenium) (0.14.0)
    Requirement already satisfied: selenium in c:\anaconda3\lib\site-packages (4.15.1)
    Collecting selenium
      Downloading selenium-4.15.2-py3-none-any.whl (10.2 MB)
         ---------------------------------------- 10.2/10.2 MB 9.2 MB/s eta 0:00:00
    Requirement already satisfied: urllib3[socks]<3,>=1.26 in c:\anaconda3\lib\site-packages (from selenium) (1.26.14)
    Requirement already satisfied: trio~=0.17 in c:\anaconda3\lib\site-packages (from selenium) (0.23.0)
    Requirement already satisfied: certifi>=2021.10.8 in c:\anaconda3\lib\site-packages (from selenium) (2022.12.7)
    Requirement already satisfied: trio-websocket~=0.9 in c:\anaconda3\lib\site-packages (from selenium) (0.11.1)
    Requirement already satisfied: idna in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (3.4)
    Requirement already satisfied: outcome in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.3.0.post0)
    Requirement already satisfied: sniffio>=1.3.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.3.0)
    Requirement already satisfied: cffi>=1.14 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.15.1)
    Requirement already satisfied: attrs>=20.1.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (22.1.0)
    Requirement already satisfied: exceptiongroup>=1.0.0rc9 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.1.3)
    Requirement already satisfied: sortedcontainers in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (2.4.0)
    Requirement already satisfied: wsproto>=0.14 in c:\anaconda3\lib\site-packages (from trio-websocket~=0.9->selenium) (1.2.0)
    Requirement already satisfied: PySocks!=1.5.7,<2.0,>=1.5.6 in c:\anaconda3\lib\site-packages (from urllib3[socks]<3,>=1.26->selenium) (1.7.1)
    Requirement already satisfied: pycparser in c:\anaconda3\lib\site-packages (from cffi>=1.14->trio~=0.17->selenium) (2.21)
    Requirement already satisfied: h11<1,>=0.9.0 in c:\anaconda3\lib\site-packages (from wsproto>=0.14->trio-websocket~=0.9->selenium) (0.14.0)
    Installing collected packages: selenium
      Attempting uninstall: selenium
        Found existing installation: selenium 4.15.1
        Uninstalling selenium-4.15.1:
          Successfully uninstalled selenium-4.15.1
    Successfully installed selenium-4.15.2
    


```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup
import time
import requests
import os

def scrape_images(search_keywords, num_images=10):
    driver = webdriver.Chrome(executable_path='path/to/chromedriver')  

    for keyword in search_keywords:
        try:
            search_images(driver, keyword, num_images)
        except Exception as e:
            print(f"An error occurred while searching for '{keyword}': {e}")

    driver.quit()

def search_images(driver, keyword, num_images):
    driver.get("https://images.google.com/")
    time.sleep(2)

    search_box = driver.find_element("name", "q")
    search_box.clear()
    search_box.send_keys(keyword)
    search_box.send_keys(Keys.RETURN)
    time.sleep(2)

    for _ in range(3):
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        time.sleep(2)

    soup = BeautifulSoup(driver.page_source, 'html.parser')
    img_tags = soup.find_all('img', class_='rg_i')

    image_urls = []
    for img_tag in img_tags[:num_images]:
        img_url = img_tag.get('data-src')
        if img_url:
            image_urls.append(img_url)

    download_images(keyword, image_urls)

def download_images(keyword, image_urls):
    save_folder = f"{keyword}_images"
    os.makedirs(save_folder, exist_ok=True)

    for i, img_url in enumerate(image_urls):
        try:
            response = requests.get(img_url)
            with open(os.path.join(save_folder, f"{keyword}_{i+1}.jpg"), 'wb') as img_file:
                img_file.write(response.content)
        except Exception as e:
            print(f"An error occurred while downloading image {i+1}: {e}")

if __name__ == "__main__":
    search_keywords = ['fruits', 'cars', 'Machine Learning', 'Guitar', 'Cakes']
    scrape_images(search_keywords)

```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Cell In[18], line 58
         56 if __name__ == "__main__":
         57     search_keywords = ['fruits', 'cars', 'Machine Learning', 'Guitar', 'Cakes']
    ---> 58     scrape_images(search_keywords)
    

    Cell In[18], line 9, in scrape_images(search_keywords, num_images)
          8 def scrape_images(search_keywords, num_images=10):
    ----> 9     driver = webdriver.Chrome(executable_path='path/to/chromedriver')  
         11     for keyword in search_keywords:
         12         try:
    

    TypeError: WebDriver.__init__() got an unexpected keyword argument 'executable_path'


# Write a python program to search for a smartphone(e.g.: Oneplus Nord, pixel 4A, etc.) on www.flipkart.com and scrape following details for all the search results displayed on 1st page. Details to be scraped: “Brand Name”, “Smartphone name”, “Colour”, “RAM”, “Storage(ROM)”, “Primary Camera”,
“Secondary Camera”, “Display Size”, “Battery Capacity”, “Price”, “Product URL”. Incase if any of the details is missing then replace it by “- “. Save your results in a dataframe and CSV.


```python
pip install selenium pandas

```

    Requirement already satisfied: selenium in c:\anaconda3\lib\site-packages (4.15.2)Note: you may need to restart the kernel to use updated packages.
    
    Requirement already satisfied: pandas in c:\anaconda3\lib\site-packages (1.5.3)
    Requirement already satisfied: trio~=0.17 in c:\anaconda3\lib\site-packages (from selenium) (0.23.0)
    Requirement already satisfied: trio-websocket~=0.9 in c:\anaconda3\lib\site-packages (from selenium) (0.11.1)
    Requirement already satisfied: certifi>=2021.10.8 in c:\anaconda3\lib\site-packages (from selenium) (2022.12.7)
    Requirement already satisfied: urllib3[socks]<3,>=1.26 in c:\anaconda3\lib\site-packages (from selenium) (1.26.14)
    Requirement already satisfied: numpy>=1.21.0 in c:\anaconda3\lib\site-packages (from pandas) (1.23.5)
    Requirement already satisfied: pytz>=2020.1 in c:\anaconda3\lib\site-packages (from pandas) (2022.7)
    Requirement already satisfied: python-dateutil>=2.8.1 in c:\anaconda3\lib\site-packages (from pandas) (2.8.2)
    Requirement already satisfied: six>=1.5 in c:\anaconda3\lib\site-packages (from python-dateutil>=2.8.1->pandas) (1.16.0)
    Requirement already satisfied: idna in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (3.4)
    Requirement already satisfied: attrs>=20.1.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (22.1.0)
    Requirement already satisfied: outcome in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.3.0.post0)
    Requirement already satisfied: sniffio>=1.3.0 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.3.0)
    Requirement already satisfied: sortedcontainers in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (2.4.0)
    Requirement already satisfied: cffi>=1.14 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.15.1)
    Requirement already satisfied: exceptiongroup>=1.0.0rc9 in c:\anaconda3\lib\site-packages (from trio~=0.17->selenium) (1.1.3)
    Requirement already satisfied: wsproto>=0.14 in c:\anaconda3\lib\site-packages (from trio-websocket~=0.9->selenium) (1.2.0)
    Requirement already satisfied: PySocks!=1.5.7,<2.0,>=1.5.6 in c:\anaconda3\lib\site-packages (from urllib3[socks]<3,>=1.26->selenium) (1.7.1)
    Requirement already satisfied: pycparser in c:\anaconda3\lib\site-packages (from cffi>=1.14->trio~=0.17->selenium) (2.21)
    Requirement already satisfied: h11<1,>=0.9.0 in c:\anaconda3\lib\site-packages (from wsproto>=0.14->trio-websocket~=0.9->selenium) (0.14.0)
    


```python
from selenium import webdriver
from selenium.webdriver.common.by import By
import pandas as pd

def scrape_flipkart_smartphones(product_name):
    driver = webdriver.Chrome(executable_path='path/to/chromedriver') 
    driver.get("https://www.flipkart.com/")

    close_popup_button = driver.find_element(By.CSS_SELECTOR, "button._2KpZ6l._2doB4z")
    if close_popup_button.is_displayed():
        close_popup_button.click()

    search_box = driver.find_element(By.NAME, "q")
    search_box.clear()
    search_box.send_keys(product_name)
    search_box.submit()

    driver.implicitly_wait(10)

    details_list = []
    product_cards = driver.find_elements(By.CSS_SELECTOR, "div._1AtVbE")

    for card in product_cards:
        details = extract_product_details(card)
        details_list.append(details)

    columns = ["Brand Name", "Smartphone Name", "Colour", "RAM", "Storage (ROM)", 
               "Primary Camera", "Secondary Camera", "Display Size", 
               "Battery Capacity", "Price", "Product URL"]
    df = pd.DataFrame(details_list, columns=columns)

    df.to_csv("flipkart_smartphones.csv", index=False)

    driver.quit()

def extract_product_details(card):
    details = {}
    details["Brand Name"] = card.find_element(By.CSS_SELECTOR, "div._2WkVRV").text
    details["Smartphone Name"] = card.find_element(By.CSS_SELECTOR, "a._1fQZEK").text
    details["Colour"] = get_text(card, "div._2o7WAb > ul > li:nth-child(4)")
    details["RAM"] = get_text(card, "div._2o7WAb > ul > li:nth-child(1)")
    details["Storage (ROM)"] = get_text(card, "div._2o7WAb > ul > li:nth-child(2)")
    details["Primary Camera"] = get_text(card, "div._2o7WAb > ul > li:nth-child(3)")
    details["Secondary Camera"] = get_text(card, "div._2o7WAb > ul > li:nth-child(5)")
    details["Display Size"] = get_text(card, "div._2o7WAb > ul > li:nth-child(6)")
    details["Battery Capacity"] = get_text(card, "div._2o7WAb > ul > li:nth-child(7)")
    details["Price"] = card.find_element(By.CSS_SELECTOR, "div._30jeq3").text
    details["Product URL"] = card.find_element(By.CSS_SELECTOR, "a._1fQZEK").get_attribute("href")

    return details

def get_text(element, selector):
    try:
        return element.find_element(By.CSS_SELECTOR, selector).text
    except:
        return "-"

if __name__ == "__main__":
    product_name = input("Enter the smartphone you want to search for on Flipkart: ")
    scrape_flipkart_smartphones(product_name)

```


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    Cell In[55], line 65
         62         return "-"
         64 if __name__ == "__main__":
    ---> 65     product_name = input("Enter the smartphone you want to search for on Flipkart: ")
         66     scrape_flipkart_smartphones(product_name)
    

    File C:\Anaconda3\lib\site-packages\ipykernel\kernelbase.py:1175, in Kernel.raw_input(self, prompt)
       1171 if not self._allow_stdin:
       1172     raise StdinNotImplementedError(
       1173         "raw_input was called, but this frontend does not support input requests."
       1174     )
    -> 1175 return self._input_request(
       1176     str(prompt),
       1177     self._parent_ident["shell"],
       1178     self.get_parent("shell"),
       1179     password=False,
       1180 )
    

    File C:\Anaconda3\lib\site-packages\ipykernel\kernelbase.py:1217, in Kernel._input_request(self, prompt, ident, parent, password)
       1214             break
       1215 except KeyboardInterrupt:
       1216     # re-raise KeyboardInterrupt, to truncate traceback
    -> 1217     raise KeyboardInterrupt("Interrupted by user") from None
       1218 except Exception:
       1219     self.log.warning("Invalid Message:", exc_info=True)
    

    KeyboardInterrupt: Interrupted by user


# 5. Write a program to scrap geospatial coordinates (latitude, longitude) of a city searched on google maps.


```python
pip install geopy
```

    Requirement already satisfied: geopy in c:\anaconda3\lib\site-packages (2.4.0)Note: you may need to restart the kernel to use updated packages.
    
    Requirement already satisfied: geographiclib<3,>=1.52 in c:\anaconda3\lib\site-packages (from geopy) (2.0)
    


```python
from geopy.geocoders import ArcGIS
nom = ArcGIS()
nom.geocode('shantinagar')
```




    Location(Shantinagar, Bangalor, Karnatak, (12.95756, 77.59693, 0.0))




```python
nom.geocode('modinagar')
```




    Location(Modinagar, Ghaziabad, Uttar Pradesh, (28.83624, 77.57679, 0.0))




```python
nom.geocode('delhi')
```




    Location(Delhi, Connaught Place, New Delhi, Delhi, (28.63409, 77.21693, 0.0))



# 6. Write a program to scrap all the available details of best gaming laptops from digit.in.


```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape_digit_gaming_laptops():
    url = "https://www.digit.in/top-products/best-gaming-laptops-40.html"
    response = requests.get(url)

    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')

        laptops = []

        laptop_cards = soup.find_all("div", class_="TopNumbeHeading")

        for card in laptop_cards:
            details = get_laptop_details(card)
            laptops.append(details)

        columns = ["Name", "Processor", "RAM", "OS", "Storage", "Display", "Price"]
        df = pd.DataFrame(laptops, columns=columns)

        print(df)
        df.to_csv("digit_gaming_laptops.csv", index=False)

    else:
        print(f"Failed to fetch data. Status code: {response.status_code}")

def get_laptop_details(card):
    details = {}
    details["Name"] = card.find("div", class_="TopNumbeHeading").text.strip()
    details["Processor"] = card.find("div", class_="prod-spec").text.strip()
    details["RAM"] = card.find("div", class_="Spcs-details").find_all("td")[1].text.strip()
    details["OS"] = card.find("div", class_="Spcs-details").find_all("td")[3].text.strip()
    details["Storage"] = card.find("div", class_="Spcs-details").find_all("td")[5].text.strip()
    details["Display"] = card.find("div", class_="Spcs-details").find_all("td")[7].text.strip()
    details["Price"] = card.find("div", class_="TopPriceBox").text.strip()

    return details

if __name__ == "__main__":
    scrape_digit_gaming_laptops()

```

    Empty DataFrame
    Columns: [Name, Processor, RAM, OS, Storage, Display, Price]
    Index: []
    

# Write a python program to scrape the details for all billionaires from www.forbes.com. Details to be scrapped: “Rank”, “Name”, “Net worth”, “Age”, “Citizenship”, “Source”, “Industry”.


```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape_forbes_billionaires():
    url = "https://www.forbes.com/billionaires/"

    response = requests.get(url)

    if response.status_code == 200:
        soup = BeautifulSoup(response.content, 'html.parser')

        table = soup.find("table", class_="data")

        if table:
            billionaires = []

            rows = table.find_all("tr")[1:] 

            for row in rows:
                details = get_billionaire_details(row)
                billionaires.append(details)

            columns = ["Rank", "Name", "Net Worth", "Age", "Citizenship", "Source", "Industry"]
            df = pd.DataFrame(billionaires, columns=columns)

            print(df)
            df.to_csv("forbes_billionaires.csv", index=False)

        else:
            print("Table not found on the page.")

    else:
        print(f"Failed to fetch data. Status code: {response.status_code}")

def get_billionaire_details(row):
    details = {}
    columns = row.find_all("td")

    details["Rank"] = columns[0].text.strip()
    details["Name"] = columns[1].text.strip()
    details["Net Worth"] = columns[2].text.strip()
    details["Age"] = columns[3].text.strip()
    details["Citizenship"] = columns[4].text.strip()
    details["Source"] = columns[5].text.strip()
    details["Industry"] = columns[6].text.strip()

    return details

if __name__ == "__main__":
    scrape_forbes_billionaires()

```

    Table not found on the page.
    

# 8. Write a program to extract at least 500 Comments, Comment upvote and time when comment was posted from any YouTube Video


```python
from selenium import webdriver
import time

# Set up the WebDriver
driver = webdriver.Chrome('path_to_chromedriver')  # Replace with the path to your WebDriver executable

# Open the YouTube video
video_url = 'https://www.youtube.com/watch?v=your_video_id'  # Replace with the URL of the YouTube video
driver.get(video_url)


scroll_pause_time = 2  
scrolls = 10  

for _ in range(scrolls):
  driver.execute_script("window.scrollTo(0, document.documentElement.scrollHeight);")
  time.sleep(scroll_pause_time)

comments = driver.find_elements_by_xpath('//yt-formatted-string[@id="content-text"]')
upvotes = driver.find_elements_by_xpath('//span[@id="vote-count-middle"]')
times = driver.find_elements_by_xpath('//a[@class="yt-simple-endpoint style-scope yt-formatted-string"]')

extracted_data = []
for comment, upvote, time in zip(comments, upvotes, times):
  extracted_data.append({
  'comment': comment.text,
  'upvote': upvote.text,
  'time': time.text
  })

driver.quit()
for data in extracted_data:
  print(data)
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\common\driver_finder.py:38, in DriverFinder.get_path(service, options)
         37 try:
    ---> 38     path = SeleniumManager().driver_location(options) if path is None else path
         39 except Exception as err:
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\common\selenium_manager.py:75, in SeleniumManager.driver_location(self, options)
         68 """Determines the path of the correct driver.
         69 
         70 :Args:
         71  - browser: which browser to get the driver path for.
         72 :Returns: The driver path to use
         73 """
    ---> 75 browser = options.capabilities["browserName"]
         77 args = [str(self.get_binary()), "--browser", browser]
    

    AttributeError: 'str' object has no attribute 'capabilities'

    
    During handling of the above exception, another exception occurred:
    

    AttributeError                            Traceback (most recent call last)

    Cell In[70], line 5
          2 import time
          4 # Set up the WebDriver
    ----> 5 driver = webdriver.Chrome('path_to_chromedriver')  # Replace with the path to your WebDriver executable
          7 # Open the YouTube video
          8 video_url = 'https://www.youtube.com/watch?v=your_video_id'  # Replace with the URL of the YouTube video
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\chrome\webdriver.py:45, in WebDriver.__init__(self, options, service, keep_alive)
         42 service = service if service else Service()
         43 options = options if options else Options()
    ---> 45 super().__init__(
         46     DesiredCapabilities.CHROME["browserName"],
         47     "goog",
         48     options,
         49     service,
         50     keep_alive,
         51 )
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\chromium\webdriver.py:51, in ChromiumDriver.__init__(self, browser_name, vendor_prefix, options, service, keep_alive)
         47 self.vendor_prefix = vendor_prefix
         49 self.service = service
    ---> 51 self.service.path = DriverFinder.get_path(self.service, options)
         53 self.service.start()
         55 try:
    

    File C:\Anaconda3\lib\site-packages\selenium\webdriver\common\driver_finder.py:40, in DriverFinder.get_path(service, options)
         38     path = SeleniumManager().driver_location(options) if path is None else path
         39 except Exception as err:
    ---> 40     msg = f"Unable to obtain driver for {options.capabilities['browserName']} using Selenium Manager."
         41     raise NoSuchDriverException(msg) from err
         43 if path is None or not Path(path).is_file():
    

    AttributeError: 'str' object has no attribute 'capabilities'


# Write a python program to scrape a data for all available Hostels from https://www.hostelworld.com/ in “London” location. You have to scrape hostel name, distance from city centre, ratings, total reviews, overall reviews, privates from price, dorms from price, facilities and property description.


```python
import requests
from bs4 import BeautifulSoup

url = "https://www.hostelworld.com/hostels/London"
response = requests.get(url)
soup = BeautifulSoup(response.content, "html.parser")
hostels = soup.find_all("div", class_="fabresult")

for hostel in hostels:
    name = hostel.find("h2", class_="fabresult-title").text.strip()
    distance = hostel.find("span", class_="distance").text.strip()
    ratings = hostel.find("div", class_="rating").text.strip()
  
    total_reviews = hostel.find("div", class_="reviews").text.strip()
    overall_reviews = hostel.find("div", class_="overall").text.strip()
    privates_price = hostel.find("div", class_="price-col").find("div", class_="price").text.strip()
    dorms_price = hostel.find("div", class_="price-col").find("div", class_="price").find_next_sibling("div", class_="price").text.strip()
    facilities = hostel.find("div", class_="facilities").text.strip()
    description = hostel.find("div", class_="description").text.strip()
  
  
    print("Hostel Name:", name)
    print("Distance from City Centre:", distance)
    print("Ratings:", ratings)
    print("Total Reviews:", total_reviews)
    print("Overall Reviews:", overall_reviews)
    print("Privates from Price:", privates_price)
    print("Dorms from Price:", dorms_price)
    print("Facilities:", facilities)
    print("Property Description:", description)
    print()
```


```python

```
