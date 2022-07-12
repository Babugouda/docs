create table student(id int not null AUTO_INCREMENT,firstname varchar(20),lastname varchar(20), primary key (id));


- insert into student values (1,'babu','patil');
- insert into student values (2,'babu2','patil');
- insert into student values (3,'babu3','patil3');
- insert into student values (4,'babu4','patil');
- insert into student values (5,'babu5','patil');
- insert into student values (6,'babu6','patil');

@Entity 
Required annotation for pojo class

@Table(name="Employee")
Option parameter for pojo class, default class name will be table if @Table not mentioned

@Id
Required annotation for pojo class, it's used to mention primary key

@column(name="column_name")

GenerationType.AUTO: Pick appropriate strategy for the underlying database

GenerationType.IDENTITY: Assign primary keys using database identity column, generator will always require a database hit for fetching the primary key value
JDBC batching is disabled, Database is responsible to auto generate the primary key
    
```java
   @Id
   @GeneratedValue(strategy = GenerationType.IDENTITY)
   Integer id;
 ```

GenerationType.SEQUENCE: Assign primary keys using database sequence
```java
   @Id
   @GeneratedValue(strategy = GenerationType.SEQUENCE)
   Integer id;
 ```

GenerationType.TABLE: Assign primary keys using database table to ensure uniqueness

We can create our custom GenerationType by extending the hibernate class org.hibernate.id.SequenceGenerator and override 

the generate method


SessionFactory
Reads the hibernate configuration file
Create session objects
Heavy weight object
Only create once per application
It's thread safe

Session
It wraps the JDBC connection
Short lived, light weight object
It's retrieved from SessionFactory
It's used for crud operations
It's not thread safe

Why hibernate:
1) Caching mechanism: Support 1st and 2nd level caching
2) Different fetch types: Eager and lazy loading
3) Relationship management and provides code for mapping an object to the data
4) The developer is free from writing code to load/store data into the database.
5) database independent


OneToMany Bidirectional Sample
@OneToMany(fetch=FetchType.LAZY, mappedBy = "instructor", cascade = { CascadeType.DETACH, CascadeType.MERGE, 

CascadeType.PERSIST,
			CascadeType.REFRESH })
	private List<Course> courses;

@ManyToOne(cascade = { CascadeType.DETACH, CascadeType.MERGE, CascadeType.PERSIST, CascadeType.REFRESH })
	@JoinColumn(name = "instructor_id")
	private Instructor instructor;

When we access the lazy data after closing the session, it will throw the lazyinitialization exeption

validate: validate the schema, makes no changes to the database.
update: update the schema.
create: creates the schema, destroying previous data.
create-drop: drop the schema when the SessionFactory is closed explicitly, typically when the application is stopped.
Source: community documentation

#### Composite Key -
is a candidate key that consists of two or more attributes (table columns) that together uniquely identify an entity occurrence (table row).

Example:
```java
@Entity
class Time {
@EmbeddedId
TimeId id;

    String src;
    String dst;
    Integer distance;
    Integer price;
}

@Embeddable
class TimeId implements Serializable {
Integer levelStation;
Integer confPathID;
}
```