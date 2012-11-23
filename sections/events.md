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
  "event": {
    // Fields to update
  }
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
  <!-- Fields to update -->
</event>
```

## Updating an event

### JSON
#### Request
```
PUT /api/v1/events/{event_id}.json?auth_token=YOUR_API_TOKEN
```
#### Response
```js
// 204 No Content
}
```

### XML
#### Request
```
GET /api/v1/events/{event_id}.xml?auth_token=YOUR_API_TOKEN
```
#### Response
```xml
<!-- 204 No Content -->
```