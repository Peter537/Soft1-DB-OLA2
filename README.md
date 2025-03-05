# Databasedesign af Fitnesscenter

## Gruppe

- Oskar (Ossi-1337, cph-oo221)
- Peter (Peter537, cph-pa153)
- Yusuf (StylizedAce, cph-ya56)

## 1️⃣ ER-Modellering

Vi har benyttet os af https://dbdiagram.io/ til at lave vores ER Model. [Database Markup Language (DBML)](https://dbml.dbdiagram.io/docs/)'en som er brugt til at lave ER-Modellen kan findes i [ER-diagram.md](ER-diagram.md) filen.

[ INDSÆT BILLEDE AF ER-MODEL HER ]

### Forklaring af ER-model

### Overvejelser

## 2️⃣ Normalisering af ER-modellen

### 1NF

**Hvordan 1NF kunne være brudt**

1. Fx hvis training_teams-tabellen havde haft en attribute som 'members' som var en liste af member_id'er i stedet for at have bookings-tabellen.
2. Fx hvis member kunne have flere telefon-numre ved at bruge 'phone_number1', 'phone_number2' osv.

### 2NF

**Hvordan 2NF kunne være brudt**

1. Fx hvis bookings-tabellen havde en attribute som 'member_name' som var afhængig af 'member_id' og ikke hele primary key'en.

### 3NF

**Hvordan 3NF kunne være brudt**

1. Fx hvis members-tabellen havde haft membership_price som attribut i stedet for vi havde memberships-tabellen.

### BCNF

**Hvordan BCNF kunne være brudt**

1. Fx hvis training_teams-tabellen havde haft en attribut som 'instructor_name' som var afhængig af 'instructor_id'

## 3️⃣ Mapping til Relationel Model

```

```
