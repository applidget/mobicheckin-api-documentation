# Guests

## Retrieve the list of guests

MobiCheckin provides a paginaged API to retrieve the list of guests of your event.

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
    // [...]
  },
  {
    // [...]
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
    [...]
  </guest>
  <guest>
    [...]
  </guest>
</guest>
```

You can increment the `page` query string parameter until you get an empty array.

## Create a new guest

You can add a new person to your event guest list.

### JSON
#### Request
```
POST /api/v1/{event_id}/guests.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
{
  "guest": {
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

    // You can pass any other information abotu the guest in this hash.
    "guest_metadata": [
      { "name": "Has a dog", "value": "Yes" },
      { "name": "Birth year", "value": "1960" }
    ]
  }
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
   "access_privileges": [],
   "badge_url": null,
   "qrcode_url": null,
   "guest_metadata":
   [
      { "name": "Has a dog",  "value": "Yes" },
      { "name": "Birth year", "value": "1960" }
   ]
}
```

### XML
#### Request
```
POST /api/v1/{event_id}/guests.xml?auth_token=YOUR_API_TOKEN
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
</guest>
```
#### Response
```xml
<!-- 200 OK -->
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
  <access-privileges type="array"/>
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