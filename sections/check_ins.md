# Check-ins

## Retrieve the list of check-ins

MobiCheckin provides a paginaged API to retrieve the list of check-ins of your event.
Starts at `page=1`. Paginate every 500 check-ins.
If you want to know how many check-ins took place at your event, you should
query `GET /api/v1/events/{id}.format` and look at the property `check_in_count`.

### JSON
#### Request
```
GET /api/v1/events/{event_id}/access_controls.json?page=1&auth_token=YOUR_API_TOKEN
```
#### Response
```js
// 200 OK
[
  {
    "_id": "{id}",

    // When did the check-in took place? (UTC, use event.timezone for display purposes)
    "access_stamp": "2012-11-26T15:44:44+01:00",

    // The UID found in the scanned QR code. Beware, guest.uid != guest.id
    "guest_uid": "68vju8yxgl",

    // ACCESS_GRANTED = 0, GUEST_UNKNOWN = 1, NOT_AUTHORIZED = 2, TOO_MANY_CHECK_INS = 3
    "access_status": 0,

    // The check-in point where the check-in took place
    "accesspoint_id": "{check_in_point_id}",

    "event_id": "{event_id}",
    "created_at": "2012-11-26T15:44:44+01:00",
    "updated_at": "2012-11-26T15:44:44+01:00"

  },
  {
    "_id": "{id}",
    "access_stamp": "2012-11-26T15:44:44+01:00",
    "guest_uid": "5ch0b30mnc",
    "access_status": 0,
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
GET /api/v1/events/{event_id}/access_controls.xml?page=1&auth_token=YOUR_API_TOKEN
```
#### Response
```xml
<!-- 200 OK -->
<?xml version="1.0" encoding="UTF-8"?>
<access-controls type="array">
  <access-control>
    <_id>{id}</_id>
    <access-stamp type="datetime">2012-11-26T14:44:44Z</access-stamp>
    <access-status type="integer">0</access-status>
    <guest-uid>68vju8yxgl</guest-uid>
    <accesspoint-id>{check_in_point_id}</accesspoint-id>
    <event-id>{event_id}</event-id>
    <created-at type="datetime">2012-11-26T14:44:44Z</created-at>
    <updated-at type="datetime">2012-11-26T14:44:44Z</updated-at>
  </access-control>
  <access-control>
    <_id>{id}</_id>
    <access-stamp type="datetime">2012-11-26T14:44:44Z</access-stamp>
    <access-status type="integer">0</access-status>
    <guest-uid>5ch0b30mnc</guest-uid>
    <accesspoint-id>{check_in_point_id}</accesspoint-id>
    <event-id>{event_id}</event-id>
    <created-at type="datetime">2012-11-26T14:44:44Z</created-at>
    <updated-at type="datetime">2012-11-26T14:44:44Z</updated-at>
  </access-control>
</access-controls>
```

You can increment the `page` query string parameter until you get an empty array.

## Check-in someone

This is basically the method called by our
[iPad guest list app](https://itunes.apple.com/app/mobicheckin/id510125877?mt=8).
each time you check-in someone (touching the list or scanning a QR code).

### JSON
#### Request
```
POST /api/v1/{event_id}/access_controls.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
{
  // Where did the check-in took place?
  "accesspoint_id":    "{check_in_point_id}",

  // For which UID?
  "guest_uid":         "19j38293ju",

  // When (UTC)? If you don't provide this, the server will set the access_stamp to DateTime.now
  "access_stamp":      "2012-11-26T14:44:44Z"
}
```
#### Response
```js
// 201 Created
{
   // The check-in created. You can have a look at access_status, if 0 then
   // that was a "green" check-in. Otherwise it's red.
}
```

### XML
#### Request
```
POST /api/v1/{event_id}/access_controls.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<access-control>
  <!-- Where did the check-in took place? -->
  <accesspoint-id>{check_in_point_id}"</accesspoint-id>

  <!-- For which UID? -->
  <guest-uid>19j38293ju</guest-uid>

  <!-- When (UTC)? If you don't provide this, the server will set the access_stamp to DateTime.now -->
  <access-stamp>2012-11-26T14:44:44Z"</access-stamp>
</access-control>
```
#### Response
```xml
<!-- 201 Created -->
<?xml version="1.0" encoding="UTF-8"?>
<access-control>
  <!-- The check-in created. You can have a look at access_status, if 0 then
       that was a "green" check-in. Otherwise it's red. -->
</access-control>
```

## Batch insert check-ins

Again, another method used by our
[iPad guest list app](https://itunes.apple.com/app/mobicheckin/id510125877?mt=8).
when in Offline mode, to sync up check-ins once the network is up again.

You must submit an array of check-ins to create. If all of them are valid, and
insertion is completed successfully, you will receive a `201 Created` as answer.
But if at least **one** of the check-ins fails to be inserted, then you will
receieve a `422 Unprocessable Entity`. In that case, check out the body response,
which will be an array of created check-ins an error report (in the same order
as the array you submitted in the request payload).

### JSON
#### Request
```
POST /api/v1/{event_id}/access_controls/batch_create.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
[
  {
    "accesspoint_id":    "{check_in_point_id}",
    "guest_uid":         "19j38293ju",
    // "access_stamp":   "2012-11-26T14:44:44Z"
  },
  {
    "accesspoint_id":    "{check_in_point_id}",
    "guest_uid":         "ieuwh8w7wn",
    // "access_stamp":   "2012-11-26T15:44:44Z"
  }
}
```
#### Response
```js
// 201 Created
[
  {
    // The first check-in created
  },
  {
    // The second check-in created
  }
]
```

### XML
#### Request
```
POST /api/v1/{event_id}/access_controls/batch_create.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<access-controls type="array">
  <access-control>
    <accesspoint-id><{check_in_point_id}/accesspoint-id>
    <guest-uid>19j38293ju</guest-uid>
    <!-- <access-stamp>2012-11-26T14:44:44Z</access-stamp> -->
  </access-control>
  <access-control>
    <accesspoint-id><{check_in_point_id}/accesspoint-id>
    <guest-uid>ieuwh8w7wn</guest-uid>
    <!-- <access-stamp>2012-11-26T15:44:44Z</access-stamp> -->
  </access-control>
</access-controls>
```

#### Response
```xml
<!-- 201 Created -->
<access-controls type="array">
  <access-control>
    <!-- The first check-in created -->
  </access-control>
  <access-control>
    <!-- The second check-in created -->
  </access-control>
</access-controls>
```

## Delete a check-in
You can delete guests (which is irreversible).

### JSON
#### Request
```
DELETE /api/v1/{event_id}/access_controls/{id}.json?auth_token=YOUR_API_TOKEN
```
### Response
```js
// 204 No Content
```

### XML
#### Request
```
DELETE /api/v1/{event_id}/access_controls/{id}.xml?auth_token=YOUR_API_TOKEN
```
### Response
```xml
<!-- 204 No Content -->
```
