# UFC Scorecard Scraper

The UFC Scorecard Scraper collects official event scorecards from the UFC website, downloading scorecard images and organizing them by event. It compiles these images into PDFs for each event, allowing the images to be easily reviewed or parsed with an OCR engine.

![UFC Scorecard - Results Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Results%20Page.png)

## Program Overview

1. **Collects Scorecard Data**: Scrapes the UFC results page for event links.
2. **Processes Scorecard Links**: Identifies scorecard links, downloads images, and compiles them into PDFs.
3. **File Management**: Organizes images into folders and identifies missing PDFs.
4. **Manual PDF Creation**: Allows for manual PDF creation if automated creation fails.

---

## Overview of the `main()` Function

The `main()` function orchestrates the scraping process by following these steps:

1. **Collect Event Links**: The `find_results_links()` function gathers links to recent UFC events by scraping the UFC results page. It filters out links irrelevant to fight results (like weigh-ins) and handles pagination to access all available events.

2. **Identify Scorecard Links**: For each event, `find_event_scorecard_links()` extracts links to the official scorecards. If no direct link to scorecards is available, it searches the event page to locate any embedded "official scorecard" links.

3. **Download and Compile Scorecard Images**: The `download_images_from_event_scorecard_link_and_create_pdf()` function processes each scorecard link, downloading images and compiling them into a PDF, named by event. Images are saved in folders structured by event for easy navigation.

4. **Check for Missing PDFs**: After processing, the `find_folders_without_pdfs()` function identifies any event folders missing PDFs, ensuring completeness in case of download errors.

5. **Manual PDF Creation**: If any images are missing PDFs, `create_pdf_from_images()` manually generates a PDF from all images in a specified folder, providing a fallback if the automated process encounters issues.

By the end of `main()`, the program has organized and stored scorecards from multiple events, ensuring a comprehensive dataset of official UFC scorecards.

---

## Function Descriptions -- Ordered by Execution in `main()`

### 1. `sanitize_filename(filename)`
- **Description**: Cleans a filename by removing special characters and truncating it to 50 characters to ensure compatibility with different file systems.
- **Returns**: A sanitized string that can be used as a valid filename.

### 2. `extract_url_text_to_compare_previous_link(url)`  
 - **Description**: Extracts the unique identifier from the URL by focusing on the part between `'ufc'` and `'vs'`. This helps detect redundant entries in the URLs.  
 - **Returns**: The unique identifier string if found, otherwise `None`.

### 3. `extract_unique_identifier(url)`  
 - **Description**: Extracts a broader identifier from the UFC event URL using the section after `'/news/'`. Helps in identifying URLs with varied structures to ensure data completeness.  
 - **Returns**: The unique identifier string if found, otherwise `None`.

### 4. `find_results_links(url)`  
 - **Description**: Scrapes the UFC results page to collect event links, filtering out non-result pages like weigh-ins or bonus announcements. Uses pagination to gather links until no more pages are available.  
 - **Returns**: A list of URLs linking to individual event results pages.

### 5. `find_event_scorecard_links(results_links)`  
 - **Description**: Processes each event link to identify scorecard links. If no explicit scorecard link exists, it visits the page and finds any "official scorecard" links within.  
 - **Returns**: A list of URLs that contain scorecards for each event.

### 6. `remove_redundant_links(event_scorecard_links)`  
 - **Description**: Cleans up the scorecard links list by removing duplicates or links with unnecessary query parameters. This function is critical to ensure clean, non-redundant data.  
 - **Returns**: Two lists — one with cleaned links and another with redundant links removed.

### 7. `add_manual_event_scorecard_links(existing_links, manual_links)`  
 - **Description**: Adds manually identified scorecard links that may have been missed in automated scraping, ensuring each link is added only once.  
 - **Returns**: A combined list of existing and manual links, avoiding duplicates.

### 8. `parse_folder_name_from_event_scorecard_link(event_scorecard_link)`  
 - **Description**: Generates a valid folder name from a scorecard link by converting hyphens to underscores and removing invalid characters.  
 - **Returns**: A formatted folder name string.

### 9. `download_images_from_event_scorecard_link_and_create_pdf(event_scorecard_link, event_index)`  
 - **Description**: Scrapes and downloads images from a given scorecard link, stores them in a specific folder, and compiles the images into a PDF.  
 - **Returns**: Creates a folder with images and a PDF file for the event scorecard if successful.

### 10. `image_processing(event_scorecard_links, starting_event_number=1, ending_event_number=None)`
- **Description**: Processes a specified range of event scorecard links by downloading images and creating PDFs for each. The starting and ending parameters allow for processing a subset of events as needed.
- **Returns**: Downloads images and creates PDFs within designated folders for each processed event.

### 11. `find_folders_without_pdfs(parent_directory)`  
 - **Description**: Checks each subdirectory within a parent directory and identifies folders that do not contain a PDF file.  
 - **Returns**: A list of folder names without PDFs.

### 12. `create_pdf_from_images(folder_path)`  
 - **Description**: Creates a PDF file from all `.jpg` or `.jpeg` images within a specified folder. Used to manually compile PDFs if the automated process fails.  
 - **Returns**: A PDF file if images exist, otherwise outputs an error message.
---

## Execution Flow

The `main()` function initiates the scraping process, collecting event links, identifying scorecard links, downloading images, and creating PDFs. Finally, it verifies completeness by checking each folder for a corresponding PDF. This function ensures the entire workflow runs smoothly from data collection to output organization.

### 1. Collect Event Links

The scraper begins by retrieving event links from the UFC results page using `find_results_links()`. It filters out non-fight links (such as weigh-ins or bonus announcements) and processes pagination to ensure comprehensive data collection.

![UFC Scorecard - Results Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Results%20Page.png)

- **Description**: The results page lists completed UFC events, each linking to individual event pages.
- **Function**: `find_results_links()` collects links to relevant fight events.

---

### 2. Identify Scorecard Links

With event links gathered, the program uses `find_event_scorecard_links()` to locate links to official scorecards. For events without direct scorecard links, it parses the event pages to extract embedded "official scorecard" links.

![UFC Scorecard - Event Row Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Event%20Row%20Page.png)
![UFC Scorecard - Load More](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Load%20More.png)

- **Description**: The "Load More" button on the UFC results page allows for accessing older events, facilitating data retrieval for larger datasets.
- **Function**: `find_event_scorecard_links()` gathers all event-specific scorecard links.

---

### 3. Process Event Pages for Scorecards

The program processes each event page to verify scorecard links. If a scorecard link is available, it is saved directly; otherwise, the program scans the page for an "Official Scorecard" link.

![UFC Scorecard - Event Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Event%20Page.png)

- **Description**: Each event row on the event page allows for accessing main and preliminary fight results.
- **Function**: The scraper identifies specific scorecards for each event.

---

### 4. Download and Compile Scorecard Images

Using the scorecard links, `download_images_from_event_scorecard_link_and_create_pdf()` downloads each event’s scorecard images and compiles them into a PDF.

- **Description**: The scorecard images are stored in organized folders by event, ensuring easy navigation and future reference.
- **Function**: The scraper downloads images and compiles a PDF for each event’s scorecard.

---

### 5. Verify Scorecards and Create Missing PDFs

Once all scorecard images are downloaded, `find_folders_without_pdfs()` checks each folder for PDFs, identifying any events that may be missing them. `create_pdf_from_images()` can then manually generate PDFs from available images.

- **Description**: This step verifies that all scorecards have PDFs for easy access and analysis.
- **Function**: Ensures every event has a corresponding scorecard PDF, with manual creation for any missing PDFs.

---

## Example of an Event Scorecard

The scraper collects individual scorecard images and compiles them into PDFs for each event, such as the one shown below:

![UFC Scorecard - Fight Scorecard](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Fight%20Scorecard.png)

- **Description**: Each fight scorecard details the judges' scores for each round.
- **Function**: These images provide the basis for comprehensive scorecard PDFs for each event.

---

