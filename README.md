Approach


STEP 1:
Initially i have scraped data from the URL provided in NLP_task/Provided_data/Input.xlsx  file using libraries
Requests, BeautifulSoup.
Then i have done some preprocessing on the extracted text such as:
Remove numbers, remove url , remove emails and corrected spelling using libraries
re, TextBlob.
Then formed a dataframe of the scraped and preprocessed data as  : NLP_task/Web_scraping/web_scraped_data.csv


STEP 2:
Imported Scraped dataset NLP_task/Web_scraping/web_scraped_data.csv
and Output dataset NLP_task/Provided_data/Output_Data_Structure.xlsx
which contains the structure of final output.
Imported some of directories for sentimental analysis
SENTIMENTAL ANALYSIS
1.Cleaning using Stop Words Lists
Firstly i have combined all the Stopword list that were provided to me into single SET stopwords = load_stop_words(mydir) where mydir = 'NLP_task/Provided_data/Sentimental_analysis_data/StopWords'
as easy to check values from set if they are present or not.
Then simply removed the Stopwords from every rows of Scapred Dataset
2.Creating a dictionary of Positive and Negative word
	I have have loaded both positive and negative word text file into:
positive_words = load_word_list(positivedictionaydir)
negative_words = load_word_list(negativedictionaydir) 

3.Extracting Derived variables 
Perform sentiment analysis based on positive and negative word lists using function positive_negative_score  which basically iterates through Scraped data and count positive and negative score
Then we  Calculate polarity score and subjectivity score using these formulas
Polarity_Score = (positive_score - negative_score) / (positive_score + negative_score + 0.000001)
Subjectivity_Score = (positive_score + negative_score) / (words_count + 0.000001)
Finally process all these steps on each row of Scraped Dataset and append the values in Output Dataset and export it
output_dataset.to_csv('NLP_task/Sentimental_Analysis/Output_Data_Structure.csv', index=False)


STEP 3:
1. Initialized dataset
dataset = pd.read_csv('NLP_task/Web_scraping/web_scraped_data.csv')
output_dataset = pd.read_csv('NLP_task/Sentimental_Analysis/Output_Data_Structure.csv')                                                  

2.  Imported required libraries
import pandas as pd
import re
import spacy
from nltk.corpus import cmudict
import json
from nltk.corpus import stopwords
from tensorflow.keras.preprocessing.text import Tokenizer

3. As data contained few words like acb.zyx we removed that ‘.’ with DOT do that there will be no issue while splitting the text dot_removal_text

4. Then we counted sentences. First I used split(‘\n’) to split every sentence because when we use the input dataset it contain ‘\n’ to split the line  then I used nlp = spacy.load('en_core_web_sm') to separate line more thoroughly and made a plain_text which contains the whole text splitted into more sophisticated way containing only ‘.’ as line separator left then we split the sentences with ‘.’  and counted it.
dot_removed_text = dot_removal_text(plaintext)
total_sen = total_sentence(article_text)


5. We used NLTK stopwords libraries to get cleaned text and converted into SET because it's easy to use as compared to a dictionary. Then used Regular Expressions to remove PUNCTUATIONs from the text and counted number of words in cleaned text
stopwords_removed = remove_stopwords(article_text)
article_no_punctuation = remove_punctuation(stopwords_removed)
total_words = counts(article_no_punctuation)

6. Then we calculated Average Word Length using formula
 total_characters / total_words
average_word_length_value = calculate_average_word_length(article_no_punctuation)
used article_no_puntuation because now it's the cleaned we have acquired from above steps

7. Now we calculated the syllable count which will be a dictionary of the number of syllables in each word and to do that I have created a no_vowels function which basically counts the number of vowels in each word and exceptions for ‘ed’ and ‘es’ were made. Then I have used the no_vowels function in syllable count function while iterating through each word and finding its values and storing it in the dictionary and used artucle_no_punctutation  text as it’s cleaned and used .split() to provide single words. 
syllable_words = syllable_count_per_word(article_no_punctuation.split())
syllable_words_str = json.dumps(syllable_words)
 and converted it into json to append it into output_dataset

8. Now we calculate the number of complex word which means words with more than    two syllables so we simply used syllable_words as an input for out function which will iterate whole dictionary and check for condition syllable_count > min_syllable
and count number of words 
complex_words = complex_word_count(syllable_words)

9. Now we have calculated number of personal pronouns using Regular Expression 
pronouns_pattern = r'\b(I|we|my|ours|us)\b'
pronouns_count = count_personal_pronouns(article_text)
 
10. At last few more values were created with help of the output we gained above with formulas :
avg_word_length = average_word_length_value
Average_Sentence_Length = total_words / total_sen if total_sen > 0 else 0
Percentage_of_Complex_words = complex_words / total_words if total_words > 0 else 0
Fog_Index = 0.4 * (Average_Sentence_Length + Percentage_of_Complex_words)

Finally process all these steps on each row of Scraped Dataset and append the values in Output Dataset and export it
output_dataset.to_csv('NLP_task/Task_2_to_8/final_output.csv', index=False)
as our final output file

How to run the .py file 
Structure your file into this format

├── Provided_data
│   ├── Input.xlsx
│   ├── Output_Data_Structure.xlsx
│   └── Sentimental_analysis_data
│       ├── MasterDictionary
│       │   ├── negative-words.txt
│       │   └── positive-words.txt
│       └── StopWords
│           ├── StopWords_Auditor.txt
│           ├── StopWords_Currencies.txt
│           ├── StopWords_DatesandNumbers.txt
│           ├── StopWords_Generic.txt
│           ├── StopWords_GenericLong.txt
│           ├── StopWords_Geographic.txt
│           └── StopWords_Names.txt
├── Sentimental_Analysis
│   ├── Output_Data_Structure.csv
│   └── Sentimental_analysis.ipynb
├── Task_2_to_8
│   ├── Task_2_to_8.ipynb
│   └── final_output.csv
└── Web_scraping
    ├── data_extraction.ipynb
    └── web_scraped_data.csv
    
DEPENDENCIES
1: Commands to install dependencies:
pip install requests beautifulsoup4 pandas regex textblob spacy nltk tensorflow chardet

2: After installing nltk, you'll need to download additional data (like stopwords and tokenizers):
import nltk 
nltk.download('punkt') 
nltk.download('stopwords') 
nltk.download('cmudict')

3: After installing spacy, you'll need to download the required language model:
python -m spacy download en_core_web_sm
