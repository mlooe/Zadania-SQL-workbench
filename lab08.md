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
```

## Zadanie 3

```sql
#pkt 1
select s.nazwa, ifnull(count(ew.sektor),0) from sektor s
left join etapy_wyprawy ew on s.id_sektora = ew.sektor
group by s.nazwa;                 # nie działa kiedy group by s.id_sektora ??

#pkt 2
select k.nazwa, if (count(u.id_uczestnika) > 0, 'brał udział w wyprawie', 'nie brał udziału w wyprawie') as czy_bral_udzial from kreatura k 
left join uczestnicy u on u.id_uczestnika = k.idKreatury
group by k.nazwa;        # znowu group by k.rodzaj nie działa ??
```

## Zadanie 4

```sql
#pkt 1
select w.nazwa, sum(length(dziennik)) from etapy_wyprawy ew 
inner join wyprawa w on ew.idWyprawy = w.id_wyprawy 
group by w.nazwa having sum(length(dziennik)) < 400;

#pkt 2
select w.id_wyprawy, w.nazwa, avg(sum(waga * ilosc)) as 'średnia waga zasobów' from wyprawa w 
inner join zasob z on w.id_wyprawy = z.idZasobu;

select u.id_wyprawy, w.nazwa, sum(e.ilosc * z.waga) / count(distinct(u.id_uczestnika)) as 'średnia waga zasobów' from uczestnicy u 
inner join ekwipunek e on u.id_uczestnika = e.idKreatury
inner join zasob z on z.idZasobu = e.idZasobu 
inner join wyprawa w on w.id_wyprawy = u.id_wyprawy
group by u.id_wyprawy, w.nazwa;
```

## Zadanie 5

```sql
#pkt 1
select w.id_wyprawy, s.nazwa, k.nazwa, datediff(w.data_rozpoczecia, k.dataur) as 'data rozpoczecia w dniach'
from kreatura k inner join uczestnicy u on u.id_uczestnika=k.idKreatury
inner join wyprawa w on w.id_wyprawy=u.id_wyprawy 
inner join etapy_wyprawy ew on ew.idwyprawy=w.id_wyprawy
inner join sektor s on s.id_sektora=ew.sektor where ew.sektor=7;

```
