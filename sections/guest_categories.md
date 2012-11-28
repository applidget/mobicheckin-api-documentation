# Guest Categories

Each guest belongs to a guest category. A lot of MobiCheckin settings ar bound to
it.

## Retrieve the list of guest categories

### JSON
#### Request
```
GET /api/v1/events/{event_id}/guest_categories.json?auth_token=YOUR_API_TOKEN
```
#### Response
```js
// 200 OK
[
  {
    // the first guest category
  },
  {
    // the second guest category
  }
]
```

### XML
#### Request
```
GET /api/v1/events/{event_id}/guest_categories.xml?auth_token=YOUR_API_TOKEN
```
#### Response
```xml
<!-- 200 OK -->
<?xml version="1.0" encoding="UTF-8"?>
<guest-categories type="array">
  <guest-category>
    <!-- [...] -->
  </guest-category>
  <guest-category>
    <!-- [...] -->
  </guest-category>
</guest-categories>
```

## Get information about a given guest category

If you want to know if the badge is enabled for a guest category, or which
check-in points a guest in this guest category can access, use this:

### JSON
#### Request
```
GET /api/v1/events/{event_id}/guest_categories/{id}.json?auth_token=YOUR_API_TOKEN
```
#### Response
```js
// 200 OK
{
  "_id": "{id}",
  "badge_enabled": true,
  "badge_qr_code_enabled": true,
  "badge_ecard_in_qr_code_enabled": true,
  "confirmation_email_enabled": false,
  "rsvp_enabled": false,
  "moderated_registrations_enabled": false,
  "wufoo_identity_mapping": { },
  "wufoo_checkin_point_mapping": { },
  "name": "VIPs",
  "event_id": "{event_id}",
  "updated_at": "2012-11-22T15:07:24Z",
  "created_at": "2012-11-22T15:07:24Z",
  "guest_category_accesspoints": [
    {
      "access_once": false,
      "accesspoint_id": "{vip_room_id}",
    }
  ]
}
```

### XML
#### Request
```
GET /api/v1/events/{event_id}/guest_categories/{id}.xml?auth_token=YOUR_API_TOKEN
```
#### Response
```xml
<!-- 200 OK -->
<guest-category>
  <_id>{id}</_id>
  <badge-enabled type="boolean">true</badge-enabled>
  <badge-qr-code-enabled type="boolean">true</badge-qr-code-enabled>
  <badge-ecard-in-qr-code-enabled type="boolean">true</badge-ecard-in-qr-code-enabled>
  <confirmation-email-enabled type="boolean">false</confirmation-email-enabled>
  <rsvp-enabled type="boolean">false</rsvp-enabled>
  <moderated-registrations-enabled type="boolean">false</moderated-registrations-enabled>
  <wufoo-identity-mapping/>
  <wufoo-checkin-point-mapping/>
  <name>VIPs</name>
  <event-id>{event_id}</event-id>
  <updated-at type="datetime">2012-11-22T15:07:24Z</updated-at>
  <created-at type="datetime">2012-11-22T15:07:24Z</created-at>
  <guest-category-accesspoints type="array">
    <guest-category-accesspoint>
      <access-once type="boolean">false</access-once>
      <accesspoint-id>{vip_room_id}</accesspoint-id>
    </guest-category-accesspoint>
  </guest-category-accesspoints>
</guest-category>
```

## Create a guest category

### JSON
#### Request
```
POST /api/v1/{event_id}/guest_category.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
{
  // Fields of the guest category to create (see GET request for a complete list)
}
```
#### Response
```js
// 201 Created
{
   // Your newly created guest category as a JSON object
}
```

### XML
#### Request
```
POST /api/v1/{event_id}/guest_category.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<guest-category>
  <!-- Fields of the guest category to create (see GET request for a complete list) -->
</guest-category>
```
#### Response
```xml
<!-- 201 Created -->
<?xml version="1.0" encoding="UTF-8"?>
<guest-category>
  <!-- Your newly created guest category as a JSON object -->
</guest>
```

## Update a guest category

### JSON
#### Request
```
PUT /api/v1/events/{event_id}/guest_categories/{id}.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
{
  // Fields of the guest category to update (see GET request for a complete list)
}
```
#### Response
```js
// 204 No Content
```

### XML
#### Request
```
PUT /api/v1/events/{event_id}/guest_categories/{id}.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<guest-category>
  <!-- Fields of the guest category to update (see GET request for a complete list) -->
</guest-category>
```
#### Response
```xml
<!-- 204 No Content -->
```