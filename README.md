# Book Scraper

A Scrapy-based web scraper that extracts book information from [Books to Scrape](https://books.toscrape.com/) and stores it in MongoDB.

## Features

- Scrapes book title, price, and URL from all pages
- Automatic pagination handling
- MongoDB storage with duplicate prevention
- Built-in logging and error handling

## Installation

```bash
# Clone and setup
git clone https://github.com/nixAddict/book-scraper.git
cd book-scraper
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
pip install scrapy pymongo scrapy-user-agents

# Setup MongoDB
mongosh
use books_db
db.createCollection("books")
```

## Project Structure

```
book-scraper/
├── books/
│   ├── books/
│   │   ├── spiders/
│   │   │   └── book.py          # Spider definition
│   │   ├── items.py             # Item definitions
│   │   ├── pipelines.py         # Data processing pipelines
│   │   └── settings.py          # Project settings
│   ├── tests/
│   │   ├── test_book.py         # Unit tests
│   │   └── sample.html          # Test fixtures
│   ├── scrapy.cfg               # Scrapy configuration
│   └── book_scraper.log         # Log file
├── LICENSE
└── README.md
```

## Configuration

Key settings in `books/settings.py`:

```python
# MongoDB
MONGO_URI = "mongodb://localhost:27017"
MONGO_DATABASE = "books_db"

# Logging
LOG_LEVEL = "WARNING"
LOG_FILE = "book_scraper.log"

# Politeness
DOWNLOAD_DELAY = 2
CONCURRENT_REQUESTS_PER_DOMAIN = 1
ROBOTSTXT_OBEY = True

# Retry policy
RETRY_TIMES = 3
RETRY_HTTP_CODES = [500, 429]

# User agent rotation
DOWNLOADER_MIDDLEWARES = {
    "scrapy.downloadermiddlewares.useragent.UserAgentMiddleware": None,
    "scrapy_user_agents.middlewares.RandomUserAgentMiddleware": 400,
}
```

## Usage

```bash
# Navigate to project directory
cd books

# Run the spider
scrapy crawl book

# Test the spider
scrapy check book

# Run unit tests
python -m unittest

# Interactive shell for testing selectors
scrapy shell https://books.toscrape.com/
```

## How It Works

The spider extracts data using CSS selectors and stores each book as:

```javascript
{
  "_id": "hash_of_url",     // Unique identifier
  "url": "catalogue/...",   // Book URL
  "title": "Book Title",    // Book title
  "price": "£XX.XX"         // Price
}
```

Duplicates are prevented using SHA-256 hash of the URL.

## Resources

Based on the [Real Python tutorial](https://realpython.com/web-scraping-with-scrapy-and-mongodb/).

- [Scrapy Documentation](https://docs.scrapy.org/)
- [MongoDB Python Driver](https://pymongo.readthedocs.io/)

## License

Educational purposes only. Scrape responsibly.
