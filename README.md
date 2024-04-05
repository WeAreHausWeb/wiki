# Synka siter för utveckling
#
För att börja jobba på en wp site som satts upp m.h.a. [Sitemanager](https://sitemanager.haus.se) behöver du synka ner siten först.
Eftersom vi inte längre använder något deployscript eller composer så finns här lite bra kommandon för att synka nergrejer från servern.


***Förutsättningar:***
- Du har satt upp din ssh config för att kunna köra ex: srv01 som ett server alias.
- Du har service ssh nyckelparet in din ssh mapp.
- [WP CLI](https://formulae.brew.sh/formula/wp-cli) finns installerat på din dator

----

### Sätt upp lokal miljö
> [!NOTE]
> Nedan refereras `hausXX` som ssh användarnamnet för servern och `~/Sites/xxx.se` är din lokala Sites mapp med det namn du vill att siten ska få lokalt.
- Skapa en lokal databas
- Synka ner alla filer från servern:
  ```
  rsync -a hausXX@srv01:public_html/ ~/Sites/xxx.se
  ```
- Ändra till rätt databasuppgifter i `wp-config.php`
- Hämta databasen från servern samt importera den i ditt projekt:
  ```
  ssh hausXX@srv01:public_html/ "wp db export db.sql"
  scp hausXX@srv01:public_html/db.sql ~/Sites/xxx.se
  ssh hausXX@srv01:public_html/ "rm db.sql"

  cd ~/Sites/xxx.se && wp db import db.sql
  wp db search-replace "serverUrl.se" "lokalUrl.se"
  ```

----

### Egen kod
När du ska skapa något eget så gör vi det via ett site specifikt plugin. Addera alla dina funktioner och widgets i denna plugin.
- För att skapa ett site specifikt plugin (`webien-site-widgets`), hämta den färdiga plugin mallen in till ditt lokala projekt.
```
wget https://github.com/WeAreHausWeb/webien-site-widgets/archive/refs/heads/master.zip -P ~/Sites/xxx.se/wp-content/plugins/
```
 
