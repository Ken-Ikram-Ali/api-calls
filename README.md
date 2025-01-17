# API Calls for Program Board Items in Kendis

This repository provides examples of how to use the Kendis API for retrieving program board items, metadata for sprints, teams, and statuses. Follow the examples below to interact with the Kendis API effectively.

## Getting Started

### Prerequisites
- **Python 3.x** installed on your machine.
- The `requests` library installed. You can install it using:
  ```bash
  pip install requests
  ```
- A valid **Kendis API Key**. To retrieve your API Key, please follow the instructions in the [Retrieve API Key](#retrieve-api-key) section.

### Retrieve API Key
1. Log in to your Kendis account.
2. Navigate to your **Profile**.
3. Click on the **API** tab.
4. Copy your **API Key**.

### Configuration
To authenticate your requests, you need to provide your Kendis username, API Key, and company prefix. Update the placeholders in the code examples with your specific details:
- `username`: Your Kendis username.
- `password`: Your Kendis API Key.
- `company_prefix`: Your company prefix.
- `board_id`: The ID of the board you are working with.

### Fetching Board ID
To retrieve all items of a specific board, you will first need to get the collection or workspace ID associated with that board. Once you have the collection/workspace ID, you can use it to fetch the corresponding board ID. Ensure that you have both IDs before making item-related API calls.

---

## API Examples

### 1. Retrieve Collection ID

To retrieve the Collection ID (or Workspace ID), use the following API call:

Endpoint URL: GET api/<Company_Prefix>/collections

Example:
https://rest.kendis.io/api/<Company_Prefix>/collections

Parameters:
- `auth` (required): Basic auth credentials (Kendis email and API Key)

Example Request:
```json
GET api/<Company_Prefix>/collections
Authorization: Basic Auth {Kendis email & API Key as Password}
```

Example Response:
```json
{
 "data": [
        {
            "id": "4gd435cd1aaf664fdr4556c",
            "title": "Collection Title"
        }
    ]
}
```

### 2. Retrieve Board ID

To retrieve the Board ID for a specific collection, use the following API call:

Endpoint URL: GET api/<Company_Prefix>/boards

Example:
https://rest.kendis.io/api/<Company_Prefix>/boards?collectionId=4gd435cd1aaf664fdr4556c,64c0345dd1waf623863d38fc

Parameters:
- `auth` (required): Basic auth credentials (Kendis email and API Key)
- `collectionId` (optional): Comma-separated list of collection IDs

Example Request:
```json
GET api/<Company_Prefix>/boards?collectionId=Col1_Id,Col2_Id
Authorization: Basic Auth {Kendis email & API Key as Password}
```

Example Response:
```json
{
    "data": [
        {
            "id": "your board Id",
            "state": {
                "id": "your state Id",
                "title": "Draft"
            },
            "title": "Board Name"
        }
    ]
}
```

### 3. Retrieve List of Program Board Items

#### Example 1: Fetch all items of a Board
This endpoint retrieves all the items on the board.
```python
import json
import requests

username = "your_username"  # Add your Kendis username
password = "your_api_key"  # Add your Kendis API Key

company_prefix = "your_company_prefix"  # Add your company prefix
board_id = "*****"
url = f'https://rest.kendis.io/api/{company_prefix}/items'

payload = {
    "boardId": board_id
}

raw_json_body = json.dumps(payload)
response = requests.post(url, data=raw_json_body, auth=(username, password), headers={"Content-Type": "application/json"})

if response.status_code == 200:
    list_items_data = response.json()
    print("Data retrieved successfully:")
    print(list_items_data)
else:
    print(f"Failed to retrieve data. HTTP Status Code: {response.status_code}")
    print("Response content:", response.text)
```

#### Example 2: Fetch 20 Features and Their Children on Page 1
Note: The `pageStart` value starts from 0. For example, if your `pageSize` is 100, the first call will return items 0 to 99. To fetch the next set of records, set `pageStart` to 100 for items 100 to 199, and so on.

```python
payload = {
    "boardId": board_id,
    "pageStart": 1,
    "pageSize": 20,
    "fields": ["children"]
}
```

#### Example 3: Fetch Selective Fields
```python
payload = {
    "boardId": board_id,
    "fields": ["createdOn", "cardType", "itemType", "iterationPath"],
    "pageStart": 1,
    "pageSize": 20
}
```

---

### 4. Retrieve Meta for Sprints

Retrieve metadata for sprints associated with a specific board.
```python
url = f'https://rest.kendis.io/api/{company_prefix}/sprints'

payload = {
    "boardId": board_id
}

raw_json_body = json.dumps(payload)
response = requests.post(url, data=raw_json_body, auth=(username, password), headers={"Content-Type": "application/json"})

if response.status_code == 200:
    sprint_data = response.json()
    print("Data retrieved successfully:")
    print(sprint_data)
else:
    print(f"Failed to retrieve data. HTTP Status Code: {response.status_code}")
    print("Response content:", response.text)
```

---

### 5. Retrieve Meta for Teams

Retrieve metadata for teams associated with a specific board.
```python
url = f'https://rest.kendis.io/api/{company_prefix}/teams'

payload = {
    "boardId": board_id
}

raw_json_body = json.dumps(payload)
response = requests.post(url, data=raw_json_body, auth=(username, password), headers={"Content-Type": "application/json"})

if response.status_code == 200:
    teams_data = response.json()
    print("Data retrieved successfully:")
    print(teams_data)
else:
    print(f"Failed to retrieve data. HTTP Status Code: {response.status_code}")
    print("Response content:", response.text)
```

---

### 6. Retrieve Meta for Statuses

Retrieve metadata for statuses associated with a specific board.
```python
url = f'https://rest.kendis.io/api/{company_prefix}/statuses'

payload = {
    "boardId": board_id
}

raw_json_body = json.dumps(payload)
response = requests.post(url, data=raw_json_body, auth=(username, password), headers={"Content-Type": "application/json"})

if response.status_code == 200:
    statuses_data = response.json()
    print("Data retrieved successfully:")
    print(statuses_data)
else:
    print(f"Failed to retrieve data. HTTP Status Code: {response.status_code}")
    print("Response content:", response.text)
```

---

### Notes
- Always ensure that your API Key is kept confidential.
- Handle errors appropriately in production environments.

---

### License
This repository is licensed under the [MIT License](LICENSE).

Thank you for using the Kendis API utilities. For further assistance, feel free to contact us or raise an issue in this repository.
