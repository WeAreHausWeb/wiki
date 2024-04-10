# Utveckling av siter
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
> Nedan refereras `hausXX` som ssh användarnamnet för servern och `~/Sites/xxx.se` är din lokala Sites mapp med det namn du vill att siten ska få lokalt. Användarnamnet hittas i [WHM](https://haus-srv01.oderland.com:2087/cpsess4391671936/scripts4/listaccts)
- Skapa din lokala mapp och ställ dig in den.
 ```
 mkdir ~/Sites/xxx.se && cd ~/Sites/xxx.se
  ```
- Skapa en lokal databas
- Synka ner alla filer från servern:
  ```
  rsync -a hausXX@srv01:public_html/ ./ --exclude={'.well-known','.wp-cli','cgi-bin'}
  ```
- Ta bort objekt cache, används ej lokalt:
  ```
  rm rf ./wp-content/object-cache.php
  ```
- Ändra till rätt databasuppgifter i `wp-config.php`
- Hämta databasen från servern samt importera den i ditt projekt:
  ```
  ssh hausXX@srv01 "cd www && wp db export db.sql"
  scp hausXX@srv01:www/db.sql ./
  ssh hausXX@srv01 "cd www && rm db.sql"

  wp db import db.sql
  wp search-replace "serverUrl.se" "lokalUrl.se"
  ```

----

### Egen kod
När du ska skapa något eget så gör vi det via ett site specifikt plugin. Addera alla dina funktioner och widgets i denna plugin.
- De flesta siterna har redan ett github repo, även de som inte har någon custom kod sedan innan. [Identifiera](https://github.com/orgs/WeAreHausWeb/repositories) om det finns ett repo för siten först och använd isof det som repo för din kod.
  
- För att hämta ett nytt site specifikt plugin (`site-widgets`), hämta den färdiga plugin mallen in till ditt lokala projekt.
```
wget https://github.com/WeAreHausWeb/site-widgets/archive/refs/heads/master.zip -P ./wp-content/plugins/ && unzip ./wp-content/plugins/master.zip -d ./wp-content/plugins/ && mv ./wp-content/plugins/site-widgets-master ./wp-content/plugins/site-widgets  && rm ./wp-content/plugins/master.zip
```
- Sätt upp git i mappen och koppla ihop ditt plugin med det repo som troligtvis finns enligt ovan.
- Läs [README.md](https://github.com/WeAreHausWeb/site-widgets/blob/master/README.md) för info kring utveckling samt deploy.

 
