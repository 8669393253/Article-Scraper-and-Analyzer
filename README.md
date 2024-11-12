# Article Scraper and Analyzer

This project is a Python script that scrapes articles from a provided URL, searches for related articles using the Google Custom Search API, extracts unique points from the articles, and saves the results into a Word document. The main goal of this script is to gather and summarize relevant and non-repetitive information from articles across the web.

## Features

- **Article Scraping**: Fetches the content of an article from a given URL.
- **Google Custom Search Integration**: Retrieves related articles based on the title of the scraped article using Google's Custom Search API.
- **Text Preprocessing**: Cleans up text by removing punctuation, converting to lowercase, and removing stop words.
- **Unique Sentence Extraction**: Uses TF-IDF (Term Frequency-Inverse Document Frequency) to identify unique, non-repetitive sentences from multiple articles.
- **Word Document Output**: Saves the extracted unique points to a Word document (.docx).

## Requirements

Before running the script, make sure you have the following Python libraries installed:

- requests: For making HTTP requests to fetch web pages and search results.
- BeautifulSoup4 (bs4): For parsing and scraping HTML content.
- sklearn: For TF-IDF vectorization and cosine similarity calculations.
- python-docx: For creating and saving a Word document.
- csv: For saving article summaries to a CSV file.
- re and string: For text preprocessing.
- datetime: For handling timestamps when saving documents.

You can install these dependencies using `pip`:
pip install requests beautifulsoup4 scikit-learn python-docx


## Setup and Configuration

1. **Google API Key and Custom Search Engine ID**:
   - You will need a Google API key and a Google Custom Search Engine ID to use the Google Custom Search API.
   - You can obtain these from the [Google Developer Console](https://console.developers.google.com/).
   - Store the API key and Search Engine ID in the script or use environment variables for better security.

2. **Folder Path**:
   - By default, the script saves the output Word document to a folder located at `D:/Desktop/Unique_Articles`. 
   - You can modify the `custom_folder_path` variable to specify a different directory for the output files.
   
3. **Set Up the Script**:
   - Edit the API_KEY and `SEARCH_ENGINE_ID` in the script to include your credentials.
   - Set the url variable to the article you want to scrape.
   - If needed, change the folder path where the resulting Word document will be saved by modifying the `custom_folder_path` variable.

## How It Works

### 1. **Fetching the Article**:
   The script first scrapes the content of an article from the provided URL. It attempts to extract the main body of the article by finding all `<p>` tags, which are typically used for paragraph text. This content is then cleaned and saved.

### 2. **Google Custom Search**:
   The script uses the title of the article to search for related articles using the Google Custom Search API. It sends a request with the article title as a query and retrieves the first set of search results.

### 3. **Extracting Content from Related Articles**:
   For each related article found, the script fetches its content and extracts the main article text. It filters out any irrelevant sections such as ads, footers, or navigation menus.

### 4. **Preprocessing the Text**:
   The text from all articles is preprocessed by converting it to lowercase and removing punctuation. This helps standardize the text for further analysis.

### 5. **Identifying Unique Sentences**:
   The script splits the text of each article into individual sentences and uses the TF-IDF (Term Frequency-Inverse Document Frequency) algorithm to convert the sentences into vectors. The cosine similarity between each sentence and all other sentences is computed. Sentences that are too similar (based on a defined threshold) are discarded, leaving only unique, non-repetitive sentences.

### 6. **Saving to a Word Document**:
   The unique sentences are saved into a Microsoft Word document using the `python-docx` library. If a document with the same name already exists, the script appends a timestamp to the filename to ensure the new document does not overwrite the existing one.

### 7. **Saving Article Summary to CSV**:
   The title and a brief summary (first three paragraphs) of the original article are saved to a CSV file called `article_summary.csv`.

## Code Overview

### Functions

#### 1. get_search_results(query, api_key, search_engine_id)
   - **Purpose**: Sends a search query to the Google Custom Search API and retrieves a list of related articles.
   - **Parameters**:
     - query: The search query (typically the title of the article).
     - api_key: Your Google API key.
     - search_engine_id: Your Custom Search Engine ID.
   - **Returns**: A list of search result items, each containing a link to a related article.

#### 2. fetch_article_content(url)
   - **Purpose**: Fetches the content of an article from a given URL.
   - **Parameters**:
     - url: The URL of the article to scrape.
   - **Returns**: The main body content of the article as a string.

#### 3. preprocess_text(text)
   - **Purpose**: Preprocesses the text by removing punctuation and converting to lowercase.
   - **Parameters**:
     - text: The input text to be preprocessed.
   - **Returns**: The cleaned text.

#### 4. `extract_unique_points(articles)`
   - **Purpose**: Extracts unique sentences from a list of articles by calculating cosine similarity.
   - **Parameters**:
     - articles: A list of article contents.
   - **Returns**: A list of unique, non-repetitive sentences.

#### 5. `save_to_word_document(unique_points, folder_path)`
   - **Purpose**: Saves the unique points to a Word document.
   - **Parameters**:
     - unique_points: A list of unique sentences.
     - folder_path: The folder where the Word document should be saved.
   - **Returns**: None. The document is saved to the specified folder.

### Main Script Execution

1. The URL of the article to scrape is provided.
2. The article is fetched, and the first three paragraphs are saved to a CSV file.
3. The script searches for related articles based on the article title.
4. The content of related articles is scraped and processed.
5. Unique points are extracted from the content using cosine similarity.
6. The unique points are saved to a Word document in the specified folder.

## Example Usage

1. **Running the Script**:
   - Ensure that you have installed the required libraries and configured your API credentials.
   - Set the `url` and `custom_folder_path` variables in the script.
   - Run the script:
     python article_scraper.py
   

2. **CSV Output**:
   - The script will save a file called `article_summary.csv` with the title and summary of the article.

3. **Word Document Output**:
   - The script will create a `.docx` file with unique points from the articles, saved in the folder you specified.

## Error Handling

- **Network Issues**: If there's a problem fetching a URL (e.g., the website is down), the script will print an error message and skip that article.
- **API Errors**: If there’s an issue with the Google Custom Search API (e.g., invalid API key), the script will print the error and stop searching for related articles.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### Conclusion

This script is a useful tool for scraping, analyzing, and summarizing articles from the web. It integrates Google Custom Search to find related content, processes the text to remove redundancies, and outputs unique insights into a Word document for easy sharing and analysis.
