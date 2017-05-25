# Babler
[![Build Status](https://travis-ci.org/gidim/Babler.svg?branch=master)](https://travis-ci.org/gidim/Babler)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)


Web Data Collection System For NLP/Speech Recognition 
 
 
## Introduction
Babler is a program that collects textual data from the internet autonomously for Natural Language Processing and other Machine learning tasks. Unlike other tools we focus on conversational data in the form of tweets, blog posts and forum posts. Conversational data is usefl for many NLP tasks. We used this tool to collect data for training language models, improving keyword search, collecting code-switch data, sentiment analysis data and emotion recognition. Babler language identifcation supports 504 languages in a majorty vote over an ensamble of classifiers which makes it a great tool for collecting low resource languages.

 
## Installation
 
#### Dependencies
* Java 1.7 or greater
* JDK 1.7 or greater
* Apache Maven
* MongoDB
* [CLD2 - Google Compact Language Detector](https://github.com/CLD2Owners/cld2) (Optional)
 
#### Babler Install
```shell
git clone https://github.com/gidim/Babler.git
cd Babler
./install.sh
```
 
#### Config File
Babler requires a few things before you can start collecting. The minimum is the API key for your source (twitter/bing etc) and the path to a folder where the data would be saved. The config file uses the Java properties format and there's an example config in /config.properties. Twitter supports multiple keys which should be separated with a semicolon.
Make sure you set `productionScarpingFolderPath` to some directory where you have write permissions.
 
#### Building 
If you made any code changes you must rebuild the project. To do this run
```shell
$ mvn clean install
```
 
 
 
## Usage Examples
 
#### Collecting Spanish Tweets that contains a word from a word list
 
In this example we're interested in collecting tweets that contains specific domain related words. For example
we can build a list of medical terms and then save all the tweets that contain them. Additionally we want to make sure
the tweets we find are in Spanish. 
 
```shell
$ java -jar target/Babel-1.0.jar --module twitter -wordlist /path/to/wordlist.txt  --lang spa --config config.properties
```
 
In this example we set the module to twitter, use the wordlist argument to provide a path to a file that contains words (one per line) and force the results to be in Spanish using the ISO639-2 language code format. We also use the --config flag to provide the path to the config file that contains the API keys.
 
 
 
#### Collecting Tweets that contains a word from a word list, simultaneously for multiple languages
 
In this case we're interested in collecting tweets in English and Spanish with different words for each language.
This is very similar to the previous example but it allows us to run the same task to multiple languages/word lists.
```shell
$ java -jar target/Babel-1.0.jar -m twitter --langs mon spa heb --seedsFolder /path/to/seed_folder/ --config config.properties
```
 
Similarly to the previous example we use the twitter module. In this case we use the --langs flag to collect data in Mongolian, Spanish and Hebrew. We also use the --seedsFolder to provide a path to the respective wordlists. Your seeds folder should follow the following structure:
```shell
$ ls /path/to/seed_folder/
mon.txt spa.txt heb.txt
```
 
 
#### Collecting Code-Switch data for a language pair
     java -jar target/Babel-1.0.jar -m twitterCodeSwitch --langs en es --seedsFolder /Users/Gideon/test/
 
Notice we changed the module to twitterCodeSwitch. Here we request the English/Spanish language pairs.
This module will force Spanish language detection for English, and English LID for Spanish.
* For code switching use ISO-639-1 Lang Codes
 
## Reading Data
Once you finished your data collection there are a few methods to read the data:
1. Use the text files stored in your `productionScarpingFolderPath`. The folder would follow this structure:
```shell
$ cd productionScarpingFolderPath
$ ls
 
langCode1/
	log2.txt # A log file with some metadata on the collected documents saved
	twitter/
    	doc_id.txt
        .
        .
        .
        doc_id.txt
    
    module_name/
    	doc_id.txt
        .
        .
        .
        doc_id.txt
        
langCode2/
        .
        .
        .
 
```
 
 
3. 2. Connect to MongoDB
 
    You can connect directly to the MongoDB database and interacted with the saved documents. The DB name
    is "scraping" and each module uses its own collection ("twitter","blogposts","forumposts")
 
4. Use helper script

  For you convenience we included a python script that prints all the documents to stdout.
  To print all the Mongolian data we can run this:
  ```shell
	$   python scripts/echo_langcode.py LANGCODE > mongolian_data.txt
  ```
Where LANGCODE is the ISO639-2 language code you used for collection.
 
 ## Additional Stuff
[List of ISO639-2 language codes](https://www.loc.gov/standards/iso639-2/php/code_list.php)
 
 
 ## References 
 If you use Babler for research or academic purposes please cosider citing our paper
 [1] Gideon Mendels, Erica Cooper, Julia Hirschberg, [*Babler - Data Collection from the Web to Support Speech Recognition and Keyword Search*](http://www.aclweb.org/anthology/W16-26#page=82)

 ```
@inproceedings{mendels2016babler,
  title={Babler-Data Collection from the Web to Support Speech Recognition and Keyword Search},
  author={Mendels, Gideon and Cooper, Erica and Hirschberg, Julia},
  booktitle={Proc. of Web as Corpus Workshop (WAC-X) and the EmpiriST Shared Task},
  pages={72--81},
  year={2016}
}
```


## Acknowledgements

This work is supported by the Intelligence Advanced Research
Projects Activity (IARPA) via Department of Defense
U.S. Army Research Laboratory (DoD/ARL) contract number
W911NF-12-C-0012. The U.S. Government is authorized to
reproduce and distribute reprints for Governmental purposes
notwithstanding any copyright annotation thereon. Disclaimer:
The views and conclusions contained herein are those of the authors
and should not be interpreted as necessarily representing
the official policies or endorsements, either expressed or implied,
of IARPA, DoD/ARL, or the U.S. Government.
