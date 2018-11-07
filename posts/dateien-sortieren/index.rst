.. title: Dateien sortieren
.. slug: dateien-sortieren
.. date: 2016-01-19 21:10:31 UTC+01:00
.. tags: Software, Linux
.. category: Software
.. link: 
.. description: 
.. type: text

**Dateien nach Name sortieren in Thunar und Gnome Commander**

Wenn man im Dateimanager Thunar oder Gnome Commander Dateien nach Namen
sortiert, dann werden zusammenstehende Ziffern als ganze Zahlen
interpretiert. Dies führt in einem Beispiel zu folgender
Sortierreihenfolge:

- datei1
- datei5
- datei10

Würden die Dateimanager die Ziffern nicht zu einer Zahl zusammenfassen,
so erhielte man folgendes Ergebnis:

- datei1
- datei10
- datei5

Beispielsweise verhält sich der Midnight Commander (mc) so.

Diesen Umstand sollte man bedenken, wenn man Dateien sortieren möchte,
bei denen Ziffern am Anfang des Dateinamens stehen, wie im unten
angehängten Bild dargestellt:

.. image:: /images/2016-01-19-Thunar.png
