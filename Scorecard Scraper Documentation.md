# UFC Scorecard Scraper

This program is designed to scrape event data and scorecards from the UFC website, downloading images of the scorecards, and compiling them into PDF files. It automatically filters and organizes results, processes images, and enables manual creation of PDFs if necessary.

## Function Descriptions

### 1. `extract_url_text_to_compare_previous_link(url)`  
   **Description**: Extracts the unique identifier from the URL by focusing on the part between `'ufc'` and `'vs'`. This helps detect redundant entries in the URLs.  
   **Returns**: The unique identifier string if found, otherwise `None`.

### 2. `extract_unique_identifier(url)`  
   **Description**: Extracts a broader identifier from the UFC event URL using the section after `'/news/'`. Helps in identifying URLs with varied structures to ensure data completeness.  
   **Returns**: The unique identifier string if found, otherwise `None`.

### 3. `find_results_links(url)`  
   **Description**: Scrapes the UFC results page to collect event links, filtering out non-result pages like weigh-ins or bonus announcements. Uses pagination to gather links until no more pages are available.  
   **Returns**: A list of URLs linking to individual event results pages.

### 4. `find_event_scorecard_links(results_links)`  
   **Description**: Processes each event link to identify scorecard links. If no explicit scorecard link exists, it visits the page and finds any "official scorecard" links within.  
   **Returns**: A list of URLs that contain scorecards for each event.

### 5. `remove_redundant_links(event_scorecard_links)`  
   **Description**: Cleans up the scorecard links list by removing duplicates or links with unnecessary query parameters. This function is critical to ensure clean, non-redundant data.  
   **Returns**: Two lists — one with cleaned links and another with redundant links removed.

### 6. `add_manual_event_scorecard_links(existing_links, manual_links)`  
   **Description**: Adds manually identified scorecard links that may have been missed in automated scraping, ensuring each link is added only once.  
   **Returns**: A combined list of existing and manual links, avoiding duplicates.

### 7. `parse_folder_name_from_event_scorecard_link(event_scorecard_link)`  
   **Description**: Generates a valid folder name from a scorecard link by converting hyphens to underscores and removing invalid characters.  
   **Returns**: A formatted folder name string.

### 8. `download_images_from_event_scorecard_link_and_create_pdf(event_scorecard_link, event_index)`  
   **Description**: Scrapes and downloads images from a given scorecard link, stores them in a specific folder, and compiles the images into a PDF.  
   **Returns**: Creates a folder with images and a PDF file for the event scorecard if successful.

### 9. `find_folders_without_pdfs(parent_directory)`  
   **Description**: Checks each subdirectory within a parent directory and identifies folders that do not contain a PDF file.  
   **Returns**: A list of folder names without PDFs.

### 10. `create_pdf_from_images(folder_path)`  
   **Description**: Creates a PDF file from all `.jpg` or `.jpeg` images within a specified folder. Used to manually compile PDFs if the automated process fails.  
   **Returns**: A PDF file if images exist, otherwise outputs an error message.

## Program Execution Flow

### 1. **Start Data Collection with `find_results_links(url)`**  
   The script starts by collecting event result links using the `find_results_links()` function, which navigates the UFC results page to gather relevant URLs.

### 2. **Collect and Filter Scorecard Links with `find_event_scorecard_links(results_links)`**  
   Each event result link is processed by `find_event_scorecard_links()` to identify the official scorecard links, either directly or by searching within the page.

### 3. **Remove Redundant Links**  
   After collecting the scorecard links, `remove_redundant_links()` filters out duplicates, ensuring only unique links are retained.

### 4. **Add Manual Links**  
   Any missed scorecards are added using `add_manual_event_scorecard_links()`, ensuring complete data collection.

### 5. **Download Scorecard Images and Create PDFs**  
   The `download_images_from_event_scorecard_link_and_create_pdf()` function downloads each event’s scorecard images and compiles them into a PDF.

6. **Identify Folders Missing PDFs**  
   `find_folders_without_pdfs()` scans the folders to identify any missing PDFs, highlighting cases where the automated process may have failed.

7. **Manually Compile PDFs (if needed)**  
   If automated PDF creation fails, `create_pdf_from_images()` can be used to compile images from specified folders manually.


## Starting Point of the Program

## UFC Scorecard Images

![UFC Scorecard - Results Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Results%20Page.png)
![UFC Scorecard - Load More](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Load%20More.png)
![UFC Scorecard - Event Row Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Event%20Row%20Page.png)
![UFC Scorecard - Event Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Event%20Page.png)
![UFC Scorecard - Official Scorecard Link](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Official%20Scorecard%20Link.png)
![UFC Scorecard - Fight Scorecard](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Fight%20Scorecard.png)
