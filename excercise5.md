# Helper API Client

This is a Python client for the "Helper" API, designed to interface with the Example Image Service. The main purpose is to provide an easy-to-use way to search for images, retrieve a specific image's details, and download an image.

## Features

- **Easy Authorization**: The client uses a predefined authorization token structure to seamlessly authenticate API calls.
- **Flexible Search**: Users can pass any number of parameters for a flexible search experience.
- **Dedicated Download**: The client provides a dedicated method for downloading images.

## Usage

### Setup

First, import the `Helper` class:

```python
from helper_api_client import Helper
```

Ensure you have the required `requests` library installed:

```
pip install requests
```

### Initialize the Client

You can create an instance of the Helper class:

```python
helper_client = Helper()
```

### Set the Authorization Token

Before making any API calls, ensure you set the authorization token:

```python
Helper.AUTHORIZATION_TOKEN = {
    'access_token': 'YOUR_ACCESS_TOKEN',
    'token_type': 'Bearer',
    'expires_in': 3600,
    'refresh_token': 'YOUR_REFRESH_TOKEN'
}
```

### Search for Images

To search for images, use the `search_images` method. You can pass in various keyword arguments that fit the search criteria:

```python
response = helper_client.search_images(keyword="sunrise", category="nature")
```

### Get Details of a Specific Image

To retrieve the details of a specific image, use the `get_image` method and pass in the image ID:

```python
response = helper_client.get_image(image_id=12345)
```

### Download an Image

To download an image, use the `download_image` method and provide the image ID:

```python
response = helper_client.download_image(image_id=12345)
```

## Code Implementation

```python
import requests

class Helper:
    DOMAIN = 'http://example.com'
    SEARCH_IMAGES_ENDPOINT = 'search/images'
    GET_IMAGE_ENDPOINT = 'image'
    DOWNLOAD_IMAGE_ENDPOINT = 'downloads/images'

    AUTHORIZATION_TOKEN = {
        'access_token': None,
        'token_type': None,
        'expires_in': 0,
        'refresh_token': None
    }

    def _send_request(self, endpoint, method="get", **kwargs):
        """Private method to send request."""
        token_type = self.AUTHORIZATION_TOKEN['token_type']
        access_token = self.AUTHORIZATION_TOKEN['access_token']
        
        headers = {
            'Authorization': f'{token_type} {access_token}',
        }

        URL = f'{self.DOMAIN}/{endpoint}'
        
        if method == "get":
            return requests.get(URL, headers=headers, params=kwargs)
        elif method == "post":
            return requests.post(URL, headers=headers, data=kwargs)
        else:
            raise ValueError(f"Unsupported method: {method}")
    
    def search_images(self, **kwargs):
        return self._send_request(self.SEARCH_IMAGES_ENDPOINT, "get", **kwargs)
        
    def get_image(self, image_id, **kwargs):
        endpoint = f'{self.GET_IMAGE_ENDPOINT}/{image_id}'
        return self._send_request(endpoint, "get", **kwargs)

    def download_image(self, image_id, **kwargs):
        endpoint = f'{self.DOWNLOAD_IMAGE_ENDPOINT}/{image_id}'
        return self._send_request(endpoint, "post", **kwargs)
```

## Refactoring Highlights

- A single private method, `_send_request`, is introduced to handle the common functionalities of sending requests to the API, reducing code redundancy.
- The client uses the `_send_request` method in the other three public methods (`search_images`, `get_image`, and `download_image`) for consistency and maintainability.

## Dependencies

- requests
