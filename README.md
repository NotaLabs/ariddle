Ein Rätsel
=======

##Intro

###Was du bekommen hast

Wenn alles richtig gelaufen ist und sich durch geänderte Naturgesetze nicht spontan eine der drei Tüten mitsamt Inhalt aufgelöst hast solltest du jetzt folgende Dinge vor dir liegen haben.

* Ein Rotes Steckboard mit 170 Kontakten
* 4 grüne Kabel mit Steckverbindern am Ende
* Schwarzes Styropor in dem mehrere kleine ICs mit _8 Beinen_ ( :smile: ) stecken (Du hast mehrere falls du bei einem ausversehen die original Datenstruktur zerstörst. Also bewahre dir einen so lange auf bis du dir 99% sicher bist dass dein Programm funktioniert (Mehr dazu weiter unten)).
* (Außerdem brauchst du noch den RasberryPi um die "Herausforderung" zu lösen)


###Was du machen musst

Mit hilfe des Steckboards und den Kabel musst du einen der ICs mit dem RasberryPi verbinden und die Daten darauf auslesen.

###Wie auslesen, was IC ?

_Fachsimpel Modus an_

IC bedeutet Integrated Circuit (Integrierter Schaltkreis), die schwarzen Kästchen sind ICs um Daten zu speichern (so genannte EEPROMs). Um auf diese zuzugreifen benutzt der Rasberry Pi das I2C Protokoll. Dieses dient zur Übertragung von Daten zwischen dem EEPROM und dem Rasberry Pi, es funktioniert auf Byte-Ebene, was bedeutet das du bestimmte Bytes an das Gerät sendest und auch bestimmte Bytes zurückbekommst. 

(Das ist auch das was du machen musst um an dein extra Geschenk zu kommen  :smile: ).

**Kannst du auch googlen falls meine Erklärung zu schlecht ist. (Google nach I2C / EEPROM )**

_Fachsimpel Modus aus_


###Die Konkrete Aufgabe

####Anschließen

Erstmal musst du den EEPROM an den Rasberry Pi anschließen, dafür nimm dir einfach folgende erstmal verwirrende Erklärung zur Hand. Um die Erklärung anzuwenden musst du die beiden Anschlusserkärungen des Pis und des EEPROMs lesen (unten verlinkt).

| EEPROM  | RasberryPi |
| ------------- | ------------- |
| Vcc  | 3.3V **(Nicht mit 5V verbinden der EEPROM überlebt das, der Pi wahrscheinlich nicht)**  |
| Vss  | GND  |
| SCL  | SCL  |
| SDA  | SDA  |

EEPROM Anschluss Namen:
https://cdn-reichelt.de/documents/datenblatt/A300/ST24C32_ST24C64%23STM.pdf

Darstellung 2A auf Seite 2, beachte die halbkreisförmige Markierung die sich auch an den EEPROMs die du hast wiederfinden lässt um die Pins richtig zu identifizieren.

RasberryPi GPIO Namen:
http://developer-blog.net/wp-content/uploads/2013/09/raspberry-pi-rev2-gpio-pinout.jpg

Hier ein Bild wie es bei mir aussah damit du ungefähr sicher gehen kannst.
https://onedrive.live.com/redir?resid=5A49630B8FC52EAA!13597&authkey=!AIJAytvU520nYQ4&v=3&ithint=photo%2cjpg

####Testen 

Für Grundlegende Funktionen des Übertragungs Protokolls zwischen Pi und EEPROM gibt es die i2c-tools für Linux, folge einfach dieser Anleitung zum installieren und konfigurieren.

http://www.netzmafia.de/skripten/hardware/RasPi/RasPi_I2C.html

Jetzt kannst du mit `sudo i2cdump 1 0x50 i` dir einen Ausschnitt des EEPROM Speichers anzeigen lassen. Du kannst zwar schon erkennen um welches Dateiformat es sich handelt, merkst aber bei genauerer Betrachtung das die angezeigten Daten nicht komplett logisch zusammen hängen.

##Das Rätsel

So jetzt zum echten Rätsel, deine Aufgabe ist es ein Programm zu schreiben welches die Daten per I2C wieder aus dem Speicher des EEPROMS ausliest, und sie in eine Datei einzufügen um am Ende eine grafische Darstellung zu erhalten die dich dann zu deinem Geschenk leitet (yay) !

Die normalen I2C Tools funktionieren nicht da der Bytecode zum lesen mehr Bytes lang ist als der für "normale" EEPROMS

Wenn du Python benutzen willst benutz die Quick2Wire Bibliothek die hat bei mir funktioniert. Wenn du C benutzen willst ist das dein Problem :smile: .

Um Daten auszulesen musst du eine bestimmte Byte Reihe an den EEPROM senden.
Was du genau senden musst findest du im Datenblatt des EEPROMS auf Seite ) Figure 8. Random Adress Read

https://cdn-reichelt.de/documents/datenblatt/A300/ST24C32_ST24C64%23STM.pdf

DevSel ist die ByteAdresse des EEPROMS die du vorher schon ausgelsen hast 0x50 wahrscheinlich, Byte Addr ist die 2 Byte lange Adresse des Speichers. 0x00 0x00 ist die erste Adresse im Speicher und  0x20 0xff. 

Ich habe beim schreiben meiner Datei auf den EEPROM 0x00 0x00 angefangen und dann erst den 2 Adressbyte um einen erhöht (0x00 0x01 ...) und wenn ich bei 0x00 0xff angelangt bin den ersten Adressbyte um einen erhöht und den 2 wieder auf 0x00 gesetzt (0x01 0x00)

#####Viel Erfolg 

*Falls etwas unklar ist (was es wahrscheinlich sein wird :smile: ) Google oder frag mich bei WhatsApp


##Linux Tipps

`sudo` um etwas mit Administratorrechten auszuführen

`nanon dateinemamitendung` öffnet die datei dateinamemitendung in einem TextEditor

`python3 scriptname.py`  führt das Python-Skrip scriptname.py  mit Python3 aus (zwingend notwendig für die Quick2Wire Bibliothek

`make` falls du python und quick2wire benutzt musst du die Bibliothek erst von GitHub downloaden und kompilieren, dafür ist der Befehl make notwendig, konsultiere aber die Anleitungen die sich in den jeweiligen GitHub Distors finden lassen für ausführlichere Erklärungen.

##FAQ

**Kannst du mir nicht einfach so mein Geschenk geben ?**

Nö !

**Ich versteh nichts und fühle mich überschätzt !**

Wenn ich das schaffe schaffst du das auch.

**Das konnte ich total schnell lösen, ich fühle mich unterschätzt !**

Neues Jahr neues Glück (mir fällt bestimmt noch eine "tolle" Idee ein.

**Deine FAQ ist demotivierend und nicht wirklich hilfreich**

http://31.media.tumblr.com/26b3dab1f4d1eecc4d6cfd59b300774f/tumblr_mhlk2cHyHz1s3jolto1_500.gif

**Ich kann mich nicht motivieren !**

Motivation bringt dich auch nie so weit wie Ehrgeiz.

**Ja toll ich hab auch kein Ehrgeiz**

Aber Geiz ist doch ...

**Kannst du dir das scheiß Wortspiel nicht sparen und mir eine kleine Hilfe geben ?**

Na okay aber nur gucken wenn du wirklich nicht weiter weißt:

http://pastebin.com/zN3WZr6V

**Junge kennst du Grammatik/Rechtschreibung/Zeichensetzung ?**

Löse erst die Herausforderung und du bekommst für jeden Rechtschreibfehler den du findest 10 Cent (Oder was anderes falls du durch das Rätsel deinen Enthusiasmus für Embbed Development entdeckt hast)  :smile: .




