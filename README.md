# Crawler

import requests
from bs4 import BeautifulSoup
from urllib.parse import urljoin
import time

def crawl_website(url):
    try:
        # Sending a request to the website
        response = requests.get(url)
        response.raise_for_status()
        
        # Parsing the HTML content using BeautifulSoup
        soup = BeautifulSoup(response.text, 'html.parser')

        # Extracting all the anchor tags (<a>)
        links = set()
        for anchor in soup.find_all('a', href=True):
            # Getting the absolute URL
            link = urljoin(url, anchor['href'])
            if link.startswith('http'):
                links.add(link)

        # Checking the status code for each link
        for link in links:
            try:
                link_response = requests.get(link)
                print(f"Link: {link} | Status Code: {link_response.status_code}")
                time.sleep(0.5)  # Delay to avoid overwhelming the server
            except requests.RequestException as e:
                print(f"Could not access {link}: {e}")

    except requests.RequestException as e:
        print(f"Error while accessing {url}: {e}")

if __name__ == "__main__":
    start_url = 'replace with any domain'
    crawl_website(start_url)
