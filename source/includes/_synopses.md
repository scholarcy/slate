# Generate an Abstractive Synopsis

The API endpoints at `https://summarizer.scholarcy.com/summarize` will generate a short synopsis (70-100 words) or a mini-review (around 150-300 words).

## POST a local file to generate a synopsis

```ruby
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
            :file => File.new(file_path, 'rb'),
            :wiki_links => true,
            :format_summary => true
          })
response = request.execute
puts(response.body)
```

```python
import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://summarizer.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/summarize'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

with open(file_path, 'rb') as file_data:
    file_payload = {
    'file': file_data,
    'wiki_links': True,
    'format_summary': True
    }
    r = requests.post(POST_ENDPOINT,
          headers=headers,
          files=file_payload,
          timeout=timeout)
    print(r.json())

```

```shell
curl "https://summarizer.scholarcy.com/summarize" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf" \
  -F "wiki_links=true" \
  -F "format_summary=true"
```


> The above command returns JSON structured like this:

```json
{
    "response": {
        "abbreviations": {
            "EPOSA": "European Project on OSteoArthritis",
            "GPS": "Global Positioning System",
            "ISD": "Integrated Surface Database",
            "OR": "odds ratio"
        },
        "headline": "Researchers have used smartphone data to investigate the relationship between pain and weather conditions, and found that there is a small but significant relationship.",
        "keywords": [
            {
                "term": "physical activity",
                "url": "https://en.wikipedia.org/wiki/physical_activity"
            },
            {
                "term": "osteoarthritis",
                "url": "https://en.wikipedia.org/wiki/osteoarthritis"
            },
            {
                "term": "atmospheric pressure",
                "url": "https://en.wikipedia.org/wiki/atmospheric_pressure"
            },
            {
                "term": "rheumatoid arthritis",
                "url": "https://en.wikipedia.org/wiki/rheumatoid_arthritis"
            },
            {
                "term": "Global Positioning System",
                "url": "https://en.wikipedia.org/wiki/Global_Positioning_System"
            },
            {
                "term": "fibromyalgia",
                "url": "https://en.wikipedia.org/wiki/fibromyalgia"
            },
            {
                "term": "arthritis",
                "url": "https://en.wikipedia.org/wiki/arthritis"
            },
            {
                "term": "smartphone app",
                "url": "https://en.wikipedia.org/wiki/smartphone_app"
            },
            {
                "term": "relative humidity",
                "url": "https://en.wikipedia.org/wiki/relative_humidity"
            },
            {
                "term": "chronic pain",
                "url": "https://en.wikipedia.org/wiki/chronic_pain"
            },
            {
                "term": "odds ratio",
                "url": "https://en.wikipedia.org/wiki/odds_ratio"
            },
            {
                "term": "Parkinson disease",
                "url": "https://en.wikipedia.org/wiki/Parkinson_disease"
            },
            {
                "term": "joint pain",
                "url": "https://en.wikipedia.org/wiki/joint_pain"
            },
            {
                "term": "wind speed",
                "url": "https://en.wikipedia.org/wiki/wind_speed"
            },
            {
                "term": "cohort study",
                "url": "https://en.wikipedia.org/wiki/cohort_study"
            }
        ],
        "message": "",
        "metadata": {
            "citation": "William G. Dixon, Anna L. Beukenhorst, Belay B. Yimer, Louise Cook, Antonio Gasparrini, Tal El-Hay, Bruce Hellman, Ben James, Ana M. Vicedo-Cabrera, Malcolm Maclure, Ricardo Silva, John Ainsworth, Huai Leng Pisaniello, Thomas House, Mark Lunt, Carolyn Gamble, Caroline Sanders, David M. Schultz, Jamie C. Sergeant, John McBeth (2019). How the weather affects the pain of citizen scientists using a smartphone app. npj Digital Medicine 2. https://www.nature.com/articles/s41746-019-0180-3",
            "citation_affiliation": "",
            "citation_author": "William G. Dixon et al.",
            "citation_date": 2019,
            "citation_title": "How the weather affects the pain of citizen scientists using a smartphone app",
            "citation_url": "https://www.nature.com/articles/s41746-019-0180-3"
        },
        "readership_level": "technical-readership-accurate",
        "summary": "<a class=\"has-tooltip\" title=\"Read the article\" target=\"_blank\" href=\"https://www.nature.com/articles/s41746-019-0180-3\">William Dixon et al. (2019)</a> studied how the weather affects the pain of citizen scientists using a smartphone app. Weather has been thought to affect symptoms in patients with chronic disease since the time of Hippocrates over 2000 years ago.\nMultivariable case-crossover analysis including the four state weather variables demonstrated that an increase in relative humidity was associated with a higher odds of a pain event with an OR of 1.139 (95% confidence interval 1.099\u20131.181) per 10 percentage point increase.\nThis study has demonstrated that higher relative humidity and wind speed, and lower atmospheric pressure, were associated with increased pain severity in people with long-term pain conditions.\nThe \u2018worst\u2019 combination of weather variables would increase the odds of a pain event by just over 20% compared to an average day.<br/><br/>There were 2658 patients involved in the research. Discussing potential improvements, \u201cThere are potential limitations to this study.\nIt is possible only people with a strong belief in a weather\u2013pain relationship participated.\nRain and cold weather were the most common pre-existing beliefs, authors say,\u201d they admit. ",
        "title": "How the weather affects the pain of citizen scientists using a smartphone app"
    }
}
```

This endpoint generates a synopsis from a local file. File formats supported are:

- PDF (.pdf)
- Word (.docx)
- Rich Text (.rtf)
- Powerpoint (.pptx)
- BibTeX (.bib)
- RIS (.ris)
- XML (.xml)
- HTML (.html or .htm)
- Plain Text (.txt)
- LaTeX (.tex)


### HTTP Request

`POST http://summarizer.scholarcy.com/summarize`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
file | null | A file object.
url | null | URL of public, open-access document. Can be a DOI but must be qualified with a resolver domain, e.g. <https://doi.org/10.1177/0846537120913497>
input_text | null | You can pass a text string directly to the endpoint, instead of uploading a file or passing a URL.
structured_summary | false | Take the document structure into account, considering specific sections such as Introduction, Background, Methods, Results, Discussion, Conclusion.
summary_type | combined | Level of detail of summary: `overview`: abstractive synopsis of Scholarcy highlights. `detail`: abstractive synopsis of Scholarcy summary. `combined`: union of overview and detail. `merged`: an abstractive synopsis of the union of Scholarcy highlights and Scholarcy summary.
focus_level | 4 | This internal hyperparameter controls whether the summary takes a narrow focus on a specific fact or a wider focus on multiple facts within the source. `4`: wide focus. `3`: medium focus. `2`: narrow focus. `1`: narrowest focus
readership_level | technical-readership- accurate | This controls the level of language complexity and amount of paraphrasing in the output. `technical-readership-accurate`: output is for a technical/academic reader with a high level of factual accuracy in relation to the source text. `technical-readership-fast`: output is for a technical/academic reader and provides a little more paraphrasing, which may result in a slight loss in accuracy. However, it is 2x faster than `technical-readership-accurate`. `lay-readership-accurate`: output is for a lay/non-expert reader, with moderate paraphrasing and good level of accuracy in relation to the source text. `lay-readership-fast`: output is for a lay/non- expert reader, with much paraphrasing and reasonable level of accuracy in relation to the source text. However, it is 2x faster than `lay-readership-accurate`.
wiki_links | false | Map extracted key terms to Wikipedia entries.
format_summary | false |  Format the summary so it can be more easily used as part of a referenced report: 1) Personal pronouns referring to the authors are replaced with the author names. 2) The summary is correctly cited with author and date. 3) A formatted reference to the source is generated


<aside class="success">
Remember to include your Authentication token with every request.
</aside>


## GET a synopsis from a URL

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://summarizer.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/summarize'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}

request = RestClient::Request.new(
          :method => :get,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :url => 'https://www.nature.com/articles/s41746-019-0180-3',
            :wiki_links => true,
            :format_summary => true
          })
response = request.execute
puts(response.body)

```

```python
import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://summarizer.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/summarize'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

payload = {
'url': 'https://www.nature.com/articles/s41746-019-0180-3',
'wiki_links': True,
'format_summary': True
}
r = requests.get(POST_ENDPOINT,
      headers=headers,
      params=payload,
      timeout=timeout)
print(r.json())

```

```shell
curl "https://summarizer.scholarcy.com/summarize" \
  -H "Authorization: Bearer abcdefg" \
  -d "url=https://www.nature.com/articles/s41746-019-0180-3" \
  -d "wiki_links=true" \
  -d "format_summary=true"

```


> The above command returns JSON structured as for the POST endpoint:

```json
{
    "response": {
        "abbreviations": {
            "EPOSA": "European Project on OSteoArthritis",
            "GPS": "Global Positioning System",
            "ISD": "Integrated Surface Database",
            "OR": "odds ratio"
        },
        "headline": "Researchers have used smartphone data to investigate the relationship between pain and weather conditions, and found that there is a small but significant relationship.",
        "keywords": [
            {
                "term": "physical activity",
                "url": "https://en.wikipedia.org/wiki/physical_activity"
            },
            {
                "term": "osteoarthritis",
                "url": "https://en.wikipedia.org/wiki/osteoarthritis"
            },
            {
                "term": "atmospheric pressure",
                "url": "https://en.wikipedia.org/wiki/atmospheric_pressure"
            },
            {
                "term": "rheumatoid arthritis",
                "url": "https://en.wikipedia.org/wiki/rheumatoid_arthritis"
            },
            {
                "term": "Global Positioning System",
                "url": "https://en.wikipedia.org/wiki/Global_Positioning_System"
            },
            {
                "term": "fibromyalgia",
                "url": "https://en.wikipedia.org/wiki/fibromyalgia"
            },
            {
                "term": "arthritis",
                "url": "https://en.wikipedia.org/wiki/arthritis"
            },
            {
                "term": "smartphone app",
                "url": "https://en.wikipedia.org/wiki/smartphone_app"
            },
            {
                "term": "relative humidity",
                "url": "https://en.wikipedia.org/wiki/relative_humidity"
            },
            {
                "term": "chronic pain",
                "url": "https://en.wikipedia.org/wiki/chronic_pain"
            },
            {
                "term": "odds ratio",
                "url": "https://en.wikipedia.org/wiki/odds_ratio"
            },
            {
                "term": "Parkinson disease",
                "url": "https://en.wikipedia.org/wiki/Parkinson_disease"
            },
            {
                "term": "joint pain",
                "url": "https://en.wikipedia.org/wiki/joint_pain"
            },
            {
                "term": "wind speed",
                "url": "https://en.wikipedia.org/wiki/wind_speed"
            },
            {
                "term": "cohort study",
                "url": "https://en.wikipedia.org/wiki/cohort_study"
            }
        ],
        "message": "",
        "metadata": {
            "citation": "William G. Dixon, Anna L. Beukenhorst, Belay B. Yimer, Louise Cook, Antonio Gasparrini, Tal El-Hay, Bruce Hellman, Ben James, Ana M. Vicedo-Cabrera, Malcolm Maclure, Ricardo Silva, John Ainsworth, Huai Leng Pisaniello, Thomas House, Mark Lunt, Carolyn Gamble, Caroline Sanders, David M. Schultz, Jamie C. Sergeant, John McBeth (2019). How the weather affects the pain of citizen scientists using a smartphone app. npj Digital Medicine 2. https://www.nature.com/articles/s41746-019-0180-3",
            "citation_affiliation": "",
            "citation_author": "William G. Dixon et al.",
            "citation_date": 2019,
            "citation_title": "How the weather affects the pain of citizen scientists using a smartphone app",
            "citation_url": "https://www.nature.com/articles/s41746-019-0180-3"
        },
        "readership_level": "technical-readership-accurate",
        "summary": "<a class=\"has-tooltip\" title=\"Read the article\" target=\"_blank\" href=\"https://www.nature.com/articles/s41746-019-0180-3\">William Dixon et al. (2019)</a> studied how the weather affects the pain of citizen scientists using a smartphone app. Weather has been thought to affect symptoms in patients with chronic disease since the time of Hippocrates over 2000 years ago.\nMultivariable case-crossover analysis including the four state weather variables demonstrated that an increase in relative humidity was associated with a higher odds of a pain event with an OR of 1.139 (95% confidence interval 1.099\u20131.181) per 10 percentage point increase.\nThis study has demonstrated that higher relative humidity and wind speed, and lower atmospheric pressure, were associated with increased pain severity in people with long-term pain conditions.\nThe \u2018worst\u2019 combination of weather variables would increase the odds of a pain event by just over 20% compared to an average day.<br/><br/>There were 2658 patients involved in the research. Discussing potential improvements, \u201cThere are potential limitations to this study.\nIt is possible only people with a strong belief in a weather\u2013pain relationship participated.\nRain and cold weather were the most common pre-existing beliefs, authors say,\u201d they admit. ",
        "title": "How the weather affects the pain of citizen scientists using a smartphone app"
    }
}
```

This endpoint generates synopsis from a remote URL.
The remote URL can resolve to a document type of any of the formats listed for the POST endpoint

### HTTP Request

`GET http://summarizer.scholarcy.com/summarize`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
url | null | URL of public, open-access document. Can be a DOI but must be qualified with a resolver domain, e.g. <https://doi.org/10.1177/0846537120913497>
input_text | null | You can pass a text string directly to the endpoint, instead of uploading a file or passing a URL.
structured_summary | false | Take the document structure into account, considering specific sections such as Introduction, Background, Methods, Results, Discussion, Conclusion.
summary_type | combined | Level of detail of summary: `overview`: abstractive synopsis of Scholarcy highlights. `detail`: abstractive synopsis of Scholarcy summary. `combined`: union of overview and detail. `merged`: an abstractive synopsis of the union of Scholarcy highlights and Scholarcy summary.
focus_level | 4 | This internal hyperparameter controls whether the summary takes a narrow focus on a specific fact or a wider focus on multiple facts within the source. `4`: wide focus. `3`: medium focus. `2`: narrow focus. `1`: narrowest focus
readership_level | technical-readership- accurate | This controls the level of language complexity and amount of paraphrasing in the output. `technical-readership-accurate`: output is for a technical/academic reader with a high level of factual accuracy in relation to the source text. `technical-readership-fast`: output is for a technical/academic reader and provides a little more paraphrasing, which may result in a slight loss in accuracy. However, it is 2x faster than `technical-readership-accurate`. `lay-readership-accurate`: output is for a lay/non-expert reader, with moderate paraphrasing and good level of accuracy in relation to the source text. `lay-readership-fast`: output is for a lay/non- expert reader, with much paraphrasing and reasonable level of accuracy in relation to the source text. However, it is 2x faster than `lay-readership-accurate`.
wiki_links | false | Map extracted key terms to Wikipedia entries.
format_summary | false |  Format the summary so it can be more easily used as part of a referenced report: 1) Personal pronouns referring to the authors are replaced with the author names. 2) The summary is correctly cited with author and date. 3) A formatted reference to the source is generated

<aside class="success">
Remember to include your Authentication token with every request.
</aside>
