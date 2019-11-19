# Differences between SQL and NoSQL

## SQL

* In SQL DBs data are stored in tables that have columns and rows.
* To avoid blank values we use schemas

### Create Table

**Schema** is like a blueprint. It specifies

* column names
* data types
* if required //constraints

```
CREATE TABLE Customers (
  Id INT(5) NOT NULL AUTO_INCREMENT,
  FirstName VARCHAR(100) NOT NULL,
  Birthday DATE,
  Location VARCHAR(250),
  PRIMARY KEY(Id)
)
```

### Insert into Table
```
INSERT INTO Customers
  (FirstName, Birthday, Location)
VALUES
  ("David", Now(), "SF");

```

Error if I try to insert a value to column that does not exist:

ERROR: LastName does not exist!

### Alter Table

If you want to add LastName we need an **Alter Statement**:

```
ALTER TABLE Customers
ADD COLUMN LastName
VARCHAR(100) NOT NULL
```

### Wrap Up
SQL DBs are strict to ensure integrity. But if you have a rapidly changing data structure you constantly altering your schema and migrating data. Also, when you are modelling data relationaly not always everything just fits. This is where NoSQL DBs like Firebase come into play.


## NoSQL - Firebase

Firebase is a NoSQL JSON DB. A fancy way to say it is a JSON object.

JSON is really simple, It has keys and values.

```
{
  "customers": {
    "customer_one": {
      "firstName": "David",
      "birthday": 1475189812156,
      "location": "SF"
    }
  }
}
```

How we know, what each key will accept as a value? It there a schema that we can use?

The Firebase DB as most NoSQL DBs are Schema-Less, which means you don't have to decide what your data structure is up front, to start inserting data. It provides a lot of flexibility

But you can validate the types of data that being saved in the DB.

### Rule Types

Security Rules allows you to specify the size and shape of data before they get Saved to the the DB.

You can think of Security Rules as constrains in SQL.

Every time there is one new child saved the rule check to make sure that firstName is a String, birthDay is a Number and location also a String. If this expression fails then data is not saved.

```
{
  "rules": {
    "customers": {
      "$uid": {
        ".validate": "newData.child('firstName').isString() &&
                      newData.child('birthday').isNumber() &&
                      newData.child('location').isString()"
      }
    }
  }
}
```

## Insert Data

Use the Firebase SDK:

```
const db = firebase.database();
const customers = db.ref().child("customers");

customers.child(primaryKey).set({
  "firstName": "David",
  "lastName": "East",
  "location": "SF"
});
```

The key is unique due to the nature of Firebase.



## Main Differences between SQL and NoSQL

* Integrity (SQL DB)
* Flexibility (NoSQL)