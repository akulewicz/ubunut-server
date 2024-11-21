# Konfiguracja serwera MariaDB

## Instalacja MariaDB

Aby rozpocząć pracę należy zainstalować pakiet mariadb-server:

```shell
sudo apt install mariadb-server
```

Następnie dodajemy zabezpieczenia:

```shell
sudo mysql_secure_installation
```

Aby uzyskać dostęp do powłoki MariaDB możemy użyć poleceń:

```shell
sudo mariadb
```

lub

```shell
mariadb -u root -p
```
## Zarządzanie bazami danych MariaDB

Na początku tworzymy noweg użytkownika administracyjnego:

```shell
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
```

Tworzy użytkownika o nazwie admin, który może się logować tylko z hosta localhost (czyli z maszyny, na której działa baza danych).
'admin' – Nazwa użytkownika.
'localhost' – Ogranicza dostęp tego użytkownika tylko do lokalnego połączenia.
```shell
IDENTIFIED BY 'password
```
Określa hasło użytkownika. W tym przypadku hasło to passwor.
Uwaga: W MariaDB/MySQL hasło jest automatycznie haszowane i przechowywane w bezpiecznej formie w systemie.

```shell
FLUSH PRIVILEGES
```
odświeża pamięć podręczną serwera dotyczącą uprawnień użytkowników.

W kolejnym kroku nadajemy uprawnienia adminowi za pomocą polecenia GRANT. Oznacza, *.* oznacza, że uprawnienia dotyczą wszystkich baz danych (*) i wszystkich tabel (*) na serwerze.:

```shell
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost';
FLUSH PRIVILEGES;
```
Teraz możemy zalogować się na nowego użytkownika:

```shell
mariadb -u admin -p
```
Tworzmy konto użytkownika mającego prawa tylko do odczytu:

```shell
GRANT SELECT ON *.* TO 'readonlyuser'@'localhost' IDENTIFIED BY 'password';
```

Tworzymy bazę danych:

```shell
CREATE DATABASE sampledb;
```

Tworzymy użytkownika, nadajemy jemu hasło i uprawnienia:

```shell
GRANT ALL ON sampledb.* TO 'appuser'@'localhost' IDENTIFIED BY 'password';
```

Wyświetlamy zestaw uprawnień dla użytkownika:

```shell
SHOW GRANTS FOR 'appuser'@'localhost';
```

## Tworzenie kopii zapasowej

Kopię zapasową można zrobić za pomocą polecenia `mysqldump`. Polecenie wydajemy w standardowej powłoce Linuksa

`mysqldump -u admin -p --databases sampledb > sampledb.sql`

Przywracanie kopii bazy:

`sudo mariadb -u admin -p < sampledb.sql`

### Configuration

It is recommended that this package should only be used on a local environment for security reasons. You should install it via composer using the --dev option like this:

```shell
composer require reliese/laravel --dev
```

