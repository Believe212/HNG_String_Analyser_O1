# String Analyzer Service - HNG Stage 1 Backend Task

A simple RESTful API service that analyzes strings and stores their computed properties.

## Features

- Analyze strings and compute properties (length, palindrome, character frequency, etc.)
- Store analyzed strings in SQLite database
- Filter strings using query parameters
- Natural language query support
- Full CRUD operations

## Tech Stack

- **Python 3.8+**
- **Flask** - Web framework
- **SQLite** - Database (no setup required)

## Local Setup Instructions

### Prerequisites
- Python 3.8 or higher
- pip (Python package manager)

### Installation Steps

1. **Clone the repository**
```bash
git clone <your-repo-url>
cd string-analyzer-service
```

2. **Create virtual environment (recommended)**
```bash
python -m venv venv

# On Windows
venv\Scripts\activate

# On Mac/Linux
source venv/bin/activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Run the application**
```bash
python app.py
```

The server will start on `http://localhost:5000`

## API Endpoints

### 1. Create/Analyze String
**POST** `/strings`

```json
{
  "value": "hello world"
}
```

Response (201):
```json
{
  "id": "sha256_hash",
  "value": "hello world",
  "properties": {
    "length": 11,
    "is_palindrome": false,
    "unique_characters": 8,
    "word_count": 2,
    "sha256_hash": "...",
    "character_frequency_map": {"h": 1, "e": 1, ...}
  },
  "created_at": "2025-10-23T10:00:00Z"
}
```

### 2. Get Specific String
**GET** `/strings/{string_value}`

Example: `GET /strings/hello%20world`

### 3. Get All Strings with Filtering
**GET** `/strings?is_palindrome=true&min_length=5&max_length=20&word_count=2&contains_character=a`

Query Parameters:
- `is_palindrome`: true/false
- `min_length`: integer
- `max_length`: integer
- `word_count`: integer
- `contains_character`: single character

### 4. Natural Language Filtering
**GET** `/strings/filter-by-natural-language?query=all%20single%20word%20palindromic%20strings`

Supported queries:
- "all single word palindromic strings"
- "strings longer than 10 characters"
- "strings containing the letter z"
- "palindromic strings that contain the first vowel"

### 5. Delete String
**DELETE** `/strings/{string_value}`

Returns 204 No Content on success.

## Testing the API

### Using curl:

```bash
# Create a string
curl -X POST http://localhost:5000/strings \
  -H "Content-Type: application/json" \
  -d '{"value": "racecar"}'

# Get a string
curl http://localhost:5000/strings/racecar

# Get all palindromes
curl "http://localhost:5000/strings?is_palindrome=true"

# Natural language query
curl "http://localhost:5000/strings/filter-by-natural-language?query=all%20single%20word%20palindromic%20strings"

# Delete a string
curl -X DELETE http://localhost:5000/strings/racecar
```

### Using Python requests:

```python
import requests

# Create
response = requests.post('http://localhost:5000/strings', 
    json={'value': 'hello world'})
print(response.json())

# Get
response = requests.get('http://localhost:5000/strings/hello%20world')
print(response.json())
```

## Deployment on PythonAnywhere

### Step 1: Upload Files
1. Go to PythonAnywhere.com and create a free account
2. Go to "Files" tab
3. Upload `app.py` and `requirements.txt`

### Step 2: Create Web App
1. Go to "Web" tab
2. Click "Add a new web app"
3. Choose "Flask" and Python 3.8+
4. Set the path to your `app.py`

### Step 3: Configure WSGI
1. Click on the WSGI configuration file link
2. Replace contents with:
```python
import sys
path = '/home/yourusername/your-project-folder'
if path not in sys.path:
    sys.path.append(path)

from app import app as application
```

### Step 4: Install Dependencies
1. Open a Bash console
2. Run:
```bash
pip install --user Flask
```

### Step 5: Reload
1. Go back to "Web" tab
2. Click "Reload" button
3. Your API will be live at `https://yourusername.pythonanywhere.com`

## Project Structure

```
string-analyzer-service/
│
├── app.py                 # Main Flask application
├── requirements.txt       # Python dependencies
├── .gitignore            # Git ignore file
├── README.md             # This file
└── strings.db            # SQLite database (auto-created)
```

## Dependencies

- Flask==3.0.0

No additional dependencies required! Everything else is Python standard library.

## Environment Variables

No environment variables needed for this simple implementation.

## Error Handling

The API returns appropriate HTTP status codes:
- `200 OK` - Successful GET
- `201 Created` - Successfully created
- `204 No Content` - Successfully deleted
- `400 Bad Request` - Invalid request
- `404 Not Found` - Resource not found
- `409 Conflict` - Duplicate string
- `422 Unprocessable Entity` - Invalid data type

## Database

Uses SQLite with a simple schema:
- Stores string value and all computed properties
- Automatically creates database on first run
- No migration needed

## Notes

- Case-insensitive palindrome checking
- SHA-256 used as unique identifier
- Character frequency includes all characters (including spaces)
- Word count based on whitespace separation

## Author

[Believe Nosakhare]
[believenosak@gmail.com]

## HNG Internship

Built for HNG Internship Stage 1 Backend Task.

