# Guests

## Retrieve the list of guests

MobiCheckin provides a paginaged API to retrieve the list of guests of your event.

```
GET /api/v1/events/{event_id}/guests.xml?page=1&auth_token=YOUR_API_TOKEN
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<guests type="array">
  <guest>
    ...
  </guest>
  <guest>
    ...
  </guest>
</guest>
```