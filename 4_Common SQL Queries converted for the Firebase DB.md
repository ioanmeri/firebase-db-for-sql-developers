# Common SQL Queries converted for the Firebase Database

## SQL

**Users Table** 

| UID(PK) | Name    | Email             | Age | Location |
|---------|---------|-------------------|-----|----------|
| 1       | "David" | "david@email.com  | 99  | "SF      |
| 9       | "Alice" | "alice@gmail.com" | 28  | "Berlin" |


### 1. Select a user by UID
```
SELECT * FROM Users WHERE UID = 1;
```

### 2. Find a user by email address
```
SELECT * FROM Users WHERE Email = 'alice@email.com';
```

### 3. Limit to 10 users
```
SELECT * FROM Users LIMIT 10;
```

### 4. Get all users names that start with 'D'
```
SELECT * FROM Users WHERE Name LIKE 'D%';
```

### 5. Get all users who are less than 50
```
SELECT * FROM Users WHERE Age < 50;
```

### 6. Get all users who are greater than 50
```
SELECT * FROM Users WHERE Age > 50;
```

### 7. Get all users who are between 20 and 100
```
SELECT * FROM Users WHERE Age >= 20 & Age <=100;
```

### 8. Get all users who are 28 and live in Berlin
```
SELECT * FROM Users WHERE Age = 28 && Location = 'Berlin';
```



## Firebase
```
{
  "users": {
    "1": {
      "name": "David",
      "email": "david@email.com",
      "age": 99,
      "location": "SF",
      "age_location": "99_SF"
    },
    "9": {
      "name": "Alice",
      "email": "alice@email.com",
      "age": 28,
      "location": "Berlin",
      "age_location": "28_Berlin"
    },
  }
}
```

```
const rootRef = firebase.database().ref();
```

### 1. Select a user by UID
```
const oneRef = rootRef.child('users').child('1');
```

### 2. Find a user by email address
```
const twoRef = rootRef.child('users').orderByChild('email').equalTo('alice@email.com');
```

### 3. Limit to 10 users
```
const threeRef = rootRef.child('users').orderByKey().limitToFirst(10);
```
or the same but better:

```
const threeRef = rootRef.child('users').limitToFirst(10);
```

### 4. Get all users names that start with 'D'
```
const fourRef = rootRef.child('users').orderByChild('name').startAt('D').endAt('D\uf8ff');
```

### 5. Get all users who are less than 50
```
const fiveRef = rootRef.child('users').orderByChild('age').endAt(49);
```

### 6. Get all users who are greater than 50
```
const sixRef = rootRef.child('users').orderByChild('age').startAt(51);
```

### 7. Get all users who are between 20 and 100
```
const severRef = rootRef.child('users').orderByChild('age').startAt(20).endAt(100);
```


### 8. Get all users who are 28 and live in Berlin
```
const eightRef = rootRef.child('users').orderByChild('age_location').equalTo('28_Berlin');
```

