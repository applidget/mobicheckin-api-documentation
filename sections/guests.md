# Guests

## Retrieve the list of guests

MobiCheckin provides a paginaged API to retrieve the list of guests of your event.

```
GET /api/v1/events/{event_id}/guests.json?page=1&auth_token=YOUR_API_TOKEN
```

```json
<!-- HTTP/1.1 200 OK -->

[
  {
    [...]
  },
  {
    [...]
  }
]

```

```
GET /api/v1/events/{event_id}/guests.xml?page=1&auth_token=YOUR_API_TOKEN
```

```xml
<!-- HTTP/1.1 200 OK -->

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

You can increment the page query string parameter until you get an empty array.