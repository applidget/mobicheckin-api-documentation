# Events

## Retrieve the list of events

### JSON

You can easily get a JSON array of all the events you manage with the
following `GET` request.

#### Request
```
GET /api/v1/events.json?auth_token=YOUR_API_TOKEN
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
GET /api/v1/events.xml?auth_token=YOUR_API_TOKEN
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

If you are interested in only one event and already have his `id`, you can
issue the following `GET` request. You will receive a JSON object representing
your event.

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
  "reply_to_email": "sheldon@cooper.org",
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
  <reply-to-email>sheldon@cooper.org</reply-to-email>
  <badge-complete-hook-url nil="true"/>
  <guest-count type="integer">12</guest-count>
</event>
```

## Updating an event

### JSON
#### Request
```
PUT /api/v1/events/{event_id}.json?auth_token=YOUR_API_TOKEN
Content-Type: application/json; charset=utf-8
```
```js
{
  // Fields that can be updated. You can include only fields you want to update.
  "title": "Fun with Flags Party",
  "description": "Sheldon Cooper and Amy Farrah Fowler are throwing a celebration party for reaching their tenth watcher.",
  "locale": "en",
  "mobinetwork_enabled": false,
  "organizer": "Sheldon Cooper",
  "start_date": "2012-11-22T03:00:00+00:00",
  "end_date": "2012-11-22T05:00:00+00:00",
  "timezone": "Pacific Time (US &amp; Canada)",
  "badge_complete_hook_url": "http://my.company.org/handle_mobicheckin_guest_created.json",
  "reply_to_email": "your.email@company.org"
}

```
#### Response
```js
// 204 No Content
```

### XML
#### Request
```
PUT /api/v1/events/{event_id}.xml?auth_token=YOUR_API_TOKEN
Content-Type: application/xml; charset=utf-8
```
```xml
<event>
  <!-- Fields that can be updated. You can include only fields you want to update. -->
  <title>Fun with Flags Party</title>
  <description>Sheldon Cooper and Amy Farrah Fowler are throwing a celebration party for reaching their tenth watcher.</description>
  <locale>en</locale>
  <organizer>Sheldon Cooper</organizer>
  <plan>pro</plan>
  <start-date type="datetime">2012-11-22T03:00:00Z</start-date>
  <end-date type="datetime">2012-11-22T05:00:00Z</end-date>
  <timezone>Pacific Time (US & Canada)</timezone>
  <mobinetwork-enabled type="boolean">false</mobinetwork-enabled>
  <badge-complete-hook-url>http://my.company.org/handle_mobicheckin_guest_created.json</badge-complete-hook-url>
  <reply-to-email>your.email@company.org</reply-to-email>
</event>
```
#### Response
```xml
<!-- 204 No Content -->
```