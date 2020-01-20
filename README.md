# Adatbázis, távoli elérés és MySQL

## Távoli szerver

Egy web alkalmazás esetén szükség van egy `futtató környezetre`, mely biztosítja a számítási kapacitást, a memóriát, a tárhelyet, ...stb ami ahhoz kell ,hogy az alkalmazás működőképes legyen. Ez a kiszolgáló környezet lehet akár a saját számítógépünk, mely esetben egy `lokális` környezetről beszélünk, lehet azonban egy fizikailag távol, akár a felhőben lévő környezet. Ezt szokás `távoli` szervernek nevezni. 

### AWS

A tesztelendő alkalmazás az `Amazon Web Services` által biztosított felhő alapu környezetben fut. Ez egy virtuális számítógép, mely biztosítja az app számára a folyamatos működéshez szükséges erőforrásokat és a hálózati kapcsolatot. 

## Hálózati alapfogalmak 

Az AWS környezetében futó alkalmazás számára elengedhetetlen a kapcsolat az internettel, hiszen a hálózaton keresztül, a megfelelő IP cím és port szám segítségével tudjuk azt elérni és interakcióba lépni vele. 

### IP cím

Az IP-cím (Internet Protocol-cím) egy egyedi `hálózati azonosító`, amelyet az Internet Protokoll (__IP__) segítségével kommunikáló számítógépek egymás azonosítására használnak. 

- Minden internetre kapcsolódott eszköznek van IP címe
- Egy-egy cím nem kötődik feltétlenül egy géphez (pl. dinamikus IP címek a lakossági internet szolgáltatók esetén)
- IPv4: négy darab, 0-255 közötti, ponttal elválaszott szám. Pl.: `172.16.254.1`

### Port

Számítógépes hálózatok esetében egy port a hálózati kommunikáció egy végpontja, egy `bemeneti-kimeneti kapu` melyen keresztül a adatátvitel történhet.

- A portok logikai konstrukciók, melyek lehetővé teszik, hogy a számítógépen futó alkalmazások a beérkező csomagokból csak a nekik szólót kapják meg
- A portokat számokkal azonosítjuk
- A webalkalmazásokat a szerver általában a 80-as porton teszi elérhetővé.
- Csak a `nyitott` portok érhetőek el hálózati kapcsolaton keresztül. 
- A tesztelendő alkalmazás mögött lévő adatbázis a 3306-os port számon kommunikál, ami főleg biztonsági okok miatt egy`zárt` port, azaz hálózati kapcsolaton keresztül közvetlenül nem elérhető.  

### SSH

Mivel a 3306-os port nem érhető el kívülről, ezért szükség van egy módszerre, amivel `biztonságos csatornán`, távolról be tudunk jelentkezni a szerver környezetbe. Ebben segít az `SSH` (Secure Shell) protokoll, ami egy lokális és egy távoli számítógép közötti biztonságos csatorna kiépítését teszi lehetővé.

### Port forwarding - *__TODO__*

### Windows 10 SSH Client

A 2017-es Fall Creators Update előtt a Windows 10 nem rendelkezett beépített SSH klienssel. Olyan külső programok használatára volt szükség mint például a [PuTTY](https://www.putty.org/)

A frissítés óta azonban szerencsére rendelkezésre áll a beépített kliens, amivel egyszerűen, a parancssorból tudunk SSH kapcsolatot létesíteni, port forwardinggal együtt. Lássuk hogyan:

1. SSH kliens ellenőrzése: Gépház -> Alkalmazások -> Opcionális szolgáltatások
2. Windows PowerShell megnyitása
3. Csatlakozás a távoli számítógépre:

```bash
ssh -i path/to/key/2019augPrivateAWSKey.pem -L 3307:localhost:3306 ubuntu@example.progmasters.hu
```

## MySQL

A MySQL a `relációs adatbázisok`, illetve a bennük tárolt adatok manipulálását lehetővé tévő rendszer. 

### Mi az a relációs adatbázis ? 

Egy relációs adatbázis egymással kapcsolatban álló adatok gyűjtménye, két dimenziós táblákba szervezve. Egy-egy tábla hasonló egy Excel táblázathoz, meghatározott számú, megnevezett oszlopokkal, valamint további szerkezeti elemekkel: _rekord_ , _mező_ , _kulcsok_

#### Elsődleges kulcs

- Az elsődleges kulcs egy olyan mező, amely egyedi módon azonosítja a rekordokat a táblán belül.
- Az elsődleges kulcs táblaszintű épséget biztosít, és segít a táblák összekapcsolásában.
- Az adatbázis minden táblájának kötelező elsődleges kulccsal rendelkeznie.

_Persons_

ID (Elsődleges kulcs) | Name | Age 
--- | --- | ---
1 | Jóska | 23
2 | Pista | 32

#### Idegen kulcs

- Egy tábla olyan mezője, amivel egy másik tábla elsődleges kulcsára hivatkozunk
- Az idegen kulcsok segítségével tudunk táblákat összekpacsolni (__join__)

_Cars_

ID (Elsődleges kulcs) | OwnerID (Idegen kulcs) | Make | Model 
--- | --- | --- | ---
99 | 1 | Ford |Focus
34 | 2 | Tesla | Model S

### Adat típusok

A MySQL számos SQL adat típust támogat. Az alábbi, kategóriák szerinti bontás csak a fontossabb típusokat tartalmazza.

#### NUMERIKUS

- __BOOL, BOOLEAN__: A 0 érték `hamis`, minden nem nulla érték `igaz`.
- __INT, INTEGER__: 32 bites szám érték. Tartomány: `- 2147483648-tól 2147483647-ig`
- __BIGINT__: 64 bites szám érték. Tartomány: `- 9223372036854775808-tól 9223372036854775807-ig`
- __DEC, DECIMAL__: `Törtszámok` tárolására használt typus. Maximum 65 számjegyet képes kezelni, melyből maximum 30 lehet a tizdespont után.

#### DÁTUM ÉS IDŐ

- __DATE__:  Dátum érték `1000-01-01` és `9999-12-31` között.
- __TIME__: Idő érték, __hh:mm:ss[.fraction]__ formátumban. (Törtmásodperceket maximum 6 tizedesjegy pontosságig lehet tárolni.)
  - Az értéktartomány `-838:59:59` és `838:59:59` között mozog, ugyanis az idő érték kifejezhet eltelt időt, vagy idő intervallumot is. 
- __DATETIME__: Dátum és idő értékek együtt, `1000-01-01 00:00:00.000000` és `9999-12-31 23:59:59.999999` között.
  - Új érték bevitelekor a dátum és idő között a szóköz helyett egy T betű is elfogadott: `'2012-12-31T11:30:45'`
 
> __Érdemes tudni__
> - A dátum részt mindíg __év-hónap-nap__ (YYYY-mm-dd) formátumban kell megadni. 
> - Numerikus környezetben a dátum és idő étékek szám értékké konvertálódnak. 

####  String

- __CHAR__: Maximum __255__ karakterből álló `karakterlánc`  Alapbeállításként az `utf8` karakterkészletet használja.
- __VARCHAR__: Szintén `karakterlánc` tárolására alkalmas. Szintén az `utf8` készletet használja. Alapbeállításokkal az __elméleti maximum 21844__ karakter.

> __Hasznos funkciók__
> - LOWER() - a kapott stringet kisbetűssé alakítja
> - UPPER() - a kapott stringet nagybetűssé alakítja
> - CONCAT() - a kapott stingeket összefűzi


### MySQL Workbench

A MySQL Workbench egy grafikus adatbázis tervező és kezelő eszköz. Főbb szolgáltatások: 

- Adatbázis kapcsolat és instance menedzsment
- SQL szerkesztő
- Adat modellezés
- Teljesítmény monitorozás

A parancssorból történő adatbázis manipulációnál kényelmesebb, szofisztikáltabb eszköz. Ezt fogjuk használni.

#### Standalone telepítés [Ezen a linken](https://dev.mysql.com/downloads/workbench/5.2.html)

### SQL nyelv

Az SQL segítségével tudunk az adatbázisba teszt adatokat bevinni, illetve a tárolt adatok alapján lekérdezéseket végezni. Az SQL jelentése `Structured Query Language` azaz strukturált lekérdező nyelv. 

> __Megjegyzés__
> - Napjainban már több különböző SQL _nylevjárás_ létezik, ugyanis a különböző adatbázis kezelők eltérő módon valósították meg a nyelvbe később beépült elemeket. 

Az SQL nyelv parancsait két nagy kategóriára lehet osztani, melyek a `Data Definition Language` (azaz __DDL__), valamint a `Data Manipulation Language` (azaz __DML__).

#### DML

A DML kategóriába tartozó utasítások segítségével tudjuk __maipuláni__ a tárolt adatokat, valamitn __lekérdezéseket__ végrehajtani. 

##### __Manipuláció__

###### __INSERT__

```sql
INSERT INTO table_name (column_1, column_2, column_3, ...)
VALUES (value_1, value_2, value_3, ...); --új rekord beszúrása
```

###### __UPDATE__
```sql
UPDATE table_name SET column_1 = value, column_2 = value
WHERE condition; -- Módosítja a megadott értékeket a megadott
-- táblában, azon rekordokon, amelyekre igaz a WHERE utáni feltétel. Ha nincs condition, akkor minden rekord módosul.

UPDATE Customers
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;
```
###### __DELETE__

```sql
DELETE FROM table_name
WHERE condition; -- Törli azon rekordokoat(sorokat) a táblából, melyekre igaz a WHERE condition. Ha nincs cnodition akkor minden rekordot töröl. 
```


##### __Lekérdezés__

Lekérdezések segítségével tudunk szűrni, rendszerezni, összefüggéseket keresni az adatbázis adatai között. A lekérdező parancsokat a `SELECT` kulcsszóval indítjuk. 

###### __FROM__

```sql
SELECT * FROM table_name; --Egyszerű lekérdezés, visszaadja a megadott tábla összes rekodrját, az összes oszloppal

SELECT column_1, column_2 FROM table_name; -- Visszaadja a megadott tábla összes rekordját, de csak a megadott oszlopnevekhez
-- tartozó adatokkal
```

###### __WHERE__

```sql
SELECT * FROM table_name
WHERE condition AND/OR another_condition...;
```

A `WHERE` kulcsszó segítségével szűkíteni a SELECT által visszaadott rekordok körét. Itt mindíg egy feltételt kell meghatározni, melyet minden egyes sorra megvizsgál az SQL és csak azokkal a rekordokkal tér vissza, ahol igaz a feltétel.
Példa:

```sql
SELECT * FROM Customers
WHERE Country='Mexico'; -- csak azon rekordok térnek vissza, ahol a Country értéke megegyezik a feltételben meghatározottal. 
```

A WHERE feltételben használható [operátorok](https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html)

> __Szöveges tartalom esetén:__
> - A feltétel értékét idézőjelek között kell megadni!
> - __%:__ nulla vagy több karaktert helyettesítő joker karakter (Csak LIKE és NOT LIKE operátorral működik)
> - __-:__ egy karaktert helyettesítő joker karakter (Csak LIKE és NOT LIKE operátorral működik)ű


###### __DISTINCT, ORDER BY, LIMIT, OFFSET__

```sql
SELECT DISTINCT column FROM table_name
WHERE condition
ORDER BY column_name ASC | DESC
LIMIT number OFFSET number
```
- A `DISTINCT` kulcsszó a találtok közül csak az elsőt tartja meg, a további duplikált értékeket elveti.
- Az `ORDER BY` segítségével megjelölhetünk egy (vagy több oszlopot) mely(ek) értékei alapján szeretnék sorbarendezni a találatokat
- A `LIMIT` kulcsszó hatására a lekérdezés a megadott számú találattal tér vissza
- Az `OFFSET` kulcsszó hatására a lekérdezés a megadott sorszámú elemtől kezdi a keresést

###### __JOINS__

A valóságban gyakran előfordul, hogy egy üzleti entitás tulajdonságait le kell bontani több táblára (főleg az adatismétlés elkerülése miatt). Ez pedig azt eredményezi, hogy több tábla adatait összekapcsolni képes lekérdezésekre van szükségünk. Ilyen lekérdezéseket a `JOIN` kulcszó segítségével tudunk írni.

```sql
SELECT column, another_table_column FROM mytable
INNER JOIN another_table ON mytable.id = another_table.id
WHERE condition...;
```

Az `INNER JOIN` kulcsszóval úgy kötünk össze két táblát, hogy a meghatározott oszlopok az üzleti entitás ugyan azon tulajdonságára mutatnak. 

Further reading on [joins](https://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins) 

functions (sum, min, max)

#### DDL

A DDL utastások segítségével tudunk adatbázisokat, valamint táblákat lérehozni, módosítani illetve törölni, azonban mielőtt hozzáfognánk, a `show` parancs segítségével megvizsgálhatjuk a meglévő elemeket is.

##### __SHOW__

```sql
SHOW DATABASES; -- kiírja azon adatbázisok nevét, melyeket jogunk van látni

SHOW TABLES [FROM db_name]; -- kiírja az aktuális adatbázis illetve az abban található táblák nevét

SHOW COLUMNS FROM table_name [FROM db_name]; --  kírja a megjelölt tábla adatmezőire vonatkozó információkat.
```
```
+-------------+----------+------+-----+---------+----------------+
| Field       | Type     | Null | Key | Default | Extra          |
+-------------+----------+------+-----+---------+----------------+
| ID          | int(11)  | NO   | PRI | NULL    | auto_increment |
| Name        | char(35) | NO   |     |         |                |
| District    | char(20) | NO   |     |         |                |
| Population  | int(11)  | NO   |     | 0       |                |
+-------------+----------+------+-----+---------+----------------+
```

> Az aktuálisan aktív adatbázis megváltoztatásához a `USE` parancsot használhatjuk: `USE db_name`


##### __CREATE__

```sql
CREATE DATABASE [IF NOT EXISTS] db_name; -- létrehoz egy adatbázist a megadott névvel (ha nem létezik). Ha vannak a néveben speciális karakterek, vagy számmal kezdődik, akkor idézőjelek között kell a nevet megadni.

CREATE TABLE [IF NOT EXISTS] table_name (
  column_name DATATYPE(argument) [NOT NULL] [OPTIONS],
  other_column_name DATATYPE(argument) [NOT NULL] [OPTIONS],
  ...
  PRIMARY KEY (column name),
  FOREIGN KEY (column_name) REFERENCES Other_table_name(column_name_in_other_table)  
); -- 

-- Example:

CREATE TABLE IF NOT EXISTS Orders (
    OrderID int NOT NULL AUTO_INCREMENT,
    OrderNumber int NOT NULL
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```

##### __ALTER__

```sql
ALTER TABLE table_name ADD (column_name DATATYPE(argument), other_column_name DATATYPE(argument)); -- adatmező hozzáadása 

ALTER TABLE table_name CHANGE original_column_name new_column_name DATATYPE(argument) [OPTIONS]; --adatmező módosítása

ALTER TABLE table_name DROP column_name; -- adatmező eltávolítása

ALTER TABLE talbe_name RENAME TO new_table_name; -- tábla átnevezése
```

##### __DROP__

```sql
DROP DATABASE [IF EXISTS] db_name; --elveti az adatbázis összes tábláját, majd törli az adatbázist

DROP TABLE [IF EXISTS] table_name; -- elveti az adott táblát, minden adatával együtt
```

##### __TRUNCATE__

```sql
TRUNCATE table_name; -- a tábla tartalmát törli, de a táblát nem
```


