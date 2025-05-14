## Getting Registrants for a Vimeo Video via API

Important Note: Accessing the list of registrants via the Vimeo API necessitates a Vimeo Enterprise account with the requisite capabilities enabled. Please refer to the Vimeo API documentation concerning the `"Lead Capture"` endpoints for comprehensive details regarding account prerequisites, authentication procedures, API usage guidelines, error handling, and the format of the registrant data returned.

To obtain the list of users who have registered through the lead capture form associated with a specific Vimeo video, the following `curl` command can be used:

~~~```bash
curl -H "Authorization: Bearer {YOUR_VIMEO_API_TOKEN}" "https://api.vimeo.com/lead_capture/{resource_type}/{resource_id}/registrants"
~~~

Explanation:

![image](https://github.com/user-attachments/assets/8f8a3949-3323-430f-86c0-0fe807a2286a)


* `-H "Authorization: Bearer {YOUR_VIMEO_API_TOKEN}"`: This sets the necessary authorization header for the Vimeo API.

* Replace `{YOUR_VIMEO_API_TOKEN}` with your actual Vimeo API access token.

* `{resource_type}`(string) refers to the type of resource (e.g. albums - The resource is a showcase; live_events - The resource is an event; videos - The resource is a video.)

* Important: This particular API endpoint requires a token with the public scope and the `CAPABILITY_ACCESS_LEADS` capability. Please ensure your token has these permissions enabled.

* `"https://api.vimeo.com/lead_capture/videos/{VIDEO_ID}/registrants"`: This is the API endpoint you're calling.
Replace `{VIDEO_ID}` with the unique identifier of the Vimeo video you're interested in. The `videos` part of the URL specifies the resource type.

Possible Responses:

A successful request (indicated by an HTTP status code of `200 OK`) will return a JSON response. This JSON will contain an array of objects, where each object represents a user who has registered. The information provided for each registrant is likely to include:

* The registrant's email address.
* Their first name and last name (if they were provided in the lead capture form).
* Potentially other data that was captured by the form's fields.
* Timestamps related to when they registered.

Pagination:

If the video has a large number of registrants, the API might return the results in paginated form. You can use the optional query parameters `page` and `per_page` to navigate through these results. For example, to fetch the second page of registrants, with 50 results per page, you would use a command like this:

Bash:

~~~```bash
curl -H "Authorization: Bearer {YOUR_VIMEO_API_TOKEN}" "[https://api.vimeo.com/lead_capture/videos/](https://api.vimeo.com/lead_capture/videos/){VIDEO_ID}/registrants?page=2&per_page=50"
~~~

Remember to substitute `{YOUR_VIMEO_API_TOKEN}` with your actual API token and `{VIDEO_ID}` with the specific ID of the video you're querying.

Other:

You can use the query parameters to get more specific or organized data:

* `page`: If there are more registrants than the per_page limit (currently 25 by default), you can use the page parameter to navigate through the results (e.g., `?page=2` to get the next set of registrants). The paging section in your response indicates if there are more pages (`"next": "/lead_capture/videos/1083907696/registrants?page=2"`).

* `per_page`: You can adjust the number of registrants returned per page, up to a maximum of 100 (e.g., ?per_page=100).

* `sort`: You can sort the registrants based on specific attributes:

* `created_at`: Sort by the time the registrant signed up. This could be useful to see the order of registrations.
* `email`: Sort alphabetically by the registrant's email address.
* `sort_direction`: You can specify the sorting order:

* `asc`: Ascending order (e.g., oldest to newest for `created_at`, A to Z for `email`).
* `desc`: Descending order (e.g., newest to oldest for `created_at`, Z to A for `email`).
