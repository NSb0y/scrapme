# Scrapme

A comprehensive web scraping framework featuring both static and dynamic content extraction, automatic Selenium/geckodriver management, rate limiting, proxy rotation, and Unicode support (including Georgian). Built with BeautifulSoup4 and Selenium, it provides an intuitive API for extracting text, tables, links and more from any web source.

## Features

- 🚀 Simple and intuitive API
- 🔄 Support for JavaScript-rendered content using Selenium
- 🛠️ Automatic geckodriver management
- ⏱️ Built-in rate limiting
- 🔄 Proxy rotation with health tracking
- 📊 Automatic table parsing to Pandas DataFrames
- 🌐 Full Unicode support (including Georgian)
- 🧹 Clean text extraction
- 🎯 CSS selector support
- 🔍 Multiple content extraction methods

## Installation

```bash
pip install scrapme
```

Pypi.org ([https://pypi.org/project/loggerbot/](https://pypi.org/project/scrapme/))


## Quick Start

### Basic Usage (Static Content)

```python
from scrapme import WebScraper

# Initialize scraper
scraper = WebScraper()

# Get text content
text = scraper.get_text("https://example.com")
print(text)

# Extract all links
links = scraper.get_links("https://example.com")
for link in links:
    print(f"Text: {link['text']}, URL: {link['href']}")

# Parse tables into pandas DataFrames
tables = scraper.get_tables("https://example.com")
if tables:
    print(tables[0].head())
```

### Dynamic Content (JavaScript-Rendered)

```python
from scrapme import SeleniumScraper

# Initialize with automatic geckodriver management
scraper = SeleniumScraper(headless=True)

# Get dynamic content
text = scraper.get_text("https://example.com")
print(text)

# Execute JavaScript
title = scraper.execute_script("return document.title;")
print(f"Page title: {title}")

# Handle infinite scrolling
scraper.scroll_infinite(max_scrolls=5)
```

### Custom Geckodriver Path

```python
from scrapme import SeleniumScraper
import os

# Use custom geckodriver path
driver_path = os.getenv('GECKODRIVER_PATH', '/path/to/geckodriver')
scraper = SeleniumScraper(driver_path=driver_path)
```

### Rate Limiting and Proxy Rotation

```python
from scrapme import WebScraper

# Initialize with rate limiting and proxies
proxies = [
    'http://proxy1.example.com:8080',
    'http://proxy2.example.com:8080'
]

scraper = WebScraper(
    requests_per_second=0.5,  # One request every 2 seconds
    proxies=proxies
)

# Add new proxy at runtime
scraper.add_proxy('http://proxy3.example.com:8080')

# Update rate limit
scraper.set_rate_limit(0.2)  # One request every 5 seconds
```

### Unicode Support (Including Georgian)

```python
from scrapme import WebScraper

# Initialize with Georgian language support
scraper = WebScraper(
    headers={'Accept-Language': 'ka-GE,ka;q=0.9'},
    encoding='utf-8'
)

# Scrape Georgian content
text = scraper.get_text("https://example.ge")
print(text)
```

## Advanced Features

### Content Selection Methods

```python
# Using CSS selectors
elements = scraper.find_by_selector("https://example.com", "div.content > p")

# By class name
elements = scraper.find_by_class("https://example.com", "main-content")

# By ID
element = scraper.find_by_id("https://example.com", "header")

# By tag name
elements = scraper.find_by_tag("https://example.com", "article")
```

### Selenium Wait Conditions

```python
from scrapme import SeleniumScraper

scraper = SeleniumScraper()

# Wait for element presence
soup = scraper.get_soup(url, wait_for="#dynamic-content")

# Wait for element visibility
soup = scraper.get_soup(url, wait_for="#loading", wait_type="visibility")
```

## Error Handling

The package provides custom exceptions for better error handling:

```python
from scrapme import ScraperException, RequestException, ParsingException

try:
    scraper.get_text("https://example.com")
except RequestException as e:
    print(f"Failed to fetch content: {e}")
except ParsingException as e:
    print(f"Failed to parse content: {e}")
except ScraperException as e:
    print(f"General scraping error: {e}")
```

## Best Practices

1. **Rate Limiting**: Always use rate limiting to avoid overwhelming servers:
   ```python
   scraper = WebScraper(requests_per_second=0.5)
   ```

2. **Proxy Rotation**: For large-scale scraping, rotate through multiple proxies:
   ```python
   scraper = WebScraper(proxies=['proxy1', 'proxy2', 'proxy3'])
   ```

3. **Resource Management**: Use context managers or clean up Selenium resources:
   ```python
   scraper = SeleniumScraper()
   try:
       # Your scraping code
   finally:
       del scraper  # Closes browser automatically
   ```

4. **Error Handling**: Always implement proper error handling:
   ```python
   try:
       scraper.get_text(url)
   except ScraperException as e:
       logging.error(f"Scraping failed: {e}")
   ```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

For support, please open an issue on the GitHub repository or contact info@ubix.pro.
