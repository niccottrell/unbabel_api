Unbabel API
===========

This github project contains the documentation for interacting with the Unbabel API.

Authentication
================================

Unbabel supports an api key-based authentication mechanism. The key is the api key associated with an account and can be obtained in the unbabel website on your profile. This key should be passed as a request header with each request. All API calls described in this document require authentication.


Translation
============

Request Translation
-------------


```shell
curl -H "Authorization: ApiKey username:token" 
     -H "Content-Type: application/json" 
     -X POST http://www.unbabel.co/tapi/v2/translation/ 
     --data 'data'
```

Where data is a json file which accepts the following arguments:

* text (required) - the text to be translated.
* target_language (required) - the language to translate the text to.
* source_language - the language of text. If not supplemented it will be auto-detected from the text.
* call_back_url - Once the job is done the result will be posted to this endpoint.
* type - The type of the job, free or paid (default). Note that you need to ask for special permissions to submit free jobs ( email contact@unbabel.co ). 
* visibility - If the job will be private (default) or public. Public jobs will mention the translators that made them. To submit a free job you need to submit the url where the job will be available.
* public_url - Url where the job will be posted.
* formality - The tone that should be used in the translation.
* instructions - Client instructions for the translator.

Example:

```json
{   
    "text":"In the era of Siri",
    "target_language":"pt",
}
```

Response:

```json
{
    "status": "new",
    "text":"In the era of Siri",
    "target_language":"pt",
    "source_language": "en",
    "uid": "5d10df62d3",
    "price":5
}
```

* uid - The unique translation identifier. This will be used for the user to joaccessd the job.
* status - The current status of the job, can be one of: 
** new - The translation has not been started yet. 
** processing -  The translation is being proceed.
** finshed - The translation has been terminated. The client can access the translation.
** accepted - The translation has been accepted by the client. A job is accepted if the clinet does fill a complaint in 48 hours.
* source_language - The detected source language
* price - The total price of the requested translation in euro cents.

Query a Translation
-------------

Returns the current state of a translation.

```shell
curl -H "Authorization: ApiKey username:token" 
     -H "Content-Type: application/json" 
     -X GET http://www.unbabel.co/tapi/v2/translation/uid 
```

Response:

```json
{
     "price": 4,
     "status": "accepted",
     "text":"In the era of Siri",
     "target_language":"pt",
     "source_language": "en",
     "translated_text": "teste super legal ordem.",
     "uid": "29de9551d9"
     
}
```


Query all Translations
-------------

Returns a list of translations done by this user.

```shell
curl -H "Authorization: ApiKey username:token" 
     -H "Content-Type: application/json" 
     -X GET http://www.unbabel.co/tapi/v2/translation/ 
```

Response:

```json

{
     "meta": {
               "limit": 20,
               "next": "/tapi/v2/translation/?limit=20&offset=20",
               "offset": 0,
               "previous": null,
               "total_count": 45
          },
     "objects": [
          {
               "price": 5,
               "status": "new",
               "text": "This is a test task ",
               "uid": "a281dab6e1"
          },
          {
               "price": 4,
               "status": "accepted",
               "text":"In the era of Siri",
               "target_language":"pt",
               "source_language": "en",
               "translated_text": "teste super legal ordem.",
               "uid": "29de9551d9"
          }

     ...
     ]
}
```




     Language Pairs
================================

Language pairs currently available in the unbabel platform:

```shell
curl -H "Authorization: ApiKey username:f4c76b650164c179cc1b199c98ea8ef75d7dbfa6" 
     -H "Content-Type: application/json" 
     -X GET http://www.unbabel.co/tapi/v2/language_pair/ 
```

Response:

This query returns a list of all language pairs currently supported by the unbabel platform. Each language pair entry contains the iso639-1 language code and language full name for both the source and target language.

```json
{
    "objects": [
                {
                    "lang_pair": {  
                                    "source_language": {"name": "Portuguese", "shortname": "pt"}, 
                                    "target_language": {"name": "English", "shortname": "en"}
                                 }
                }, 
                ...
                {
                    "lang_pair": {  
                                    "source_language": {"name": "German", "shortname": "de"}, 
                                    "target_language": {"name": "Portuguese", "shortname": "pt"}
                                 }
                }, 
                ]
}
```


Tones
================================

Language Tones available in the unbabel platform:

```shell
curl -H "Authorization: ApiKey username:f4c76b650164c179cc1b199c98ea8ef75d7dbfa6" 
     -H "Content-Type: application/json" 
     -X GET http://www.unbabel.co/tapi/v2/tone/ 
```

Response:

This query returns a list of all language tones supported by the unbabel platform. 

```json

{
    "objects": [
                {"tone": {"description": "Informal style", "name": "Informal"}}, 
                {"tone": {"description": "Friendly style", "name": "Friendly"}}, 
                {"tone": {"description": "Business style", "name": "Business"}}, 
                {"tone": {"description": "Formal style", "name": "Formal"}}
                ]
}

```
