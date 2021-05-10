# Extract and Parse References

The API endpoint at
`https://ref.scholarcy.com/api/references/download`

extracts and downloads references into a variety of formats:

- BibTeX
- RIS
- XML (CrossRef format)
- JATS (XML)
- text
- CROCI (CSV format)

The API endpoint at
`https://ref.scholarcy.com/api/references/extract`

extracts references in JSON format and optionally provides a link resolver URL for each.

## POST a local file and download references in your chosen format

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://ref.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/references/download'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}
file_path = '/path/to/local/file.pdf'

request = RestClient::Request.new(
          :method => :post,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :multipart => true,
            :file => File.new(file_path, 'rb'),
            :reference_format => 'jats',
            :reference_style => 'ensemble'
          })
response = request.execute
puts(response.body)
```

```python
import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/references/download'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

with open(file_path, 'rb') as file_data:
    file_payload = {
    'file': file_data,
    'reference_format': 'jats',
    'reference_style': 'ensemble'
    }
    r = requests.post(POST_ENDPOINT,
          headers=headers,
          files=file_payload,
          timeout=timeout)
    print(r.json())

```

```shell
curl "https://ref.scholarcy.com/api/references/download" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf" \
  -F "reference_format=jats" \
  -F "reference_style=ensemble"
```


> The above command returns XML structured like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE article PUBLIC "-//NLM//DTD Journal Archiving and Interchange DTD v2.3 20070202//EN" "https://jats.nlm.nih.gov/archiving/1.1d1/JATS-archivearticle1.dtd">
<article xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mml="http://www.w3.org/1998/Math/MathML" article-type="research-article">
    <front>
        <article-meta>
            <article-id pub-id-type="doi">10.1371/journal.pone.0111913</article-id>
        </article-meta>
    </front>
    <back>
        <ref-list>
            <title>References</title>
            <ref id="ref_1">
                <element-citation publication-type="journal">
                    <person-group person-group-type="author">
                        <string-name>Teuten, E.</string-name>
                        <string-name>Rowland, S.</string-name>
                        <string-name>Galloway, T.</string-name>
                        <string-name>Thompson, R.</string-name>
                    </person-group>
                    <year>2007</year>
                    <article-title>Potential for plastics to transport hydrophobic contaminants</article-title>
                    <source>Environ Sci Technol</source>
                    <volume>41</volume>
                    <fpage>7759</fpage>
                </element-citation>
            </ref>

            <ref id="ref_2">
                <element-citation publication-type="journal">
                    <person-group person-group-type="author">
                        <string-name>Mato, Y.</string-name>
                        <string-name>Isobe, T.</string-name>
                        <string-name>Takada, H.</string-name>
                        <string-name>Kanehiro, H.</string-name>
                        <string-name>Ohtake, C.</string-name>
                    </person-group>
                    <year>2001</year>
                    <article-title>Plastic resin pellets as a transport medium for toxic chemicals in the marine environment</article-title>
                    <source>Environ Sci Technol</source>
                    <volume>35</volume>
                    <fpage>318</fpage>
                </element-citation>
            </ref>
          </ref-list>
      </back>
  </article>
```

This endpoint extracts references in the chosen output format from a local file. File formats supported are:

- PDF (.pdf)
- Word (.docx)
- Plain Text (.txt)


### HTTP Request

`POST http://ref.scholarcy.com/api/references/download`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
file | null | A file object.
document_type | full_paper | `full_paper`: a complete research paper, chapter or thesis. `bibliography`: a file containing just bibliographic items.
references | null | Optional. If you don't want to upload a file, you can pass a text string containing line-delimited references that you wish to parse into the desired output format.
reference_style | ensemble | Referencing style used by the document. If unsure, use the default `ensemble` or choose `experimental`. Other options include: `acs`, `ama`, `apa`, `chicago`, `harvard`, `ieee`, `mhra`, `mla`, `nature`, `vancouver`.
reference_format | ris | Output format. Options include: `bibtex`: standard BibTeX format. `ris`: Reference Interchange Specification (Endnote). `xml`: CrossRef's XML format. `jats`: The Journal Article Tag Suite XML format for references.
parent_doi | null | Only required for CrossRef XML output. If the DOI of the input document is not easily extractable from the document itself, then you can provide it here.
parent_title | null | Only requried for CrossRef XML output. If the title of the input document is not easily extractable from the document itself, then you can provide it here.
engine | v1 | PDF processing engine. `v1`: uses the XPDF tool and works best for most PDFs. `v2`: uses the poppler tool and may work better for PDFs with marginal line numbering, with multiple columns, or for those PDFs where `v1` fails to extract useful information.


<aside class="success">
Remember to include your Authentication token with every request.
</aside>


## GET references from a remote URL and download them in your chosen format

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://ref.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/references/download'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}

request = RestClient::Request.new(
          :method => :get,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :url => 'https://www.nature.com/articles/s41746-019-0180-3.pdf',
            :reference_format => 'jats',
            :reference_style => 'ensemble'
          })
response = request.execute
puts(response.body)

```

```python
import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://ref.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/references/download'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

payload = {
'url': 'https://www.nature.com/articles/s41746-019-0180-3.pdf',
'reference_format': 'jats',
'reference_style': 'ensemble'
}
r = requests.get(POST_ENDPOINT,
      headers=headers,
      params=payload,
      timeout=timeout)
print(r.json())

```

```shell
curl "https://ref.scholarcy.com/api/references/download" \
  -H "Authorization: Bearer abcdefg" \
  -d "url=https://www.nature.com/articles/s41746-019-0180-3.pdf" \
  -d "reference_format=jats" \
  -d "reference_style=ensemble"

```


> The above command returns XML structured as for the POST endpoint:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE article PUBLIC "-//NLM//DTD Journal Archiving and Interchange DTD v2.3 20070202//EN" "https://jats.nlm.nih.gov/archiving/1.1d1/JATS-archivearticle1.dtd">
<article xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mml="http://www.w3.org/1998/Math/MathML" article-type="research-article">
    <front>
        <article-meta>
            <article-id pub-id-type="doi">10.1371/journal.pone.0111913</article-id>
        </article-meta>
    </front>
    <back>
        <ref-list>
            <title>References</title>
            <ref id="ref_1">
                <element-citation publication-type="journal">
                    <person-group person-group-type="author">
                        <string-name>Teuten, E.</string-name>
                        <string-name>Rowland, S.</string-name>
                        <string-name>Galloway, T.</string-name>
                        <string-name>Thompson, R.</string-name>
                    </person-group>
                    <year>2007</year>
                    <article-title>Potential for plastics to transport hydrophobic contaminants</article-title>
                    <source>Environ Sci Technol</source>
                    <volume>41</volume>
                    <fpage>7759</fpage>
                </element-citation>
            </ref>
          </ref-list>
      </back>
  </article>
```

This endpoint extracts references in the chosen output format from a remote URL.
The remote URL can resolve to a document type of any of the formats listed for the POST endpoint

### HTTP Request

`GET http://ref.scholarcy.com/api/references/download`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
url | null | URL of public, open-access document.
document_type | full_paper | `full_paper`: a complete research paper, chapter or thesis. `bibliography`: a file containing just bibliographic items.
references | null | Optional. If you don't want to upload a file, you can pass a text string containing line-delimited references that you wish to parse into the desired output format.
reference_style | ensemble | Referencing style used by the document. If unsure, use the default `ensemble` or choose `experimental`. Other options include: `acs`, `ama`, `apa`, `chicago`, `harvard`, `ieee`, `mhra`, `mla`, `nature`, `vancouver`.
reference_format | ris | Output format. Options include: `bibtex`: standard BibTeX format. `ris`: Reference Interchange Specification (Endnote). `xml`: CrossRef's XML format. `jats`: The Journal Article Tag Suite XML format for references.
parent_doi | null | Only required for CrossRef XML output. If the DOI of the input document is not easily extractable from the document itself, then you can provide it here.
parent_title | null | Only requried for CrossRef XML output. If the title of the input document is not easily extractable from the document itself, then you can provide it here.
engine | v1 | PDF processing engine. `v1`: uses the XPDF tool and works best for most PDFs. `v2`: uses the poppler tool and may work better for PDFs with marginal line numbering, with multiple columns, or for those PDFs where `v1` fails to extract useful information.

<aside class="success">
Remember to include your Authentication token with every request.
</aside>



## POST a local file and return references as JSON data

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://ref.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/references/extract'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}
file_path = '/path/to/local/file.pdf'

request = RestClient::Request.new(
          :method => :post,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :multipart => true,
            :file => File.new(file_path, 'rb'),
            :reference_style => 'ensemble',
            :resolve_references => True
          })
response = request.execute
puts(response.body)
```

```python
import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://api.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/references/extract'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

file_path = '/path/to/local/file.pdf'

with open(file_path, 'rb') as file_data:
    file_payload = {
    'file': file_data,
    'reference_style': 'ensemble',
    'resolve_references': True
    }
    r = requests.post(POST_ENDPOINT,
          headers=headers,
          files=file_payload,
          timeout=timeout)
    print(r.json())

```

```shell
curl "https://ref.scholarcy.com/api/references/extract" \
  -H "Authorization: Bearer abcdefg" \
  -F "file=@/path/to/local/file.pdf" \
  -F "reference_style=ensemble" \
  -F "resolve_references=true"
```


> The above command returns JSON structured like this:

```json
{
  "filename": "plastic_pollution.pdf",
  "metadata": {
    "arxiv": null,
    "doi": "10.1371/journal.pone.0111913",
    "isbn": null,
    "date": 2014
  },
  "references": [
    "1. Teuten E, Rowland S, Galloway T, Thompson R (2007) Potential for plastics to transport hydrophobic contaminants. Environ Sci Technol 41: 7759–7764.",
    "2. Mato Y, Isobe T, Takada H, Kanehiro H, Ohtake C, et al. (2001) Plastic resin pellets as a transport medium for toxic chemicals in the marine environment. Environ Sci Technol 35: 318–324.",
    "3. Rochman C, Browne M, Halpern B, Hentschel B, Hoh E, et al. (2013) Classify plastic waste as hazardous. Nature 494: 169–171.",
    "4. Barnes D, Galgani F, Thompson R, Barlaz M (2009) Accumulation and fragmentation of plastic debris in global environments. Philos Trans R Soc Lond B Biol Sci 364: 1985–1998.",
    "5. Barnes D, Walters A, Goncalves L (2010) Macroplastics at sea around Antarctica. Mar Environ Res 70: 250–252.",
    "6. Law K, Moret-Ferguson S, Maximenko N, Proskurowski G, Peacock E, et al. (2010) Plastic accumulation in the North Atlantic Subtropical Gyre. Science 329: 1185–1188.",
    "7. Eriksen M, Maximenko N, Thiel M, Cummins A, Lattin G, et al. (2013) Plastic marine pollution in the South Pacific Subtropical Gyre. Mar Pollut Bull 68: 71–76.",
    "8. Goldstein M, Titmus A, Ford M (2013) Scales of spatial heterogeneity of plastic marine debris in the northeast Pacific Ocean, PloS one 8: doi: 10.1371/journal.pone.0080020.",
    "9. Law K, Moret-Ferguson S, Goodwin D, Zettler E, DeForce E, et al. (2014) Distribution of surface plastic debris in the eastern Pacific Ocean from an 11-year dataset. Environ Sci Technol: doi: 10.1021/ es4053076.",
    "10. Reisser J, Shaw J, Wilcox C, Hardesty B, Proietti M (2013) Marine plastic pollution in the waters around Australia: Characteristics, concentrations and pathways. PloS one 8: doi:10.1371/ journal.pone.0080466.",
  ],
  "bibtex": "@article{teuten2007a,\n  author = {Teuten, E. and Rowland, S. and Galloway, T. and Thompson, R.},\n  date = {2007},\n  title = {Potential for plastics to transport hydrophobic contaminants},\n  journal = {Environ Sci Technol},\n  volume = {41},\n  pages = {7759–7764},\n  language = {}\n}\n@article{mato2001a,\n  author = {Mato, Y. and Isobe, T. and Takada, H. and Kanehiro, H. and Ohtake, C.},\n  date = {2001},\n  title = {Plastic resin pellets as a transport medium for toxic chemicals in the marine environment},\n  journal = {Environ Sci Technol},\n  volume = {35},\n  pages = {318–324},\n  more-authors = {true},\n  language = {}\n}\n@article{rochman2013a,\n  author = {Rochman, C. and Browne, M. and Halpern, B. and Hentschel, B. and Hoh, E.},\n  date = {2013},\n  title = {Classify plastic waste as hazardous},\n  journal = {Nature},\n  volume = {494},\n  pages = {169–171},\n  more-authors = {true},\n  language = {}\n}\n@article{barnes2009a,\n  author = {Barnes, D. and Galgani, F. and Thompson, R. and Barlaz, M.},\n  date = {2009},\n  title = {Accumulation and fragmentation of plastic debris in global environments},\n  journal = {Philos Trans R Soc Lond B Biol Sci},\n  volume = {364},\n  pages = {1985–1998},\n  language = {}\n}\n@article{barnes2010a,\n  author = {Barnes, D. and Walters, A. and Goncalves, L.},\n  date = {2010},\n  title = {Macroplastics at sea around Antarctica},\n  journal = {Mar Environ Res},\n  volume = {70},\n  pages = {250–252},\n  language = {}\n}\n@article{law2010a,\n  author = {Law, K. and Moret-Ferguson, S. and Maximenko, N. and Proskurowski, G. and Peacock, E.},\n  date = {2010},\n  title = {Plastic accumulation in the North Atlantic Subtropical Gyre},\n  journal = {Science},\n  volume = {329},\n  pages = {1185–1188},\n  more-authors = {true},\n  language = {}\n}\n@article{eriksen2013a,\n  author = {Eriksen, M. and Maximenko, N. and Thiel, M. and Cummins, A. and Lattin, G.},\n  date = {2013},\n  title = {Plastic marine pollution in the South Pacific Subtropical Gyre},\n  journal = {Mar Pollut Bull},\n  volume = {68},\n  pages = {71–76},\n  more-authors = {true},\n  language = {}\n}\n@book{goldstein2013a,\n  author = {Goldstein, M. and Titmus, A. and Ford, M.},\n  date = {2013},\n  title = {Scales of spatial heterogeneity of plastic marine debris in the northeast Pacific Ocean},\n  publisher = {PloS one 8},\n  doi = {doi: 10.1371/journal.pone.0080020},\n  language = {}\n}\n@article{law2014a,\n  author = {Law, K. and Moret-Ferguson, S. and Goodwin, D. and Zettler, E. and DeForce, E.},\n  date = {2014},\n  title = {Distribution of surface plastic debris in the eastern Pacific Ocean from an 11-year dataset},\n  journal = {Environ Sci Technol},\n  doi = {doi: 10.1021/es4053076},\n  more-authors = {true},\n  language = {}\n}\n@book{reisser2013a,\n  author = {Reisser, J. and Shaw, J. and Wilcox, C. and Hardesty, B. and Proietti, M.},\n  date = {2013},\n  title = {Marine plastic pollution in the waters around Australia: Characteristics, concentrations and pathways},\n  publisher = {PloS one 8},\n  doi = {doi: 10.1371/journal.pone.0080466},\n  language = {}\n}\n",
  "ris": "TY  - JOUR\nAU  - Teuten, E.\nAU  - Rowland, S.\nAU  - Galloway, T.\nAU  - Thompson, R.\nPY  - 2007\nDA  - 2007\nTI  - Potential for plastics to transport hydrophobic contaminants\nT2  - Environ Sci Technol\nVL  - 41\nSP  - 7759\nEP  - 7764\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Mato, Y.\nAU  - Isobe, T.\nAU  - Takada, H.\nAU  - Kanehiro, H.\nAU  - Ohtake, C.\nPY  - 2001\nDA  - 2001\nTI  - Plastic resin pellets as a transport medium for toxic chemicals in the marine environment\nT2  - Environ Sci Technol\nVL  - 35\nSP  - 318\nEP  - 324\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Rochman, C.\nAU  - Browne, M.\nAU  - Halpern, B.\nAU  - Hentschel, B.\nAU  - Hoh, E.\nPY  - 2013\nDA  - 2013\nTI  - Classify plastic waste as hazardous\nT2  - Nature\nVL  - 494\nSP  - 169\nEP  - 171\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Barnes, D.\nAU  - Galgani, F.\nAU  - Thompson, R.\nAU  - Barlaz, M.\nPY  - 2009\nDA  - 2009\nTI  - Accumulation and fragmentation of plastic debris in global environments\nT2  - Philos Trans R Soc Lond B Biol Sci\nVL  - 364\nSP  - 1985\nEP  - 1998\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Barnes, D.\nAU  - Walters, A.\nAU  - Goncalves, L.\nPY  - 2010\nDA  - 2010\nTI  - Macroplastics at sea around Antarctica\nT2  - Mar Environ Res\nVL  - 70\nSP  - 250\nEP  - 252\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Law, K.\nAU  - Moret-Ferguson, S.\nAU  - Maximenko, N.\nAU  - Proskurowski, G.\nAU  - Peacock, E.\nPY  - 2010\nDA  - 2010\nTI  - Plastic accumulation in the North Atlantic Subtropical Gyre\nT2  - Science\nVL  - 329\nSP  - 1185\nEP  - 1188\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Eriksen, M.\nAU  - Maximenko, N.\nAU  - Thiel, M.\nAU  - Cummins, A.\nAU  - Lattin, G.\nPY  - 2013\nDA  - 2013\nTI  - Plastic marine pollution in the South Pacific Subtropical Gyre\nT2  - Mar Pollut Bull\nVL  - 68\nSP  - 71\nEP  - 76\nC1  - true\nLA  - \nER  - \n\nTY  - BOOK\nAU  - Goldstein, M.\nAU  - Titmus, A.\nAU  - Ford, M.\nPY  - 2013\nDA  - 2013\nTI  - Scales of spatial heterogeneity of plastic marine debris in the northeast Pacific Ocean\nPB  - PloS one 8\nDO  - 10.1371/journal.pone.0080020\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Law, K.\nAU  - Moret-Ferguson, S.\nAU  - Goodwin, D.\nAU  - Zettler, E.\nAU  - DeForce, E.\nPY  - 2014\nDA  - 2014\nTI  - Distribution of surface plastic debris in the eastern Pacific Ocean from an 11-year dataset\nT2  - Environ Sci Technol\nDO  - 10.1021/es4053076\nC1  - true\nLA  - \nER  - \n\nTY  - BOOK\nAU  - Reisser, J.\nAU  - Shaw, J.\nAU  - Wilcox, C.\nAU  - Hardesty, B.\nAU  - Proietti, M.\nPY  - 2013\nDA  - 2013\nTI  - Marine plastic pollution in the waters around Australia: Characteristics, concentrations and pathways\nPB  - PloS one 8\nDO  - 10.1371/journal.pone.0080466\nLA  - \nER  - \n\n,
  "reference_links": [
    {
      "id": "1",
      "entry": "1. Teuten E, Rowland S, Galloway T, Thompson R (2007) Potential for plastics to transport hydrophobic contaminants. Environ Sci Technol 41: 7759–7764.",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Teuten%2C%20E.%20Rowland%2C%20S.%20Galloway%2C%20T.%20Thompson%2C%20R.%20Potential%20for%20plastics%20to%20transport%20hydrophobic%20contaminants%202007",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Teuten%2C%20E.%20Rowland%2C%20S.%20Galloway%2C%20T.%20Thompson%2C%20R.%20Potential%20for%20plastics%20to%20transport%20hydrophobic%20contaminants%202007"
    },
    {
      "id": "2",
      "entry": "2. Mato Y, Isobe T, Takada H, Kanehiro H, Ohtake C, et al. (2001) Plastic resin pellets as a transport medium for toxic chemicals in the marine environment. Environ Sci Technol 35: 318–324.",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Mato%2C%20Y.%20Isobe%2C%20T.%20Takada%2C%20H.%20Kanehiro%2C%20H.%20Plastic%20resin%20pellets%20as%20a%20transport%20medium%20for%20toxic%20chemicals%20in%20the%20marine%20environment%202001",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Mato%2C%20Y.%20Isobe%2C%20T.%20Takada%2C%20H.%20Kanehiro%2C%20H.%20Plastic%20resin%20pellets%20as%20a%20transport%20medium%20for%20toxic%20chemicals%20in%20the%20marine%20environment%202001"
    },
    {
      "id": "3",
      "entry": "3. Rochman C, Browne M, Halpern B, Hentschel B, Hoh E, et al. (2013) Classify plastic waste as hazardous. Nature 494: 169–171.",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Rochman%2C%20C.%20Browne%2C%20M.%20Halpern%2C%20B.%20Hentschel%2C%20B.%20Classify%20plastic%20waste%20as%20hazardous%202013",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Rochman%2C%20C.%20Browne%2C%20M.%20Halpern%2C%20B.%20Hentschel%2C%20B.%20Classify%20plastic%20waste%20as%20hazardous%202013"
    },
    {
      "id": "4",
      "entry": "4. Barnes D, Galgani F, Thompson R, Barlaz M (2009) Accumulation and fragmentation of plastic debris in global environments. Philos Trans R Soc Lond B Biol Sci 364: 1985–1998.",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Barnes%2C%20D.%20Galgani%2C%20F.%20Thompson%2C%20R.%20Barlaz%2C%20M.%20Accumulation%20and%20fragmentation%20of%20plastic%20debris%20in%20global%20environments%202009",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Barnes%2C%20D.%20Galgani%2C%20F.%20Thompson%2C%20R.%20Barlaz%2C%20M.%20Accumulation%20and%20fragmentation%20of%20plastic%20debris%20in%20global%20environments%202009"
    },
    {
      "id": "5",
      "entry": "5. Barnes D, Walters A, Goncalves L (2010) Macroplastics at sea around Antarctica. Mar Environ Res 70: 250–252.",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Barnes%2C%20D.%20Walters%2C%20A.%20Goncalves%2C%20L.%20Macroplastics%20at%20sea%20around%20Antarctica%202010",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Barnes%2C%20D.%20Walters%2C%20A.%20Goncalves%2C%20L.%20Macroplastics%20at%20sea%20around%20Antarctica%202010"
    },
    {
      "id": "6",
      "entry": "6. Law K, Moret-Ferguson S, Maximenko N, Proskurowski G, Peacock E, et al. (2010) Plastic accumulation in the North Atlantic Subtropical Gyre. Science 329: 1185–1188.",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Law%2C%20K.%20Moret-Ferguson%2C%20S.%20Maximenko%2C%20N.%20Proskurowski%2C%20G.%20Plastic%20accumulation%20in%20the%20North%20Atlantic%20Subtropical%20Gyre%202010",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Law%2C%20K.%20Moret-Ferguson%2C%20S.%20Maximenko%2C%20N.%20Proskurowski%2C%20G.%20Plastic%20accumulation%20in%20the%20North%20Atlantic%20Subtropical%20Gyre%202010"
    },
    {
      "id": "7",
      "entry": "7. Eriksen M, Maximenko N, Thiel M, Cummins A, Lattin G, et al. (2013) Plastic marine pollution in the South Pacific Subtropical Gyre. Mar Pollut Bull 68: 71–76.",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Eriksen%2C%20M.%20Maximenko%2C%20N.%20Thiel%2C%20M.%20Cummins%2C%20A.%20Plastic%20marine%20pollution%20in%20the%20South%20Pacific%20Subtropical%20Gyre%202013",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Eriksen%2C%20M.%20Maximenko%2C%20N.%20Thiel%2C%20M.%20Cummins%2C%20A.%20Plastic%20marine%20pollution%20in%20the%20South%20Pacific%20Subtropical%20Gyre%202013"
    },
    {
      "id": "8",
      "entry": "8. Goldstein M, Titmus A, Ford M (2013) Scales of spatial heterogeneity of plastic marine debris in the northeast Pacific Ocean, PloS one 8: doi: 10.1371/journal.pone.0080020.",
      "crossref": "https://dx.doi.org/10.1371/journal.pone.0080020"
    },
    {
      "id": "9",
      "entry": "9. Law K, Moret-Ferguson S, Goodwin D, Zettler E, DeForce E, et al. (2014) Distribution of surface plastic debris in the eastern Pacific Ocean from an 11-year dataset. Environ Sci Technol: doi: 10.1021/ es4053076.",
      "crossref": "https://dx.doi.org/10.1021/es4053076",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=https%3A//dx.doi.org/10.1021/es4053076"
    },
    {
      "id": "10",
      "entry": "10. Reisser J, Shaw J, Wilcox C, Hardesty B, Proietti M (2013) Marine plastic pollution in the waters around Australia: Characteristics, concentrations and pathways. PloS one 8: doi:10.1371/ journal.pone.0080466.",
      "crossref": "https://dx.doi.org/10.1371/journal.pone.0080466"
    }
  ]
}
```

This endpoint extracts references from a local file, returned JSON data with embedded RIS and BibTeX content.
File formats supported are:

- PDF (.pdf)
- Word (.docx)
- Plain Text (.txt)


### HTTP Request

`POST http://ref.scholarcy.com/api/references/extract`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
file | null | A file object.
document_type | full_paper | `full_paper`: a complete research paper, chapter or thesis. `bibliography`: a file containing just bibliographic items.
references | null | Optional. If you don't want to upload a file, you can pass a text string containing line-delimited references that you wish to parse into the desired output format.
resolve_references | true | If `true`, parse each reference into BibTeX and RIS data and create link resolvers for each reference.
reference_style | ensemble | Referencing style used by the document. If unsure, use the default `ensemble` or choose `experimental`. Other options include: `acs`, `ama`, `apa`, `chicago`, `harvard`, `ieee`, `mhra`, `mla`, `nature`, `vancouver`.
engine | v1 | PDF processing engine. `v1`: uses the XPDF tool and works best for most PDFs. `v2`: uses the poppler tool and may work better for PDFs with marginal line numbering, with multiple columns, or for those PDFs where `v1` fails to extract useful information.


<aside class="success">
Remember to include your Authentication token with every request.
</aside>


## GET references from a remote document URL and return JSON data

```ruby
require 'rest-client'
AUTH_TOKEN = 'abcdef' # Your API key
API_DOMAIN = 'https://ref.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/references/extract'
headers = {"Authorization": "Bearer " + AUTH_TOKEN}

request = RestClient::Request.new(
          :method => :get,
          :url => POST_ENDPOINT,
          :headers => headers,
          :payload => {
            :url => 'https://www.nature.com/articles/s41746-019-0180-3.pdf',
            :resolve_references => True,
            :reference_style => 'ensemble'
          })
response = request.execute
puts(response.body)

```

```python
import requests
timeout = 30

AUTH_TOKEN = 'abcdefg' # Your API key
API_DOMAIN = 'https://ref.scholarcy.com'
POST_ENDPOINT = API_DOMAIN + '/api/references/extract'
headers = {'Authorization': 'Bearer ' + AUTH_TOKEN}

payload = {
'url': 'https://www.nature.com/articles/s41746-019-0180-3.pdf',
'resolve_references': True,
'reference_style': 'ensemble'
}
r = requests.get(POST_ENDPOINT,
      headers=headers,
      params=payload,
      timeout=timeout)
print(r.json())

```

```shell
curl "https://ref.scholarcy.com/api/references/extract" \
  -H "Authorization: Bearer abcdefg" \
  -d "url=https://www.nature.com/articles/s41746-019-0180-3.pdf" \
  -d "resolve_references=true" \
  -d "reference_style=ensemble"

```


> The above command returns JSON data as for the POST endpoint:

```json
{
  "filename": "s41746-019-0180-3.pdf",
  "metadata": {
    "arxiv": null,
    "doi": "10.1038/s41746-019-0180-3",
    "isbn": null,
    "date": 2019
  },
  "references": [
    "2. Hagglund, K. J. et al. Weather, beliefs about weather, and disease severity among patients with fibromyalgia. Arthritis Rheum. 7, 130–135 (1994).",
    "3. Timmermans, E. J. et al. Self-perceived weather sensitivity and joint pain in older people with osteoarthritis in six European countries: Results from the European Project on OSteoArthritis (EPOSA). BMC Musculoskelet. Disord., https://doi.org/10.1186/1471-2474-15-66 (2014).",
    "4. Smedslund, G. & Hagen, K. B. Does rain really cause pain? A systematic review of the associations between weather factors and severity of pain in people with rheumatoid arthritis. Eur. J. Pain. 15, 5–10 (2011).",
    "5. Aikman, H. The association between arthritis and the weather. Int. J. Biometeorol. 40, 192–199 (1997).",
    "6. Brennan, S. A. et al. Influence of weather variables on pain severity in end-stage osteoarthritis. Int. Orthop. 36, 643–646 (2012).",
    "7. Smedslund, G. et al. Does the weather really matter? A cohort study of influences of weather and solar conditions on daily variations of joint pain in patients with rheumatoid arthritis. Arthritis Rheum. 61, 1243–1247 (2009).",
    "8. Bossema, E. R. et al. Influence of weather on daily symptoms of pain and fatigue in female patients with fibromyalgia: a multilevel regression analysis. Arthritis Care Res. (Hoboken) 65, 1019–1025 (2013).",
    "9. Duong, V. et al. Does weather affect daily pain intensity levels in patients with acute low back pain? A prospective cohort study. Rheumatol. Int. 36, 679–684 (2016).",
    "10. Guedj, D. & Weinberger, A. Effect of weather conditions on rheumatic patients. Ann. Rheum. Dis. 49, 158–159 (1990).",
  ],
  "bibtex": "@article{hagglund1994a,\n  author = {Hagglund, K.J.},\n  title = {beliefs about weather, and disease severity among patients with fibromyalgia},\n  journal = {Arthritis Rheum},\n  volume = {7},\n  pages = {130–135},\n  date = {1994},\n  more-authors = {true},\n  language = {}\n}\n@article{timmermans2014a,\n  author = {Timmermans, E.J.},\n  title = {Self-perceived weather sensitivity and joint pain in older people with osteoarthritis in six European countries: Results from the European Project on OSteoArthritis (EPOSA). BMC Musculoskelet},\n  journal = {Disord},\n  url = {https://doi.org/10.1186/1471-2474-15-66},\n  date = {2014},\n  more-authors = {true},\n  language = {}\n}\n@article{smedslund2011a,\n  author = {Smedslund, G. and Hagen, K.B.},\n  title = {Does rain really cause pain? A systematic review of the associations between weather factors and severity of pain in people with rheumatoid arthritis},\n  journal = {Eur. J. Pain},\n  volume = {15},\n  pages = {5–10},\n  date = {2011},\n  language = {}\n}\n@article{aikman1997a,\n  author = {Aikman, H.},\n  title = {The association between arthritis and the weather},\n  journal = {Int. J. Biometeorol},\n  volume = {40},\n  pages = {192–199},\n  date = {1997},\n  language = {}\n}\n@article{brennan2012a,\n  author = {Brennan, S.A.},\n  title = {Influence of weather variables on pain severity in end-stage osteoarthritis},\n  journal = {Int. Orthop},\n  volume = {36},\n  pages = {643–646},\n  date = {2012},\n  more-authors = {true},\n  language = {}\n}\n@article{smedslund2009a,\n  author = {Smedslund, G.},\n  title = {Does the weather really matter? A cohort study of influences of weather and solar conditions on daily variations of joint pain in patients with rheumatoid arthritis},\n  journal = {Arthritis Rheum},\n  volume = {61},\n  pages = {1243–1247},\n  date = {2009},\n  more-authors = {true},\n  language = {}\n}\n@article{bossema2013a,\n  author = {Bossema, E.R.},\n  title = {Influence of weather on daily symptoms of pain and fatigue in female patients with fibromyalgia: a multilevel regression analysis},\n  journal = {Arthritis Care Res. (Hoboken)},\n  volume = {65},\n  pages = {1019–1025},\n  date = {2013},\n  more-authors = {true},\n  language = {}\n}\n@article{duong2016a,\n  author = {Duong, V.},\n  title = {Does weather affect daily pain intensity levels in patients with acute low back pain? A prospective cohort study},\n  journal = {Rheumatol. Int},\n  volume = {36},\n  pages = {679–684},\n  date = {2016},\n  more-authors = {true},\n  language = {}\n}\n@article{guedj1990a,\n  author = {Guedj, D. and Weinberger, A.},\n  title = {Effect of weather conditions on rheumatic patients},\n  journal = {Ann. Rheum. Dis},\n  volume = {49},\n  pages = {158–159},\n  date = {1990},\n  language = {}\n}\n",
  "ris": "TY  - JOUR\nAU  - Hagglund, K.J.\nTI  - beliefs about weather, and disease severity among patients with fibromyalgia\nT2  - Arthritis Rheum\nVL  - 7\nSP  - 130\nEP  - 135\nPY  - 1994\nDA  - 1994\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Timmermans, E.J.\nTI  - Self-perceived weather sensitivity and joint pain in older people with osteoarthritis in six European countries: Results from the European Project on OSteoArthritis (EPOSA). BMC Musculoskelet\nT2  - Disord\nUR  - https://doi.org/10.1186/1471-2474-15-66\nPY  - 2014\nDA  - 2014\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Smedslund, G.\nAU  - Hagen, K.B.\nTI  - Does rain really cause pain? A systematic review of the associations between weather factors and severity of pain in people with rheumatoid arthritis\nT2  - Eur. J. Pain\nVL  - 15\nSP  - 5\nEP  - 10\nPY  - 2011\nDA  - 2011\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Aikman, H.\nTI  - The association between arthritis and the weather\nT2  - Int. J. Biometeorol\nVL  - 40\nSP  - 192\nEP  - 199\nPY  - 1997\nDA  - 1997\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Brennan, S.A.\nTI  - Influence of weather variables on pain severity in end-stage osteoarthritis\nT2  - Int. Orthop\nVL  - 36\nSP  - 643\nEP  - 646\nPY  - 2012\nDA  - 2012\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Smedslund, G.\nTI  - Does the weather really matter? A cohort study of influences of weather and solar conditions on daily variations of joint pain in patients with rheumatoid arthritis\nT2  - Arthritis Rheum\nVL  - 61\nSP  - 1243\nEP  - 1247\nPY  - 2009\nDA  - 2009\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Bossema, E.R.\nTI  - Influence of weather on daily symptoms of pain and fatigue in female patients with fibromyalgia: a multilevel regression analysis\nT2  - Arthritis Care Res. (Hoboken)\nVL  - 65\nSP  - 1019\nEP  - 1025\nPY  - 2013\nDA  - 2013\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Duong, V.\nTI  - Does weather affect daily pain intensity levels in patients with acute low back pain? A prospective cohort study\nT2  - Rheumatol. Int\nVL  - 36\nSP  - 679\nEP  - 684\nPY  - 2016\nDA  - 2016\nC1  - true\nLA  - \nER  - \n\nTY  - JOUR\nAU  - Guedj, D.\nAU  - Weinberger, A.\nTI  - Effect of weather conditions on rheumatic patients\nT2  - Ann. Rheum. Dis\nVL  - 49\nSP  - 158\nEP  - 159\nPY  - 1990\nDA  - 1990\nLA  - \nER  - \n\n",
  "reference_links": [
    {
      "id": "2",
      "entry": "2. Hagglund, K. J. et al. Weather, beliefs about weather, and disease severity among patients with fibromyalgia. Arthritis Rheum. 7, 130–135 (1994).",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Hagglund%2C%20K.J.%20beliefs%20about%20weather%2C%20and%20disease%20severity%20among%20patients%20with%20fibromyalgia%201994",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Hagglund%2C%20K.J.%20beliefs%20about%20weather%2C%20and%20disease%20severity%20among%20patients%20with%20fibromyalgia%201994"
    },
    {
      "id": "3",
      "entry": "3. Timmermans, E. J. et al. Self-perceived weather sensitivity and joint pain in older people with osteoarthritis in six European countries: Results from the European Project on OSteoArthritis (EPOSA). BMC Musculoskelet. Disord., https://doi.org/10.1186/1471-2474-15-66 (2014).",
      "crossref": "https://dx.doi.org/10.1186/1471-2474-15-66",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=https%3A//dx.doi.org/10.1186/1471-2474-15-66"
    },
    {
      "id": "4",
      "entry": "4. Smedslund, G. & Hagen, K. B. Does rain really cause pain? A systematic review of the associations between weather factors and severity of pain in people with rheumatoid arthritis. Eur. J. Pain. 15, 5–10 (2011).",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Smedslund%2C%20G.%20Hagen%2C%20K.B.%20Does%20rain%20really%20cause%20pain%3F%20A%20systematic%20review%20of%20the%20associations%20between%20weather%20factors%20and%20severity%20of%20pain%20in%20people%20with%20rheumatoid%20arthritis%202011",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Smedslund%2C%20G.%20Hagen%2C%20K.B.%20Does%20rain%20really%20cause%20pain%3F%20A%20systematic%20review%20of%20the%20associations%20between%20weather%20factors%20and%20severity%20of%20pain%20in%20people%20with%20rheumatoid%20arthritis%202011"
    },
    {
      "id": "5",
      "entry": "5. Aikman, H. The association between arthritis and the weather. Int. J. Biometeorol. 40, 192–199 (1997).",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Aikman%2C%20H.%20The%20association%20between%20arthritis%20and%20the%20weather%201997",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Aikman%2C%20H.%20The%20association%20between%20arthritis%20and%20the%20weather%201997"
    },
    {
      "id": "6",
      "entry": "6. Brennan, S. A. et al. Influence of weather variables on pain severity in end-stage osteoarthritis. Int. Orthop. 36, 643–646 (2012).",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Brennan%2C%20S.A.%20Influence%20of%20weather%20variables%20on%20pain%20severity%20in%20end-stage%20osteoarthritis%202012",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Brennan%2C%20S.A.%20Influence%20of%20weather%20variables%20on%20pain%20severity%20in%20end-stage%20osteoarthritis%202012"
    },
    {
      "id": "7",
      "entry": "7. Smedslund, G. et al. Does the weather really matter? A cohort study of influences of weather and solar conditions on daily variations of joint pain in patients with rheumatoid arthritis. Arthritis Rheum. 61, 1243–1247 (2009).",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Smedslund%2C%20G.%20Does%20the%20weather%20really%20matter%3F%20A%20cohort%20study%20of%20influences%20of%20weather%20and%20solar%20conditions%20on%20daily%20variations%20of%20joint%20pain%20in%20patients%20with%20rheumatoid%20arthritis%202009",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Smedslund%2C%20G.%20Does%20the%20weather%20really%20matter%3F%20A%20cohort%20study%20of%20influences%20of%20weather%20and%20solar%20conditions%20on%20daily%20variations%20of%20joint%20pain%20in%20patients%20with%20rheumatoid%20arthritis%202009"
    },
    {
      "id": "8",
      "entry": "8. Bossema, E. R. et al. Influence of weather on daily symptoms of pain and fatigue in female patients with fibromyalgia: a multilevel regression analysis. Arthritis Care Res. (Hoboken) 65, 1019–1025 (2013).",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Bossema%2C%20E.R.%20Influence%20of%20weather%20on%20daily%20symptoms%20of%20pain%20and%20fatigue%20in%20female%20patients%20with%20fibromyalgia%3A%20a%20multilevel%20regression%20analysis%202013",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Bossema%2C%20E.R.%20Influence%20of%20weather%20on%20daily%20symptoms%20of%20pain%20and%20fatigue%20in%20female%20patients%20with%20fibromyalgia%3A%20a%20multilevel%20regression%20analysis%202013"
    },
    {
      "id": "9",
      "entry": "9. Duong, V. et al. Does weather affect daily pain intensity levels in patients with acute low back pain? A prospective cohort study. Rheumatol. Int. 36, 679–684 (2016).",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Duong%2C%20V.%20Does%20weather%20affect%20daily%20pain%20intensity%20levels%20in%20patients%20with%20acute%20low%20back%20pain%3F%20A%20prospective%20cohort%20study%202016",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Duong%2C%20V.%20Does%20weather%20affect%20daily%20pain%20intensity%20levels%20in%20patients%20with%20acute%20low%20back%20pain%3F%20A%20prospective%20cohort%20study%202016"
    },
    {
      "id": "10",
      "entry": "10. Guedj, D. & Weinberger, A. Effect of weather conditions on rheumatic patients. Ann. Rheum. Dis. 49, 158–159 (1990).",
      "scholar_url": "https://scholar.google.co.uk/scholar?q=Guedj%2C%20D.%20Weinberger%2C%20A.%20Effect%20of%20weather%20conditions%20on%20rheumatic%20patients%201990",
      "oa_query": "https://ref.scholarcy.com/oa_version?query=Guedj%2C%20D.%20Weinberger%2C%20A.%20Effect%20of%20weather%20conditions%20on%20rheumatic%20patients%201990"
    }
  ]
}

```

This endpoint extracts references from a remote URL, returning JSON data with embedded RIS and BibTeX content.
The remote URL can resolve to a document type in any of the formats listed for the POST endpoint.

### HTTP Request

`GET http://ref.scholarcy.com/api/references/extract`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
url | null | URL of public, open-access document.
document_type | full_paper | `full_paper`: a complete research paper, chapter or thesis. `bibliography`: a file containing just bibliographic items.
references | null | Optional. If you don't want to upload a file, you can pass a text string containing line-delimited references that you wish to parse into the desired output format.
resolve_references | true | If `true`, parse each reference into BibTeX and RIS data and create link resolvers for each reference.
reference_style | ensemble | Referencing style used by the document. If unsure, use the default `ensemble` or choose `experimental`. Other options include: `acs`, `ama`, `apa`, `chicago`, `harvard`, `ieee`, `mhra`, `mla`, `nature`, `vancouver`.
engine | v1 | PDF processing engine. `v1`: uses the XPDF tool and works best for most PDFs. `v2`: uses the poppler tool and may work better for PDFs with marginal line numbering, with multiple columns, or for those PDFs where `v1` fails to extract useful information.

<aside class="success">
Remember to include your Authentication token with every request.
</aside>
