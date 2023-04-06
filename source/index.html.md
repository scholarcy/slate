---
title: Scholarcy API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell
  - ruby
  - python

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - posters
  - highlights
  - metadata
  - keywords
  - synopses
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

Welcome to the Scholarcy APIs. We have two core API servies:

1. **Metadata extraction API** at <https://api.scholarcy.com>
   This is a developer API that comprises a number of endpoints for extracting machine-readable knowledge as JSON data from documents in many formats.
   The service is optimised to work with research papers and articles, but should provide useful results for any document in any format.
2. **Synopsis API** at <https://summarizer.scholarcy.com/>
   This includes a web front end for testing and a developer endpoint at `/summarize`

We provide examples in Shell, Ruby, and Python. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> Authentication headers must be sent with every request:

```ruby

# 1. Metadata API:

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
            :file => File.new(file_path, 'rb')
          })
response = request.execute
puts(response.body)
```

```ruby

# 2. Synopsis API:

require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://summarizer.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/summarize'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}
file_path = '/path/to/local/file.pdf'

request = RestClient::Request.new(
          :method => :post,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :multipart => true,
            :file => File.new(file_path, 'rb')
          })
response = request.execute
puts(response.body)
```

```python

# 1. Metadata API:

import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/posters/generate'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

with open(file_path, 'rb') as file_data:
    file_payload = {'file': file_data}
    r = requests.post(POST_ENDPOINT,
          headers=headers,
          files=file_payload,
          timeout=timeout)
    print(r.json())
```

```python

# 2. Synopsis API:

import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://summarizer.scholarcy.com/'
POST_ENDPOINT = API_DOMAIN + '/summarize'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

with open(file_path, 'rb') as file_data:
    file_payload = {'file': file_data}
    r = requests.post(POST_ENDPOINT,
          headers=headers,
          files=file_payload,
          timeout=timeout)
    print(r.json())
```

```shell

# 1. Metadata API:

# With shell, you can just pass the correct header with each request
curl "https://api.scholarcy.com/api/posters/generate" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf"

  curl "https://api.scholarcy.com/api/posters/generate" \
    -H "Authorization: Bearer abcdefg" \
    -d "url=https://www.nature.com/articles/s41746-019-0180-3"
```

```shell

# 2. Synopsis API:

# With shell, you can just pass the correct header with each request
curl "https://summarizer.scholarcy.com/summarize" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf"

  curl "https://summarizer.scholarcy.com/summarize" \
    -H "Authorization: Bearer abcdefg" \
    -d "url=https://www.nature.com/articles/s41746-019-0180-3"
```

> Make sure to replace `abcdefg` with your API key.

Each API service (metadata and synopsis) requires a separate key.

The Scholarcy APIs expects the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer abcdefg`

<aside class="notice">
You must replace <code>abcdefg</code> with your partner API key.
</aside>
