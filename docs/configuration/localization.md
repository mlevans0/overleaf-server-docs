## i18n Languages ##

Both {{versions['community-edition-short']}} and {{versions['server-pro-short']}} have been translated into multiple languages. 

The language can be set via `SHARELATEX_SITE_LANGUAGE` with one of the following options:

* `en` - English (default)
* `es` - Spanish
* `pt` - Portuguese
* `de` - German
* `fr` - French
* `cs` - Czech
* `nl` - Dutch
* `sv` - Swedish
* `it` - Italian
* `tr` - Turkish
* `zh-CN` - Chinese (simplified)
* `no` - Norwegian
* `da` - Danish
* `ru` - Russian
* `ko` - Korean
* `ja` - Japanese
* `pl` - Polish
* `fi` - Finnish

Some of these are more complete than others, we always endeavour to have all supported languages fully translated, if you would like to help with our translations please get in touch!

## Multi language support

Overleaf can support multiple languages per container. This is done via different domains configured to respond in a set language using the env variable `SHARELATEX_LANG_DOMAIN_MAPPING` with an escaped JSON string.

```
SHARELATEX_LANG_DOMAIN_MAPPING = '
    {
        "www": {
        "lngCode": "en",
        "url": "https:\/\/www.example.com"
        },
        "fr": {
        "lngCode": "fr",
        "url": "https:\/\/fr.example.com"
        },
        "de": {
        "lngCode": "de",
        "url": "https:\/\/de.example.com"
        },
        "sv": {
        "lngCode": "sv",
        "url": "https:\/\/sv.example.com"
        },
        "zh": {
        "lngCode": "zh-CN",
        "url": "https:\/\/zh.example.com"
        },
        "es": {
        "lngCode": "es",
        "url": "https:\/\/es.example.com"
        }
    }'
```