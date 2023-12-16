# lab 06

## Zadanie 1
```sql

#pkt 1
create table kreatura as select * from wikingowie.kreatura;
create table zasob as select * from wikingowie.zasob;
create table ekwipunek as select * from wikingowie.ekwipunek;

# lub -------

#dodawanie tabel

create table ekwipunek like wikingowie.ekwipunek;
create table kreatura like wikingowie.kreatura;
create table zasob like wikingowie.zasob;
#dodawanie wartości

insert into ekwipunek select * from wikingowie.ekwipunek;
insert into kreatura select * from wikingowie.kreatura;
insert into zasob select * from wikingowie.zasob;

#pkt 2
select * from zasob;
select * from kreatura;

#pkt 3
select * from zasob where rodzaj = 'jedzenie';

#pkt 4
select idZasobu,ilosc,idKreatury from ekwipunek where idKreatury IN(1,3,5);
```

## Zadanie 2

```sql
#pkt 1
select * from kreatura where not rodzaj = 'wiedzma' and udzwig >= 50;

#pkt 2
select * from zasob where waga between 2 and 5;

#pkt 3
select * from kreatura where nazwa like '%or%' and udzwig between 30 and 50;
```

## Zadanie 3

```sql
#pkt 1
select * from zasob where month(dataPozyskania) between 7 and 8;

#pkt 2
select * from zasob where rodzaj is not null order by waga asc;

#pkt 3
select * from kreatura where dataUr is not null order by dataUr asc limit 5;
```

## Zadanie 4

```sql
#pkt 1
select distinct rodzaj from kreatura;

#pkt 2
select concat(nazwa, 'to id=', idKreatury) from kreatura;  #concat pobiera wartości i scala jest w jedność

#pkt 3
select * from zasob where waga = ilosc * waga and year(dataPozyskania) between 2000 and 2007;
```

## Zadanie 5

```sql
#pkt 1
select * from zasob where waga = waga * 0.7;

#pkt 2
select * from zasob where rodzaj is null;

#pkt 3
select distinct rodzaj from zasob where nazwa like 'Ba%' or nazwa like '%os' order by nazwa asc;
```


