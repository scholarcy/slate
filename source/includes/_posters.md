# Generate a Poster

These API endpoints will extract the information needed to populate data into your own poster-creation services, and will also generate a basic Powerpoint template for you to use as a starting point for editing.

## POST a local file to generate a poster

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/posters/generate'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}
file_path = '/path/to/local/file.pdf'

request = RestClient::Request.new(
          :method => :post,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :multipart => true,
            :file => File.new(file_path, 'rb'),
            :type => 'headline',
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
POST_ENDPOINT = API_DOMAIN + '/api/posters/generate'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

with open(file_path, 'rb') as file_data:
    file_payload = {
    'file': file_data,
    'type': 'headline',
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
curl "https://api.scholarcy.com/api/posters/generate" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf" \
  -F "type=headline" \
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
    "references": [
      "1. Smith, J. (2001) A study on self-citation. J. Chem. Biol., 123, 456-789.",
      "2. Jones, R. (2015) He didn't write this one. Science, 101, 101010."
    ],
    "emails": [
      "smith.j@uni.ac.uk"
    ],
    "figure_captions": [
      {
        "id": "1",
        "caption": "Figure 1 caption"
      },
      {
        "id": "2",
        "caption": "Figure 2 caption"
      }
    ],
    "figure_urls": [
      "https://api.scholarcy.com/images/file.pdf_agtuhsnt_images_1x1uj_5t/img-000.png",
      "https://api.scholarcy.com/images/file.pdf_agtuhsnt_images_1x1uj_5t/img-002.png"
    ],
    "poster_url": "https://api.scholarcy.com/posters/file.pdf_agtuhsnt.pptx",
    "keywords": [
      "atomic force microscopy",
      "dna nanostructure",
      "drug release",
      "single-stranded DNA",
      "double-stranded DNA",
      "dox molecule"
    ],
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
    "summary": {
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
}
```

This endpoint generates a poster from a local file. File formats supported are:

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

`POST http://api.scholarcy.com/api/posters/generate`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
file | null | A file object.
url | null | URL of public, open-access document.
type | full | The type of poster to generate. `full` will create a large, landscape poster with blocks for each section. `headline` will create a portrait poster containing the main takeaway finding and a single image.
start_page | 1 | Start reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
end_page | null | Stop reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.


<aside class="success">
Remember to include your Authentication token with every request.
</aside>


## GET a poster from a URL

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/posters/generate'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}
file_path = '/path/to/local/file.pdf'

request = RestClient::Request.new(
          :method => :post,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :multipart => true,
            :url => 'https://www.nature.com/articles/s41746-019-0180-3',
            :type => 'full',
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
POST_ENDPOINT = API_DOMAIN + '/api/posters/generate'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_payload = {
'url': 'https://www.nature.com/articles/s41746-019-0180-3',
'type': 'full',
'start_page': 1
}
r = requests.post(POST_ENDPOINT,
      headers=headers,
      files=file_payload,
      timeout=timeout)
print(r.json())

```

```shell
curl "https://api.scholarcy.com/api/posters/generate" \
  -H "Authorization: Bearer abcdefg" \
  -d "url=https://www.nature.com/articles/s41746-019-0180-3" \
  -d "type=full" \
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
    ],
    "identifiers": {
      "arxiv": null,
      "doi": "10.1010/101010.10.10.1101010",
      "isbn": null,
      "doc_id": null
    },
    "abstract": "This is a very exciting paper. Please read it.",
    "references": [

    ],
    "emails": [
      "smith.j@uni.ac.uk"
    ],
    "figure_captions": [
      {
        "id": "1",
        "caption": "Figure 1 caption"
      },
      {
        "id": "2",
        "caption": "Figure 2 caption"
      }
    ],
    "figure_urls": [

    ],
    "poster_url": "https://api.scholarcy.com/posters/file.pdf_agtuhsnt.pptx",
    "keywords": [
    ],
    "abbreviations": {
    },
    "headline": "We prove some important facts in this paper",
    "highlights": [
    ],
    "summary": {
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
}
```

This endpoint generates a poster from a remote URL.
The remote URL can resolve to a document type of any of the formats listed for the POST endpoint

### HTTP Request

`GET http://api.scholarcy.com/api/posters/generate`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
url | null | URL of public, open-access document.
type | full | The type of poster to generate. `full` will create a large, landscape poster with blocks for each section. `headline` will create a portrait poster containing the main takeaway finding and a single image.
start_page | 1 | Start reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
end_page | null | Stop reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.

<aside class="success">
Remember to include your Authentication token with every request.
</aside>
