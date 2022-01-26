# Extract Highlights

The API endpoints at `https://api.scholarcy.com/api/highlights/extract` will pull out the key findings/highlights of an article and also provide a longer, extractive summary.

## POST a local file to extract highlights

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/highlights/extract'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}
file_path = '/path/to/local/file.pdf'

request = RestClient::Request.new(
          :method => :post,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :multipart => true,
            :file => File.new(file_path, 'rb'),
            :start_page => 24,
            :end_page => 37
          })
response = request.execute
puts(response.body)
```

```python
import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/highlights/extract'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

params = {'wiki_links': True, 'reference_links': True}
with open(file_path, 'rb') as file_data:
    file_payload = {'file': file_data}
    r = requests.post(POST_ENDPOINT,
          headers=headers,
          files=file_payload,
          data=params,
          timeout=timeout)
    print(r.json())

```

```shell
curl "https://api.scholarcy.com/api/highlights/extract" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf" \
  -F "start_page=24" \
  -F "end_page=37"
```


> The above command returns JSON structured like this:

```json
{
  "filename": "filename.pdf",
  "content_type": "application/pdf",
  "file_size": 123456,
  "metadata": {
    "title": "Article title",
    "author": "Smith, J.",
    "pages": "15",
    "date": 2021,
    "affiliations": [
      "Department of Silly Walks, University of Life, London, UK",
    ],
    "identifiers": {
      "arxiv": null,
      "doi": "10.1010/101010.10.10.1101010",
      "isbn": null,
      "doc_id": null
    },
    "abstract": "This is a very exciting paper. Please read it.",
    "funding": [
      {
        "award-group": [
          {
            "funding-source": "FEDER/COMPETE",
            "award-id": [
              "AAA/BBB/04007/2019"
            ]
          }
        ],
        "funding-statement": "We acknowledge financial support from Fundação para a Ciência e a Tecnologia and FEDER/COMPETE (grant AAA/BBB/04007/2019)"
      }
    ]
  },
  "keywords": [
    "atomic force microscopy",
    "dna nanostructure",
    "drug release",
    "single-stranded DNA",
    "double-stranded DNA",
    "dox molecule"
  ],
  "keyword_relevance": {
    "atomic force microscopy": 0.345678,
    "dna nanostructure": 0.23456,
    "drug release": 0.12345,
    "single-stranded DNA": 0.034567,
    "double-stranded DNA": 0.02345,
    "dox molecule": 0.01234
  },
  "abbreviations": {
    "DONs": "DNA origami nanostructures",
    "ROS": "reactive oxygen species",
    "DNase I": "deoxyribonuclease I",
    "PEG": "polyethylene glycol"
  },
  "headline": "We prove some important facts in this paper",
  "highlights": [
    "Facts are very important.",
    "The force is strong in this one.",
    "We ran some tests and this is what we found"
  ],
  "findings": [
    "A statistically significant difference was noted between the four groups on the combined dependent variables",
    "We also noted significant differences when we performed a one-way between-groups analysis of variance on each of the 14 items (P < 0.001)"
  ],
  "summary": [],
  "structured_summary": {
    "Introduction": [
      "Introduction paragraph 1",
      "Introduction paragraph 2",
    ],
    "Methods": [
      "We mixed some chemicals.",
      "We heated them up.",
      "We distilled the mixture."
    ],
    "Results": [
      "There was a big explosion",
      "But the crystals were pure",
      "We identified a new compound",
    ],
    "Conclusion": [
      "We proved some important things and we summarise them here.",
      "Further work is necessary"
    ]
  }
}
```

This endpoint extracts highlights from a local file. File formats supported are:

- PDF
- Word (.docx)
- Rich Text (.rtf)
- Powerpoint (.pptx)
- BibTeX (.bib)
- RIS (.ris)
- XML
- HTML
- Plain Text (.txt)
- LaTeX (.tex)


### HTTP Request

`POST https://api.scholarcy.com/api/highlights/extract`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
file | null | A file object.
url | null | URL of public, open-access document.
text | null | Plain text content to be processed.
start_page | 1 | Start reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
end_page | null | Stop reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
external_metadata | false | If true, fetch article metadata from the relevant remote repository (e.g. CrossRef).
wiki_links | false | If true, map extracted key terms to their Wikipedia pages
reference_links | false | If true, parse and link each reference to its full text location
replace_pronouns | false | If true, replace first-person pronouns with third-person mentions (the author(s)?, they).
key_points | 5 | The number of key points/key takeaway items to extract.
sampling | representative | For large documents, when extracting key terms, use either a representative sample of the full content, or the fulltext content.
extract_snippets | true | If true, sample snippets from each section, otherwise, sample the full text.
add_background_info | false | If true, generate an introductory sentence. Useful generating an abstract from an article.
add_concluding_info | false | If true, generate an concluding sentence. Useful generating an abstract from an article.
structured_summary | false | If true, summarise each of the main sections separately, and then provide a summary structured according to those sections.
summary_engine | v1 | v1: Best for articles. v2: best for book chapters.
highlights_algorithm | weighted | weighted: attend more closely to the results and conclusion. unweighted: attend to all content equally.
headline_from | highlights | highlights: use the highest scoring highlight as the headline. summary: use the first summary sentence as the headline. conclusions: use the first conclusion statement as a headline. claims: use the main claim as the headline.


<aside class="success">
Remember to include your Authentication token with every request.
</aside>


## GET highlights from a URL

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/highlights/extract'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}

request = RestClient::Request.new(
          :method => :get,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :url => 'https://www.nature.com/articles/s41746-019-0180-3',
            :start_page => 1
          })
response = request.execute
puts(response.body)

```

```python
import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/highlights/extract'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

payload = {
'url': 'https://www.nature.com/articles/s41746-019-0180-3',
'start_page': 1
}
r = requests.get(POST_ENDPOINT,
      headers=headers,
      params=payload,
      timeout=timeout)
print(r.json())

```

```shell
curl "https://api.scholarcy.com/api/highlights/extract" \
  -H "Authorization: Bearer abcdefg" \
  -d "url=https://www.nature.com/articles/s41746-019-0180-3" \
  -d "start_page=1"

```


> The above command returns JSON structured as for the POST endpoint:

```json
{
  "filename": "filename.pdf",
  "content_type": "application/pdf",
  "file_size": 123456,
  "metadata": {
    "title": "Article title",
    "author": "Smith, J.",
    "pages": "15",
    "date": 2021,
    "affiliations": [
      "Department of Silly Walks, University of Life, London, UK",
    ],
    "identifiers": {
      "arxiv": null,
      "doi": "10.1010/101010.10.10.1101010",
      "isbn": null,
      "doc_id": null
    },
    "abstract": "This is a very exciting paper. Please read it.",
    "funding": [
      {
        "award-group": [
          {
            "funding-source": "FEDER/COMPETE",
            "award-id": [
              "AAA/BBB/04007/2019"
            ]
          }
        ],
        "funding-statement": "..."
      }
    ]
  },
  "keywords": [],
  "keyword_relevance": {},
  "abbreviations": {},
  "headline": "We prove some important facts in this paper",
  "highlights": [],
  "findings": [],
  "summary": [],
  "structured_summary": {
    "Introduction": [
    ],
    "Methods": [
    ],
    "Results": [
    ],
    "Conclusion": [
    ]
  }
}
```

This endpoint extracts highlights from a remote URL.
The remote URL can resolve to a document type of any of the formats listed for the POST endpoint

### HTTP Request

`GET https://api.scholarcy.com/api/highlights/extract`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
url | null | URL of public, open-access document.
text | null | Plain text content to be processed.
start_page | 1 | Start reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
end_page | null | Stop reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
external_metadata | false | If true, fetch article metadata from the relevant remote repository (e.g. CrossRef).
wiki_links | false | If true, map extracted key terms to their Wikipedia pages
reference_links | false | If true, parse and link each reference to its full text location
replace_pronouns | false | If true, replace first-person pronouns with third-person mentions (the author(s)?, they).
key_points | 5 | The number of key points/key takeaway items to extract.
sampling | representative | For large documents, when extracting key terms, use either a representative sample of the full content, or the fulltext content.
extract_snippets | true | If true, sample snippets from each section, otherwise, sample the full text.
add_background_info | false | If true, generate an introductory sentence. Useful generating an abstract from an article.
add_concluding_info | false | If true, generate an concluding sentence. Useful generating an abstract from an article.
structured_summary | false | If true, summarise each of the main sections separately, and then provide a summary structured according to those sections.
summary_engine | v1 | v1: Best for articles. v2: best for book chapters.
highlights_algorithm | weighted | weighted: attend more closely to the results and conclusion. unweighted: attend to all content equally.
headline_from | highlights | highlights: use the highest scoring highlight as the headline. summary: use the first summary sentence as the headline. conclusions: use the first conclusion statement as a headline. claims: use the main claim as the headline.

<aside class="success">
Remember to include your Authentication token with every request.
</aside>
