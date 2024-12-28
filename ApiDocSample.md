# API Documentation for [API Name]

**Table of Contents**:

- [Introduction](#introduction)
- [API Base URL](#api-base-url)
- [Security Schemes](#security-schemes)
  - [Authentication](#authentication)
- [Headers](#headers)
- [Query Parameters](#query-parameters)
- [Servers List](#servers-list)
- [Paths](#paths)
  - [GET](#get)
  - [POST](#post)
  - [PUT](#put)
  - [DELETE](#delete)
- [Responses](#responses)
  - [Example Responses](#example-responses)
- [Related Topics](#related-topics)
- [Additional Notes](#additional-notes)
- [Feedback](#feedback)

## Introduction

The [API Name] is designed to [describe the general purpose]. It facilitates [mention the primary functions and advantages], ensuring [mention any important performance or security features].

### Diagram - Flow Chart

![image](https://www.gliffy.com/sites/default/files/image/2020-06/Screen-Shot-2017-11-08-at-3.jpg) 


## API Base URL

All API requests should be made to the base URL:
```
https://api.[domain].com/v1
```

## Security Schemes

### Authentication

This API uses [type of authentication, e.g., OAuth 2.0, API keys] for securing endpoints. The following section describes the methods used for authenticating API requests.

## Headers

| Header Name      | Value              | Description                |
|------------------|--------------------|----------------------------|
| `Authorization`  | `Bearer {token}`   | API token for access       |
| `Content-Type`   | `application/json` | Media type of the request  |

## Query Parameters

| Parameter | Type   | Description       | Allowed Values          | Required |
|-----------|--------|-------------------|-------------------------|----------|
| `id`      | String | Unique identifier | e.g., `123`, `abc`      | Yes      |
| `type`    | String | Type of the item  | `basic`, `premium`      | No       |

## Servers List

- Production Server: `https://api.[domain].com/v1`
- Development Server: `https://dev-api.[domain].com/v1`

## Paths

### GET

- **Description:** Retrieve information about...
- **Endpoint:** `/endpoint`
- **Parameters:** [List parameters if any]
- **Response Codes:** 
  - `200 OK`: Successfully retrieved.
  - `404 Not Found`: No entries found.

### POST

> :information_source:
> : Ensure that all required fields are filled correctly to avoid errors during the creation process.

- **Description:** Create a new...
- **Endpoint:** `/endpoint`
- **Body:** 
  ```json
  {
    "name": "example"
  }
  ```
- **Response Codes:** 
  - `201 Created`: Successfully created.
  > :white_check_mark: All done! Your new [entity] has been successfully created.

### PUT

- **Description:** Update existing...
- **Endpoint:** `/endpoint/{id}`
- **Body:** 
  ```json
  {
    "name": "new example"
  }
  ```
- **Response Codes:** 
  - `200 OK`: Successfully updated.
  > :white_check_mark: Update confirmed! The [entity] details have been updated.

### DELETE

- **Description:** Delete an existing...
- **Endpoint:** `/endpoint/{id}`
- **Response Codes:** 
  - `204 No Content`: Successfully deleted.
  > :white_check_mark: Deletion successful! The [entity] has been removed from the system.

## Responses

### Example Responses

- `200 OK`
  ```json
  {
    "status": "success",
    "data": {
      "id": "1",
      "name": "example"
    }
  }
  ```
- `400 Bad Request`
  ```json
  {
    "status": "error",
    "message": "Invalid request parameters"
  }
  ```
- `401 Unauthorized`
  ```json
  {
    "status": "error",
    "message": "Authentication credentials were not provided."
  }
  ```

## Related Topics

- [Link to additional API guidelines](#)
- [Using the API securely](#)
- [Troubleshooting and common issues](#)

## Additional Notes

- For more details about each API method, refer to the specified sections above.
- Ensure that all API interactions are conducted over HTTPS to secure data in transit.

>[!NOTE]
   >
   > Changes in the API or the way it's accessed can be frequent. Always check this documentation for the latest updates before making calls.

## Feedback

Was this article helpful? (Link to Forms)
|[:heavy_check_mark: Yes](teste)|[:x: No](teste)|
|---|---|
