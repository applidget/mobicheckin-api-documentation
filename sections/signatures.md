# Signatures

## Retrieve the list of signatures

MobiCheckin provides a paginaged API to retrieve the list of guest signatures of your event.
Starts at `page=1`. Paginate every 500 signatures.
If you want to know how many signatures were gathered at your event, you should
query `GET /api/v1/events/{id}.format` and look at the property `signature_count`.

### JSON
#### Request
```
GET /api/v1/events/{event_id}/signatures.json?page=1&auth_token=YOUR_API_TOKEN
```
#### Response
```js
// 200 OK
[
  {
    "_id": "{id}",

    // When did the signature took place? (UTC, use event.timezone for display purposes)
    "access_stamp": "2012-11-26T15:44:44+01:00",

    // The UID found in the scanned QR code. Beware, guest.uid != guest.id
    "guest_uid": "68vju8yxgl",

    // The check-in point where the signature took place
    "accesspoint_id": "{check_in_point_id}",

    // The SVG representation of the signature
    "points": {
      "width": "276",
      "height": "266",
      "lines": [
        {
          "x2": "250.625",
          "x1": "272",
          "stroke-width": "2.3333332538604736",
          "y2": "65.75",
          "y1": "92"
        },
        {
          "x2": "218.75",
          "x1": "250.625",
          "stroke-width": "1.7055554389953613",
          "y2": "42.375",
          "y1": "65.75"
        },
        /...
      ]
    }

    "event_id": "{event_id}",
    "created_at": "2012-11-26T15:44:44+01:00",
    "updated_at": "2012-11-26T15:44:44+01:00"
  },
  {
    "_id": "{id}",
    "access_stamp": "2012-11-26T15:44:44+01:00",
    "guest_uid": "5ch0b30mnc",
    "accesspoint_id": "{check_in_point_id}",
    "event_id": "{event_id}",
    "created_at": "2012-11-26T15:44:44+01:00",
    "updated_at": "2012-11-26T15:44:44+01:00"
  },
  // ...
]
```

### XML
#### Request
```
GET /api/v1/events/{event_id}/signatures.xml?page=1&auth_token=YOUR_API_TOKEN
```
#### Response
```xml
<!-- 200 OK -->
<?xml version="1.0" encoding="UTF-8"?>
<signatures type="array">
  <signature>
    <_id>{id}</_id>
    <access-stamp type="datetime">2012-11-26T14:44:44Z</access-stamp>
    <guest-uid>68vju8yxgl</guest-uid>
    <accesspoint-id>{check_in_point_id}</accesspoint-id>
    <event-id>{event_id}</event-id>
    <points>
      <width>276</width>
      <height>266</height>
      <lines type="array">
        <line>
          <x1>272</x1>
          <x2>250.625</x2>
          <stroke-width>2.3333332538604736</stroke-width>
          <y1>92</y1>
          <y2>65.75</y2>
        </line>
        <!-- [...] -->
      </lines>
    </points>
    <created-at type="datetime">2012-11-26T14:44:44Z</created-at>
    <updated-at type="datetime">2012-11-26T14:44:44Z</updated-at>
  </signature>
  <signature>
    <!-- [...] -->
  </signature>
</signatures>
```

You can increment the `page` query string parameter until you get an empty array.

## Register a new signature

This is basically the method called by our
[iPad guest list app](https://itunes.apple.com/app/mobicheckin/id510125877?mt=8).
each time a guest signs on the touch screen.

### JSON
#### Request
```
POST /api/v1/{event_id}/signatures.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
{
  // Where did the signature took place?
  "accesspoint_id":    "{check_in_point_id}",

  // For which UID?
  "guest_uid":         "19j38293ju",

  // When (UTC)? If you don't provide this, the server will set the access_stamp to DateTime.now
  "access_stamp":      "2012-11-26T14:44:44Z"

  // The SVG representation of the signature
  "points": {
    "width": "276",
    "height": "266",
    "lines": [
      {
        "x2": "250.625",
        "x1": "272",
        "stroke-width": "2.3333332538604736",
        "y2": "65.75",
        "y1": "92"
      },
      {
        "x2": "218.75",
        "x1": "250.625",
        "stroke-width": "1.7055554389953613",
        "y2": "42.375",
        "y1": "65.75"
      },
      /...
    ]
  }
}
```
#### Response
```js
// 201 Created
{
   // The signature has been created.
}
```

### XML
#### Request
```
POST /api/v1/{event_id}/signatures.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<signature>
  <!-- Where did the signature took place? -->
  <accesspoint-id>{check_in_point_id}"</accesspoint-id>

  <!-- For which UID? -->
  <guest-uid>19j38293ju</guest-uid>

  <!-- When (UTC)? If you don't provide this, the server will set the access_stamp to DateTime.now -->
  <access-stamp>2012-11-26T14:44:44Z"</access-stamp>

  <!-- The SVG representation of the signature -->
  <points>
    <width>276</width>
    <height>266</height>
    <lines type="array">
      <line>
        <x1>272</x1>
        <x2>250.625</x2>
        <stroke-width>2.3333332538604736</stroke-width>
        <y1>92</y1>
        <y2>65.75</y2>
      </line>
      <!-- [...] -->
    </lines>
  </points>
</signature>
```
#### Response
```xml
<!-- 201 Created -->
<?xml version="1.0" encoding="UTF-8"?>
<signature>
  <!-- The signature -->
</signature>
```

## Batch insert signatures

Again, another method used by our
[iPad guest list app](https://itunes.apple.com/app/mobicheckin/id510125877?mt=8).
when in Offline mode, to sync up signatures once the network is up again.

You must submit an array of signatures to create. If all of them are valid, and
insertion is completed successfully, you will receive a `201 Created` as answer.
But if at least **one** of the signatures fails to be inserted, then you will
receieve a `422 Unprocessable Entity`. In that case, check out the body response,
which will be an array of created signatures an error report (in the same order
as the array you submitted in the request payload).

### JSON
#### Request
```
POST /api/v1/{event_id}/signatures/batch_create.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
[
  {
    "accesspoint_id":    "{check_in_point_id}",
    "guest_uid":         "19j38293ju",
    "points": "[...]"
  },
  {
    "accesspoint_id":    "{check_in_point_id}",
    "guest_uid":         "ieuwh8w7wn",
    "points": "[...]"
  }
}
```
#### Response
```js
// 201 Created
[
  {
    // The first signature created
  },
  {
    // The second signature created
  }
]
```

### XML
#### Request
```
POST /api/v1/{event_id}/signatures/batch_create.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<signatures type="array">
  <signature>
    <accesspoint-id><{check_in_point_id}/accesspoint-id>
    <guest-uid>19j38293ju</guest-uid>
    <points><!-- [...] --></points>
  </signature>
  <signature>
    <accesspoint-id><{check_in_point_id}/accesspoint-id>
    <guest-uid>ieuwh8w7wn</guest-uid>
    <points><!-- [...] --></points>
  </signature>
</signatures>
```

#### Response
```xml
<!-- 201 Created -->
<signatures type="array">
  <signature>
    <!-- The first signature created -->
  </signature>
  <signature>
    <!-- The second signature created -->
  </signature>
</signatures>
```
