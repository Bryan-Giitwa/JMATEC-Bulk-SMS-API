# JMATEC Bulk SMS API

JMATEC Bulk SMS API provides a lightweight REST interface for sending transactional and promotional SMS messages. It supports single and bulk sends, per-recipient delivery details, sender ID validation, phone-number normalization (to 254XXXXXXXXX), and clear error/status codes to simplify integration .

## Base URL:

https://api.jmatecsystems.com/api/v1/sms

---

## Endpoints:

### 1. **Send SMS**

- **Endpoint:** `/sendsms`
- **Method:** `POST`
- **Description:** Sends SMS to one or multiple recipients.
- **Request Body:**

  ```json
  {
    "api_key": "your_api_key",
    "sender_id": "approved_sender_id",
    "message": "Your SMS message",
    "phone": "comma_separated_phone_numbers"
  }
  ```

  - `api_key` (required): API key for authentication.
  - `sender_id` (optional): Approved sender ID. If not provided, the default sender ID will be used.
  - `message` (required): The SMS message to send.
  - `phone` (required): SIngle or Comma-separated list of phone numbers (e.g., `254712345678,254798765432`).

- **Response:**

  ```json
  {
    "status_code": 2000,
    "status": "complete",
    "message": "SMS sending completed",
    "data": [
      {
        "phone": "254712345678",
        "status": "success",
        "message_id": "unique_message_id",
        "sms_cost": 1.0
      }
    ]
  }
  ```

  - `status_code`: Status code of the operation.
  - `status`: Status of the request (`complete` or `failed`).
  - `message`: Description of the result.
  - `data`: Array of SMS delivery details (phone, status, message ID, and cost).

- **Error Responses:**

  - Missing API key:

    ```json
    {
      "status": "failed",
      "status_code": 4008,
      "message": "Invalid or missing API key"
    }
    ```

  - Missing phone number(s):

    ```json
    {
      "status": "failed",
      "status_code": 4002,
      "message": "Phone number(s) are required"
    }
    ```

  - Invalid phone number format:

    ```json
    {
      "status": "failed",
      "status_code": 4006,
      "message": "Invalid phone number format"
    }
    ```

  - Missing message:

    ```json
    {
      "status": "failed",
      "status_code": 4004,
      "message": "Message is required"
    }
    ```

  - Sender ID not found:

    ```json
    {
      "status": "failed",
      "status_code": 4007,
      "message": "Sender ID not found or not approved for this user"
    }
    ```

  - Sender ID not approved:

    ```json
    {
      "status": "failed",
      "status_code": 4005,
      "message": "No approved sender ID found for this user"
    }
    ```

---

### 2. **Get SMS Balance**

- **Endpoint:** `/credit-balance`
- **Method:** `POST`
- **Description:** Retrieves the SMS credit balance for the authenticated user.
- **Request Body:**

  ```json
  {
    "api_key": "your_api_key"
  }
  ```

  - `api_key` (required): keyAPI for authentication.

- **Response:**

  ```json
  {
    "status_code": 2000,
    "status": "success",
    "balance": 100.0
  }
  ```

  - `status_code`: Status code of the operation.
  - `status`: Status of the request (`success` or `failed`).
  - `balance`: Remaining SMS credits.

- **Error Responses:**
  - Missing API key:
    ```json
    {
      "status_code": 4008,
      "status": "Invalid or missing API key",
      "balance": null
    }
    ```
  - No credits available:
    ```json
    {
      "status_code": 4009,
      "status": "Not enough credits",
      "balance": 0
    }
    ```

---

## Helper Notes:

1. **Phone Number Format:**

   - All phone numbers must be normalized to the format `254XXXXXXXXX`.
   - Examples:
     - `0712345678` → `254712345678`
     - `254712345678` → `254712345678`

2. **Sender ID Validation:**

   - The `sender_id` must be approved for the user or default to the system's sender ID.

3. **Status Codes:**

   - `2000`: Success.
   - `4002`: Missing phone number(s).
   - `4004`: Missing message.
   - `4005`: Sender ID not approved.
   - `4006`: Invalid phone number format.
   - `4007`: Sender ID not found.
   - `4008`: Missing or invalid API key.
   - `4009`: Not enough credits.
   - `5000`: Internal server error.

---

## Example Usage:

### Sending SMS:

```bash
curl -X POST https://api.jmatecsystems.com/api/v1/sms/sms/sendsms \
-H "Content-Type: application/json" \
-d '{
  "api_key": "your_api_key",
  "sender_id": "JMATEC_SYS",
  "message": "Hello, this is a test message",
  "phone": "254712345678,254798765432"
}'
```
