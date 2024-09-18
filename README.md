# Sentiment Analysis and Text Readability Analysis

This project performs sentiment analysis and readability analysis on text data extracted through web scraping. The analysis includes cleaning the data, deriving sentiment metrics, and calculating readability indices such as word counts, syllable counts, and Fog Index.

## Table of Contents
- [Problem Statement](#problem-statement)
- [Approach](#approach)
- [Solution](#solution)
- [Technologies Used](#technologies-used)
- [How to Run the Script](#how-to-run-the-script)
- [Project Structure](#project-structure)
- [Challenges Faced](#challenges-faced)
- [Future Improvements](#future-improvements)
- [License](#license)

## Problem Statement

The task involves scraping data from a list of URLs, processing the extracted text, and performing sentiment and readability analysis on the cleaned data. The goal is to derive useful insights such as sentiment polarity, subjectivity, and various readability metrics.

## Approach

### STEP 1: **Web Scraping**
1. **Data Extraction**: I used the `Requests` and `BeautifulSoup` libraries to scrape the data from URLs specified in `NLP_task/Provided_data/Input.xlsx`. The scraped text was preprocessed by:
   - Removing numbers, URLs, and emails.
   - Correcting spelling using `TextBlob`.
2. **Data Storage**: The preprocessed data was stored as a CSV file: `NLP_task/Web_scraping/web_scraped_data.csv`.

### STEP 2: **Sentiment Analysis**
1. **Stopwords Handling**: Combined all provided stopword lists into a single set and removed them from the text.
2. **Word Lists for Sentiment**: Created dictionaries for positive and negative words from the provided text files (`positive-words.txt`, `negative-words.txt`).
3. **Sentiment Metrics**:
   - **Positive/Negative Scores**: Counted occurrences of positive and negative words.
   - **Polarity Score**: `(positive_score - negative_score) / (positive_score + negative_score + 0.000001)`
   - **Subjectivity Score**: `(positive_score + negative_score) / (words_count + 0.000001)`
4. **Output**: Appended these values to the final dataset and saved it as `NLP_task/Sentimental_Analysis/Output_Data_Structure.csv`.

### STEP 3: **Readability Analysis**
1. **Sentence Count**: Split the text into sentences using `spacy` and counted the number of sentences.
2. **Word Count**: Removed punctuation, stopwords, and counted the number of words.
3. **Average Word Length**: Calculated as `total_characters / total_words`.
4. **Syllable Count**: Counted the number of syllables per word using a custom function. Stored syllable counts as a JSON object.
5. **Complex Words**: Counted words with more than two syllables.
6. **Personal Pronouns**: Counted occurrences of personal pronouns using regular expressions.
7. **Readability Metrics**:
   - **Average Sentence Length**: `total_words / total_sen`
   - **Percentage of Complex Words**: `complex_words / total_words`
   - **Fog Index**: `0.4 * (Average_Sentence_Length + Percentage_of_Complex_words)`

The output of the readability analysis was appended to the dataset and saved as `NLP_task/Task_2_to_8/final_output.csv`.

## Solution

1. **Web Scraping and Preprocessing**: Scraped the text from the URLs, removed unwanted elements (e.g., numbers, emails), and corrected the spelling.
2. **Sentiment Analysis**: Removed stopwords, calculated positive/negative scores, and derived polarity and subjectivity scores.
3. **Readability Analysis**: Extracted text features such as sentence count, syllable count, and calculated readability metrics.
4. **Final Output**: Saved the final output with all derived metrics into CSV files.

## Technologies Used

- **Data Extraction**:
  - `Requests`
  - `BeautifulSoup`
  
- **Data Processing and Sentiment Analysis**:
  - `pandas`
  - `TextBlob`
  - `nltk`
  
- **Readability Analysis**:
  - `spacy`
  - `nltk`
  - `re`
  - `tensorflow`
  
## How to Run the Script

### Prerequisites:
- Python installed with necessary libraries.
- Required stopword lists and sentiment dictionaries available in their respective folders.

### Steps:
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/manu2576/NLP.git
   cd NLP
   ```

2. **Install Dependencies**:
   ```bash
   pip install requests beautifulsoup4 pandas regex textblob spacy nltk tensorflow chardet
   ```
   
3. **Download Additional NLTK and Spacy Data**:
   ```python
   import nltk
   nltk.download('punkt')
   nltk.download('stopwords')
   nltk.download('cmudict')
   python -m spacy download en_core_web_sm
   ```

4. **Run the Python Script**:
   - Navigate to the appropriate folder and run the `.ipynb` or `.py` file.
   ```bash
   python data_extraction.py
   ```

5. **View Output**: The output CSV files will be saved in their respective directories (`NLP_task/Sentimental_Analysis/Output_Data_Structure.csv`, `NLP_task/Task_2_to_8/final_output.csv`).

## Project Structure

```
├── Provided_data/
│   ├── Input.xlsx
│   ├── Output_Data_Structure.xlsx
│   └── Sentimental_analysis_data/
│       ├── MasterDictionary/
│       │   ├── negative-words.txt
│       │   └── positive-words.txt
│       └── StopWords/
│           ├── StopWords_Auditor.txt
│           ├── StopWords_Currencies.txt
│           ├── StopWords_DatesandNumbers.txt
│           ├── StopWords_Generic.txt
│           ├── StopWords_GenericLong.txt
│           ├── StopWords_Geographic.txt
│           └── StopWords_Names.txt
├── Sentimental_Analysis/
│   ├── Output_Data_Structure.csv
│   └── Sentimental_analysis.ipynb
├── Task_2_to_8/
│   ├── Task_2_to_8.ipynb
│   └── final_output.csv
└── Web_scraping/
    ├── data_extraction.ipynb
    └── web_scraped_data.csv
```

## Challenges Faced

1. **Cleaning Text**: Handling edge cases in the text (e.g., URLs, emails) during preprocessing was challenging.
2. **Syllable Count Accuracy**: Ensuring accurate syllable counts while accounting for irregular words was complex.
3. **Stopwords and Sentiment Dictionaries**: Combining multiple stopword lists and correctly applying positive/negative word dictionaries was key to accurate sentiment analysis.

## Future Improvements

- **Refine Readability Metrics**: Improve accuracy in calculating complex words and other readability features.
- **Additional Preprocessing**: Implement more sophisticated text cleaning techniques, especially for domain-specific data.
- **Enhance Sentiment Analysis**: Use advanced NLP techniques for sentiment analysis, such as using pretrained models for context-aware sentiment.

## License

This project is licensed under the MIT License. See the LICENSE file for more details.
