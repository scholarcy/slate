# Extract Structured Content

The API endpoints at `https://api.scholarcy.com/api/metadata/extract` and `/api/metadata/basic` will convert a document into structured, machine-readable data in JSON format.

## POST a local file to extract content

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/metadata/extract'
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
POST_ENDPOINT = API_DOMAIN + '/api/metadata/extract'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

params = {'start_page': 24, 'end_page': 37}
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
curl "https://api.scholarcy.com/api/metadata/extract" \
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
    "references": [],
    "emails": [
      "author@email.com"
    ],
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
    ],
    "table_captions": [
      {
        "id": "1",
        "caption": "Sample demographics and characteristics"
      },
      {
        "id": "2",
        "caption": "Construct measurements"
      },
    ],
    "figure_captions": []
  },
  "sections": {
    "introduction": [
      "Introduction section contents"
    ],
    "methodology": [
      "Methods section contents"
    ],
    "findings": [
      "Main results contents"
    ],
    "conclusion": [
      "Concluding remarks"
    ],
    "limitations": [
      "There are also several limitations to this research. Small sample size was an issue."
    ],
    "acknowledgements": [
      "We'd like to thank our supervisors for support, tea and biscuits."
    ],
    "funding": [
      "The authors acknowledge financial support from Fundação para a Ciência e a Tecnologia and FEDER/COMPETE (grant AAA/BBB/04007/2019)"
    ],
    "future_work": [
      "More research is needed to better understand what is going on."
    ],
    "objectives": [
      "The aim of this research is to provide insights into the inner workings of cellular processes."
    ]
  },
  "structured_content": [
    {
      "heading": "ABSTRACT",
      "content": [
        "This is a very exciting paper. Please read it."
      ]
    },
    {
      "heading": "INTRODUCTION",
      "content": [
        "Introduction paragraph 1",
        "Introduction paragraph 2"
      ]
    },
    {
      "heading": "RESEARCH METHODOLOGY",
      "content": [
        "Methods paragraph 1",
        "Methods paragraph 2"
      ]
    },
    {
      "heading": "FINDINGS AND DISCUSSION",
      "content": [
        "Results paragraph 1",
        "Results paragraph 2",
      ]
    },
    {
      "heading": "CONCLUSION",
      "content": [
        "Conclusion paragraph 1",
        "Conclusion paragraph 2",
      ]
    }
  ],
  "participants": [
    {
      "participant": "Patients",
      "number": 15,
      "context": "Fifteen patients participated in the study."
    },
  ],
  "statistics": [
    {
      "tests": {
        "context": "We performed exploratory factor analysis using SPSS 20",
        "tests": [
          {
            "test": "exploratory factor analysis"
          }
        ]
      }
    },
    {
      "tests": {
        "context": "We performed confirmatory factor analyses with AMOS 20 using the maximum likelihood estimation method",
        "tests": [
          {
            "test": "confirmatory factor analyses"
          },
          {
            "test": "maximum likelihood estimation method"
          }
        ]
      }
    },
    {
      "p_value": "P < 0.001",
      "context": "We also noted significant differences when we performed a one-way between-groups analysis of variance on each of the 14 items (P < 0.001)</mark>",
      "tests": {
        "tests": [
          {
            "test": "analysis of variance",
            "value": "P < 0.001"
          }
        ]
      }
    }
  ],
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
  "top_statements": [
    "Facts are very important.",
    "The force is strong in this one.",
    "We ran some tests and this is what we found"
  ],
  "findings": [
    "A statistically significant difference was noted between the four groups on the combined dependent variables",
    "We also noted significant differences when we performed a one-way between-groups analysis of variance on each of the 14 items (P < 0.001)"
  ],
  "facts": [],
  "claims": [],
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

This endpoint extracts structured content from a local file. File formats supported are:

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

`POST http://api.scholarcy.com/api/metadata/extract`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
file | null | A file object.
url | null | URL of public, open-access document.
text | null | Plain text content to be processed.
start_page | 1 | Start reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
end_page | null | Stop reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
external_metadata | false | If true, fetch article metadata from the relevant remote repository (e.g. CrossRef).
parse_references | false | If true, parse into BibTeX and link each reference to its full text location.
reference_style | ensemble | Referencing style used by the document, if known, or use the default. Available values : acs, ama, anystyle, apa, chicago, ensemble, experimental, harvard, ieee, mhra, mla, nature, vancouver.
reference_format | text | Output references in plain text or bibtex format.
generate_summary | true | Create an extractive summary of the article.
summary_engine | v1 | v1: Best for articles. v2: best for book chapters.
replace_pronouns | false | If true, replace first-person pronouns in summary with third-person mentions (the author(s)?, they).
strip_dialogue | false | If true, remove dialog and quoted text from input prior to summarising.
summary_size | 400 | Length of summary in words.
summary_percent | 0 | Length of summary as a % of the original article.
structured_summary | false | If true, summarise each of the main sections separately, and then provide a summary structured according to those sections.
keyword_method | sgrank+acr | Available values : sgrank, sgrank+np, sgrank+acr, textrank, np, regex.
keyword_sample | representative | For large documents, when extracting key terms, use either a representative sample of the full content, or the fulltext content.
keyword_limit | 25 | Target number of key terms to extract.
abbreviation_method | schwartz | Select an abbreviation extraction method. Available values: schwartz, statistical, ensemble.
wiki_links | false | If true, map extracted key terms to their Wikipedia pages.
extract_facts | true | Extract SVO-style factual statements from the article.
extract_claims | true | Extract specific claims made by the article.
key_points | 5 | The number of key points/key takeaway items to extract.
citation_contexts | false | If true, extract the inline citation contexts (preceding and current sentences).
inline_citation_links | false | If true, link inline citations to their identifiers in the references.
extract_pico | true | Extract population, intervention, control, outcome data.
extract_tables | false | If true, extract tabular data as CSV/Excel files.
extract_figures | false | If true, extract figures and images as PNG files.
require_captions | true | Requires an accompanying caption to trigger figure/table extraction.
extract_sections | true | Extracts section headers and paragraphs.
include_bodytext | true | If extracting sections, includes the main body text content for each section.
unstructured_content | false | If true, include a raw, unstructured text dump of the file.
extract_snippets | true | If true, sample snippets from each section, otherwise, sample the full text.
engine | v1 | PDF text extraction engine. v1: best general purpose. v2: best for articles containing marginal line numbering or narrow column gutters.
image_engine | v1 | Image extraction engine. v1: best for bitmap images. v2: best for line images. Available values : v1, v2, v1+v2.

<aside class="success">
Remember to include your Authentication token with every request.
</aside>


### Output fields

Field     | Description
--------- | -----------
filename  | The filename of the uploaded document, or input URL slug
content_type | The file or URL MIME type
**metadata** | Structured article metadata
  message | Any error or status messages
  title | Article title
  author | List of authors
  pages | Number of pages in the document
  date | Article date
  affiliations | Author affiliations
  journal | Journal title (from CrossRef)
  volume | Journal volume (from CrossRef)
  page | Journal page range (from CrossRef)
  cited_by | Citation count (from CrossRef)
  identifiers | Any identifier extracted from the document, such as DOI, ISBN, arXiv ID, or other identifier. <br /> If an open-access version of the paper is available, the URL to that version will be displayed here.
  abstract | The author-written abstract, if available, or a proxy for the abstract, such as background, introduction, preface etc.
  keywords | Author-supplied keywords
  references | The plain reference strings extracted from the end of the article, or from the footnotes
  emails | Email addresses of the authors
  type | Article type: journal-article, book-chapter, preprint, web-page, review-article, case-study, report
  references_ris | RIS parse of the references
  links | Any URLs identified in the document
  author_conclusions | Author-stated conclusions/takeaways
  funding | Funding statement structured as follows: `"award-group": [{"funding-source": "National Institutes of Health", "award-id": ["R43HL137469"] }]`
  table_captions | Table captions
  figure_captions | Figure captions
  tables_url | Link to download the tables as Excel
  figure_urls | List of links to download extracted images as PNG files
  word_count | A range representing maximum and minimum estimated word count. <br/> The maximum includes appendices and supplementary information. <br /> The minimum includes the core article body text. <br /> Both exclude references and footnotes.
  is_oa | Boolean flag if the document is open access or not. <br /> This flag is only present if the input is a DOI URL, e.g. https://doi.org/10.1177/0846537120913497
  oa_status | Open access status: closed, bronze, green, or gold. <br /> This flag is only present if the input is a DOI URL, e.g. https://doi.org/10.1177/0846537120913497
**sections** | Snippets from each main section in the article
  introduction, methods, results, conclusion | If section headings can be mapped to standard names such as Introduction, Methods, Results, Conclusions, these snippets are shown here
  funding | Any funding statements
  disclosures | Any disclosures of conflicts of interest
  ethical_compliance | Any information about consent and ethical regulations
  data_availability | Any information about data and code availability related to this study
  limitations | Any discussion of study limitations
  future_work | Any information about further research needed and future work
  registrations | Any study registration identifiers
structured_content | The section headings as they appear in the source document, along with their full section content.
participants | Quantifiable information about the study subjects
statistics | Information about statistical tests and analysis performed in the study
populations | Quantifiable information about the population background
keywords | A combination of the author-supplied keywords, plus new keywords or key terms extracted from the document
keyword_relevance | keywords ranked by their relevance scores
species | Any Latin species names detected
summary | An extractive summary of the main points of the entire article.
structured_summary | An extractive summary structured according to the main sections of the article.
reference_links | Shown if reference parsing has been enabled. <br /> This contains links to the full text for each of the references in the paper
facts | Subject-predicate-object statements expressed in the article
claims | Claims made by the authors of the study
findings | Any important, quantitative findings extracted from the document, such as statistically significant results
key_statements | A longer set of important sentences, from which the `top_statements` are selected.
top_statements | The top 3-7 key points in the document.  <br /> Typically, these highlights will include introductory and concluding information, as well as the main claims and findings of the article
headline | A short, one line summary of the entire article. <br /> This headline attempts to express the main finding or main result of the paper.
abbreviations | Abbreviations and their fully spelt out names, extracted from the document



## GET structured content from a URL

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/metadata/extract'
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
POST_ENDPOINT = API_DOMAIN + '/api/metadata/extract'
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
curl "https://api.scholarcy.com/api/metadata/extract" \
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
    "references": [],
    "emails": [
      "author@email.com"
    ],
    "funding": [],
    "table_captions": [],
    "figure_captions": []
  },
  "sections": {
    "introduction": [],
    "methodology": [],
    "findings": [],
    "conclusion": [],
    "limitations": [],
    "acknowledgements": [],
    "funding": [],
    "future_work": [],
    "objectives": []
  },
  "structured_content": [],
  "participants": [],
  "statistics": [],
  "keywords": [],
  "keyword_relevance": {},
  "abbreviations": {},
  "headline": "We prove some important facts in this paper",
  "top_statements": [],
  "findings": [],
  "facts": [],
  "claims": [],
  "summary": [],
  "structured_summary": {}
}
```

This endpoint extracts structured content from a remote URL.
The remote URL can resolve to a document type of any of the formats listed for the POST endpoint

### HTTP Request

`GET http://api.scholarcy.com/api/metadata/extract`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
url | null | URL of public, open-access document.
text | null | Plain text content to be processed.
start_page | 1 | Start reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
end_page | null | Stop reading the document from this page (PDF urls only). Useful for processing a single article/chapter within a larger file.
external_metadata | false | If true, fetch article metadata from the relevant remote repository (e.g. CrossRef).
parse_references | false | If true, parse into BibTeX and link each reference to its full text location.
reference_style | ensemble | Referencing style used by the document, if known, or use the default. Available values : acs, ama, anystyle, apa, chicago, ensemble, experimental, harvard, ieee, mhra, mla, nature, vancouver.
reference_format | text | Output references in plain text or bibtex format.
generate_summary | true | Create an extractive summary of the article.
summary_engine | v1 | v1: Best for articles. v2: best for book chapters.
replace_pronouns | false | If true, replace first-person pronouns in summary with third-person mentions (the author(s)?, they).
strip_dialogue | false | If true, remove dialog and quoted text from input prior to summarising.
summary_size | 400 | Length of summary in words.
summary_percent | 0 | Length of summary as a % of the original article.
structured_summary | false | If true, summarise each of the main sections separately, and then provide a summary structured according to those sections.
keyword_method | sgrank+acr | Available values : sgrank, sgrank+np, sgrank+acr, textrank, np, regex.
keyword_sample | representative | For large documents, when extracting key terms, use either a representative sample of the full content, or the fulltext content.
keyword_limit | 25 | Target number of key terms to extract.
abbreviation_method | schwartz | Select an abbreviation extraction method. Available values: schwartz, statistical, ensemble.
wiki_links | false | If true, map extracted key terms to their Wikipedia pages.
extract_facts | true | Extract SVO-style factual statements from the article.
extract_claims | true | Extract specific claims made by the article.
key_points | 5 | The number of key points/key takeaway items to extract.
citation_contexts | false | If true, extract the inline citation contexts (preceding and current sentences).
inline_citation_links | false | If true, link inline citations to their identifiers in the references.
extract_pico | true | Extract population, intervention, control, outcome data.
extract_tables | false | If true, extract tabular data as CSV/Excel files.
extract_figures | false | If true, extract figures and images as PNG files.
require_captions | true | Requires an accompanying caption to trigger figure/table extraction.
extract_sections | true | Extracts section headers and paragraphs.
include_bodytext | true | If extracting sections, includes the main body text content for each section.
unstructured_content | false | If true, include a raw, unstructured text dump of the file.
extract_snippets | true | If true, sample snippets from each section, otherwise, sample the full text.
engine | v1 | PDF text extraction engine. v1: best general purpose. v2: best for articles containing marginal line numbering or narrow column gutters.
image_engine | v1 | Image extraction engine. v1: best for bitmap images. v2: best for line images. Available values : v1, v2, v1+v2.

<aside class="success">
Remember to include your Authentication token with every request.
</aside>
