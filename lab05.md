# lab 05

## Zadanie 1

```sql
#pkt 1

delete from postac where wiek = 403;
delete from postac where wiek = 389;

# lub

delete * from postac where nazwa <> 'Bjorn' and rodzaj = 'wiking' order by data_ur asc limit 2;



#pkt 2

#usunięcie kluczy obcych

alter table walizka drop foreign key walizka_ibfk_1;

alter table przetwory drop foreign key przetwory_ibfk_1;

alter table przetwory drop foreign key przetwory_ibfk_2;

#usunięcie klucza głównego

alter table postac change id_postaci id_postaci int; alter table postac modify id_postaci int;
```

## Zadanie 2

```sql
#pkt 1

# varchar(n) vs. char(n)
# varchar -> max n znaków
# char -> dokładnie n znaków

alter table postac add column pesel char(11)first;

update postac set pesel='64536748732' + id_postaci;

#pkt 2
alter table postac modify rodzaj enum('wiking','ptak','kobieta','syrena');

# select 5;
# select 5 + 3;
# select now();
# select '6' + 5; (daje nam to 11)

#pkt 3
#mamy unikalne wartości w kolumnie pesel, możemy więc dodać klucz główny
alter table postac add primary key(pesel);

insert into postac values(11234567812,10,'Gertruda Nieszczera','syrena','1888-09-21',165,NULL,NULL);
```

## Zadanie 3

```sql
select nazwa from postac where nazwa like'%a%';
# % - dowolny ciąg znaków
# _ - dokładnie jeden znak
# regexp - szukanie fragmentu dokumentacji
# ^ - zaczyna się od
# $ - kończy się na
# [] - znak dopuszczalnych zbiorów
# {} - limit znaków, ile ma szukać 
# select nazwa from postac where nazwa regexp '[0-9]{2}-[0-9]{3}';   (np. szukanie polskich kodów pocztowych)


#pkt 1
update postac set jaki_Statek = 'SS Stone' where nazwa like '%a%'; 

#pkt 2
update statek set data_wodowania = '1950-01-01' where nazwa_statku = 'Galileo';

select * from statek where data_wodowania >= '1901-01-01' and data_wodowania <= '2000-12-31';

update statek set max_ladownosc = max_ladownosc * 0.7 where year(data_wodowania) between 1901 and 2000; # statek mial 100 teraz ma 70

#pkt 3
#check(warunek) - sprawdza nam warunek poprawności danych dla kolumny lub kolumn danej tabeli

alter table postac add check(wiek < 1000);

update postac set wiek = 2000 where nazwa = 'Bjorn'; # nie możemy dodać bo wiek musi być mniejszy niż 1000
```

## Zadanie 4

```sql
#pkt 1
# krok 1 - dodanie nowej wartości 'wąż' w definicji
# krok 2 - dodajemy węża loko
alter table postac modify rodzaj enum('wiking','ptak','kobieta', 'syrena', 'waz');
insert into postac values ('64536748732', 8, 'Loko', 'waz', '2000-03-05', 23, null, null);

#pkt 2

create table marynarz like postac;
insert into marynarz select * from postac where jaki_Statek is not null;
# opcjonalnie create table marynarz2 as select * from postac where statek is not null; (nie kopiuje primary key, foreign key, etc.)

#pkt 3

show create table marynarz;
alter table marynarz add foreign key (jaki_Statek) references statek(nazwa_Statku);
alter table marynarz add check(wiek < 1000);
select * from marynarz;
```

## Zadanie 5

```sql
#pkt 1
update postac set jaki_Statek = null;

#pkt 2
delete from postac where nazwa = 'Andrzej';

#pkt 3
alter table marynarz drop foreign key marynarz_ibfk_1;
delete from statek;

#pkt 4
alter table postac drop foreign key postac_ibfk_1;
drop table statek;

#pkt 5
create table zwierz(
id_zwierza int primary key auto_increment,
nazwa varchar(30),
wiek int
);

#pkt 6
insert into zwierz select id,postaci, nazwa, wiek from postac where rodzaj = 'ptak' or rodzaj = 'wąż';
select * from zwierz;
```




