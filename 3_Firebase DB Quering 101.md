# Firebase Database Quering 101

You tend to not need powerfull quering in NoSQL DBs because you can structure your data properly. But still it's very important to know how to query.

## SQL
1. Select 
2. Restrict using where

```
SELECT *
FROM Events
WHERE Name == "Firebase Meetup";
```

## NoSQL

Also 2 steps

1. Create a refernce to the parent key
2. Use an ordering Function
  * optionally, use a query Function for more advanced restricting

```
const db = firebase.database();
const eventsRef = db.child('events');

eventsRef.orderFunction().queryFunction();
```

Example: Retrieve the first 10 events

```
eventsRef.orderByKey().limitToFirst(10);
```

### Ordering Function

4 different ordering Functions:

* orderByKey()
  * allows you to query based upon the child keys, 
  * are always string based (on limiting, or basic paging)

* orderByChild('child_property')
  * specify a child property (similar to sql where) like name and age and then you can query based upon these values

* orderByValue()
  * all the children are order by their values
  * works really well for **numeric values**

* orderByPriority()
  * old way of doing things



### Quering functions

More advanced functions to further restrict your data

* startAt('value')
* endAt('value')
* equalTo('child_key')
* limitToFirst(10)
* limitToLast(10)

startAt & endAt to create a range query.

limitToFirst & limitToLast is how you create a limiting.

### Use Quering Functions in combination

```
const db = firebase.database();
const events = db.child('events');
const query = events
                .orderByChild('name')
                .equalTo('Firebase Meetup')
                .limitToFirst(1);

query.on('value', snap => {
  // render data to HTML
})
```