# 📌 **Obligatorisk afleveringsopgave: Databasedesign af Fitnesscenter (Assignment 2 - 10 study points)**

## **🎯 Opgavens Formål**

Denne opgave har til formål at træne dig i **datamodellering**, **normalisering** og **mapping til en relationel model**.
I skal udarbejde en **ER-model for et fitnesscenter**, normalisere den og derefter konvertere den til en **relationel model**. I skal ikke oprette en fysisk database.

---

## **📌 1️⃣ ER-Modellering**

### **📖 Baggrund**

Fitnesscentret tilbyder **medlemskaber**, **holdtræning**, og har **instruktører**, der underviser på holdene. Centret vil registrere, hvilke medlemmer der deltager på hvilke hold, samt hvilke instruktører der er tilknyttet hvilke hold.

### **🏋️ Nøglefunktioner i systemet**

- **Medlemskaber:** Basis, Premium, Elite – med forskelle i adgang til faciliteter.
- **Træningshold:** Spinning, CrossFit, Yoga m.m. – hvor medlemmer kan tilmelde sig.
- **Instruktører:** Tilknyttet hold og kan administrere deltagerlister.
- **Booking:** Medlemmer kan booke pladser på hold eller personlig træning. Hold har typisk begrænset antal pladser.
- **Betaling:** Månedsvis abonnement eller klippekort.
- **Medlemsrabatter:** Kan gives i særlige tilfælde.

📌 **Opgaven:**

1. Identificér de **entiteter**, der er relevante for fitnesscentret.
2. Definér **attributter** for hver entitet.
3. Identificér **relationer og multiplicitet** mellem entiteterne.
4. Tegn et **ER-diagram** (håndtegnet eller ved hjælp af et værktøj som dbdiagram.io, Lucidchart eller MySQL Workbench). I er velkome til at opsøge mere viden om eksisterende fitnesscentre og benytte som input til jeres ER modellering.
5. Kommentér jeres ER model - hvilke overvejelser I har gjort undervejs, er der noget særligt man skal hæfte sig ved i jeres model etc.

---

## **📌 2️⃣ Normalisering af ER-modellen**

Når I har udarbejdet **ER-modellen**, skal I kvalitetstjekke modellen (for at undgå dataredundans og opdateringsanomalier) **trinvist** fra **1. normalform (1NF)** til **3. normalform (3NF)**. I kan vælge også at inddrage den mere avancerede **Boyce-Codd formalform (BCNF)**.

📌 **Opgaven:**

1. **1NF:** Sikr jer, at alle attributter er **atomare** og at der ikke er gentagne grupper.
2. **2NF:** Fjern **partielle funktionelle afhængigheder** (sørg for, at ikke-nøgle attributter afhænger af hele primærnøglen).
3. **3NF:** Fjern **transitive afhængigheder** (sørg for, at alle ikke-nøgle attributter kun afhænger af primærnøglen).

🔹 **Dokumentér hver normaliseringstrin med en kort forklaring og en tabel, der viser ændringerne.**
🔹 Hvis jeres ER model allerede er normaliseret inden anvendelse af normalformerne, så giv et eksempel på et design, hvor 2. og 3. normalform ville være brudt.

---

## **📌 3️⃣ Mapping til Relationel Model**

Når jeres **ER-model er normaliseret**, skal I **mappe den til en relationel model**.

📌 **Opgaven:**

1. Definér **relationer** (tabeller) med **primærnøgler (PK) og fremmednøgler (FK)**.
2. Dokumentér resultatet som en **relationsmodel**, f.eks.:

```
MEMBER (member_id PK, name VARCHAR(100), start_date DATE, membertype ENUM('Basis', 'Premium', 'Elite'))
TEAM (team_id PK, description VARCHAR(50), max_participants INT, instructor_id FK)
```

---

## **📌 Aflevering**

### **📁 I skal aflevere:**

1. **ER-diagram** (håndtegnet eller digitalt) med evt. kommentarer til jeres design.
2. **Dokumentation for normalisering** (for hver normalform, inkl. forklaringer og tabeller).
3. **Den relationelle model** (relationer med PK og FK).

- I må arbejde sammen **gruppevis** (max. 4 personer, men gerne mindre grupper)
- Deadline for aflevering: søndag 16. marts kl 22.00 (**github-link** og **navne på gruppemedlemmer**, som sendes på mail til ...)

---

🎯 **God fornøjelse med opgaven!** 🚀
