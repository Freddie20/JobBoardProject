# JobBoardProject

## 🕸️ Web Scraping Job Listings

### Project Overview

This project is a collection of Python scripts used for scraping job listings from various websites. The primary goal was to extract job titles, locations, and URLs, despite the challenges presented by dynamically loaded content, complex HTML structures, and inconsistent website layouts.

### Technologies Used

- **Python**: The core programming language used for the scraping scripts.
- **Selenium**: For automating browser interactions and handling dynamically loaded content.
- **BeautifulSoup**: For parsing HTML and extracting data.
- **XPath & CSS Selectors**: For precise and flexible data extraction from HTML elements.

### Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/your-repository-name.git
   cd your-repository-name
   ```

2.	**Install the required packages:
Make sure you have Python installed. Then, install the necessary packages using pip:
```bash
pip install -r requirements.txt
```

3.	**Set up the ChromeDriver:
Download the appropriate version of ChromeDriver for your Chrome browser and place it in the specified directory. Update the driver_path variable in the scripts with your actual path:
```bash
driver_path = "/Users/....../...../chromedriver"
```
4.	**Run the scraping scripts:
Run the scripts to start scraping job listings from the target websites. Each script targets a different website and can be customized as needed:
```bash
python scrape_site_1.py
python scrape_site_2.py
```


