# Denormalization is normal with the Firebase Database

Simply means **duplicate data**. Simplify or reduce querying.

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

You might not want to join eventAttendees to their users profile because when your access pattern goes larger and you running the join all the time, you might want to eliminate this.

You might want to just read the data one time. By storing the entire user object.

```
"eventAttendees": {
  "fm": {
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
  }
```

## When do I denormalize ?

Rule of thumb: Structure your data after your view.

## What about consistency ?

How can you ensure that when it changes in one spot, it changes everywhere. 

## Wrap up

Duplicating data makes really simple to read your data but you have to do a complex join or a query. But the problem is with consistency. By duplicating data, it's to make sure it's all consistent. You can fix that with multi-path updates.