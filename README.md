# SearchEngine

A web-based search engine application built with Flask that provides filtered search results using Google Custom Search API with intelligent content filtering and relevance tracking.

## Features

- **Web Search Interface**: Clean, simple web interface for searching
- **Google Custom Search Integration**: Uses Google Custom Search API for search results
- **Intelligent Filtering**: 
  - Tracker filtering to demote sites with excessive tracking scripts
  - Content filtering to prioritize pages with substantial content
- **Relevance Learning**: Users can mark results as relevant to improve future rankings
- **Caching**: Stores search results in SQLite database to avoid repeated API calls
- **Content Scraping**: Downloads and analyzes page content for filtering

## Project Structure

```
SearchEngine/
├── app.py          # Flask web application and routes
├── search.py       # Search API integration and page scraping
├── filter.py       # Content and tracker filtering logic
├── storage.py      # SQLite database operations
├── settings.py     # Configuration settings
├── requirements.txt # Python dependencies
└── README.md       # This file
```

## Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd SearchEngine
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure API credentials**:
   - Get a Google Custom Search API key from [Google Cloud Console](https://console.cloud.google.com/)
   - Get a Custom Search Engine ID from [Google Custom Search](https://cse.google.com/)
   - Update `settings.py` with your credentials:
     ```python
     SEARCH_KEY = "your-api-key-here"
     SEARCH_ID = "your-search-engine-id-here"
     ```

4. **Create blacklist file** (optional):
   Create a `blacklist.txt` file with domains to filter out (one per line).

## Usage

1. **Start the application**:
   ```bash
   python app.py
   ```

2. **Access the web interface**:
   Open your browser and navigate to `http://localhost:5000`

3. **Search**:
   - Enter your search query in the text field
   - Click "Search" to get filtered results
   - Click "Relevant" next to any result to mark it as relevant

## How It Works

### Search Process
1. **Query Processing**: User enters a search query
2. **Database Check**: First checks if results exist in local database
3. **API Search**: If not cached, queries Google Custom Search API
4. **Content Scraping**: Downloads HTML content from search results
5. **Filtering**: Applies tracker and content filters to rank results
6. **Storage**: Saves results to database for future use

### Filtering System
- **Tracker Filter**: Analyzes pages for tracking scripts and demotes sites with excessive tracking
- **Content Filter**: Evaluates page content length and demotes pages with insufficient content
- **Relevance Learning**: User feedback improves result rankings over time

### Database Schema
The SQLite database stores search results with the following structure:
- `query`: Search query
- `rank`: Result ranking
- `link`: URL of the result
- `title`: Page title
- `snippet`: Search snippet
- `html`: Full HTML content
- `created`: Timestamp
- `relevance`: User relevance score

## Configuration

Key settings in `settings.py`:
- `SEARCH_KEY`: Google Custom Search API key
- `SEARCH_ID`: Custom Search Engine ID
- `COUNTRY`: Search country code (default: "in")
- `RESULT_COUNT`: Number of results to fetch (default: 20)

## Dependencies

- **Flask**: Web framework
- **pandas**: Data manipulation
- **requests**: HTTP requests
- **beautifulsoup4**: HTML parsing

## API Endpoints

- `GET /`: Display search form
- `POST /`: Process search query and return results
- `POST /relevant`: Mark a result as relevant

## Security Notes

- API keys are stored in `settings.py` - consider using environment variables for production
- The application includes a `private.py` import option for additional configuration
- HTML content is escaped to prevent XSS attacks

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project is open source. Please check the license file for details.
