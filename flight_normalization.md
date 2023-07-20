# Flight dataset Normalization 

<img src="flight_normalized.png" alt="Flight Normalization" width="600" height="500">

The first step I took for this project is to normalized this database. Normalization is useful for the database as it:

1. Ensure data integrity
2. Minimize redundancy
3. Avoid anomalies

To do so, I created 5 different tables by inserting information from the original flight table  

## Passenger table 
To create the `passenger` table, I inserted data from the original `flight` table, including `email`, `firstname`, `lastname`, `gender`, `country`, `region`, and `postalcode`.

```sql
INSERT INTO project1.passenger (`email`, `firstname`, `lastname`, `gender`, `country`, `region`, `postalcode`)
SELECT `email`, `firstname`, `lastname`, `gender`, `country`, `region`, `postalcode`
FROM project1.flight;
```
## Airline table 

The airline table was created to store distinct airline names from the flight table.

 ```sql
INSERT INTO project1.airline (`name`)
SELECT DISTINCT  `airline`
FROM project1.flight;
```

## Country table 
For the country table, I inserted distinct origin values from the flight table since both origin and destination share the same countries.

```sql
INSERT INTO project1.country (`name`)
SELECT DISTINCT `origin`
FROM project1.flight; 
```

## seatclass table 

The seatclass table was created to store distinct seatclass names from the flight table.

```sql
INSERT INTO project1.seatclass (`name`)
SELECT DISTINCT seatclass
FROM project1.flight;
```

## Booking table 
Finally, I created the booking table to store booking information, including id, date, amount, user_id, origin_id, destination_id, airline_id, and seatclass_id. The data for this table was obtained by joining the normalized data from other tables.

```sql
INSERT INTO project1.booking (`id`, `date`, `amount`, `user_id`, `origin_id`, `destination_id`, `airline_id`, `seatclass_id` )
SELECT f.bookingid, 
			f.bookingdate, 
			f.amount,
			p.id,
      o.id AS origin_id,
      d.id AS destination_id,
      a.id AS airline_id,
			s.id AS seatclass_id

FROM project1.flight f
JOIN project1.passenger p ON  f.email = p.email
JOIN project1.airline a ON f.airline = a.name
JOIN project1.seatclass s ON f.seatclass = s.name
JOIN project1.country o ON f.origin = o.name
JOIN project1.country d ON f.destination = d.name;
```

