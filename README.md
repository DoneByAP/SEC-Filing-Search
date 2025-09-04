# SEC Research API 📊

Powerful SEC filing extractor for Python. Download & parse SEC EDGAR filings (10-K, 10-Q, 8-K, etc.) with date filtering, 11+ form categories, plain text extraction, and intelligent file chunking. Features parallel processing, rate limiting compliance, and token-based splitting for LLM compatibility.

## Features

- **Date Range Filtering**: Select from 6 preset periods or custom date ranges
- **11 Filing Categories**: Quarterly/Annual reports, Current events (8-K), Proxies, Insider trading, Institutional holdings, Ownership stakes, Registration statements, Foreign issuers, Prospectuses, Amendments
- **Plain Text Extraction**: Converts HTML, XBRL, and EDGAR formats to clean text
- **Smart File Chunking**: 150KB max file size with 65,300 token limits per chunk
- **Content Overlap**: Maintains 2,500 token overlap between chunks for context
- **Parallel Processing**: Multi-threaded downloads (5-10 concurrent connections)
- **SEC Compliance**: 0.11 second rate limiting between requests
- **Structured Data Extraction**: Extracts revenue, net income, EPS from financial reports

## Installation


# Required packages
pip install tiktoken requests

# Clone and run
git clone https://github.com/DoneByAP/SEC-Filing-Search.git
cd SEC-Filing-Search
python sec_get.py


## Usage

### Interactive CLI
1. Enter stock ticker (e.g., AAPL, MSFT, TSLA)
2. Select time period:
   - Last Quarter (90 days)
   - Last 6 Months (180 days)
   - Last Year (365 days)
   - Last 2 Years (730 days)
   - Last 5 Years (1825 days)
   - Custom date range
3. Choose filing types:
   - ALL filings
   - Single category (1-11)
   - Multi-select categories
   - Custom form names
4. Enable/disable parallel processing

### Output Format

Files named: `{TICKER}_SEC_{DATERANGE}_{FORMS}_{TIMESTAMP}.txt`

Example: `AAPL_SEC_20240101_to_20241231_10K_10Q_20241215.txt`

Multi-part files if content exceeds limits:
- `AAPL_SEC_..._part0001.txt` (first 65,300 tokens)
- `AAPL_SEC_..._part0002.txt` (next chunk with overlap)

## Filing Types Supported

| Category | SEC Forms |
|----------|-----------|
| Quarterly Reports | 10-Q, 10-Q/A |
| Annual Reports | 10-K, 10-K/A |
| Current Reports | 8-K, 8-K/A |
| Proxy Statements | DEF 14A, PRE 14A, DEFM14A, DEFC14A, DEF 14C |
| Insider Trading | 3, 4, 5, 3/A, 4/A, 5/A, 144 |
| Institutional Holdings | 13F, 13F-HR, 13F-NT, N-PORT, 13-H |
| Beneficial Ownership | SC 13D, SC13D, SC 13G, SC13G, SC 13D/A, SC 13G/A |
| Registration Statements | S-1, S-3, S-4, S-8, S-11, F-1, F-3, F-4, D, D/A |
| Foreign Issuers | 20-F, 40-F, 6-K, 20-F/A, 40-F/A |
| Prospectuses | 424B1-B7, FWP, POSASR |
| Amendments | EX-, NT 10-K, NT 10-Q, NT 20-F, NT 11-K |

## Configuration

Update User-Agent header (line ~464):
```python
self.headers = {'User-Agent': 'YourName/1.0 (your.email@example.com)'}
```

## Technical Details

- **Token Counting**: Uses OpenAI's tiktoken (cl100k_base encoding)
- **Rate Limiting**: Thread-safe with 0.11s minimum between requests
- **File Size**: 150KB limit with 20KB safety margin
- **Token Limit**: 65,300 tokens per file chunk
- **Overlap**: 2,500 tokens repeated between chunks
- **Batch Size**: 5 sequential, 10 parallel
- **Max Workers**: 5 concurrent threads in parallel mode

## Output Content Structure

Each file contains:
1. **Header**: Company info, date range, filing types
2. **Summary**: Filing counts by type
3. **Per Filing**:
   - Extracted structured data (revenue, EPS, etc.)
   - Key sections (Business Overview, Risk Factors, MD&A)
   - Tables (first 5, max 20 rows each)
   - Full plain text content
4. **Footer**: Processing statistics

## SEC Compliance

- Follows SEC.gov rate limits (10 requests/second max)
- Includes required User-Agent with contact info
- Caches company ticker mappings locally
- Respects robots.txt guidelines

## Requirements

- Python 3.7+
- tiktoken (for token counting)
- requests (for HTTP requests)
- ~500MB disk space for large extractions

## License

MIT License - Open Source


Created by Azani Peterkin - Original implementation

## Contributing

Pull requests welcomed. Please maintain SEC compliance and rate limiting.

## Disclaimer

This tool is for educational and research purposes. Users are responsible for complying with SEC terms of use and any applicable regulations.
