# Events

## Retrieve the list of events

### JSON
#### Request
```
GET /api/v1/events.json?page=1&auth_token=YOUR_API_TOKEN
```
#### Response
```js
// 200 OK
[
  {
    // the first event
  },
  {
    // the second event
  }
]
```

### XML
#### Request
```
GET /api/v1/events.xml?page=1&auth_token=YOUR_API_TOKEN
```
#### Response
```xml
<!-- 200 OK -->
<?xml version="1.0" encoding="UTF-8"?>
<events type="array">
  <event>
    [...]
  </event>
  <event>
    [...]
  </event>
</events>
```

## Get information about a given event

### JSON
#### Request
```
GET /api/v1/events/{event_id}.json?auth_token=YOUR_API_TOKEN
```
#### Response
```js
// 200 OK
{
  "_id": "{event_id}",
  "badge_complete_hook_url": null,
  "description": "Sheldon Cooper and Amy Farrah Fowler are throwing a celebration party for reaching their tenth watcher.",
  "end_date": "2012-11-22T05:00:00+00:00",
  "locale": "en",
  "mobinetwork_enabled": false,
  "organizer": "Sheldon Cooper",
  "plan": "pro",
  "start_date": "2012-11-22T03:00:00+00:00",
  "timezone": "Pacific Time (US &amp; Canada)",
  "title": "Fun with Flags Party",
  "guest_count": 12
}
```

### XML
#### Request
```
GET /api/v1/events/{event_id}.xml?auth_token=YOUR_API_TOKEN
```
#### Response
```xml
<!-- 200 OK -->
<event>
  <_id>{event_id}</_id>
  <title>Fun with Flags Party</title>
  <description>Sheldon Cooper and Amy Farrah Fowler are throwing a celebration party for reaching their tenth watcher.</description>
  <locale>en</locale>
  <organizer>Sheldon Cooper</organizer>
  <plan>pro</plan>
  <start-date type="datetime">2012-11-22T03:00:00Z</start-date>
  <end-date type="datetime">2012-11-22T05:00:00Z</end-date>
  <timezone>Pacific Time (US & Canada)</timezone>
  <mobinetwork-enabled type="boolean">false</mobinetwork-enabled>
  <guest-count type="integer">12</guest-count>
</event>
```

## Updating an event

### JSON
#### Request
```
PUT /api/v1/events/{event_id}.json?auth_token=YOUR_API_TOKEN
```
```js
{
  "event": {
    // Fields to update
  }
}

```
#### Response
```js
// 204 No Content
```

### XML
#### Request
```
GET /api/v1/events/{event_id}.xml?auth_token=YOUR_API_TOKEN
```
```xml
<event>
  <!-- Fields to update -->
</event>
```
#### Response
```xml
<!-- 204 No Content -->
```