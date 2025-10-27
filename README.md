# PyMeta
<p align="left">
  <img src="https://img.shields.io/badge/License-GPL%203.0-green.svg"/>&nbsp;
  <a href="https://www.twitter.com/m8sec">
        <img src="https://img.shields.io/badge/Twitter-@m8sec-gray?logo=twitter"/>
    </a>&nbsp;
    <img src="https://img.shields.io/badge/python-3.6%20|%203.7%20|%203.8%20|%203.9%20-blue.svg"/>&nbsp;
 </p>

PyMeta is a Python3 rewrite of the tool [PowerMeta](https://github.com/dafthack/PowerMeta), created by [dafthack](https://twitter.com/dafthack) in PowerShell. It uses specially crafted search queries to identify and download the following file types (**pdf, xls, xlsx, csv, doc, docx, ppt, pptx**) from a given domain using Google and Bing scraping.

Once downloaded, metadata is extracted from these files using Phil Harvey's [exiftool](https://sno.phy.queensu.ca/~phil/exiftool/) and added to a ```.csv``` report.  Alternatively, Pymeta can be pointed at a directory to extract metadata from files manually downloaded using the ```-dir``` command line argument. See the [Usage](#Usage), or [All Options](#All-Options) section for more information.

> **Note:** Due to Google's increasingly aggressive anti-bot measures, web scraping may yield limited results. For improved reliability and more consistent results, consider using the Google Custom Search API with the `--api-key` and `--search-engine-id` flags.

#### Why?
Metadata is a common place for penetration testers and red teamers to find: domains, user accounts, naming conventions, software/version numbers, and more!


# Getting Started
### Prerequisites
[Exiftool](https://sno.phy.queensu.ca/~phil/exiftool/) is required and can be installed with:

&nbsp;&nbsp;&nbsp;&nbsp;**Ubuntu/Kali** - ```apt-get install exiftool -y```

&nbsp;&nbsp;&nbsp;&nbsp;**Mac OS** - ```brew install exiftool```

### Install:
**Recommended: Install with pipx (preferred method)**

Using [pipx](https://pypa.github.io/pipx/) is the preferred installation method as it installs PyMeta in an isolated environment, preventing conflicts with system Python packages:

```commandline
pipx install pymetasec
```

If you don't have pipx installed:
```commandline
# Ubuntu/Debian
sudo apt install pipx
pipx ensurepath

# Mac OS
brew install pipx
pipx ensurepath

# Or via pip (then restart your shell)
python3 -m pip install --user pipx
python3 -m pipx ensurepath
```

**Alternative: Install with pip**

You can also install directly with pip, though this may affect system Python packages:
```commandline
pip3 install pymetasec
```

**Install from source:**

Clone the repository and install locally:
```bash
git clone https://github.com/m8sec/pymeta
cd pymeta
pipx install .
# Or with pip: pip3 install .
```

Or install directly from GitHub without cloning:
```bash
pipx install git+https://github.com/m8sec/pymeta
# Or with pip: pip3 install git+https://github.com/m8sec/pymeta
```

## Usage

### Standard Search (Web Scraping)
* Search Google and Bing for files within example.com and extract metadata to a csv report:<br>
```pymeta -d example.com```

* Extract metadata from files within the given directory and create csv report:<br>
```pymeta -dir Downloads/```

### Google API Search
Due to Google's aggressive anti-bot protections, web scraping may produce limited results. For better reliability, use the Google API option:

```pymeta -d example.com --api-key "your_api_key_here" --search-engine-id "your_search_engine_id"```

#### Setting up Google API

**Step 1: Create Google Cloud Project**
- Go to [Google Cloud Console](https://cloud.google.com/)
- Login with a Google account
- Click "Select a project" → "New Project" 
- Enter a project name (e.g., "PyMeta-API")
- Click "Create"

**Step 2: Enable Custom Search API**
- In your project, go to "APIs & Services" → "Library"
- Search for "Custom Search API"
- Click on it and press "Enable"

**Step 3: Create API Key**
- Go to "APIs & Services" → "Credentials"
- Click "Create Credentials" → "API Key"
- Copy your API key (you'll need this for the `--api-key` flag)

**Step 4: Create Custom Search Engine**
- Go to [Google Programmable Search Engine](https://programmablesearchengine.google.com/)
- Click "Add a search engine" 
- Enter any name (e.g., "PyMeta Search")
- For "Sites to search", select "Search the entire web"
- Click "Create"
- Copy your Search Engine ID (you'll need this for the `--search-engine-id` flag)

**API Usage Notes:**
- Google provides 100 free API calls per day
- Additional requests cost $5 per 1000 queries
- API searches are more reliable than web scraping and less likely to be blocked
- When using API mode, only Google search is used (Bing searches are disabled)

> **NOTE**: Thanks to Beau Bullock [(@dafthack)](https://twitter.com/dafthack) and the [PowerMeta](https://github.com/dafthack/PowerMeta) project for the above steps on getting a Google API key.

## All Options
```
options:
  -h, --help            show this help message and exit
  -T MAX_THREADS        Max threads for file download (Default=5)
  -t TIMEOUT            Max timeout per search (Default=8)
  -j JITTER             Jitter between requests (Default=1)

Search Options:
  -s ENGINE, --search ENGINE    Search Engine (Default='google,bing')
  --file-type FILE_TYPE         File types to search (default=pdf,xls,xlsx,csv,doc,docx,ppt,pptx)
  -m MAX_RESULTS                Max results per type search

Google API Options:
  --api-key API_KEY             Google API key for Custom Search API
  --search-engine-id ID         Google Custom Search Engine ID

Proxy Options:
  --proxy PROXY         Proxy requests (IP:Port)
  --proxy-file PROXY    Load proxies from file for rotation

Output Options:
  -o DWNLD_DIR          Path to create downloads directory (Default: ./)
  -f REPORT_FILE        Custom report name ("pymeta_report.csv")

Target Options:
  -d DOMAIN             Target domain
  -dir FILE_DIR         Pre-existing directory of file
```
    
## Credit
- Beau Bullock [(@dafthack)](https://twitter.com/dafthack) - [https://github.com/dafthack/PowerMeta](https://github.com/dafthack/PowerMeta)
- Phil Harvey - [https://exiftool.org/](https://exiftool.org/)
