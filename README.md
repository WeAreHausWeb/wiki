# Synka siter för utveckling
#
För att börja jobba på en wp site som satts upp m.h.a. [Sitemanager](https://sitemanager.haus.se) behöver du synka ner siten först.
Eftersom vi inte längre använder något deployscript eller composer så finns här lite bra kommandon för att synka nergrejer från servern.


***Förutsättningar:***
- Du har satt upp din ssh config för att kunna köra ex: srv01 som ett server alias.
- Du har service ssh nyckelparet in din ssh mapp.

----

### Sätt upp lokal miljö
- Skapa en lokal databas
- Synka ner hela siten från servern:
  
```
  rsync -a hausXX@srv01:public_html/ ~/Sites/xxx.se
  ```
Där `hausXX` är ssh användarnamnet och `~/Sites/xxx.se` är din lokala Sites mapp med det namn du vill att siten ska få lokalt.

----

### Addera egen kod
När du ska skapa något eget så gör vi det via ett site specifikt plugin. Addera alla dina funktioner och widgets i denna pluggin.
- För att skapa ett site specifikt plugin (`webien-site-widgets`), hämta den färdiga plugin mallen.
```
wget https://github.com/WeAreHausWeb/webien-site-widgets/archive/refs/heads/master.zip -P ~/Sites/xxx.se/wp-content/plugins/
```
 
