# University Data Scraper

A comprehensive web scraping tool that extracts university and course information from IDP.com, including program details, entry requirements, scholarships, and funding information.

## Features

- **Comprehensive Data Extraction**: Scrapes university overviews, world rankings, entry requirements, and program details
- **Scholarship Information**: Automatically discovers and extracts scholarship details with pagination support
- **Smart Caching**: Implements university-level caching to avoid redundant requests and improve efficiency
- **Robust Error Handling**: Features exponential backoff retry mechanism for failed requests
- **Rate Limiting**: Built-in delays to respect website resources and avoid being blocked
- **Structured Output**: Saves data in organized JSON format grouped by country and university
- **Comprehensive Logging**: Detailed logging of all scraping activities and errors

## Prerequisites

- Python 3.7+
- Required Python packages (see Installation section)

##  Installation

1. **Clone or download the script**
   ```bash
   git clone https://github.com/malikwassay/Data-Scrapping-Script-IDP
   cd university-scraper
   ```

2. **Install required dependencies**
   ```bash
   pip install requests beautifulsoup4 lxml
   ```

   Or using requirements.txt (create this file):
   ```bash
   pip install -r requirements.txt
   ```

## Dependencies

```txt
requests>=2.25.1
beautifulsoup4>=4.9.3
lxml>=4.6.3
```

## Usage

### Basic Usage

Simply run the script:
```bash
python scraper.py
```

The script will:
1. Start scraping from page 1 of IDP.com's course listings
2. Extract university and course information
3. Process scholarship data with automatic pagination
4. Save results to `universities_data.json`
5. Log all activities to `scraping.log`

### Configuration

You can modify these variables at the top of the script:

```python
BASE_URL = "https://www.idp.com/find-a-course/"  # Base URL for scraping
Base_page = 1  # Starting page number
```

## Output Format

### JSON Structure

The script generates a `universities_data.json` file with the following structure:

```json
{
  "Country Name": {
    "University Name": {
      "Overview": "University description and details",
      "World Ranking": "THE World Ranking position",
      "Entry Requirements": {
        "details": ["List of entry requirements"]
      },
      "ScholarShips & Funding": "Scholarship and funding information",
      "Programs": [
        {
          "course_title": "Program name",
          "course_detail": "Detailed program description",
          "qualification": "Degree type",
          "duration": "Course duration",
          "next_intake": "Next enrollment date",
          "entry_score": "Required entry score",
          "course_fee": "Tuition fees",
          "how_to_apply": "Application requirements"
        }
      ],
      "available_scholarships": [
        {
          "title": "Scholarship name",
          "qualification": "Eligible qualifications",
          "funding_type": "Type of funding",
          "funding_details": "Funding amount and details",
          "deadline": "Application deadline",
          "eligible_intake": "Eligible intake periods",
          "study_mode": "Full-time/Part-time"
        }
      ]
    }
  }
}
```

## Logging

The script creates a `scraping.log` file that tracks:
- Request attempts and failures
- Cache hits and misses
- Processing progress
- Error details and retry attempts
- Sleep intervals and rate limiting

Log format:
```
2024-01-01 12:00:00 - INFO - Opening Base URL Page number 1
2024-01-01 12:00:05 - WARNING - Request failed (Attempt 1/5): Connection timeout
2024-01-01 12:00:07 - INFO - [CACHE HIT] Using cached data for University Name
```

## Key Functions

### Core Functions

- **`make_request()`**: Handles HTTP requests with retry logic and exponential backoff
- **`process_university()`**: Main function for processing individual university pages
- **`get_scholarship_links()`**: Discovers and processes scholarship pages with pagination
- **`scrape_universities()`**: Main orchestration function

### Data Extraction Functions

- **`parse_course_info()`**: Extracts basic course information
- **`get_entry_requirements()`**: Retrieves entry requirement details
- **`get_university_details()`**: Extracts university-specific information
- **`process_scholarship_page()`**: Processes individual scholarship pages

## Performance Features

### Caching System
- **University-level caching**: Avoids re-scraping university overview data
- **Scholarship deduplication**: Prevents duplicate scholarship entries
- **Program merging**: Combines new programs with existing university data

### Rate Limiting
- **15-second delays** between page requests
- **7-second delays** after processing scholarship batches
- **Exponential backoff** for failed requests (up to 5 retries)

## Important Considerations

### Legal and Ethical Usage
- **Respect robots.txt**: Check the website's robots.txt file
- **Rate limiting**: Built-in delays help avoid overwhelming the server
- **Terms of service**: Ensure compliance with IDP.com's terms of service
- **Data usage**: Use scraped data responsibly and in accordance with applicable laws

### Technical Limitations
- Script processes approximately **1,900 pages** of course listings
- **Large datasets**: Final JSON file can be several MB depending on data volume
- **Network dependent**: Requires stable internet connection
- **Memory usage**: Large datasets may require significant RAM

## Troubleshooting

### Common Issues

1. **Connection Timeouts**
   - Script includes automatic retry with exponential backoff
   - Check your internet connection
   - Consider increasing timeout values in `make_request()`

2. **Large File Sizes**
   - JSON file can become very large with extensive data
   - Consider implementing data chunking for very large datasets

3. **Rate Limiting Blocks**
   - Increase sleep intervals if getting blocked
   - Modify delays in the main scraping loop

4. **Memory Issues**
   - For very large datasets, consider processing in smaller batches
   - Implement periodic data flushing to disk

### Debugging

Enable more verbose logging by modifying the logging configuration:
```python
logging.basicConfig(
    level=logging.DEBUG,  # Changed from INFO
    format="%(asctime)s - %(levelname)s - %(message)s"
)
```

## Monitoring Progress

The script provides several ways to monitor progress:

1. **Console output**: Real-time page and scholarship processing updates
2. **Log file**: Detailed activity tracking in `scraping.log`
3. **JSON file**: Incremental updates to `universities_data.json`

## Future Enhancements

Potential improvements could include:
- Database storage instead of JSON files
- Multi-threading for faster processing
- Web interface for monitoring progress
- Data validation and quality checks
- Export to different formats (CSV, Excel)
- Resume functionality for interrupted scraping


**Note**: This scraper is designed to work with IDP.com's current website structure as of the development date. Website changes may require script modifications.
