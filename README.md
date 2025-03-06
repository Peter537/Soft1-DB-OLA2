# Databasedesign af Fitnesscenter

## Gruppe

- Oskar (Ossi-1337, cph-oo221)
- Peter (Peter537, cph-pa153)
- Yusuf (StylizedAce, cph-ya56)

## 1️⃣ ER-Modellering

Vi har benyttet os af https://dbdiagram.io/ til at lave vores ER Model. [Database Markup Language (DBML)](https://dbml.dbdiagram.io/docs/)'en som er brugt til at lave ER-Modellen kan findes i [ER-diagram.md](ER-diagram.md) filen.

[ INDSÆT BILLEDE AF ER-MODEL HER ]

### Constraint definitioner:

- PK: Primary Key
- FK: Foreign Key
- NN: Not Null
- U: Unique

### `members`-table

| Attribut        | Domain    | Constraints | Beskrivelse, Noter & Overvejelser |
| --------------- | --------- | ----------- | --------------------------------- |
| id              | integer   | PK          |                                   |
| email           | varchar   | NN, U       |                                   |
| first_name      | varchar   | NN          |                                   |
| middle_initial  | varchar   |             |                                   |
| last_name       | varchar   | NN          |                                   |
| join_date       | timestamp | NN          |                                   |
| membership_type | varchar   | FK, NN      |                                   |
| phone_number    | varchar   |             |                                   |

### `memberships`-table

| Attribut | Domain  | Constraints | Beskrivelse, Noter & Overvejelser |
| -------- | ------- | ----------- | --------------------------------- |
| type     | varchar | PK          |                                   |
| price    | decimal | FK, NN      |                                   |

### `training_teams`-table

| Attribut      | Domain  | Constraints | Beskrivelse, Noter & Overvejelser |
| ------------- | ------- | ----------- | --------------------------------- |
| id            | integer | PK          |                                   |
| type          | varchar |             |                                   |
| max_members   | integer | NN          |                                   |
| instructor_id | integer | FK          |                                   |

### `training_types`-table

| Attribut    | Domain  | Constraints | Beskrivelse, Noter & Overvejelser |
| ----------- | ------- | ----------- | --------------------------------- |
| type        | varchar | PK          |                                   |
| description | varchar | NN          |                                   |

### `training_times`-table

| Attribut      | Domain  | Constraints | Beskrivelse, Noter & Overvejelser |
| ------------- | ------- | ----------- | --------------------------------- |
| id            | integer | PK          |                                   |
| team_id       | integer | FK, NN      |                                   |
| start_time    | integer | NN          |                                   |
| interval      | integer | NN          |                                   |
| instructor_id | integer | FK, NN      |                                   |

### `bookings`-table

| Attribut  | Domain    | Constraints | Beskrivelse, Noter & Overvejelser |
| --------- | --------- | ----------- | --------------------------------- |
| team_id   | integer   | PK          |                                   |
| member_id | integer   | PK          |                                   |
| join_date | timestamp | NN          |                                   |

### `instructors`-table

| Attribut       | Domain    | Constraints | Beskrivelse, Noter & Overvejelser |
| -------------- | --------- | ----------- | --------------------------------- |
| id             | integer   | PK          |                                   |
| first_name     | varchar   | NN          |                                   |
| middle_initial | varchar   |             |                                   |
| last_name      | varchar   | NN          |                                   |
| email          | varchar   | NN, U       |                                   |
| phone_number   | varchar   |             |                                   |
| join_date      | timestamp | NN          |                                   |

### `payments`-table

| Attribut            | Domain    | Constraints | Beskrivelse, Noter & Overvejelser |
| ------------------- | --------- | ----------- | --------------------------------- |
| id                  | integer   | PK          |                                   |
| member_id           | integer   | FK, NN      |                                   |
| date                | timestamp | NN          |                                   |
| type                | varchar   | FK, NN      |                                   |
| actual_price        | decimal   | NN          |                                   |
| discount_percentage | decimal   | NN          |                                   |

### `payment_type`-table

| Attribut | Domain  | Constraints | Beskrivelse, Noter & Overvejelser |
| -------- | ------- | ----------- | --------------------------------- |
| type     | varchar | PK          |                                   |

Vi har sat `members`-tabellen som vores hovedtabel, da det er dem der er i centrum af fitnesscenteret.
.. vi har lavet composite attribute omkring name https://prnt.sc/1ll9t1wwbBlB
.. optional phone_number, lavet som varchar fordi der er mange slags typer af telefonnumre for forskellige lande

Vi har lavet en `payments`-tabel som en slags log over hvilke betalinger der er blevet foretaget, og hvilke medlemmer der har foretaget dem. Den indeholder ikke hvad de har betalt med etc., da det ikke er relevant for fitnesscenteret. Vi har tilføjet en `type`-attribut som FK til `payment_type` som omhandler om det har været månedligt, årligt, engangs-betaling el.lign så man kan finde ud af om de skal betale igen.

Vi har lavet en `memberships` som en tabel der indeholder hvilke slags medlemskaber der er, og hvad de koster. Det er for at hvis der kommer et nyt medlemskabs-type skal vi ikke ind og ændre en CHECK-constraint i `members`-tabellen.

Vores `bookings`-tabel er en tabel der holder øje med hvilke medlemmer der har tilsluttet sig forskellige teams. Tabellen er på 2NF fordi den har en composite key**(?)** som PK.

Vores `training_teams`-tabel er en tabel der holder på hvilket træningshold der er blevet oprettet, og så er der en `type`-attribut som FK til `training_types`-tabellen for ikke at skulle ændre CHECK-constraint hvis der kommer ny træningstype. Der er også en `instructor_id` som er den normalt satte instruktør på holdet.

`instructors`-tabellen er en tabel lidt ligesom `member`-tabellen med at have information omkring instruktøren. Ligesom members, så har vi også brugt en composite attribute omkring name.

Vores `training_times`-tabel er tabellen der holder styr på hvornår hold træner, hvor det er en log (og over fremtidige tider). Der er også tilføjet en `instructor_id`-attribut for at holde styr på hvem der faktisk var tilstede, fordi selvom der er en `instructor_id` i `training_teams`-tabellen, så kan det være at en anden instruktør har taget over for en dag.

### Overvejelser

1. Vores forståelse af `booking`-tabellen er, at det skal være tabellen der holder øje med hvilke medlemmer der har tilsluttet sig forskellige teams, og at det ikke er en tabel som man kan søge om at komme på et træningshold.

2. Vi overvejede lidt om vi burde starte alle FK's navne med `fk_` for at gøre det nemmere at se hvad der er FK

3. Vi overvejede om vi skulle sætte varchar-længder på vores attributter.

### Yderligere overvejelser

2. Ift. discounts, det er noget vi har været i tvivl om, vi har valgt at lave det som en attribut i payments-tabellen, men vi snakkede om, om man burde lave en tabel der havde forskellige slags discounts, hvor længe de varer, og en tabel til hvor members kan blive tildelt discounts. DBML eksempel:

```

```

## 2️⃣ Normalisering af ER-modellen

### 1NF

Vores ER-model opfylder 1NF, da alle attributter er atomare og at der ikke er gentagne grupper.

**Hvordan 1NF kunne være brudt**

1. Fx hvis training_teams-tabellen havde haft en attribute som 'members' som var en liste af member_id'er i stedet for at have bookings-tabellen.
2. Fx hvis member kunne have flere telefon-numre ved at bruge 'phone_number1', 'phone_number2' osv.

### 2NF

Vores ER-model opfylder 2NF, da der ikke er nogle partielle funktionelle afhængigheder.

**Hvordan 2NF kunne være brudt**

1. Fx hvis bookings-tabellen havde en attribute som 'member_name' som var afhængig af 'member_id' og ikke hele primary key'en.

### 3NF

Vores ER-moder opfylder 3NF, da der ikke er nogle transitive afhængigheder.

**Hvordan 3NF kunne være brudt**

1. Fx hvis members-tabellen havde haft membership_price som attribut i stedet for vi havde memberships-tabellen.

## 3️⃣ Mapping til Relationel Model

```

```
