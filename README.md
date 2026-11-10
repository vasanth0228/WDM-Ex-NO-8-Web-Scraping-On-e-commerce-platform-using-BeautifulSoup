# WDM-Ex-NO-8-Web-Scraping-On-e-commerce-platform-using-BeautifulSoup
## Nmae: Vasanth Kumar V
## Date: 
**AIM:**<br>
To perform Web Scraping on Amazon using (beautifulsoup) Python.<br>
**Description:**<br>
Web scraping is the process of extracting data from various websites and parsing it. In other words, it’s a technique to extract unstructured data and store that data either in a local file or in a database. There are many ways to collect data that involve a huge amount of hard work and consume a lot of time. Web scraping can save programmers many hours. Beautiful Soup is a Python web scraping library that allows us to parse and scrape HTML and XML pages. One can search, navigate, and modify data using a parser. It’s versatile and saves a lot of time.

**The basic steps involved in web scraping are:**<br>
1) Loading the document (HTML content)

2) Parsing the document

3) Extraction

4) Transformation

**Procedure:**<br>
Import necessary libraries (requests, BeautifulSoup, re, matplotlib.pyplot).
Define convert_price_to_float(price) Function: to Remove non-numeric characters from a price string and convert it to a float.
Define get_amazon_products(search_query) Function: to Scrape Amazon for product information based on the search query.
Fetch and parse the HTML content then Extract product names and prices from the search results and Sort product information based on converted prices in ascending order.
Return sorted product data as a list of dictionaries.
Call get_amazon_products(search_query) to get product data based on the user's search query.
Check if products are found; if not, display "No products found."
Visualize Product Data using a Bar Chart<br>
**Program:**
```
import requests
from bs4 import BeautifulSoup
import re
import matplotlib.pyplot as plt


# Convert price into float
def convert_price_to_float(price):
    price = re.sub(r'[^\d.]', '', price)
    return float(price) if price else 0.0


# Extract products from Amazon
def get_amazon_products(search_query):

    base_url = 'https://www.amazon.in'

    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36',
        'Accept-Language': 'en-IN,en;q=0.9'
    }

    search_query = search_query.replace(' ', '+')
    url = f'{base_url}/s?k={search_query}'

    products_data = []

    try:
        response = requests.get(
            url,
            headers=headers,
            timeout=15
        )

        print("Status Code:", response.status_code)

        if response.status_code == 200:

            soup = BeautifulSoup(
                response.content,
                'html.parser'
            )

            products = soup.find_all(
                'div',
                {'data-component-type': 's-search-result'}
            )

            for product in products:

                # Extract product name
                title_tag = product.find('h2')

                if not title_tag:
                    continue

                product_name = title_tag.get_text(
                    strip=True
                )

                # Extract product price
                price_tag = product.find(
                    'span',
                    class_='a-offscreen'
                )

                if not price_tag:
                    continue

                price = price_tag.get_text(strip=True)

                if convert_price_to_float(price) > 0:

                    products_data.append({
                        'Product': product_name,
                        'Price': price
                    })

        else:
            print("Amazon access failed.")
            print("Amazon returned status:", response.status_code)

    except requests.RequestException as e:
        print("Error connecting to Amazon:", e)

    return sorted(
        products_data,
        key=lambda x: convert_price_to_float(x['Price'])
    )


# Get product from user
search_query = input(
    'Enter product to search on Amazon: '
)

products = get_amazon_products(search_query)


# If Amazon blocks the request, use sample data
if not products:

    print("\nLive products could not be retrieved.")
    print("Using sample product data for demonstration.\n")

    products = [
        {'Product': 'Cotton Casual Dress', 'Price': '₹799'},
        {'Product': 'Floral Printed Dress', 'Price': '₹1299'},
        {'Product': 'Women Party Wear Dress', 'Price': '₹1599'},
        {'Product': 'Long Maxi Dress', 'Price': '₹999'},
        {'Product': 'Designer Evening Dress', 'Price': '₹2499'}
    ]

    products = sorted(
        products,
        key=lambda x: convert_price_to_float(x['Price'])
    )


# Display product information
print("\nProduct Details:\n")

for product in products:

    print("Product:", product['Product'])
    print("Price:", product['Price'])
    print("----------------------")


# Prepare data for bar chart
product_names = [
    product['Product'][:30]
    for product in products
]

product_prices = [
    convert_price_to_float(product['Price'])
    for product in products
]


# Display bar chart
plt.figure(figsize=(10, 6))

plt.barh(
    range(len(product_prices)),
    product_prices,
    color='skyblue'
)

plt.xlabel('Price (Rs.)')
plt.ylabel('Product')

plt.title(
    f'Products and their Prices for {search_query.capitalize()} (Ascending Order)'
)

plt.yticks(
    range(len(product_prices)),
    product_names
)

plt.tight_layout()
plt.show()
```
**Output:**<br>
<img width="476" height="527" alt="image" src="https://github.com/user-attachments/assets/cabf841c-664a-442e-9888-6f6a7b207130" />

<img width="1027" height="601" alt="image" src="https://github.com/user-attachments/assets/35b593a5-9c02-43ae-8f6a-b1e3a434eea3" />

**Result:**
Web Scraping On E-commerce platform using BeautifulSoup is implemented.

