# Errors

The Scholarcy API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The API endpoint requested is hidden for administrators only.
404 | Not Found -- The API endpoint could not be found.
405 | Method Not Allowed -- You tried to call the API with an invalid method.
406 | Not Acceptable -- You requested a format that isn't JSON.
429 | Too Many Requests -- You're making too many API requests.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
504 | Gateway Timeout -- Serving your request took longer than expected. Please try again.
