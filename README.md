# Databasedesign af Fitnesscenter

## Gruppe

- Oskar (Ossi-1337, cph-oo221)
- Peter (Peter537, cph-pa153)
- Yusuf (StylizedAce, cph-ya56)

---

## 1️⃣ ER-Modellering

Vi har benyttet os af https://dbdiagram.io/ til at lave vores ER Model. [Database Markup Language (DBML)](https://dbml.dbdiagram.io/docs/)'en som er brugt til at lave ER-Modellen kan findes i [ER-diagram.md](ER-diagram.md) filen.

[ INDSÆT BILLEDE AF ER-MODEL HER ]

### Constraint definitioner:

- PK: Primary Key
- FK: Foreign Key
- NN: Not Null
- U: Unique

### `members`-table

| Attribut        | Data Type | Constraints |
| --------------- | --------- | ----------- |
| id              | integer   | PK          |
| email           | varchar   | NN, U       |
| first_name      | varchar   | NN          |
| middle_initial  | varchar   |             |
| last_name       | varchar   | NN          |
| join_date       | timestamp | NN          |
| membership_type | varchar   | FK, NN      |
| phone_number    | varchar   |             |

**Overvejelser:**

- Vi har lavet `id` som PK og ikke `email` fordi hvis et medlem skifter email, så er det nemmere at opdaterer email'en end at skulle opdatere alle FK's i andre tabeller, men vi har stadig sat U på email for at sikre at der ikke er 2 medlemmer med samme email.
- Vi har lavet name til en composite attribute, fordi det kan bruges til flere forskellige ting såsom hvis man vil lave personlige beskeder til medlemmer (ex. 'Hej, Peter'), eller hvis man vil lave noget sortering eller filtrering på navne.
- Vi har lavet `phone_number` som en optional varchar fordi der er mange slags typer af telefonnumre for forskellige lande, og det er ikke nødvendigt at have en telefonnummer for at være medlem.
- Vi har lavet `membership_type` som FK til `memberships`-tabellen for at sikre at der ikke er nogen der har et medlemskab der ikke eksisterer. Derudover med denne FK sikrer vi også vi ikke behøver gå ind i `members`-tabellen for at ændre en CHECK-constraint hvis der kommer et nyt medlemskabstype.

### `memberships`-table

| Attribut | Data Type | Constraints |
| -------- | --------- | ----------- |
| type     | varchar   | PK          |
| price    | decimal   | NN          |

**Overvejelser:**

- Vi har lavet `type` som PK fordi typerne af medlemskaber er unikke og ikke skal kunne ændres, så derfor behøver der ikke være et autogenereret id.
- Vi har lavet `price` som decimal for at kunne håndtere priser med decimaler.

### `training_teams`-table

| Attribut      | Data Type | Constraints |
| ------------- | --------- | ----------- |
| id            | integer   | PK          |
| type          | varchar   | FK, NN      |
| max_members   | integer   | NN          |
| instructor_id | integer   | FK          |

**Overvejelser:**

- Vi har lavet et `id` som PK for at kunne identificere holdene.
- Vi har lavet `type` som en FK til `training_types`-tabellen for at sikre at der ikke er nogen der opretter et hold med en træningstype der ikke eksisterer. Derudover med denne FK sikrer vi også vi ikke behøver gå ind i `training_teams`-tabellen for at ændre en CHECK-constraint hvis der kommer en ny træningstype.
- Vi har lavet `max_members` som NN for at sikre at hold ikke kan blive for store.
- Vi har lavet en `instructor_id` på tabellen for at kunne se hvem der er den normalt tildelte instruktør på holdet.
- Der skal være en CHECK-constraint at `max_members` er et positivt heltal.

### `training_types`-table

| Attribut    | Data Type | Constraints |
| ----------- | --------- | ----------- |
| type        | varchar   | PK          |
| description | varchar   | NN          |

**Overvejelser:**

- Vi har lavet `type` som PK for at kunne identificere træningstyperne.
- Vi har lavet `description` som NN for at kunne beskrive hvad træningstypen er.

### `training_times`-table

| Attribut      | Data Type | Constraints |
| ------------- | --------- | ----------- |
| id            | integer   | PK          |
| team_id       | integer   | FK, NN      |
| start_time    | timestamp | NN          |
| interval      | interval  | NN          |
| instructor_id | integer   | FK, NN      |

**Overvejelser:**

- Vi har lavet `id` som PK for at kunne identificere træningstiderne.
- Vi har lavet `team_id` som FK til `training_teams`-tabellen og NN for at sikre at når en ny træningstid bliver oprettet er den tilføjet et hold og der ikke kan tilføjes tider uden et hold er tilknyttet.
- `start_time` og `interval` er NN for at sikre at der er en starttid og interval for træningstiden.
- `instructor_id` er på denne tabel selvom der er en `instructor_id` i `training_teams`-tabellen for at kunne se hvem der faktisk var tilstede, fordi selvom der er en `instructor_id` i `training_teams`-tabellen, så kan det være at en anden instruktør har taget over for en dag.

### `bookings`-table

| Attribut  | Data Type | Constraints |
| --------- | --------- | ----------- |
| team_id   | integer   | PK          |
| member_id | integer   | PK          |
| join_date | timestamp | NN          |

**Overvejelser:**

- Vores forståelse af `bookings`-tabellen er, at det skal være tabellen der holder øje med hvilke medlemmer der har tilsluttet sig forskellige teams, og at det ikke er en som man kan søge om at komme på et træningshold.
- `team_id` og `member_id` er en composite key for at kunne identificere hvilke medlemmer der er på hvilke hold.
- `join_date` er NN for at kunne se hvornår medlemmerne er blevet tilføjet til holdet.

### `instructors`-table

| Attribut       | Data Type | Constraints |
| -------------- | --------- | ----------- |
| id             | integer   | PK          |
| first_name     | varchar   | NN          |
| middle_initial | varchar   |             |
| last_name      | varchar   | NN          |
| email          | varchar   | NN, U       |
| phone_number   | varchar   |             |
| join_date      | timestamp | NN          |

**Overvejelser:**

- `instructor`-tabellen er meget ligesom `members`-tabellen ift. attributter, så vi vil ikke gentage os selv.

### `payments`-table

| Attribut            | Data Type | Constraints |
| ------------------- | --------- | ----------- |
| id                  | integer   | PK          |
| member_id           | integer   | FK, NN      |
| date                | timestamp | NN          |
| type                | varchar   | FK, NN      |
| actual_price        | decimal   | NN          |
| discount_percentage | decimal   | NN          |

**Overvejelser:**

- Vores forståelse af `payments`-tabellen er, at det skal være en slags log over hvilke betalinger der er blevet foretaget, og hvilke medlemmer der har foretaget dem. Den indeholder ikke hvad de har betalt med etc., da det ikke er relevant for fitnesscenteret.
- Vi har `actual_price` og `discount_percentage` som NN for at kunne se hvad prisen var og hvor meget der blev givet i rabat. Man kan bare udregne hvad prisen var før rabatten, hvor det ville bryde 3NF hvis vi havde en attribut for det.
- `type` er en FK til `payment_type`-tabellen for at kunne se hvad slags betaling der er blevet foretaget.
- Der skal være en CHECK-constraint at `discount_percentage` er et decimal mellem 0 og 100 (inklusiv begge).

### `payment_types`-table

| Attribut | Data Type | Constraints |
| -------- | --------- | ----------- |
| type     | varchar   | PK          |

**Overvejelser:**

- Vi har lavet `type` som PK fordi det skal være en liste over forskellige slags betalingstyper såsom månedligt, årligt, engangs-betaling el.lign.

---

## 2️⃣ Normalisering af ER-modellen

### 1NF

Vores ER-model opfylder 1NF, da alle attributter er atomare og at der ikke er gentagne grupper.

**Hvordan 1NF kunne være brudt**

Hvis vi skulle have brudt at alle attributer skal være atomare værdier, så kunne vi have gjort det ved at have haft en `members` attribut i `training_teams`-tabellen, som var en liste af member_id'er i stedet for at have bookings-tabellen.

Hvis vi skulle have brudt at der ikke må være gentagne grupper, så kunne vi have gjort det ved at have haft `phone_number1`, `phone_number2` osv. i `members`-tabellen, men vi har valgt at der kun kan være et telefonnummer pr. medlem, men hvis der skulle have været flere, så kunne vi have lavet en `phone_numbers`-tabel som havde en FK til `members`-tabellen og en `phone_number`-attribut.

### 2NF

Vores ER-model opfylder 2NF, da der ikke er nogle partielle funktionelle afhængigheder.

**Hvordan 2NF kunne være brudt**

Hvis vi havde haft `member_name` attributen i `bookings`-tabellen, så ville det have brudt 2NF, da `member_name` er afhængig af `member_id` og ikke af `team_id`.

### 3NF

Vores ER-moder opfylder 3NF, da der ikke er nogle transitive afhængigheder.

**Hvordan 3NF kunne være brudt**

Hvis vi havde haft `total_price` attributen i `payments`-tabellen, så ville det have brudt 3NF, da `total_price` er afhængig af `actual_price` og `discount_percentage`.

---

## 3️⃣ Mapping til Relationel Model

```

```
