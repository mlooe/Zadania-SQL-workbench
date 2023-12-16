# lab 08

## Zadanie 1

```sql

#pkt 1

# Stworzenie tabel

create table uczestnicy like wikingowie.uczestnicy;
create table etapy_wyprawy like wikingowie.etapy_wyprawy;
create table sektor like wikingowie.sektor;
create table wyprawa like wikingowie.wyprawa;

# Dodanie rekordów
insert into uczestnicy select * from wikingowie.uczestnicy;
insert into etapy_wyprawy select * from wikingowie.etapy_wyprawy;
insert into sektor select * from wikingowie.sektor;
insert into wyprawa select * from wikingowie.wyprawa;

# lub --------------------

create table uczestnicy as select * from wikingowie.uczestnicy;
create table etapy_wyprawy as select * from wikingowie.etapy_wyprawy;
create table sektor as select * from wikingowie.sektor;
create table wyprawa as select * from wikingowie.wyprawa;

#pkt 2

#Podpowiedź: left join lub podzapytanie
select k.nazwa from kreatura k left join uczestnicy u on u.id_uczestnika=k.idKreatury where id_uczestnika is null;

#pkt 3

#inner join
select w.nazwa, sum(k.udzwig) from uczestnicy u
inner join wyprawa w on u.id_wyprawy=w.id_wyprawy
inner join kreatura k on u.id_uczestnika=k.idKreatury group by w.nazwa;
```

## Zadanie 2

```sql
#pkt 1

select w.nazwa, count(k.idKreatury) as 'liczbaUczestnikow', group_concat(k.nazwa) as 'uczestnicy' from uczestnicy u
inner join wyprawa w on u.id_wyprawy=w.id_wyprawy
inner join kreatura k on u.id_uczestnika=k.idKreatury group by w.nazwa;

#pkt 2

select ew.idwyprawy, ew.dziennik, s.nazwa as 'nazwa_sektora', k.nazwa as 'nazwa_kierownika' from etapy_wyprawy ew
inner join sektor s on ew.sektor=s.id_sektora
inner join wyprawa w on ew.idwyprawy=w.id_wyprawy
inner join kreatura k on w.kierownik=k.idKreatury order by w.data_rozpoczecia asc, ew.kolejnosc asc;

# -------- Nie będzie 5.2 z lab 07 na kolokwium --------------------



