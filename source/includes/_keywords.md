# Extract Key Terms

The API endpoints at `https://api.scholarcy.com/api/keywords/extract` will pull out the key terms from an article.

## POST a local file to extract key terms

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/keywords/extract'
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
POST_ENDPOINT = API_DOMAIN + '/api/keywords/extract'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

with open(file_path, 'rb') as file_data:
    file_payload = {
    'file': file_data,
    'start_page': 24,
    'end_page': 37
    }
    r = requests.post(POST_ENDPOINT,
          headers=headers,
          files=file_payload,
          timeout=timeout)
    print(r.json())

```

```shell
curl "https://api.scholarcy.com/api/keywords/extract" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf" \
  -F "start_page=24" \
  -F "end_page=37"
```


> The above command returns JSON structured like this:

```json
{
  "filename": "article2016.pdf",
  "abbreviations": {
    "U&G": "uses and gratification",
    "SNS": "social networking sites",
    "COBRA": "Consumer Online Brand-Related Activity",
    "AVE": "average variance extracted",
    "CLF": "common latent factor"
  },
  "keywords": [
    {
      "term": "Facebook",
      "url": "https://en.wikipedia.org/wiki/Facebook"
    },
    {
      "term": "typology",
      "url": "https://en.wikipedia.org/wiki/typology"
    },
    {
      "term": "social media",
      "url": "https://en.wikipedia.org/wiki/social_media"
    },
    {
      "term": "consumer behavior",
      "url": "https://en.wikipedia.org/wiki/consumer_behavior"
    },
    {
      "term": "average variance extracted",
      "url": "https://en.wikipedia.org/wiki/average_variance_extracted"
    },
    {
      "term": "online branding",
      "url": "https://en.wikipedia.org/wiki/online_branding"
    },
    {
      "term": "cluster analysis",
      "url": "https://en.wikipedia.org/wiki/cluster_analysis"
    },
    {
      "term": "social networking sites",
      "url": "https://en.wikipedia.org/wiki/social_networking_sites"
    },
    {
      "term": "brand manager",
      "url": "https://en.wikipedia.org/wiki/brand_manager"
    }
  ],
  "keyword_relevance": {
    "Facebook": 0.3834808259587021,
    "social media": 0.16224188790560473,
    "social networking sites": 0.08259587020648967,
    "brand interaction": 0.07964601769911504,
    "typology": 0.038348082595870206,
    "brand manager": 0.038348082595870206,
    "uses and gratification": 0.035398230088495575,
    "consumer interaction": 0.032448377581120944,
    "consumer behavior": 0.02359882005899705,
    "brand communication": 0.02064896755162242,
    "cluster analysis": 0.017699115044247787,
    "main motivation": 0.017699115044247787,
    "Consumer Online Brand-Related Activity": 0.014749262536873156,
    "average variance extracted": 0.011799410029498525,
    "common latent factor": 0.008849557522123894,
    "online branding": 0.008849557522123894,
  }
}
```

> The above command can also returns CSV structured like this:

```csv
"filename","key term","wikipedia_link"
"article2016.pdf","Facebook","https://en.wikipedia.org/wiki/Facebook"
"article2016.pdf","typology","https://en.wikipedia.org/wiki/typology"
"article2016.pdf","social media","https://en.wikipedia.org/wiki/social_media"
"article2016.pdf","consumer behavior","https://en.wikipedia.org/wiki/consumer_behavior"
"article2016.pdf","average variance extracted","https://en.wikipedia.org/wiki/average_variance_extracted"
"article2016.pdf","online branding","https://en.wikipedia.org/wiki/online_branding"
"article2016.pdf","cluster analysis","https://en.wikipedia.org/wiki/cluster_analysis"
"article2016.pdf","social networking sites","https://en.wikipedia.org/wiki/social_networking_sites"
"article2016.pdf","brand manager","https://en.wikipedia.org/wiki/brand_manager"
```

This endpoint extracts key terms from a local file. File formats supported are:

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

`POST http://api.scholarcy.com/api/keywords/extract`

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
sampling | representative | For large documents, when extracting key terms, use either a representative sample of the full content, or the fulltext content.
extract_snippets | true | If true, sample snippets from each section, otherwise, sample the full text.
output_format | json | json or CSV.


<aside class="success">
Remember to include your Authentication token with every request.
</aside>


## GET key terms from a URL

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/keywords/extract'
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
POST_ENDPOINT = API_DOMAIN + '/api/keywords/extract'
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
curl "https://api.scholarcy.com/api/keywords/extract" \
  -H "Authorization: Bearer abcdefg" \
  -d "url=https://www.nature.com/articles/s41746-019-0180-3" \
  -d "start_page=1"

```


> The above command returns JSON structured as for the POST endpoint:

> The above command returns JSON structured like this:

```json
{
  "filename": "article2016.pdf",
  "abbreviations": {
    "U&G": "uses and gratification",
    "SNS": "social networking sites",
    "COBRA": "Consumer Online Brand-Related Activity",
    "AVE": "average variance extracted",
    "CLF": "common latent factor"
  },
  "keywords": [
    {
      "term": "Facebook",
      "url": "https://en.wikipedia.org/wiki/Facebook"
    },
    {
      "term": "typology",
      "url": "https://en.wikipedia.org/wiki/typology"
    },
    {
      "term": "social media",
      "url": "https://en.wikipedia.org/wiki/social_media"
    },
    {
      "term": "consumer behavior",
      "url": "https://en.wikipedia.org/wiki/consumer_behavior"
    },
    {
      "term": "average variance extracted",
      "url": "https://en.wikipedia.org/wiki/average_variance_extracted"
    },
    {
      "term": "online branding",
      "url": "https://en.wikipedia.org/wiki/online_branding"
    },
    {
      "term": "cluster analysis",
      "url": "https://en.wikipedia.org/wiki/cluster_analysis"
    },
    {
      "term": "social networking sites",
      "url": "https://en.wikipedia.org/wiki/social_networking_sites"
    },
    {
      "term": "brand manager",
      "url": "https://en.wikipedia.org/wiki/brand_manager"
    }
  ],
  "keyword_relevance": {
    "Facebook": 0.3834808259587021,
    "social media": 0.16224188790560473,
    "social networking sites": 0.08259587020648967,
    "brand interaction": 0.07964601769911504,
    "typology": 0.038348082595870206,
    "brand manager": 0.038348082595870206,
    "uses and gratification": 0.035398230088495575,
    "consumer interaction": 0.032448377581120944,
    "consumer behavior": 0.02359882005899705,
    "brand communication": 0.02064896755162242,
    "cluster analysis": 0.017699115044247787,
    "main motivation": 0.017699115044247787,
    "Consumer Online Brand-Related Activity": 0.014749262536873156,
    "average variance extracted": 0.011799410029498525,
    "common latent factor": 0.008849557522123894,
    "online branding": 0.008849557522123894,
  }
}
```

> The above command can also returns CSV structured like this:

```csv
"filename","key term","wikipedia_link"
"article2016.pdf","Facebook","https://en.wikipedia.org/wiki/Facebook"
"article2016.pdf","typology","https://en.wikipedia.org/wiki/typology"
"article2016.pdf","social media","https://en.wikipedia.org/wiki/social_media"
"article2016.pdf","consumer behavior","https://en.wikipedia.org/wiki/consumer_behavior"
"article2016.pdf","average variance extracted","https://en.wikipedia.org/wiki/average_variance_extracted"
"article2016.pdf","online branding","https://en.wikipedia.org/wiki/online_branding"
"article2016.pdf","cluster analysis","https://en.wikipedia.org/wiki/cluster_analysis"
"article2016.pdf","social networking sites","https://en.wikipedia.org/wiki/social_networking_sites"
"article2016.pdf","brand manager","https://en.wikipedia.org/wiki/brand_manager"
```

This endpoint extracts key terms from a remote URL.
The remote URL can resolve to a document type of any of the formats listed for the POST endpoint

### HTTP Request

`GET http://api.scholarcy.com/api/keywords/extract`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
url | null | URL of public, open-access document.
text | null | Plain text content to be processed.
start_page | 1 | Start reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
end_page | null | Stop reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
external_metadata | false | If true, fetch article metadata from the relevant remote repository (e.g. CrossRef).
wiki_links | false | If true, map extracted key terms to their Wikipedia pages
sampling | representative | For large documents, when extracting key terms, use either a representative sample of the full content, or the fulltext content.
extract_snippets | true | If true, sample snippets from each section, otherwise, sample the full text.
output_format | json | json or CSV.

<aside class="success">
Remember to include your Authentication token with every request.
</aside>
