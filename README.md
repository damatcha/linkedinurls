## Overview
A Python-based script that automates LinkedIn searches for names listed in an Excel file. It utilizes the LinkedIn API for user authentication and generates search URLs for people in a specified region and location based on user input.

The generated URLs are saved in an Excel file along with additional metadata for each name.

## Features

- **Automated LinkedIn Search**: Generate search URLs for multiple names.
- **OAuth 2.0 Authentication**: Uses LinkedIn's OAuth for user authentication.
- **Excel Integration**: Input names from an Excel file, and the results are saved in a new Excel file.
- **Search by Region and Location**: Customize search queries by region code and location.

## Requirements

To use this tool, you will need the following:
- LinkedIn API credentials (Client ID, Client Secret, and Redirect URI).
- Python 3.8+
- The following Python packages:
  ```bash
  pip install Flask pandas requests openpyxl
  ```

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Set up LinkedIn API credentials**:
   - Register your app on the [LinkedIn Developer Portal](https://www.linkedin.com/developers/).
   - Set your environment variables for `CLIENT_ID`, `CLIENT_SECRET`, and `REDIRECT_URI` in the script.

4. **Run the Flask server**:
   ```bash
   python script.py
   ```

## Usage

1. **Start the Flask server**:
   The server will start at `http://localhost:XXXX/auth` (replace `XXXX` with the port number in the code).

2. **Authenticate with LinkedIn**:
   After running the script, a browser window will open, prompting you to log in to LinkedIn and authorize the app. Upon successful authentication, you will receive an access token.

3. **Enter search parameters**:
   - You will be prompted to enter the path to your Excel file, region code, and location for the LinkedIn search.

4. **Output**:
   The script generates search URLs for each name in the Excel file, along with comments. These results are saved in `results.xlsx`.

### Example of how to run:

```bash
python script.py
```

### Sample Excel File Format

The input Excel file should have a column named `NAMES` containing the names you want to search.

| NAMES      |
|------------|
| John Smith |
| Alice Doe  |

## Configuration

- **CLIENT_ID, CLIENT_SECRET, and REDIRECT_URI**: These should be set up in your LinkedIn Developer Portal. Update the script with your credentials.
- **REQUEST_DELAY**: You can configure the delay between API requests to avoid hitting rate limits. The default is set to 60 seconds.

## License

This project is licensed under the MIT License.
