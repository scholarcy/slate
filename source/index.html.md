---
title: Scholarcy API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python

toc_footers:
  - <a href='https://developers.scholarcy.com'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - posters
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the Scholarcy API. We have a number of endpoints for extracting machine-readable knowledge from documents in many formats.
The service is optimised to work with research papers and articles, but should provide useful results for any document in any format.

We provid examples Shell, Ruby, and Python. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Authentication

> Authentication headers must be sent with every request:

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
            :file => File.new(file_path, 'rb')
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
    file_payload = {'file': file_data}
    r = requests.post(POST_ENDPOINT,
          headers=headers,
          files=file_payload,
          timeout=timeout)
    print(r.json())
```

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.scholarcy.com/api/posters/generate" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf"

  curl "https://api.scholarcy.com/api/posters/generate" \
    -H "Authorization: Bearer abcdefg" \
    -d "url=https://www.nature.com/articles/s41746-019-0180-3"
```

> Make sure to replace `abcdefg` with your API key.

We currently have an open API which can be used without authentication for a limited number of
documents per day with documents below 7MB in size.

For unauthenticated use, please omit the `Authorization` header,
or pass an empty string for the `Bearer` token.

For full, authenticated use of the API, you may contact us for an API key - please see our [Pricing page](https://www.scholarcy.com/pricing/).

Soon you will be able to self-register an Scholarcy API key at our [developer portal](https://developers.scholarcy.com).

The Scholarcy API expects the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer abcdefg`

<aside class="notice">
You must replace <code>abcdefg</code> with your personal API key.
</aside>
