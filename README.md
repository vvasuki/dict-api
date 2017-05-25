Table of content generated using this app

- [Introduction](#introduction)
   - [Motivations (in decreasing order of importance)](#motivations-(in-decreasing-order-of-importance))
- [Desiderata](#desiderata)
   - [Intro to the data](#intro-to-the-data)
   - [Web API (in decreasing order of importance)](#web-api-(in-decreasing-order-of-importance))
   - [UI (in decreasing order of importance)](#ui-(in-decreasing-order-of-importance))
- [Implementation](#implementation)
   - [Ideas](#ideas)
- [Design choices](#design-choices)
- [Deployment](#deployment)

# Introduction
We seek to provide a functional rest-api based webapp to access various dictionaries stored in a database. We intend for the database to serve a huge, eclectic mix of dictionaries which are not already available in a single spot.

## Motivations (in decreasing order of importance)
* Any web developer should be able to make simple REST-API calls to our backend to easily get entries for a lot of dictionaries (most of whom aren't already available in such manner).
  * Imagine being able to highlight a word and use the context menu while reading a text to get it's meanings, grammatical info (linga, vibhakti, vachana, puruSha) etc..
* End users ( sanskrit students and scholars ) should be able to thoroughly investigate a term in the dictionaries using just a browser, without having to install any software.
* End users should be able to point out errors (eg. via a link).
* Users should be able to log in and submit new words to a "user dictionary" (as in <sanskritdictionary.de> or wiktionary, but using convenience of openId and less fuss.)

# Desiderata
## Intro to the data
* We have several projects which collect dictionaries for various languages ([link](https://github.com/search?q=org%3Asanskrit-coders+stardict)). These dictionaries are further subcategorized by topic, entry language etc.. (eg. Ayurveda, sa-german etc..).
* Simplest programmatic current way to access dictionary data is through babylon files ([some here](https://github.com/search?utf8=%E2%9C%93&q=user%3Asanskrit-coders+extension%3Ababylon_final&type=Code) - open one to check out the format).
* We ultimately have the (extensible) JSON structure exemplified below for each entry:
```
{
"headwords" : ["svar","स्वर्"],
"dictionaryId": "amarakosha",
"locationLink": "http://some-link-to-a-scanned-page",
"entry": "स्वर्ग पुं। <br><br> स्वर्गः <br><br> समानार्थक:स्वर्,स्वर्ग,नाक,त्रिदिव,त्रिदशालय,सुरलोक,द्यो,दिव्,त्रिविष्टप,गो <br><br> 1।1।6।1।2 <br><br> स्वरव्ययं स्वर्गनाकत्रिदिवत्रिदशालयः। सुरलोको द्योदिवौ द्वे स्त्रियां क्लीबे त्रिविष्टपम्.।  <br><br> अवयव : देवसभा <br><br> सम्बन्धि2 : देवः <br><br> पदार्थ-विभागः : , द्रव्यम्, पृथ्वी, अचलनिर्जीवः, स्थानम्, अलौकिकस्थानम्"
}
```

* For dictionary metadata, we have the following structure:
```
{
"_id": "amarakosha",
"name": "अमरकोशः",
"authors": ["अमरसिंहः"],
"licenseLink": "http://some-link",
"canonicalSource": "http://some-link",
"categories": ["sanskrit to sanskrit", "thesaurus"],
"issuePage": "https://github.com/sanskrit-coders/stardict-sanskrit/issues"
}
```

## Web API (in decreasing order of importance)
In the ideal case, we would have the following:
* `/words/xyz` yields the appropriate entry if it exists; or returns a list of n=40 words starting with that substring 'xyz' - from all dictionaries.
* `/dictionaries/dictionaryId/words/xyz` - same as above, restricted to one dictionary.
* `/dictionaries/dictionaryCategory/words/xyz` does the same - except for all dictionaries in a given category.
* `/words_with_substring/xyz` etc..

## UI (in decreasing order of importance)
* Should be mobile friendly, with flowing text.
* Searching:
  * User is able to search for a word in multiple dictionaries.
  * As a user types, a dropdown of suggestions appear, thereby avoiding unnecessary strain.
  * User can restrict search to certain dictionaries or dictionary categories.
  * Complex (ok if slower) searches based on substrings (or eventually regular expressions), matching headwords.
    * Complex search within entries.
* Results:
  * A term can appear in many dictionaries, the user should be able to quickly navigate to the dictionary or dictionary-category of his choice.
  * Some history (what words were recently looked up).
* Stats:
  * Collect site use stats (eg. using google analytics).
  * Submit lookup stats to the 
* Example from Goldendict (desktop) [here](http://imgur.com/a/Tj6OZ?).


# Implementation
## Ideas
* Use no-sql database with good replication characteristics, like couchdb. couchdb already provides some simple API, and it is extensible.
* Leverage the rest api that couchdb provides to the extant possible.
* Use offline catching intelligently to minimize communicating with the server. Caching ideas:
  * Headword-dictionary mappings

# Design choices


# Deployment
