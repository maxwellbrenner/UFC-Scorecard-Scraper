# UFC Scorecard Scraper

![UFC Scorecard - Results Page](https://github.com/maxwellbrenner/UFC-Scorecard-Scraper/raw/main/images%20(UFC.com)/UFC%20Scorecard%20-%20Results%20Page.png)

## Project Functionality

This project provides software to scrape official UFC event scorecard images from the [UFC](https://www.ufc.com/) website. It organizes these images by event, downloads them, and compiles them into PDFs for each event. These images/PDFs can then be fed into a OCR engine (Tesseract) to parse round scores for each of the three judges.

The script is unique in the sense that all scorecard scraping software I've seen parses round scores from [MMADecisions](https://mmadecisions.com/), which only archives fights that go Decision, and does not provide information on fights that end prematurely. 

Even in the case of an early stoppage, all completed rounds are scored by all three judges. Several years ago, the UFC began publishing these scorecards on their website as image files. Though it is much more difficult to parse text from an image than from HTML, this approach nearly doubles the data set (# of completed rounds scored). 

The script is structured to capture links to events, navigate to scorecards, filter out unnecessary content, and manage pagination on the results page. It has several modular functions to ensure effective navigation and data organization.

This project enables users to collect, organize, and review official UFC scorecards efficiently, whether for analysis or personal interest.

### 1. Event Results and Scorecards

The program begins by visiting the [UFC Results Page](https://www.ufc.com/results?language_content_entity=en), gathering the list of recent events. It extracts:
- **Event Link**: URL to each event's main result page.
- **Event Filter**: Processes links to ensure only unique and relevant scorecards are captured.

Each event's page is then accessed to collect specific scorecard links, bypassing pages for weigh-ins, bonus announcements, and other non-scorecard-related content.

### 2. Image Downloading and PDF Creation

For each scorecard link, the program downloads official UFC scorecard images:
- **Image Filtering**: Selects images hosted on UFC’s CDN (dmxg5wxfqgb4u.cloudfront.net) for accuracy.
- **PDF Compilation**: All downloaded images for each event are compiled into a single PDF. The file is named based on the event details, and the PDFs are stored in a structured directory.

### 3. Redundancy and Manual Link Handling

To improve data accuracy, the script:
- **Removes Redundant Links**: Filters out unnecessary and duplicate URLs by comparing link identifiers.
- **Adds Manual Links**: Includes any official scorecard links not automatically captured in the scraping process, providing a comprehensive set of scorecards.

### 4. Folder Checking and PDF Creation for Existing Images

To maintain a complete set of PDFs, the script checks for folders without a PDF and creates one if necessary:
- **Folder Verification**: Identifies folders missing a PDF file.
- **Image PDF Creation**: Compiles available images within each folder into a PDF if one does not already exist.

### Requirements

The script requires the following Python packages:
- `requests`: For HTTP requests to the UFC Results page.
- `BeautifulSoup` from `bs4`: To parse and extract HTML content.
- `re`: For regular expression-based URL filtering.
- `os`: For managing local directories and file paths.
- `img2pdf`: To compile downloaded images into PDFs.

### Folder Structure and File Naming

- **Event Folders**: Each event has a designated folder (`event_[number]_[event_name]`) within `ufc_images/`, organizing images by event.
- **PDF Files**: PDFs for each event are named with the event number and name
    - e.g. `25_official_judges_scorecards_ufc_fight_night_allen_vs_curtis_2.pdf`

### Notes

This script is designed to handle any future UFC events published on [UFC.com](https://www.ufc.com/). It can be updated with new manual links if needed to ensure a comprehensive collection.

