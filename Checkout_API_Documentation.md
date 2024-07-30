# Checkout API Documentation

# Authentication

- For all checkout endpoints to work, you must pass the token in the request header.
- To obtain the `token`, you need to register on the platform, activate the checkout, and then go to your profile where there will be a field containing `CHECKOUT API TOKEN`.

![Captura de tela de 2024-05-27 15-11-50.png](https://www.dropbox.com/scl/fi/rwvnqom2dor0iw2d3pq51/Captura-de-tela-de-2024-05-27-15-11-50.png?rlkey=mm3wpc7fyr3ucnww81jtq8g68&dl=0&raw=1)



----

# Get Checkouts

#### URL: `GET /api/get-checkouts/`

#### Description
This endpoint retrieves a list of checkout sessions for an authenticated user. Users can authenticate either via a session or by providing a valid API token. It supports various filters and pagination.

#### Request

##### Headers
- `Content-Type: application/json`
- `token: <CHECKOUT_API_TOKEN>` 

##### Query Parameters
- `id` (int): Filter by specific checkout ID.
- `search` (string): Search by description, case-insensitive.
- `is_enabled` (string): Filter by enabled status (`true` or `false`).
- `created_at_start` (ISO 8601 date): Filter by start of creation date range.
- `created_at_end` (ISO 8601 date): Filter by end of creation date range.
- `order_by` (string): Field to order by (default: `created_at`).
- `order_direction` (string): Order direction, `asc` for ascending or `desc` for descending (default: `desc`).
- `page` (int): Page number for pagination (default: 1).

#### Responses

##### Success

- Status Code: `200 OK`
- Content:
    ```JSON
    {
      "checkouts": [
        {
          "id": 123,
          "image": "base64string",
          "image_type": "image/png",
          "description": "Example item",
          "price": 19.99,
          "redirect_url": "https://example.com/success",
          "is_enabled": true,
          "theme_color": "#0000ff",
          "payment_method": "credit_card",
          "colors": {
            "primary": "#ffffff",
            "secondary": "#f5f5f5",
            "text": "#000000",
            "button": "#ff0000",
            "textButton": "#ffffff"
          },
          "created_at": "2023-06-10T10:00:00Z",
          "url_id": "unique-url-id"
        }
      ],
      "current_page": 1,
      "total_pages": 1
    }
    ```

##### Error

- Status Code: `401 Unauthorized`
- Content: 
  ```JSON
    {
      "isAuthenticated": "invalid token"
    }
  ```

- Status Code: `500 Internal Server Error`
- Content:
    ```JSON
    {
      "detail": "Failed to get checkouts."
    }
    ```

#### Example Curl Request
```bash
curl -X GET "http://yourdomain.com/get-checkouts/?search=example&is_enabled=true&page=2" \
     -H "Content-Type: application/json" \
     -H "token: abc123"
```
---

<!-- -------------------------------------------------------------------------- -->

# Create Checkout

#### URL: `POST /api/create-checkout/`

#### Description

This endpoint allows the creation of a new checkout.


#### Request

##### Headers

- `Content-Type: multipart/form-data`
- `token: <CHECKOUT_API_TOKEN>`

##### Body Parameters

- `data` (JSON string, required): JSON string containing checkout details.
  - `colors` (object, required): Contains color configuration.
    - `primary` (string, required): Primary background color.
    - `secondary` (string, required): Secondary background color.
    - `text` (string, required): Text color.
    - `button` (string, required): Button color.
    - `textButton` (string, required): Button text color.
  - `price` (number, required): Price of the checkout item.
  - `description` (string, required): Description of the checkout item.
  - `redirect_url` (string, required): URL to redirect after checkout.
  - `payment_method` (string, required): The payment method to be used(`credit_card`, `pix` or `all`).
  - `is_enabled` (boolean, required): Indicates if the checkout is enabled.
  - `theme_color` (string, required): Theme color for the checkout. Must always be `custom`.
- `file` (file, optional): Image file for the checkout item.

##### Example

```json
{
  "token": "your_api_token",
  "data": "{\"colors\": {\"primary\": \"#FFFFFF\", \"secondary\": \"#000000\", \"text\": \"#000000\", \"button\": \"#FF0000\", \"textButton\": \"#FFFFFF\"}, \"price\": 19.99, \"description\": \"Product description\", \"redirect_url\": \"https://example.com\", \"is_enabled\": true, \"payment_method\": \"all\", \"theme_color\": \"custom\"}"
  "file": path_to_your_image.png
}
```
**Notes**: 
- The file parameter is optional. If provided, it will be validated to ensure it is a PNG file.
- Ensure the data parameter is a valid JSON string.
- Authentication is through token.

#### Responses

##### Success

- Status Code: `201 Created`
- Content:
```JSON
{
    "id": 1,
    "url_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

##### Error

- Status Code: `401 Unauthorized`
- Content:
```JSON
{
  "isAuthenticated": false
}
```

- Status Code: `500 Internal Server Error`
- Content:
```JSON
{
  "detail": "Failed to create checkout."
}
```

#### Example Curl Request

```bash
curl -X POST https://${URL}/create-checkout/ \
  -H "Content-Type: multipart/form-data" \
  -H "token=your_api_token" \
  -F "data={\"colors\": {\"primary\": \"#FFFFFF\", \"secondary\": \"#000000\", \"text\": \"#000000\", \"button\": \"#FF0000\", \"textButton\": \"#FFFFFF\"}, \"price\": 19.99, \"description\": \"Product description\", \"redirect_url\": \"https://example.com\", \"is_enabled\": true, \"theme_color\": \"custom\"}" \
  -F "file=@path_to_your_image.png"
```



---

# Update Checkout

#### URL: `PUT /api/update-checkout/<id>/`

#### Description

This endpoint allows authenticated users to update an existing checkout.

#### Request

##### Headers

- `Content-Type: multipart/form-data`
- `token: <CHECKOUT_API_TOKEN>`

#### Path Parameters

- `id` (integer, required): The ID of the checkout to be updated.
- `token` (string, required): API token for authentication if the user is not authenticated via session.

##### Body Parameters

- `data` (JSON string, required): JSON string containing checkout details.
  - `colors` (object, optional): Contains color configuration.
    - `primary` (string, optional): Primary background color.
    - `secondary` (string, optional): Secondary background color.
    - `text` (string, optional): Text color.
    - `button` (string, optional): Button color.
    - `textButton` (string, optional): Button text color.
  - `price` (number, optional): Price of the checkout item.
  - `description` (string, optional): Description of the checkout item.
  - `redirect_url` (string, optional): URL to redirect after checkout.
  - `payment_method` (string, optional): The payment method to be used(`credit_card`, `pix` or `all`).
  - `is_enabled` (boolean, optional): Indicates if the checkout is enabled.
  - `theme_color` (string, required): Must always be `custom`.
  - `change_image` (boolean, optional): Indicates if the image should be changed (default is `True`).
- `file` (file, optional): Image file for the checkout item.

**Note:** The image is optional, but if not provided while editing, it will be removed.


##### Example

```json
{
  "token": "your_api_token",
  "data": "{\"colors\": {\"primary\": \"#FFFFFF\", \"secondary\": \"#000000\", \"text\": \"#000000\", \"button\": \"#FF0000\", \"textButton\": \"#FFFFFF\"}, \"price\": 19.99, \"description\": \"Updated product description\", \"redirect_url\": \"https://example.com\", \"is_enabled\": true, \"theme_color\": \"custom\", \"change_image\": true}",
  "file": "path_to_your_new_image.png"
}
```

#### Responses

##### Success

- Status Code: `200 OK`
- Content:
```JSON
{
     "detail": "Checkout updated successfully."
}
```

##### Error

- Status Code: `401 Unauthorized`
- Content:
```JSON
{
  "isAuthenticated": false
}
```

- Status Code: `403 Forbidden`
- Content:
```JSON
{
  "error": "Permission denied"
}
```

- Status Code: `404 Not Found`
- Content:
```JSON
{
  "error": "Checkout not found"
}
```

- Status Code: `500 Internal Server Error`
- Content:
```JSON
{
  "detail": "Failed to create checkout."
}
```

#### Example Curl Request

```bash
curl -X PUT https://${URL}/update-checkout/:id/ \
  -H "Content-Type: multipart/form-data" \
  -H "token=your_api_token" \
  -F "data=$(echo '{
    "colors": {
      "primary": "#FFFFFF",
      "secondary": "#000000",
      "text": "#000000",
      "button": "#FF0000",
      "textButton": "#FFFFFF"
    },
    "price": 19.99,
    "description": "Updated product description",
    "redirect_url": "https://example.com",
    "is_enabled": true,
    "theme_color": "custom",
    "change_image": true
  }' | jq -c .)" \
  -F "file=@path_to_your_new_image.png"

```
















