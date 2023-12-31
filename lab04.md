# lab 04
## Zadanie 1
```sql
#pkt 1
create table postac(
id_postaci int auto_increment primary key, 
nazwa varchar(40), 
rodzaj enum('wiking', 'ptak', 'kobieta'), 
data_ur date, 
wiek int unsigned
);

#pkt 2
insert into postac values(default,'Bjorn','wiking','1659-07-10', 364);
insert into postac values(default,'Tesciowa','kobieta','1970-05-03', 53);
insert into postac values(default,'Drozd','kobieta','2021-11-05', 2);

#pkt 3
update postac set wiek = 88 where id_postaci = 2;

```
## Zadanie 2
```sql
#pkt 1
create table walizka(id_walizki int primary key auto_increment, 
pojemnosc int unsigned, 
kolor enum('rozowy', 'czerwony', 'teczowy', 'zolty'), 
id_wlasciciela int, 
foreign key(id_wlasciciela) references postac(id_postaci) on delete cascade
);

#pkt 2
alter table walizka alter kolor set default 'rozowy';

describe walizka;
+----------------+---------------------------------------------+------+-----+---------+----------------+
| Field          | Type                                        | Null | Key | Default | Extra          |
+----------------+---------------------------------------------+------+-----+---------+----------------+
| id_walizki     | int                                         | NO   | PRI | NULL    | auto_increment |
| pojemnosc      | int unsigned                                | YES  |     | NULL    |                |
| kolor          | enum('rozowy','czerwony','teczowy','zolty') | YES  |     | rozowy  |                |
| id_wlasciciela | int                                         | YES  | MUL | NULL    |                |
+----------------+---------------------------------------------+------+-----+---------+----------------+

#pkt 3
insert into walizka values(default, 100, default, 1);
insert into walizka values(default, 50, default, 2);

select * from walizka;

```
## Zadanie 3
```sql

#pkt 1
create table izba(
adres_budynku varchar(50) not null,
nazwa_izby varchar(50) not null,
metraz mediumint unsigned,
wlasciciel int,
primary key(adres_budynku, nazwa_izby),
foreign key (wlasciciel) 
references postac(id_postaci) on delete set null

);
# on delete or on update -> restrict|set null|cascade

#pkt 2
alter table izba add column kolor varchar(30) default 'czarny' after metraz;
describe izba;

#pkt 3
insert into izba values ('Kolejowa 23' , 'spiżarnia', 10, default, 1);
select * from izba;

# dodac klucz główny do tabeli izba
alter table izba add primary key(adres_budynku, nazwa_izby);

```
## Zadanie 4
```sql
#pkt 1
create table przetwory(
id_przetworu int not null auto_increment primary key,
rok_produkcji smallint default '1654',
id_wykonawcy int, foreign key(id_wykonawcy) references postac(id_postaci),
zawartosc varchar(100),
dodatek varchar(100) default 'papryczka chilli',
id_konsumenta int, foreign key(id_konsumenta) references postac(id_postaci)
);

#pkt 2
insert into przetwory values (default, default, 1, 'bigos', default, 3);
select * from przetwory;

```
## Zadanie 5

```sql

#pkt 1
insert into postac values (default, 'Andrzej', 'wiking', '1649-09-06', 349);
insert into postac values (default, 'Maciej', 'wiking', '1680-05-23', 359);
insert into postac values (default, 'Kuba', 'wiking', '1656-04-13', 389);
insert into postac values (default, 'Tomasz', 'wiking', '1634-10-16', 403);
insert into postac values (default, 'Michał', 'wiking', '1644-12-01', 379);


#pkt 2
create table statek(
nazwa_statku varchar(60) not null primary key,
rodzaj_statku enum('Karli', 'Knara', 'Drakkar', 'Byrding'),
data_wodowania date not null,
max_ladownosc int unsigned
);

#pkt 3
insert into statek values ('Galileo', 'Drakkar', '1682-05-12', 100);
insert into statek values ('Kanaris', 'Byrding', '1662-03-20', 70);

#pkt 4
alter table postac add column funkcja varchar(30);

#pkt 5
update postac set funkcja = 'Kapitan' where id_postaci = 1;

#pkt 6
alter table postac add foreign key(statek) references statek(nazwa_statku);

#pkt 7
alter table postac add jaki_Statek varchar(100);
alter table postac add foreign key (jaki_Statek) references statek(nazwa_statku);

update postac set jaki_Statek = 'Galileo' where rodzaj = 'wiking';

update postac set jaki_Statek = 'Kanaris' where rodzaj = 'Drozd';

#pkt 8
delete from izba where nazwa_izby = 'spiżarnia';

#pkt 9 
drop table izba;

```
