# UFC Scorecard Scraper
The script provided is part of a comprehensive system designed to automate the extraction and organization of UFC event data, particularly focusing on fight results and judge scorecards. The main objective is to capture round scores and outcomes from various UFC events, which are then stored for further analysis and used to develop predictive models like the Expected Round Calculator.

The script begins by defining a series of functions aimed at navigating through the UFC's official results webpage to systematically gather links to individual event scorecards. Utilizing Python's `requests()` library to fetch webpage content and `BeautifulSoup()` from the `bs4()` package to parse this content, the script identifies relevant links by examining the text and href attributes of each link element. The `find_results_links()` function initiates this process by collecting all unique event result links from the UFC results page, filtering out unnecessary links such as those related to weigh-ins or bonuses that do not contain actual fight results.

Once the relevant links are compiled, the script employs the `find_event_scorecard_links()` function to further narrow down the search to specific pages containing the official judges' scorecards. This function enhances accuracy by checking each link against a unique identifier extracted from the URL, ensuring that no duplicate or irrelevant links are processed. This is particularly important for maintaining a clean dataset, free from redundant entries that could skew analytical outcomes.

To refine the list of links, the `remove_redundant_links()` function is used to eliminate any links that are deemed superfluous or incorrectly captured in earlier steps. This includes links that may have been mistakenly identified as unique due to slight variations in URL structure but ultimately lead to the same content. Additionally, manually identified links that were missed during the automated scraping process are appended through the `add_manual_event_scorecard_links()` function, ensuring completeness of the data collection.

Finally, the script transitions from data collection to processing by downloading images of the scorecards from each collected link, converting these images into a single PDF file per event. This is facilitated through the `download_images_from_event_scorecard_link_and_create_pdf()` function, which not only downloads the scorecard images but also organizes them into appropriately named directories based on the event details extracted from the URL. This systematic file management aids in the later retrieval and analysis of data, forming a structured archive of UFC events that can be efficiently accessed and processed for various analytical purposes.

Overall, the script is a critical component of a larger data-driven framework aimed at enhancing the predictive accuracy and strategic analysis of UFC fights. By automating the collection and organization of fight data, the script supports deeper statistical analysis and model building efforts, like the Expected Round Calculator, which would utilize the meticulously compiled scorecards to predict fight outcomes based on real-time in-fight statistics.

Once the scorecards have been appended into PDF format, these files can be fed into an OCR engine (Tesseract) to extract all pertinent scorecard information and write it to a CSV, which can then we compiled into a dataframe and analyzed.

---

## Overview

This project achieves the following:
- **Automated Data Collection**: Scraping of UFC results pages to collect links to individual event scorecards.
- **Data Filtering and Organization**: Systematic filtering and organization of relevant links, avoiding redundancy and capturing only essential data.
- **Data Archiving**: Downloading and storing images of judge scorecards, converting them into organized PDF files per event.

## Project Flow

The project flow consists of several steps:
1. **Data Collection**: Extract relevant event links from the UFC's official results page.
2. **Link Filtering**: Filter out irrelevant links and remove redundancy, maintaining a clean dataset.
3. **Scorecard Download and PDF Creation**: Capture judge scorecard images and organize them into a structured archive.

---
