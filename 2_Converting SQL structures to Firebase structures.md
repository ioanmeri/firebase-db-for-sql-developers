# SQL structures to Firebase Structures

Relational to NoSQL model - that works well for Firebase

Many to Many Relationship

## SQL

Relation with Foreign Key. 
A key in one table that referces a primary key in another table (UID, EventId)

### Users Table
ID - Surname - FirstName - LastName - Age - Location

### Attendees
ID - UID - EventId

### Events
ID - Name - Description - Time - Category

### JOIN query

```
SELECT event.Name as EventName
      ,event.Date as EventData
      ,user.Name as AttendeeName
  FROM Events as event
  INNER JOIN Attendees as a
    ON e.Id === a.EventId
  INNER JOIN Users as user
    ON u.UId = a.UId
  WHERE e.Id == 4;
```


## NoSQL - Firebase

When you retrieve data you specify a path. 
To retrieve a **single event**, we would create a path **/events/fm**

The Firebase DB will bring back every single of data underneath that path. Which may not be what you want. You don't want to download the attendees just to display a list of events.

Break them out, to **their own root key**

```
{
  "users": {
    "1": {
      "name": "David"
    },
    "9": {
      "name": "Alice"
    },
  },
  "events": {
    "fm": {
      "name": "Firebase Meatup",
      "date": 983275235320,
    }
  },
  "eventAttendees": {
    "fm": {
      "1": "David",
      "9": "Alice"
    }
  }
}
```

This makes the DB flat. Which is a good think. We download only what we need to display.


Example of query with Firebase SDK

```
const db = firebase.database();
const events = db.child('events/fm');
const attendees = db.child('eventAttendees/fm');

// Retrieve data in Real Time

events.on('value', snap => {
  // render data to HTML
});

attendees.on('child_added', snap => {
  // append attendees to list
})
```

We can actually render to the view independently. Much easier than creating a inner listener that accesses a join.