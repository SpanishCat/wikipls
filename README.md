# What is this?
Wikipls is a Python package meant to easily scrape data out of Wikipedia (maybe more of the Wikimedia in the future), using the REST API.
This package is still in early development, but it has basic functionality all set.

# Why does it exist?
The REST API for wikimedia, isn't the most intuitive and requires some learning.
When writing code, it also requires setting up a few functions to make it more manageable and readable.
So essentially I made these functions and packaged them so that you (and I) won't have to rewrite them every time.
While I'm at it I made them more intuitive and easy to use without needing to figure out how this API even works.

# Installation
To install use: `pip install py_wikipls`

# How to use
I haven't made any documentation page yet, so for now the below will have to do.\
If anything is unclear don't hesitate to open an issue in [Issues](https://github.com/SpanishCat/py-wikipls/issues).

  ## Key
  Many functions in this package require the name of the Wiki page you want to check in a URL-friendly format.
  The REST documentation refers to that as a the "key" of an article.
  For example: 
  - The article titled "Water" is: "Water"
  - The article titled "Faded (Alan Walker song)" is: "Faded_(Alan_Walker_song)"
  - The Article titled "Georgia (U.S. state)" is: "Georgia_(U.S._state)"

  That key is what you enter in the *name* parameter of functions.

  To get the key of an article you can:
  1. Take a look at the url of the article.\
    The URL for "Faded" for example is "https://en.wikipedia.org/wiki/Faded_(Alan_Walker_song)".
    Notice it ends with "wiki/" followed by the key of the article.
  2. Take the title of the article and replace all spaces with "_", it'll probably work just fine.
  3. In the future there will be a function to get the key of a title.

  ## Direct Functions
  These functions can be used without needing to create an object. 
  In general they all require the URL-friendly name of an article as a string.
  
  ### `get_views(name: str, date: str | datetime.date, lang: str = LANG) -> int`
  Returns the number of times people visited an article on a given date.

  The date given might can be either a datetime.date object or a string formatted yyyymmdd (So March 31th 2024 will be "20240331").

  `>>> get_views("Faded_(Alan_Walker_song)", "20240331")`\
  `1144`
  
The Faded page on Wikipedia was visited 1,144 on March 31st 2024.
  
  ### `get_html(name: str) -> str`
  Returns the html of the page as a string. 
  This can be later parsed using tools like BeautifulSoup.

  `>>> get_html("Faded_(Alan_Walker_song)")[:40]`\
  `'<!DOCTYPE html>\n<html prefix="dc: http:/'`

  This example returns the beginning of the html of the "Faded" page.

  ### `get_summary(name: str) -> str`
  Returns a summary of the page.

  `>>> get_summary("Faded_(Alan_Walker_song)")[:120]`\
  `'"Faded" is a song by Norwegian record producer and DJ Alan Walker with vocals provided by Norwegian singer Iselin Solhei'`

  This examples returns the first 120 letters of the summary of the Faded page

  ### `get_media(name: str) -> tuple[dict, ...]`
  Returns all media present in the article, each media file represented as a JSON.

`get_media("Faded_(Alan_Walker_song)")[0]`\
  `{'title': 'File:Alan_Walker_-_Faded.png', 'leadImage': False, 'section_id': 0, 'type': 'image', 'showInGallery': True, 'srcset': [{'src': '//upload.wikimedia.org/wikipedia/en/thumb/d/da/Alan_Walker_-_Faded.png/220px-Alan_Walker_-_Faded.png', 'scale': '1x'}, {'src': '//upload.wikimedia.org/wikipedia/en/d/da/Alan_Walker_-_Faded.png', 'scale': '1.5x'}, {'src': '//upload.wikimedia.org/wikipedia/en/d/da/Alan_Walker_-_Faded.png', 'scale': '2x'}]}`

  This example returns the first media file in the Faded article, which is a PNG image.

  ### `get_pdf(name: str) -> bytes`
  Returns the PDF version of the page in byte-form.

  `>>> with open("faded_wiki.pdf", 'wb') as f:`\
  `      f.write(get_pdf("Faded_(Alan_Walker_song)"))`

  This example imports the Faded page in PDF form as a new file named "faded_wiki.pdf".

  ### `get_page_data(name: str) -> dict`
  Returns a few more details about the article in JSON form

  `>>> get_page_data("Faded_(Alan_Walker_song)")`\
  `{'id': 49031279, 'key': 'Faded_(Alan_Walker_song)', 'title': 'Faded (Alan Walker song)', 'latest': {'id': 1254652602, 'timestamp': '2024-11-01T00:46:48Z'}, 'content_model': 'wikitext', 'license': {'url': 'https://creativecommons.org/licenses/by-sa/4.0/deed.en', 'title': 'Creative Commons Attribution-Share Alike 4.0'}, 'html_url': 'https://en.wikipedia.org/w/rest.php/v1/page/Faded%20%28Alan%20Walker%20song%29/html'}
`
  This example gives us the page id, key, title, license and latest revision to the Faded article, in JSON form.

  ### `to_timestamp(date: datetime.date) -> str`
  Converts a datetime.date object to a URL-friendly string format (yyyymmdd)

  `>>> date = datetime.date(2024, 3, 31)`\
  `>>> to_timestamp(date)`\
  `20240331`

  This example converts the date of March 31th 2024 to URL-friendly string form.

# Bug reports
This package is in early development and I'm looking for community feedback on bugs.\
If you encounter a problem, please report it in [Issues](https://github.com/SpanishCat/py-wikipls/issues).

# What does the name mean?
Wiki = Wikipedia
Pls = Please, because you make requests

# Versions
This version of the package is written in Python. I plan to eventually make a copy of this one written in Rust (using PyO3 and maturin).
Why Rust? It's an exercise for me, and it will be way faster and less error-prone

# Plans
- Support for revisions (i.e. the ability to get an older version of an article)
- Support for more languages (Currently supports only English Wikipedia)
- Dictionary
- Citations
