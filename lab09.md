# lab 09

## Zadanie 1

```sql
show triggers;

DELIMITER //
CREATE TRIGGER kreatura_before_insert
BEFORE INSERT ON kreatura
FOR EACH ROW
BEGIN
  IF NEW.waga <= 0
  THEN
    SET NEW.waga = 1;
  END IF;
END
//
DELIMITER ;

alter table kreatura modify waga int default 0;
drop trigger kreatura_before_insert;

```
## Zadanie 2

```sql
create table archiwum_wyprawy like wyprawa;

create trigger wyprawa_before_delete


DELIMITER //
CREATE TRIGGER wyprawa_before_delete
BEFORE delete ON wyprawa
FOR EACH ROW
BEGIN
  insert into archiwum_wypraw
  select w.id_wyprawy, w.nazwa, w.data_rozpoczecia, w.data_zakonczenia, k.nazwa 
  from wyprawa w inner join kreatura k on w.kierownik=k.idKreatury where id_wyprawy=old.id_wyprawy;
END
//
DELIMITER ;
```

## Zadanie 3

```sql

#pkt 1
DELIMITER $$
CREATE PROCEDURE eliksir_sily(IN id int)
BEGIN
update kreatura set udzwig = udzwig * 1.2 where idKreatury = id;
END
$$
DELIMITER ;


call eliksir_sily(1);

#pkt 2
DELIMITER $$
CREATE PROCEDURE wielkie_litery(IN tekst varchar(100))
BEGIN
select upper(tekst);
END
$$
DELIMITER ;

call wielkie_litery('Abrakadabra, abrakadabra');
```

## Zadanie 4

```sql

#pkt 1
create table system_alarmowy(id_alarmu int, wiadomosc varchar(100));

#pkt 2


```

## Zadanie 5

```sql

#pkt 1
dokończyć


#pkt 2
dokończyć

```


