# ER-diagram

```dbml
Table members {
  id integer [PK]
  email varchar [NOT NULL, UNIQUE]
  first_name varchar [NOT NULL]
  middle_initial varchar
  last_name varchar [NOT NULL]
  join_date timestamp [NOT NULL]
  membership_type varchar [NOT NULL]
  phone_number varchar
}

Table memberships {
  type varchar [PK]
  price decimal [NOT NULL]
}

Table training_teams {
  id integer [PK]
  type varchar [NOT NULL]
  max_members int [NOT NULL]
  instructor_id int [NOTE: "The Instructor assigned to the Team"]
}

Table training_types {
  type varchar [PK]
  description varchar [NOT NULL]
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
  first_name varchar [NOT NULL]
  middle_initial varchar
  last_name varchar [NOT NULL]
  email varchar [NOT NULL, UNIQUE]
  phone_number varchar
  join_date timestamp [NOT NULL]
}

Table payments {
  id integer [PK]
  member_id integer [NOT NULL]
  date timestamp [NOT NULL]
  type varchar [NOT NULL]
  actual_price decimal [NOT NULL]
  discount_percentage integer [NOT NULL, NOTE: "We don't know why discount happened"]
}

Table payment_type {
  type varchar [PK]
}

Ref: members.id > bookings.member_id
Ref: members.id > payments.member_id
Ref: members.membership_type > memberships.type

Ref: training_teams.id > bookings.team_id
Ref: training_teams.type > training_types.type
Ref: training_teams.id > training_times.team_id

Ref: instructors.id > training_teams.instructor_id
Ref: instructors.id > training_times.instructor_id

Ref: payments.type > payment_type.type
```
