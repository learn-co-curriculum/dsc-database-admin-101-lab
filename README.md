
# Database Admin 101 - Lab

## Introduction 

In this lab you'll go through the process of designing and creating a database. From there, you'll begin to populate this table with mock data provided to you.

## Objectives

You will be able to:

* Create a SQL database
* Create a SQL table
* Create rows in a SQL table
* Alter entries in a SQL table
* Delete entries in a SQL table
* Commit changes via sqlite3


## The Scenario

You are looking to design a database for a school which will house various information from student grades to contact information, class roster lists and attendance. First, think of how you would design such a database. What tables would you include? What columns would each table have? What would be the primary means to join said tables?

## Creating the Database

Now that you've put a little thought into how you might design your database, it's time to go ahead and create it! Start by import the necessary packages. Then, create a database called **school.sqlite**.


```python
#Your code here; import necessary packages
import sqlite3
import pandas as pd
```


```python
#Your code here; create the database school.sqlite
conn = sqlite3.Connection('school.sqlite')
```

## Create a Table for Contact Information

Create a table called contactInfo to house contact information for both students and staff. Be sure to include columns for first name, last name, role (student/staff), email, telephone number, street, city, state and zipcode. Be sure to also create a primary key for the table. 


```python
#Your code here
cur = conn.cursor()
cur.execute("""CREATE TABLE contactInfo (
                                        userId INTEGER PRIMARY KEY,
                                        firstName TEXT,
                                        lastName TEXT,
                                        role TEXT,
                                        telephone INTEGER,
                                        street TEXT,
                                        city TEXT,
                                        state TEXT,
                                        zipcode TEXT
                                        );
            """)
```




    <sqlite3.Cursor at 0x112a06500>



## Populate the Table

Below, code is provided for you in order to load a list of dictionaries. Briefly examine the list. Each dictionary in the list will serve as an entry for your contact info table. Once you've briefly investigated the structure of this data, write a for loop to iterate through the list and create an entry in your table for each person's contact info.


```python
# Your code to iterate over the contact list and populate the contactInfo table here
for contact in contacts:
    firstName = contact['firstName']
    lastName = contact['lastName']
    role = contact['role']
    telephone  = contact['telephone ']
    street = contact['street']
    city = contact['city']
    state = contact['state']
    zipcode  = contact['zipcode ']
    cur.execute("""INSERT INTO contactInfo (firstName, lastName, role, telephone, street, city, state, zipcode) 
                  VALUES ('{}', '{}', '{}', '{}', '{}', '{}', '{}', '{}');
                """.format(firstName, lastName, role, telephone, street, city, state, zipcode) )
```

**Query the Table to Ensure it is populated**


```python
# Your code here

cur.execute("""SELECT * FROM contactInfo;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>userId</th>
      <th>firstName</th>
      <th>lastName</th>
      <th>role</th>
      <th>telephone</th>
      <th>street</th>
      <th>city</th>
      <th>state</th>
      <th>zipcode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Christine</td>
      <td>Holden</td>
      <td>staff</td>
      <td>2035687697</td>
      <td>1672 Whitman Court</td>
      <td>Stamford</td>
      <td>CT</td>
      <td>06995</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Christopher</td>
      <td>Warren</td>
      <td>student</td>
      <td>2175150957</td>
      <td>1935 University Hill Road</td>
      <td>Champaign</td>
      <td>IL</td>
      <td>61938</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Linda</td>
      <td>Jacobson</td>
      <td>staff</td>
      <td>4049446441</td>
      <td>479 Musgrave Street</td>
      <td>Atlanta</td>
      <td>GA</td>
      <td>30303</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Andrew</td>
      <td>Stepp</td>
      <td>student</td>
      <td>7866419252</td>
      <td>2981 Lamberts Branch Road</td>
      <td>Hialeah</td>
      <td>Fl</td>
      <td>33012</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Jane</td>
      <td>Evans</td>
      <td>student</td>
      <td>3259909290</td>
      <td>1461 Briarhill Lane</td>
      <td>Abilene</td>
      <td>TX</td>
      <td>79602</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Jane</td>
      <td>Evans</td>
      <td>student</td>
      <td>3259909290</td>
      <td>1461 Briarhill Lane</td>
      <td>Abilene</td>
      <td>TX</td>
      <td>79602</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Mary</td>
      <td>Raines</td>
      <td>student</td>
      <td>9075772295</td>
      <td>3975 Jerry Toth Drive</td>
      <td>Ninilchik</td>
      <td>AK</td>
      <td>99639</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Ed</td>
      <td>Lyman</td>
      <td>student</td>
      <td>5179695576</td>
      <td>3478 Be Sreet</td>
      <td>Lansing</td>
      <td>MI</td>
      <td>48933</td>
    </tr>
  </tbody>
</table>
</div>



## Commit Your Changes to the Database

Persist your changes by committing them to the database.


```python
#Your code here
conn.commit()
```

## Create a Table for Student Grades

Create a new table in the database called "grades". In the table, include the following fields: userId, courseId, grade.

** This problem is a bit more tricky and will require a dual key. (A nuance you have yet to see.)
Here's how to do that:

```SQL
CREATE TABLE table_name(
   column_1 INTEGER NOT NULL,
   column_2 INTEGER NOT NULL,
   ...
   PRIMARY KEY(column_1,column_2,...)
);
```


```python
#Your code here; create the grades table.
cur.execute("""CREATE TABLE grades (
                                    userId INTEGER NOT NULL,
                                    courseId INTEGER NOT NULL,
                                    grade INTEGER,
                                    PRIMARY KEY(userId, courseId)
                                    );
            """)
```




    <sqlite3.Cursor at 0x112a06500>



## Remove Duplicate Entries

An analyst just realized that there is a duplicate entry in the contactInfo table! Find and remove it.


```python
#Your code here; find the duplicate entry
cur.execute("""SELECT firstName, lastName, telephone, COUNT(*) 
               FROM contactInfo
               GROUP BY 1,2,3
               HAVING COUNT(*) > 1;""").fetchall()
```




    [('Jane', 'Evans', 3259909290, 2)]




```python
#Your code here; delete the duplicate entry
cur.execute('''DELETE FROM contactInfo WHERE telephone = 3259909290;''')
```




    <sqlite3.Cursor at 0x112a06500>




```python
#Your code here; check that the duplicate entry was removed.
cur.execute("""SELECT firstName, lastName, telephone, COUNT(*) 
               FROM contactInfo
               GROUP BY 1,2,3
               HAVING COUNT(*) > 1;""").fetchall()
```




    []



## Updating an Address

Ed Lyman just moved to `2910 Simpson Avenue York, PA 17403`. Update his address accordingly.


```python
#Your code here; update Ed's address
cur.execute('''UPDATE contactInfo
               SET street = "2910 Simpson Avenue",
                   city = 'York',
                   state = 'PA',
                   zipcode = '17403'
               WHERE firstName = "Ed" AND lastName = "Lyman";
            ''')
```




    <sqlite3.Cursor at 0x112a06500>




```python
#Your code here; Query the database to ensure the change was made
cur.execute("""SELECT * FROM contactInfo;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>userId</th>
      <th>firstName</th>
      <th>lastName</th>
      <th>role</th>
      <th>telephone</th>
      <th>street</th>
      <th>city</th>
      <th>state</th>
      <th>zipcode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Christine</td>
      <td>Holden</td>
      <td>staff</td>
      <td>2035687697</td>
      <td>1672 Whitman Court</td>
      <td>Stamford</td>
      <td>CT</td>
      <td>06995</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Christopher</td>
      <td>Warren</td>
      <td>student</td>
      <td>2175150957</td>
      <td>1935 University Hill Road</td>
      <td>Champaign</td>
      <td>IL</td>
      <td>61938</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Linda</td>
      <td>Jacobson</td>
      <td>staff</td>
      <td>4049446441</td>
      <td>479 Musgrave Street</td>
      <td>Atlanta</td>
      <td>GA</td>
      <td>30303</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Andrew</td>
      <td>Stepp</td>
      <td>student</td>
      <td>7866419252</td>
      <td>2981 Lamberts Branch Road</td>
      <td>Hialeah</td>
      <td>Fl</td>
      <td>33012</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7</td>
      <td>Mary</td>
      <td>Raines</td>
      <td>student</td>
      <td>9075772295</td>
      <td>3975 Jerry Toth Drive</td>
      <td>Ninilchik</td>
      <td>AK</td>
      <td>99639</td>
    </tr>
    <tr>
      <th>5</th>
      <td>8</td>
      <td>Ed</td>
      <td>Lyman</td>
      <td>student</td>
      <td>5179695576</td>
      <td>2910 Simpson Avenue</td>
      <td>York</td>
      <td>PA</td>
      <td>17403</td>
    </tr>
  </tbody>
</table>
</div>



## Commit Your Changes to the Database

Once again, persist your changes by committing them to the database.


```python
#Your code here
conn.commit()
```

## Summary

While there's certainly more to do with setting up and managing this database, you got a taste for creating, populating, and maintaining databases! Feel free to continue fleshing out this exercise for more practice. 
