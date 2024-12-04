# Payment Link

## Authentication

All API requests must include an `api-key` in the request headers for authentication.

**Header Example:**

```http
api-key: YOUR_API_KEY_HERE
```

Replace `YOUR_API_KEY_HERE` with your actual API key.

---

## Endpoints

### Create Link

Create a new payment link.

- **URL:** `/link/`
- **Method:** `POST`
- **Headers:**
  - `Content-Type: application/json`
  - `api-key: YOUR_API_KEY_HERE`
- **Request Body:**

  ```json
  {
    "description": "Description of the payment",
    "reusable": false,
    "amount": 100.00
  }
  ```

  - `description` *(string)*: A brief description of the payment link.
  - `reusable` *(boolean)*: Indicates whether the link can be used multiple times.
  - `amount` *(number)*: The payment amount.

- **Response:**

  - **Status Code:** `200 OK`
  - **Body:**

    ```json
    {
      "url": "http://yourdomain.com/link/unique-uuid"
    }
    ```

    - `url` *(string)*: The URL of the created payment link.

---

### Get Link

Retrieve details of an existing payment link.

- **URL:** `/link/{uuid}?format=json`
- **Method:** `GET`
- **Headers:**
  - `api-key: YOUR_API_KEY_HERE`
- **Path Parameters:**
  - `uuid` *(string)*: The unique identifier of the payment link.

- **Response:**

  - **Status Code:** `200 OK`
  - **Body:**

    ```json
    {
      "id": 663,
      "url": "61943396-9cdb-48a7-95be-b45b0d5f963e",
      "log": "---boundary 2024-12-04T14:15:12.489006-03:00---\nCreating payment link: Purchase of Item XYZ\nAmount: 50.00\nReusable: false\nURL: http://yourdomain.com/link/61943396-9cdb-48a7-95be-b45b0d5f963e\n\n",
      "created_at": "2024-12-04T17:15:12.489023Z",
      "description": "Purchase of Item XYZ",
      "amount": 50.00,
      "reusable": false,
      "merchant_id": 9,
      "disabled": false,
      "valid_until": "2025-01-03T17:15:12.488962Z"
    }
    ```

    - `id` *(integer)*: The unique identifier of the payment link.
    - `url` *(string)*: The URL identifier of the payment link.
    - `log` *(string)*: Log details related to the creation of the link.
    - `created_at` *(string)*: Timestamp of when the link was created.
    - `description` *(string)*: Description of the payment link.
    - `amount` *(number)*: The payment amount.
    - `reusable` *(boolean)*: Indicates if the link is reusable.
    - `merchant_id` *(integer)*: Identifier for the merchant.
    - `disabled` *(boolean)*: Indicates if the link is disabled.
    - `valid_until` *(string)*: Expiration timestamp of the link.

---

### Update Link

Update an existing payment link's details.

- **URL:** `/link/{uuid}`
- **Method:** `PUT`
- **Headers:**
  - `Content-Type: application/json`
  - `api-key: YOUR_API_KEY_HERE`
- **Path Parameters:**
  - `uuid` *(string)*: The unique identifier of the payment link.

- **Request Body:**

  ```json
  {
    "description": "Updated description",
    "amount": 150.00,
    "reusable": true,
    "disabled": false,
    "valid_until": "2024-12-31T23:59:59Z"
  }
  ```

  - `description` *(string)*: Updated description of the payment link.
  - `amount` *(number)*: Updated payment amount.
  - `reusable` *(boolean)*: Updated reusable status.
  - `disabled` *(boolean)*: Updated disabled status.
  - `valid_until` *(string)*: Updated expiration timestamp in ISO 8601 format.

- **Response:**

  - **Status Code:** `200 OK`
  - **Body:**

    ```json
    {
      "status": "OK"
    }
    ```
---

## Error Handling

While not covered in the scripts, it's recommended to handle potential errors such as invalid input, authentication failures, or server issues. Ensure your application gracefully handles these scenarios by checking response status codes and messages.

