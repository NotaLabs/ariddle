Ein Rätsel
=======

###Was du bekommen hast

Wenn alles richtig gelaufen ist und sich durch geänderte Naturgesetze nicht spontan eine der drei Tüten mitsamt Inhalt aufgelöst hast solltest du jetzt folgende Dinge vor dir liegen haben.

* Ein Rotes Steckboard mit 170 Kontakten
* 4 grüne Kabel mit Steckverbindern am Ende
* Schwarzes Styropor in dem mehrere kleine ICs mit _8 Beinen_ ( :smile: ) stecken 
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

Erstmal musst du den EEPROM an den Rasberry Pi anschließen, dafür nimm dir einfach folgende erstmal verwirrende Erklärung zur Hand.

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


