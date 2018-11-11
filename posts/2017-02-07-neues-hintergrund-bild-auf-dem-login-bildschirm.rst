.. title: Abwechselnde Hintergrund-Bilder beim Linux-Login
.. slug: neues-hintergrund-bild-auf-dem-login-bildschirm
.. date: 2017-02-07 22:45:10 UTC+01:00
.. tags: Software, Linux, Freizeit
.. category: Software
.. link: 
.. description: 
.. type: text

Auf der Festplatte schlummern bei uns mittlerweile so viele Bilder, aber
leider finden wir nie Zeit, diese auch mal anzuschauen. Daher habe ich
mir irgendwann überlegt, beim Starten von Linux jedes mal ein neues,
zufälliges Login-Hintergrundbild anzeigen zu lassen. Wie das bei der
Verwendung von LightDM geht, habe ich hier aufgeschrieben.

.. TEASER_END

Zunächst benötigen wir das Skript RandomPicture.sh, welches in ``/usr/local/bin/`` liegt und ausführbar sein muss. Bitte die Variablen selbst anpassen: Der Bilderordner und das Alternativbild sollte vorhanden sein::

    ---
    #!/bin/bash
    #
    # Dieses Skript waehlt aus dem Verzeichnis $BILDERORDNER ein zufaelliges
    # Bild aus und gibt seinen Namen auf der Konsole aus. Ist im
    # BILDERORDNER keine Datei vorhanden, wird $ALTERNATIVBILD
    # ausgegeben.
    
    BILDERORDNER="/usr/local/share/pictures"
    ALTERNATIVBILD="/usr/share/backgrounds/lightdm/lightdm-gtk-greeter.jpg"
    
    NUMFILES=$( ls -1 $BILDERORDNER | wc -l )
    
    if [ $NUMFILES -ne 0 ]
    then
        NUM=$[ ( $RANDOM % $NUMFILES ) + 1 ]
        FNAME=$(ls -1 $BILDERORDNER | head -n ${NUM} | tail -n 1)
        echo $BILDERORDNER/$FNAME
        else
        echo $ALTERNATIVBILD
    fi
    
    exit 0

Dieses Skript macht nichts anderes, als den vollen Pfad zu einem
zufällig gewählten Bild im Bilderordner auf der Konsole auszugeben.

Jetzt habe ich mir noch das Skript ``/usr/local/bin/lightdm-init.sh``
gebaut, welches auf dieses Bild einen Softlink im Verzeichnis ``/tmp``
ablegt::

    #!/bin/sh
    
    if [ ! -f /tmp/RandomPicture.jpg ] # Falls /tmp bei jedem Boot geleert wird
    then
        ln -s "$(/usr/local/bin/RandomPicture.sh)" /tmp/RandomPicture.jpg
    fi
    exit 0


``/tmp/RandomPicture.jpg`` soll als Hintergrund im light-dm-greeter angezeigt werden. Daher braucht man in ``/etc/lightdm/lightdm-gtk-greeter.conf`` die Einstellung::

    background=/tmp/RandomPicture.jpg


Schließlich: Bei jedem Start von lightdm muss ``/usr/local/bin/lightdm-init.sh`` ausgeführt werden, um ``/tmp/RandomPicture.jpg`` neu zu erstellen. Diese Einstellung nimmt man in ``/etc/lightdm/lightdm.conf`` vor::

    [Seat:*]
    greeter-setup-script=/usr/local/bin/lightdm-init.sh

Beim nächsten Start von Linux und LightDM müsste jetzt ein zufällig
ausgewähltes Hintergrundbild auf dem Login-Bildschirm erscheinen.
