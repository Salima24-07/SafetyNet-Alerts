# SafetyNet Alerts

SafetyNet Alerts

## Requirements

For building and running the application you need:

- [JDK 11](https://openjdk.java.net/projects/jdk/11/)
- [Maven 3](https://maven.apache.org)
- [Postgresql 13](https://www.postgresql.org/download/)

## How to Run 

* Clone this repository 
* Make sure you are using JDK 11 and Maven 3.x
* Run postgresql server
* Create 2 datebases with Postgresql:
    - Application database
    - Tests database
* Add the following variables to your environnement variables:
    - SAFETYNET_DATABASE_URL: the url to application database
    - SAFETYNET_DATABASE_TEST_URL: the url to the test database
    - SAFETYNET_DATABASE_USER: database username
    - SAFETYNET_DATABASE_PASSWORD: database password

* There are several ways to run a Spring Boot application on your local machine. One way is to execute the `main` method in the `src\main\java\com\projet\safety\safetynet\SafetyNetApplication.java` class from your IDE.

Alternatively you can use the [Spring Boot Maven plugin](https://docs.spring.io/spring-boot/docs/current/reference/html/build-tool-plugins-maven-plugin.html) like so:

```shell
mvn spring-boot:run
```
* Check the stdout to make sure no exceptions are thrown

Once the application runs you should see something like this

```
2022-01-07 20:58:24.358  INFO 43100 --- [  restartedMain] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 5454 (http) with context path ''
2022-01-07 20:58:24.396  INFO 43100 --- [  restartedMain] c.p.s.safetynet.SafetyNetApplication     : Started SafetyNetApplication in 8.002 seconds (JVM running for 8.809)
```

The application is running on **localhost:8080**


Here are some endpoints you can call:

### Get information about system health, info, metrics and httptrace.

```
http://localhost:8080/actuator/health
http://localhost:8080/actuator/info
http://localhost:8080/actuator/metrics
http://localhost:8080/actuator/httptrace
```

### Create Person

ajouter une nouvelle personne

```
POST /person
Content-Type: application/json
{

    "firstName": "test",

    "lastName": "test",

    "address": "1509 Culver St",

    "city": "Culver",

    "zip": "97451",

    "phone": "841-874-6512",

    "email": "test@email.com"

}
RESPONSE: 201 Created
```

### Update Person

mettre ?? jour une personne existante (pour le moment, supposons que le pr??nom et le nom de

famille ne changent pas, mais que les autres champs peuvent ??tre modifi??s)

```
PUT /person
Content-Type: application/json
{

    "firstName": "test",

    "lastName": "test",

    "address": "1509 Culver St",

    "city": "Culver",

    "zip": "97451",

    "phone": "841-874-6512",

    "email": "test@email.com"

}
RESPONSE: 200 OK
```

### Delete Person

supprimer une personne (utilisez une combinaison de pr??nom et de nom comme identificateur

unique)

```
DELETE /person
Content-Type: application/json
{

    "firstName": "test",

    "lastName": "test",

    "address": "1509 Culver St",

    "city": "Culver",

    "zip": "97451",

    "phone": "841-874-6512",

    "email": "test@email.com"

}
RESPONSE: 200 OK
```

### Create FireStation

ajout d'un mapping caserne/adresse

```
POST /firestation
Content-Type: application/json
{

    "address": "test",

    "station": "3"

}
RESPONSE: 201 Created
```

### Update FireStation

mettre ?? jour le num??ro de la caserne de pompiers d'une adresse

```
PUT /firestation
Content-Type: application/json
{

    "address": "test",

    "station": "3"

}
RESPONSE: 200 OK
```

### Delete FireStation

supprimer le mapping d'une caserne ou d'une adresse

```
DELETE /firestation
Content-Type: application/json
{

    "address": "test",

    "station": "3"

}
RESPONSE: 200 OK
```

### Create MedicalRecord

ajouter un dossier m??dical

```
POST /medicalrecord
Content-Type: application/json
{

    "firstName": "test",

    "lastName": "test",

    "birthdate": "01/03/1989",

    "medications": [],

    "allergies": []

}
RESPONSE: 201 Created
```

### Update MedicalRecord

mettre ?? jour un dossier m??dical existant (comme ??voqu?? pr??c??demment, supposer que le

pr??nom et le nom de famille ne changent pas) 

```
PUT /medicalrecord
Content-Type: application/json
{

    "firstName": "test",

    "lastName": "test",

    "birthdate": "01/03/1989",

    "medications": [],

    "allergies": []

}
RESPONSE: 200 OK
```

### Delete MedicalRecord

supprimer un dossier m??dical (utilisez une combinaison de pr??nom et de nom comme

identificateur unique)

```
DELETE /medicalrecord
Content-Type: application/json
{

    "firstName": "test",

    "lastName": "test",

    "birthdate": "01/03/1989",

    "medications": [],

    "allergies": []

}
RESPONSE: 200 OK
```

### Station alert

Cette url doit retourner une liste des personnes couvertes par la caserne de pompiers correspondante.

Donc, si le num??ro de station = 1, elle doit renvoyer les habitants couverts par la station num??ro 1. La liste

doit inclure les informations sp??cifiques suivantes : pr??nom, nom, adresse, num??ro de t??l??phone. De plus,

elle doit fournir un d??compte du nombre d'adultes et du nombre d'enfants (tout individu ??g?? de 18 ans ou

moins) dans la zone desservie

```
GET /firestation?stationNumber=1
RESPONSE: 200 OK
```

### Child alert

Cette url doit retourner une liste d'enfants (tout individu ??g?? de 18 ans ou moins) habitant ?? cette adresse.

La liste doit comprendre le pr??nom et le nom de famille de chaque enfant, son ??ge et une liste des autres

membres du foyer. S'il n'y a pas d'enfant, cette url peut renvoyer une cha??ne vide

```
GET /childAlert?address=1509 Culver St
RESPONSE: 200 OK
```

### Phone alert

Cette url doit retourner une liste des num??ros de t??l??phone des r??sidents desservis par la caserne de

pompiers. Nous l'utiliserons pour envoyer des messages texte d'urgence ?? des foyers sp??cifiques.

```
GET /phoneAlert?firestation=1
RESPONSE: 200 OK
```

### Fire alert

Cette url doit retourner la liste des habitants vivant ?? l???adresse donn??e ainsi que le num??ro de la caserne

de pompiers la desservant. La liste doit inclure le nom, le num??ro de t??l??phone, l'??ge et les ant??c??dents

m??dicaux (m??dicaments, posologie et allergies) de chaque personne

```
GET /fire?address=1509 Culver St
RESPONSE: 200 OK
```

### Flood alert

Cette url doit retourner une liste de tous les foyers desservis par la caserne. Cette liste doit regrouper les

personnes par adresse. Elle doit aussi inclure le nom, le num??ro de t??l??phone et l'??ge des habitants, et

faire figurer leurs ant??c??dents m??dicaux (m??dicaments, posologie et allergies) ?? c??t?? de chaque nom

```
GET /flood/stations?stations=1,2,3
RESPONSE: 200 OK
```

### Person alert

Cette url doit retourner le nom, l'adresse, l'??ge, l'adresse mail et les ant??c??dents m??dicaux (m??dicaments,

posologie, allergies) de chaque habitant. Si plusieurs personnes portent le m??me nom, elles doivent

toutes appara??tre

```
GET /personInfo?firstName=John&lastName=Boyd
RESPONSE: 200 OK
```

### Community emails

Cette url doit retourner les adresses mail de tous les habitants de la ville.

```
GET /communityEmail?city=Culver
RESPONSE: 200 OK
```
