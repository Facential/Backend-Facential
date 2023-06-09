# Facential Backend API

This API allows you to classify skin types based on facial images and retrieve skincare recommendations for each skin type. It also provides endpoints to retrieve classification results, delete classification results, and get skincare recommendations for different skin types.

## Base URL
- www.facential.site
- http://34.101.100.21/


# Design Architecture
![Cloud Computing Design Architecture](https://github.com/Facential/Backend-Facential/blob/42cb36502fef2d9d07dc2daa6ed91225ddfc7532/Cuplikan%20layar%202023-06-16%20184835.png)


# Endpoints
## GET /
This endpoint is used to check the API's availability.

### Request
```
GET /
```
### Response
• Success (200 OK)
```json
{
  "message": "Success!"
}
```
• Error (404 Not Found)
```json
{
  "message": "Error: Resource not found."
}
```
## POST /classify
This endpoint is used to classify the skin type based on a facial image and provide skincare recommendations.

### Request
```
POST /classify
Content-Type: multipart/form-data
Authorization: <user_token>

image: <image_file>
```
image: A facial image file (JPEG or PNG format).

### Response
• Success (200 OK)
```json
{
  "skin_type": "<predicted_skin_type>"
}
```
• Error (400 Bad Request)
```json
{
  "message": "Image does not contain a valid face."
}
```
## GET /getdata/{user_uid}/latest
This endpoint is used to retrieve the latest classification result for a specific user UID.

### Request
```
GET /getdata/{user_uid}/latest
```
user_uid: The unique identifier of the user.

### Response
• Success (200 OK)
```json
{
  "id": "<classification_id>",
  "user_uid": "<user_uid>",
  "skin_type": "<predicted_skin_type>",
  "image_url": "<image_url>",
  "overall": "<overall_condition_text>",
  "cleansing": "<cleansing_product>",
  "toner": "<toner_product>",
  "serum": "<serum_product>",
  "moisturizer": "<moisturizer_product>",
  "sunscreen": "<sunscreen_product>"
}
```
• Error (404 Not Found)
```json
{
  "message": "No classification results found for the specified user UID."
}
```

## GET /getdata/{user_uid}
This endpoint is used to retrieve all data from classification result for a specific user UID.

### Request
```
GET /getdata/{user_uid}
```
user_uid: The unique identifier of the user.

### Response
• Success (200 OK)
Example response body
```json
[
    {
        "id": 1,
        "image_url": "https://storage.googleapis.com/blabla/blabla.jpg",
        "skin_type": "Sensitive",
        "timestamp": "Sat, 10 Jun 2023 21:45:28 GMT",
        "user_uid": "useruid"
    },
    {
        "id": 2,
        "image_url": "https://storage.googleapis.com/blabla/blabla2.jpg",
        "skin_type": "Sensitive",
        "timestamp": "Sat, 10 Jun 2023 21:46:33 GMT",
        "user_uid": "useruid"
    }
]
```
• Error (404 Not Found)
Error when no data is found in Database.
```json
{
    "message": "No data found for the specified user UID."
}
```
• Error (500 Internal Server Error)
Error in Internal Server
```json
{
    "message": "Error: Failed to retrieve the data.",
    "error": str(e)
}
```

## GET /getdata/{user_uid}/{id}
This endpoint is used to retrieve spesific classification result for a specific user UID and ID.

### Request
```
GET /getdata/{user_uid}/{id}
```
- user_uid: The unique identifier of the user.
- id: The unique indetifier data in database.

### Response
• Success (200 OK)
```json
{
  "id": "<classification_id>",
  "user_uid": "<user_uid>",
  "skin_type": "<predicted_skin_type>",
  "image_url": "<image_url>",
  "overall": "<overall_condition_text>",
  "cleansing": "<cleansing_product>",
  "toner": "<toner_product>",
  "serum": "<serum_product>",
  "moisturizer": "<moisturizer_product>",
  "sunscreen": "<sunscreen_product>"
}
```
• Error (404 Not Found)
```json
{
  "message": "No data found for the specified user UID and ID."
}
```
• Error (500 Internal Server Error)
Error in Internal Server
```json
{
     "message": "Error: Failed to retrieve the data.",
     "error": str(e)
}
```

## DELETE /delete/{user_uid}
This endpoint is used to delete all classification results and associated images for a specific user UID.

### Request
```
DELETE /delete/{user_uid}
```
user_uid: The unique identifier of the user.

### Response
• Success (200 OK)
```json
{
  "message": "Classification results deleted successfully."
}
```
• Error (404 Not Found)
```json
{
  "message": "Classification results not found."
}
```

## DELETE /delete/{user_uid}/{id}
This endpoint is used to delete spesific classification results and associated images for a specific user UID and ID.

### Request
```
DELETE /deleteall/{user_uid}/{id}
```
- user_uid: The unique identifier of the user.
- id: The unique indetifier data in database.

### Response
• Success (200 OK)
```json
{
  "message": "Record deleted successfully."
}
```
• Error (404 Not Found)
```json
{
  "message": "Error: Record not found."
}
```
• Error (500 Internal Server Error)
Error in Internal Server
```json
{
     "message": "Error: Failed to delete the record.",
     "error": str(e)
}
```

## POST /recommendation
This endpoint retrieves skincare recommendations based on weather and air pollution data.

### Request
```
POST /recommendation
Content-Type: multipart/form-data
latitude: <latitude>
longitude: <longitude>
```
- Latitude and Longitude maximal have eight characters after point.
- Latitude and Longitude uses Decimal Degrees with Datum WGS 1984 and Projection EPSG:4326 
- Example: -6.14116367,106.81437637

### Response
• Success (200 OK)
Example response body:
```json
{
    "temperature": 28,
    "pollution_index": 75,
    "skincare_recommendation": "Use oil-free and lightweight skincare products to prevent excessive oiliness. Apply a physical barrier like a mask or use anti-pollution skincare products."
}
```
- temperature (number): The current temperature in Celsius.
- pollution_index (number or null): The air pollution index. If not available, it will be null.
- skincare_recommendation (string): The recommended skincare routine based on the temperature and pollution index.

• Error (400 Bad Request)
The request was missing the required parameters (latitude and longitude).
Example response body:
```json
{
    "message": "Latitude and longitude are required parameters."
}
```

• Error (500 Internal Server Error)
There was an error fetching weather and air pollution data.
Example response body:
```json
{
    "message": "Failed to fetch weather and air pollution data."
}
```
