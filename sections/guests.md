# Guests

## Retrieve the list of guests

MobiCheckin provides a paginaged API to retrieve the list of guests of your event.
Starts at `page=1`. Paginate every 500 guests.
If you want to know how many guests registered to your event, you should
query `GET /api/v1/events/{id}.format` and look at the property `guest_count`.

Optional query parameters are:

1. `?uid=%s`: Search a guest by his/her uid. Will return an array of 0 or 1 guest.
2. `?search=%s`: Full-text search in the guest list.
3. `?category[]=%s&category[]=%s`: Filter by the given guest category ids.

You can compound these filter parameters

### JSON
#### Request
```
GET /api/v1/events/{event_id}/guests.json?page=1&auth_token=YOUR_API_TOKEN
```
#### Response
```js
// 200 OK
[
  {
    // the first guest
  },
  {
    // the second guest
  }
]
```

### XML
#### Request
```
GET /api/v1/events/{event_id}/guests.xml?page=1&auth_token=YOUR_API_TOKEN
```
#### Response
```xml
<!-- 200 OK -->
<?xml version="1.0" encoding="UTF-8"?>
<guests type="array">
  <guest>
    <!-- [...] -->
  </guest>
  <guest>
    <!-- [...] -->
  </guest>
</guest>
```

You can increment the `page` query string parameter until you get an empty array.

## Create a new guest

You can add a new person to your event guest list.

### JSON
#### Request
```
POST /api/v1/events/{event_id}/guests.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
{
  "guest_category_id": "{guest_category_id}",
  "email":             "john.smith@acme.org",
  "first_name":        "John",
  "last_name":         "Smith",
  "company_name":      "Acme Inc.",
  "position":          "CEO",
  "phone_number":      "001122334455",

  // Use this field if you want to choose your owns ids for your guests.
  "uid": {your_custom_id},

  // Use this field if you want a message that appear on the iOS device when the guest is checked-in.
  "message": "Vegan lunch",

  // You can pass any other information abotu the guest in this array.
  "guest_metadata": [
    { "name": "Has a dog",  "value": "Yes" },
    { "name": "Birth year", "value": "1960" }
  ]

  // You can pass an array of access privileges for specific check-in points
  "access_privileges_attributes": [
    { "access_once": true,  "accesspoint_id": "{workshop_foo_id}" },
    { "access_once": false, "accesspoint_id": "{vip_room_id}" },
  ]
}
```
#### Response
```js
// 201 Created
{
   "badge_completed": false,
   "rsvp_status": "not_replied",
   "_id": "50af6426bbfa805f760005ac",
   "guest_category_id": "{guest_category_id}",
   "email": "john.smith@acme.org",
   "first_name": "John",
   "last_name": "Smith",
   "company_name": "Acme Inc.",
   "position": "CEO",
   "phone_number": "001122334455",
   "message": "Vegan lunch",
   "event_id": "{event_id}",
   "uid": "9vts5v3nqs",
   "updated_at": "2012-11-23T11:55:18Z",
   "created_at": "2012-11-23T11:55:18Z",
   "badge_url": null,
   "qrcode_url": null,
   "guest_metadata":
   [
      { "name": "Has a dog",  "value": "Yes" },
      { "name": "Birth year", "value": "1960" }
   ],
   "access_privileges":
   [
     // Array of all access privileges
   ]
}
```

### XML
#### Request
```
POST /api/v1/events/{event_id}/guests.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<guest>
  <guest-category-id>{guest_category_id}</guest-category-id>
  <email>john.smith@acme.org</email>
  <first-name>John</first-name>
  <last-name>Smith</last-name>
  <company-name>Acme Inc.</company-name>
  <position>CEO</position>
  <phone-number>001122334455</phone-number>

  <!-- Use this field if you want to choose your owns ids for your guests. -->
  <uid>9vts5v3nqs</uid>

  <!-- Use this field if you want a message that appear on the iOS device when the guest is checked-in. -->
  <message>Vegan lunch</message>

  <!-- You can pass any other information abotu the guest in this hash. -->
  <guest-metadata type="array">
    <guest-metadatum>
      <name>Has a dog</name>
      <value>Yes</value>
    </guest-metadatum>
    <guest-metadatum>
      <name>Birth year</name>
      <value>1960</value>
    </guest-metadatum>
  </guest-metadata>

  <!-- You can pass an array of access privileges if the guest can access specific check-in points -->
  <access-privileges-attributes type="array">
    <access-privilege>
      <access-once type="boolean">true</access-once>
      <accesspoint-id>{workshop_foo_id}</accesspoint-id>
    </access-privilege>
    <access-privilege>
      <access-once type="boolean">false</access-once>
      <accesspoint-id>{vip_room_id}</accesspoint-id>
    </access-privilege>
  </access-privileges-attributes>
</guest>
```
#### Response
```xml
<!-- 201 Created -->
<?xml version="1.0" encoding="UTF-8"?>
<guest>
  <_id>50af6426bbfa805f760005ac</_id>
  <badge-completed type="boolean">false</badge-completed>
  <badge-url></badge-url>
  <company-name>Acme Inc.</company-name>
  <email>john.smith@acme.org</email>
  <event-id>{event_id}</event-id>
  <first-name>John</first-name>
  <guest-category-id>{guest_category_id}</guest-category-id>
  <last-name>Smith</last-name>
  <message>Vegan lunch</message>
  <phone-number>001122334455</phone-number>
  <position>CEO</position>
  <rsvp-status>not_replied</rsvp-status>
  <uid>9vts5v3nqs</uid>
  <created-at type="datetime">2012-11-23T11:55:18Z</created-at>
  <updated-at type="datetime">2012-11-23T11:55:18Z</updated-at>
  <access-privileges type="array">
    <access-privilege>
      <!-- [...] -->
    <access-privilege>
    </access-privilege>
      <!-- [...] -->
    </access-privilege>
  </access-privileges>
  <guest-metadata type="array">
    <guest-metadatum>
      <name>Has a dog</name>
      <value>Yes</value>
    </guest-metadatum>
    <guest-metadatum>
      <name>Birth year</name>
      <value>1960</value>
    </guest-metadatum>
  </guest-metadata>
</guest>
```

## Retrieve information about a guest
With the `id` of a guest, you can details about him. We provide 2 optional
parameters to pass as query string parameters:

* `guest_metadata=true` will put the `guest_metadata` array in the object returned
* `qrcode_url=true` will put the `qrcode_url` string in the object returned
* `badges=true` will provide an array of the guest badges 
* `files=true` will provide an array of attached files

### JSON
#### Request
```
GET /api/v1/events/{event_id}/guests/{id}.json?auth_token=YOUR_API_TOKEN
GET /api/v1/events/{event_id}/guests/{id}.json?auth_token=YOUR_API_TOKEN&qrcode_url=true
GET /api/v1/events/{event_id}/guests/{id}.json?auth_token=YOUR_API_TOKEN&guest_metadata=true
GET /api/v1/events/{event_id}/guests/{id}.json?auth_token=YOUR_API_TOKEN&files=true
```
#### Response
```js
// 200 OK
{
   "_id": "{id}",
   "guest_category_id": "{guest_category_id}",
   "first_name": "John",
   "last_name": "Smith",
   // [...]
}
```

### XML
#### Request
```
GET /api/v1/events/{event_id}/guests/{id}.xml?auth_token=YOUR_API_TOKEN
GET /api/v1/events/{event_id}/guests/{id}.xml?auth_token=YOUR_API_TOKEN&qrcode_url=true
GET /api/v1/events/{event_id}/guests/{id}.xml?auth_token=YOUR_API_TOKEN&guest_metadata=true
```
#### Response
```xml
<!-- 200 OK -->
<?xml version="1.0" encoding="UTF-8"?>
<guest>
  <_id>{id}</_id>
  <guest-category-id>{guest_category_id}</guest-category-id>
  <first-name>John</first-name>
  <last-name>Smith</last-name>
  <!-- [...] -->
</guest>
```

## Update a guest
You can add complementary information, update or delete your guests fields. Be
careful that these modifications are done in place, not concatenated to existing
fields. For instance, if you provide a `guest_metadata` array in your request,
they won't be appended to existing entries in the guest, but will replace them.

### JSON
#### Request
```
PUT /api/v1/events/{event_id}/guests/{id}.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
{
  // Fields to update (View the POST request for a complete list of fields)
}
```
### Response
```js
// 204 No Content
```

### XML
#### Request
```
PUT /api/v1/events/{event_id}/guests/{id}.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<guest>
  <!-- Fields to update (View the POST request for a complete list of fields) -->
</guest>
```
### Response
```xml
<!-- 204 No Content -->
```

## Delete a guest
You can delete guests (which is irreversible).

### JSON
#### Request
```
DELETE /api/v1/events/{event_id}/guests/{id}.json?auth_token=YOUR_API_TOKEN
```
### Response
```js
// 204 No Content
```

### XML
#### Request
```
DELETE /api/v1/events/{event_id}/guests/{id}.xml?auth_token=YOUR_API_TOKEN
```
### Response
```xml
<!-- 204 No Content -->
```
