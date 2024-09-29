import os
import time
import pandas as pd
import requests
from flask import Flask, request, redirect
import threading
import webbrowser

CLIENT_ID = 'YOUR ID'
CLIENT_SECRET = 'YOUR SECRET'
REDIRECT_URI = 'YOUR REDIRECT URI'
AUTHORIZATION_BASE_URL = 'https://www.linkedin.com/oauth/v2/authorization'
TOKEN_URL = 'https://www.linkedin.com/oauth/v2/accessToken'
SEARCH_URL = 'https://api.linkedin.com/v2/me'
REQUEST_DELAY = 60  

app = Flask(__name__)

=
access_token = None

@app.route('/auth')
def auth():
    authorization_url = (f"{AUTHORIZATION_BASE_URL}?response_type=code"
                         f"&client_id={CLIENT_ID}&redirect_uri={REDIRECT_URI}"
                         f"&scope=openid%20profile%20email")
    print(f"Redirecting to LinkedIn authorization URL: {authorization_url}")
    return redirect(authorization_url)

@app.route('/auth/callback')
def callback():
    global access_token
    code = request.args.get('code')
    if not code:
        return 'Authorization code not received. Please try again.', 400
    
    response = requests.post(TOKEN_URL, data={
        'grant_type': 'authorization_code',
        'code': code,
        'redirect_uri': REDIRECT_URI,
        'client_id': CLIENT_ID,
        'client_secret': CLIENT_SECRET
    })

    if response.status_code != 200:
        return f"Error fetching access token: {response.text}", response.status_code

    response_data = response.json()
    access_token = response_data.get('access_token')
    if not access_token:
        return 'Access token not received. Please try again.', 400

    return 'Authentication successful! You can close this window and return to the terminal.'

def get_user_profile():
    headers = {'Authorization': f'Bearer {access_token}'}
    response = requests.get(SEARCH_URL, headers=headers)

    if response.status_code != 200:
        print(f"Error fetching user profile: {response.text}")
        return None

    return response.json()

def process_excel(file_path, region, location):
    try:
        df = pd.read_excel(file_path, engine='openpyxl')
        print("Columns in the Excel file:", df.columns)
    except Exception as e:
        print(f"Error reading Excel file: {e}")
        return

    name_column = 'NAMES' 
    if name_column not in df.columns:
        print(f"Error: '{name_column}' column not found in the Excel file.")
        return

    df['COMMENT'] = ''
    df['WEB SOURCE'] = ''

    all_names = df[name_column].dropna().tolist()

    for i in range(0, len(all_names), 10):
        names_batch = all_names[i:i + 10]
        print(f"Processing batch {i // 10 + 1}: Names {i + 1} to {i + len(names_batch)}")

        for name in names_batch:
            print(f"Generating search URL for {name} in region {region}...")
            search_url = (f"https://www.linkedin.com/search/results/people/"
                          f"?firstName={name}&geoUrn=%5B%22{region}%22%5D&keywords={name}&location={location}&origin=FACETED_SEARCH&sid=N73")
            df.loc[df[name_column] == name, 'WEB SOURCE'] = search_url
            df.loc[df[name_column] == name, 'COMMENT'] = "Search URL generated"

        time.sleep(REQUEST_DELAY) \

    output_path = 'results.xlsx'
    df.to_excel(output_path, index=False)
    print(f"Results saved to {output_path}")

def main():
    def run_server():
        print("Starting Flask server on http://localhost:XXXX")
        app.run(host='127.0.0.1', port=XXXX)

    server_thread = threading.Thread(target=run_server)
    server_thread.start()
    
    webbrowser.open(f'http://localhost:XXXX/auth')

    input("Press Enter once you have authenticated...")
    
    if access_token:
        profile = get_user_profile()
        if profile:
            print(f"User profile fetched: {profile}")
        else:
            print("Failed to fetch user profile.")
        
        excel_file = input("Enter the path to the Excel file: ")
        region = input("Enter the region code for the search: ")
        location = input("Enter the location name for the search: ")

        if os.path.isfile(excel_file):
            process_excel(excel_file, region, location)
        else:
            print("Excel file not found. Exiting.")
    else:
        print("Authentication failed. Please check your credentials.")

if __name__ == '__main__':
    main()
