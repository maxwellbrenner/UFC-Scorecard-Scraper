# UFC Scorecard Scraper

This program is designed to scrape event data and scorecards from the UFC website, downloading images of the scorecards, and compiling them into PDF files. It automatically filters and organizes results, processes images, and enables manual creation of PDFs if necessary.

## Function Descriptions

### 1. `extract_url_text_to_compare_previous_link(url)`  
 - **Description**: Extracts the unique identifier from the URL by focusing on the part between `'ufc'` and `'vs'`. This helps detect redundant entries in the URLs.  
 - **Returns**: The unique identifier string if found, otherwise `None`.

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

---

## Program Overview

1. **Collects Scorecard Data**: Scrapes the UFC results page for event links.
2. **Processes Scorecard Links**: Identifies scorecard links, downloads images, and compiles them into PDFs.
3. **File Management**: Organizes images into folders and identifies missing PDFs.
4. **Manual PDF Creation**: Allows for manual PDF creation if automated creation fails.

---

## Execution Flow

### 1. Starting the Data Collection

The program starts by collecting event result links using the `find_results_links(url)` function, which navigates through the UFC results page, filtering out irrelevant links (such as weigh-ins or bonus announcements).

![UFC Scorecard - Results Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Results%20Page.png)

- **Description**: The results page lists various UFC events with links to detailed results pages.
- **Function**: `find_results_links()` identifies and stores links to relevant fight events.

---

### 2. Collecting Event Scorecards

Once the program has a list of event links, it uses `find_event_scorecard_links(results_links)` to identify links to each event's official scorecard. This function helps filter down to pages that contain the actual scorecard details.

![UFC Scorecard - Load More](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Load%20More.png)

- **Description**: The "Load More" button allows access to more results on the UFC website, enabling pagination for larger datasets.
- **Function**: The script handles multiple pages, using the "Load More" functionality to collect all relevant links.

---

### 3. Processing Event Pages

The program then processes each event link to find scorecards. If an explicit scorecard link is available, it is saved directly; otherwise, the program visits the page to find links to the "Official Scorecard".

![UFC Scorecard - Event Row Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Event%20Row%20Page.png)

- **Description**: Each row on the event page represents a different event, allowing users to see both main and preliminary results.
- **Function**: This helps the scraper identify specific event rows containing scorecard information.

---

### 4. Downloading and Compiling Scorecard Images

Once scorecard links are identified, `download_images_from_event_scorecard_link_and_create_pdf()` downloads images for each event and compiles them into a PDF.

![UFC Scorecard - Event Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Event%20Page.png)

- **Description**: This is the main event page, showing the overall fight card and main event information.
- **Function**: This allows the scraper to access specific scorecards and download images from this page.

---

### 5. Official Scorecard Link and Fight Scorecard

When accessing specific fights, the scraper identifies and follows links labeled "Official Scorecard," which contains the details for each fight's score.

![UFC Scorecard - Official Scorecard Link](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Official%20Scorecard%20Link.png)

- **Description**: Links like this take users to the official scorecard for a fight, providing detailed scoring information.
- **Function**: This link ensures the scraper collects official data and avoids duplicate or irrelevant links.

---

### 6. Example of a Scorecard

The program downloads and processes individual scorecard images, compiling them into PDFs for record-keeping and analysis.

![UFC Scorecard - Fight Scorecard](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Fight%20Scorecard.png)

- **Description**: Each fight scorecard displays the judges' scores round-by-round for both fighters.
- **Function**: This image provides the basis for generating a complete scorecard PDF for each event.

---

## Key Functions

1. **extract_url_text_to_compare_previous_link(url)** - Extracts unique identifiers from URLs to prevent duplicate entries.
2. **find_results_links(url)** - Collects links to each UFC event result page.
3. **find_event_scorecard_links(results_links)** - Identifies links to the official scorecards for each event.
4. **download_images_from_event_scorecard_link_and_create_pdf(event_scorecard_link, event_index)** - Downloads scorecard images and compiles them into PDFs.
5. **find_folders_without_pdfs(parent_directory)** - Checks each folder for missing PDFs.

---

## Manual Intervention

If a PDF fails to compile, the program provides a manual function `create_pdf_from_images(folder_path)` that creates a PDF from any images within a specified folder. This ensures data completeness even if the automated process encounters issues.

---

This documentation provides an overview of the UFC Scorecard Scraper, detailing its functionality, data structure, and key workflows for scraping and organizing UFC event data.
