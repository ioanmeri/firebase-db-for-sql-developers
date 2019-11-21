# Joins in the Firebase Database

> In general, might be a **bad idea** if you want fast loading time.

The Firebase SDK doesn't have any methods for joining Data specifically. 
But that doesn't mean that you can't take data from one location and then find the related data and merge it into one set.

## Firebase Joins

JSON Top Level Keys: users, events, eventAttendees

```
{
  "users": {
    "1": {
      "name": "David",
      "bio": "Hello, Im a ...",
      "profileImg": "https://..."
    },
    "9": {
      "name": "David",
      "bio": "Hello, Im a ...",
      "profileImg": "https://..."
    }
  },
  "events": {
    "fm": {
      "name": "Firebase Meetup",
      "date": 98327523520
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

### How to Join

```
const rootRef = firebase.database().ref();
const attendees = db.child('eventAttendees/fm');

attendees.on('child_added', snap => {
  // append attendees to list
  // '1' -> 'David'
  // '9' -> 'Alice'
})
```

Attendees.on will fire back each user that is going to the event

* 1st time, '1' -> 'David'
* 2nd time, '9' -> 'Alice'

That gives us the name and not any other user data. To get them, use a join.
The snapshots key is th UID for that user

```
attendees.on('child_added', snap => {
  
  let userRef = db.child('users/' + snap.key);
  userRef.once('value').then(userSnap => {
    
  });

});
```

> Once method doesn't listen in real time

### Using on method to listen

You always have to **dispose the listener**, if you no longer need to listen to these updates **in real time**. Call ref.off with the handle.


## Real Time listeners

Complex situations when you need to keep both data sets updated together in real time.

### How to keep track of each handle

Unsubscribe for real time events

```
let handles = [];
attendees.on('child_added', snap => {
  
  let userRef = db.child('users/' + snap.key);
  let fn = userRef.on('value', userSnap => {

  });
  handles.push(fn);

});

handles.forEach(fn => userRef.off('value', fn));
```

### Example

Get an object with **once** -> all attendees:

```
const eventKey = '-KTtjCt8JVV88Nm1X6Ho';
const rootRef = firebase.database().ref();
const attendeesRef = rootRef.child('eventAttendees');
const usersRef = rootRef.child('users');

attendeesRef.child(eventKey).once('value', snap => console.log(snap.val()))
```

**Child_ added** will fire off for every user.

All of the entire data sets for each user:
```
function getUsersAtEvent(key, cb){
  attendeesRef.child(eventKey).on('child_added', snap => {
    let userRef = usersRef.child(snap.key);
    userRef.once('value', cb);  
  })
}

getUsersAtEvent(eventKey, snap => console.log(snap.val()));
```

