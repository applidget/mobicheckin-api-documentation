# MobiCheckin API

The [MobiCheckin](http://www.mobicheckin.com) API is implemented as vanilla JSON and XML over HTTP using all
four verbs (GET/POST/PUT/DELETE). Every resource, like Event, Guest, or Check-in,
has their own URL and are manipulated in isolation. In other words, we've tried
to make the API follow the REST principles.

You can explore the view part of the API (everything that's fetched with GET)
through a regular browser. Using Firefox for this is particularly nice as it has
a good, simple XML renderer. You can also get a nice view of the JSON version of
the API by installing [JSON View](https://addons.mozilla.org/fr/firefox/addon/jsonview/).

We dogfood this API as it is used by our [iPad guest list app](https://itunes.apple.com/app/mobicheckin/id510125877?mt=8).


## API Endpoints

If you want a summary of all the endpoints method served by MobiCheckin, you
can look at the [MobiCheckin API Overview](https://app.mobicheckin.com/api#endpoints).

* [Events](https://github.com/applidget/mobicheckin-api-documentation/blob/master/sections/events.md)
* [Guests](https://github.com/applidget/mobicheckin-api-documentation/blob/master/sections/guests.md)
* [Guest Categories](https://github.com/applidget/mobicheckin-api-documentation/blob/master/sections/guest_categories.md)
* [Check-in Points](https://github.com/applidget/mobicheckin-api-documentation/blob/master/sections/check_in_points.md)
* [Check-ins](https://github.com/applidget/mobicheckin-api-documentation/blob/master/sections/check_ins.md)
* [Signatures](https://github.com/applidget/mobicheckin-api-documentation/blob/master/sections/signatures.md)

(Hint: Press `t` to enable the file finder and type out the endpoint you need!)

## Authentication

When you're using the API, it's always through an existing user in MobiCheckin.
There's no special API user. So when you use the API as "john@company.org", you
get to see and work with what John is allowed to. Authenticating is done with an
authentication token, which you'll [here](https://app.mobicheckin.com/fr/api)
(you must be logged in).

Then you just need to append the `auth_token` as a query parameter of the URL.
For instance, you can retrieve a JSON array of all the events you manage by
issuing the following request:

    GET /api/v1/events.json?auth_token=YOUR_API_TOKEN

## Format

The MobiCheckin API supports both XML and JSON. You can choose the format you
prefer by simply appending `.xml` or `.json` at the end of the resource URL
you query.

    Request
      GET /api/v1/events.json?auth_token=YOUR_API_TOKEN

    Response
      HTTP/1.1 200 OK
      Content-Type: application/json; charset=utf-8

And for the XML version:

    Request
      GET /api/v1/events.xml?auth_token=YOUR_API_TOKEN

    Response
      HTTP/1.1 200 OK
      Content-Type: application/xml; charset=utf-8

In the endpoints documentation, we'll use `.format` to refer to `.xml` or `.json`.

When you want to write some data to the API with `POST` (creation) or `PUT` (update),
you'll need to specify a content-type request header.

    Request
      POST /api/v1/events/{event_id}/guests.json?auth_token=YOUR_API_TOKEN
      Content-Type: application/json; charset=utf-8

    Response
      HTTP/1.1 201 Created
      Content-Type: application/json; charset=utf-8

And for the XML version, you get the idead.

## API Return Codes

The API uses the following HTTP status code in the response. Pay attention to them
as they will tell you if an error occurred.

* [200](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.1) OK - For any `GET` method which succeds.
* [201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.2) Created - For any `POST` method which succeds.
* [204](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.5) No Content - For any `PUT` or `DELETE` method which succeds.
* [400](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.1) Bad Request - Check your request body.
* [403](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.4) Forbidden - You might have forgotten the `auth_token`.
* [404](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.5) Not Found - The resource does not exist.
* [406](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.5) Not Acceptable - You might have forgotten the `.format` or the request `Content-Type`.
* 422 Unprocessable Entity - Check your request body, the JSON or XML might be malformed.

## Error messages

If you don't receive a `2xx` HTTP status code in the response, it means that there
is an error. You can look at the response body to get more insight about why
your API call was not successful.

### JSON
If there is a general error, we'll send you this:
```
DELETE /api/v1/events.json
```
```js
// 404 Not Found
{
  "error": "No API endpoint found at /api/v1/events and method DELETE"
}
```
Now if there is an error with the payload provided to create (`POST`) or update
(`PUT`) a resource, we'll detail the validation errors like this:

#### HTTP Request
```
POST /api/v1/events/{event_id}.json
Content-Type: application/json; charset=utf-8

{ "first_name": "Sébastien", "company_name": "Applidget" }
```
```js
// 422 Unprocessable Entity
{
  "errors":
  {
    "uid":
    [
      "is invalid",
      "can't be blank"
    ],
    "guest_category_id":
    [
      "can't be blank"
    ],
    "last_name":
    [
      "should be set, or at least one other element of identity: email, company or uid."
    ]
  }
}
```

### XML
For the same exemple above, you will have the following responses:

```xml
<!-- 404 Not Found -->
<?xml version="1.0" encoding="UTF-8"?>
<errors>
  <error>No API endpoint found at /api/v1/events and method DELETE</error>
</errors>
```

```xml
<!-- 422 Unprocessable Entity -->
<?xml version="1.0" encoding="UTF-8"?>
<errors>
  <error>Guest Uid is invalid</error>
  <error>Guest Uid can't be blank</error>
  <error>Guest Category can't be blank</error>
  <error>Last Name should be set, or at least one other element of identity: email, company or uid.</error>
</errors>
```