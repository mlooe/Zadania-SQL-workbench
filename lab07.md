# lab 07

## Zadanie 1

```sql
# funkcje agragujące
# avg(), sum(), count(), min(), max()

#pkt 1
select avg(waga) from kreatura where rodzaj = 'wiking';
select sum(waga), count(waga) from kreatura where rodzaj = 'wiking';

#pkt 2
select rodzaj, avg(waga), count(waga) from kreatura group by rodzaj;

#count(*) liczy wszystko razem z Nullem, count(waga) liczy wszystko w grupie waga które nie są nullem

#pkt 3
select rodzaj, avg(2023 - year(dataUr)) as sredni_wiek from kreatura group by rodzaj;
select year(now()) - year(dataUr) as wiek from kreatura;
```

## Zadanie 2

```sql
#pkt 1
select rodzaj, sum(waga * ilosc) as suma_wag from zasob group by rodzaj;

#pkt 2
select nazwa, avg(waga) from zasob where ilosc >= 4 group by nazwa having sum(waga) > 10;

#pkt 3
select rodzaj, count(distinct nazwa) as liczba from zasob where ilosc > 1 group by rodzaj having liczba > 1;

#gdyby nie było konieczności użycia having

select rodzaj, count(distinct nazwa) as liczba from zasob where ilosc > 1 group by rodzaj;
```

## Zadanie 3

```sql
#pkt 1
select nazwa, ilosc from kreatura, ekwipunek where kreatura.idKreatury = ekwipunek.idKreatury;

#lub inner join

select k.nazwa,e.ilosc from kreatura k inner join ekwipunek e on k.idKreatury=e.idKreatury;

#pkt 2
select k.nazwa, e.idZasobu, e.ilosc from kreatura k inner join ekwipunek e
on k.idKreatury = e.idKreatury inner join zasob z on z.idZasobu = e.idZasobu;

#pkt 3
select * from kreatura k left join ekwipunek e on k.idKreatury = e.idKreatury where e.idKreatury is null;

#podzapytanie
select idKreatury from kreatura where idKreatury not in (select distinct idKreatury from ekwipunek where idKreatury is not null);
```

## Zadanie 4

```sql
#pkt 1
select kreatura.nazwa, kreatura.dataUr, zasob.nazwa from kreatura
natural join zasob, ekwipunek where year(dataUr) between 1670 and 1679;    #natural join nie działa po prostu??

select k.nazwa, z.nazwa, k.dataUr from kreatura k inner join ekwipunek e
on k.idKreatury=e.idKreatury inner join zasob z
on z.idZasobu=e.idZasobu having year(k.dataUr) like '167_';

#pkt 2
select * from kreatura k inner join ekwipunek e on k.idKreatury=e.idKreatury inner join zasob z
on z.idZasobu=e.idZasobu where z.rodzaj = 'jedzenie' order by k.nazwa asc limit 5;

#pkt 3

#połączenie tabeli samej ze soba
select concat(k1.nazwa,' ',k2.nazwa) from kreatura k1 inner join kreatura k2 where k1.idKreatury - k2.idKreatury = 5;
```

## Zadanie 5

```sql

# krok 1 - gdzie są potrzebne dane?
# krok 2 - które kolumny mi są potrzebne?
# krok 3 - nakładam warunki (filtry) z WHERE
# krok 4 - grupowanie, agregację, group by, having, limit...

#pkt 1

select k.rodzaj, avg(e.ilosc * z.waga) from kreatura k 
inner join ekwipunek e on k.idKreatury = e.idKreatury 
inner join zasob z on e.idZasobu = z.idZasobu 
where k.rodzaj not in ('malpa', 'waz') group by k.rodzaj having sum(e.ilosc) < 30;

#pkt 2

#1. sposób (podzapytanie):

select a.nazwa, a.rodzaj, a.dataUr from kreatura a,
(select min(dataUr) min, max(dataUr) max from kreatura group by rodzaj) b where b.min = a.dataUr or b.max=a.dataUr;

#2. sposób (select ... union select ....)
# nie wiem jak
```

