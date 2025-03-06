# ER-diagram

```
// Use DBML to define your database structure
// Docs: https://dbml.dbdiagram.io/docs

Table members {
  id integer [PK]
  email varchar [NOT NULL, UNIQUE]
  first_name varchar [NOT NULL]
  middle_initial varchar
  last_name varchar [NOT NULL]
  join_date timestamp [NOT NULL]
  membership_type string [NOT NULL]
  phone_number varchar
}

Table memberships {
  type string [PK]
  price decimal [NOT NULL]
}

Table training_teams {
  id integer [PK]
  type string [NOT NULL]
  max_members int [NOT NULL]
  instructor_id int [NOTE: "The Instructor assigned to the Team"]
}

Table training_types {
  type string [PK]
  description string [NOT NULL]
}

Table training_times {
  id integer [PK]
  team_id integer [NOT NULL]
  start_time timestamp [NOT NULL]
  interval interval [NOT NULL]
  instructor_id integer [NOT NULL, NOTE: "The Instructor present for the time"]
}

Table bookings {
  team_id integer [PK]
  member_id integer [PK]
  join_date timestamp [NOT NULL]
}

Table instructors {
  id integer [PK]
  name varchar [NOT NULL]
  email varchar [NOT NULL, UNIQUE]
  phone_number varchar
  join_date timestamp [NOT NULL]
}

Table payments {
  id integer [PK]
  member_id integer [NOT NULL]
  date timestamp [NOT NULL]
  type varchar [NOT NULL]
  total_price decimal [NOT NULL]
  actual_price decimal [NOT NULL]
  discount_id integer
}

Table payment_type {
  type varchar [PK]
}

Table member_discounts {
  id integer [PK]
  member_id integer [NOT NULL]
  percentage decimal [NOT NULL, NOTE: "Between 0-100"]
}

Ref: members.id > bookings.member_id

Ref: training_teams.id > bookings.team_id

Ref: members.membership_type > memberships.type

Ref: instructors.id > training_teams.instructor_id

Ref: members.id > member_discounts.member_id

Ref: training_teams.type > training_types.type

Ref: training_teams.id > training_times.team_id

Ref: instructors.id > training_times.instructor_id

Ref: members.id > payments.id

Ref: member_discounts.id > payments.discount_id

Ref: payments.type > payment_type.type
```
