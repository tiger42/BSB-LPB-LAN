[Zurück zum Inhaltsverzeichnis](inhaltsverzeichnis.md)  
[Zurück zu Anhang C](anhang_c.md)    
    
---
        
# Anhang D: Hinweise für Nutzer des veralteten Setups Adapter v2 + Arduino Mega 2560

Für Nutzer des veralteten Setups sind im Folgenden einige Fragen und Punkte aufgeführt, die evtl. der Klärung bedürfen oder die es zu beachten gilt. Etwaige weitere Fragen diesbzgl. stelle bitte im entspr. [Thread des FHEM Forums](https://forum.fhem.de/index.php/topic,29762.0.html).  
Bitte habe jedoch Verständnis, dass wir nicht auf Fragen eingehen werden, die sich bspw. darauf beziehen, sich nach der erfolgten Umstellung auf den Adapter v3/v4 jetzt noch einen Adapter v2 zu bauen.  
**PCBs v2 sind nicht mehr verfügbar, Stand der Technik ist die Kombination Adapter v4.x + Due/ESP32.**    
  
---  
  
***Hinweis:  
Es ist möglich, den Adapter v2 durch eine Vollbestückung und kleinere Anpassungen mit einem ESP32 zu verwenden. Auf diese Weise könnte die aktuelle BSB-LAN-Version genutzt werden, ohne auf den aktuellen Adapter wechseln zu müssen. Für weitere Informationen lies bitte das [Kap. 1.3.3](kap01.md#133-esp32-mit-veraltetem-bsb-lan-adapter-v2).***
  
---  
  
- ***Muss ich zwingend auf das neue Setup Adapter v3/v4 + Due wechseln?***  
Nein, wenn du zufrieden mit dem veralteten Setup bist und der Funktionsumfang von BSB-LAN deinen Ansprüchen bisher genügte, 
dann kannst du das alte Setup natürlich weiterhin verwenden.  

- ***Gibt es bzgl. der BSB-LAN-Versionen etwas zu beachten?***  
    Ja. Die letzte 'offiziell'  getestete und empfohlene Version für dein Setup ist die **[Version v0.44](https://github.com/fredlcore/bsb_lan/releases/tag/v0.44)**. Im zip-file befindet sich auch die letzte 'Mega2560-spezifische' Version des Handbuchs (als PDF).    
  
    Aber: Es hat sich bei mehreren Usern gezeigt, dass auch die **[v1.1](https://github.com/fredlcore/bsb_lan/releases/tag/v1.1)** noch ohne große Einschränkungen läuft, aufgrund des Speichermangels des Mega 2560 vermutlich aber schon nicht mehr mit allen verfügbaren Optionen, die BSB-LAN bietet.  
  
    Ab **v2.x** ist es dann definitiv nötig, einzelne Module zu deaktivieren und somit auf spezifische Funktionen zu verzichten, die BSB-LAN bietet. Hinweise diesbzgl. findest du in [Kap. 2.2.2](kap02.md#222-konfiguration-durch-anpassen-der-datei-bsb_lan_configh) bzw in den Kommentaren der Datei *BSB_LAN_config.h*. Besonderes Augenmerk ist auf die letzten Punkte zu richten, die u.a. ein komfortables Deaktivieren einzelner Module (bspw. Webconfig, MQTT, IPWE etc.) an zentraler Stelle ermöglicht.  
  
- **Was gilt es zu beachten, wenn ich die aktuelle v2.x nutzen möchte?**  
    Wie bereits erwähnt muss die v2.x in der Konfiguration prinzipiell so angepasst werden, dass sie mit dem geringeren Speicher des Mega problemlos kompiliert und lauffähig ist. Neben dem bereits erwähnten Deaktivieren einzelner Module gibt es weitere Möglichkeiten:  
    
    1.) Die Größe der Variablen von bspw. `PASSKEY[]`, `avg_parameters[]`, `log_parameters[]`, `ipwe_parameters[]`, `max_device_list[]` kann verkleinert werden (wenn bspw. weniger Parameter als maximal möglich verwendet werden), um ein wenig Speicher einzusparen.   
    
    2.) In der Datei *BSB_LAN_config.h* finden sich im unteren Abschnitt verschiedene Definements für Mega-User, die bei bestimmten Einsatzszenarien aktiviert werden können, um nochmals Speicher zu sparen. Hinweise diesbzgl. findest du in der Datei selbst.  
    
    3.) Erstellung einer reglerspezifischen *BSB_LAN_defs.h*:  
    Im Repo liegt ein Perlscript namens *selected_defs.pl* sowie ein Windows-Executable namens *selected_defs.exe*, das die Datei *BSB_LAN_defs.h* nach ausgewählten Gerätefamilien filtert und eine spezifische Datei für den eigenen Reglertyp erstellt. Die Ersparnis beträgt im Schnitt etwa 20 bis 25 kB Flash-Speicher, den man dann für die (Re-)Aktivierung von anderen Funktionen nutzen kann. Im Falle eines Reglerwechsels (= andere Gerätefamilie) muss die Datei natürlich entsprechend neu generiert werden.  
    Das Script läuft unter Perl, was auf Mac- und Linux-Rechnern standardmäßig installiert ist, lässt sich aber auch auf Windows nachinstallieren.       
    
    Vorgehensweise zur Erstellung einer reglerspezifischen defs-Datei:  
    - Parameter 6225 "Gerätefamilie" via BSB-LAN abrufen und den Wert notieren.  
    - Datei *selected_defs.pl* bzw. *selected_defs.exe* vor dem Ausführen in den gleichen Ordner kopieren, in dem auch die Datei *BSB_LAN_defs.h* liegt.  
    - Öffne ein Terminal, wechsle in den entspr. Ordner und erstelle die reduzierte Datei namens *BSB_LAN_defs_filtered.h* mit Hilfe des Perlscripts bzw. des Windows-Executables, die nur die für die spezifische  Gerätefamilie(n) relevanten Parameter enthält. Bei nur einem angeschlossenen Regler, bspw. mit der Gerätefamilie 162, lautet der Befehl  
    `./selected_defs.pl 162 > BSB_LAN_defs_filtered.h` bzw.  
    `selected_defs.exe 162 > BSB_LAN_defs_filtered.h`.  
    Wenn man bspw. zwei Geräte am Bus mit den Gerätefamilien 162 und 90 hat, kann man den Befehl um den zweiten Wert erweitern:  
    `./selected_defs.pl 162 90 > BSB_LAN_defs_filtered.h` bzw.  
    `selected_defs.exe 162 90 > BSB_LAN_defs_filtered.h`.    
    - Verschiebe die originale Datei *BSB_LAN_defs.h* aus dem "BSB_LAN"-Verzeichnis an einen beliebigen Ort. Verschiebe dann die neu erzeugte Datei *BSB_LAN_defs_filtered.h* in das Verzeichnis "BSB_LAN" (falls du die Datei nicht bereits im Ordner "BSB_LAN" erstellt hast).  
    - *Wichtig: Die neu erzeugte Datei nun in "BSB_LAN_defs.h" umbenennen!*  
       
- ***Gibt es bzgl. der zu verwendenden Pineinstellungen etwas zu beachten?***  
    Ja! Solltest du eine neuere Version als v0.44 auf dem Mega testen wollen, so achte darauf, dass du die zur jeweiligen Version zugehörige Datei BSB_LAN_config.h.default verwendest und entsprechend anpasst:  
    - Bei BSB-LAN-Versionen **vor v2.x** ist die Anpassung der Zeile `BSB bus(19,18);` zwingend notwendig: Der DUE verwendet (im Gegensatz zum Mega) die HardwareSerial-Schnittstelle und andere RX-/TX-Pins als der Mega, was hier bereits voreingestellt ist. Bei Verwendung mit dem Mega muss die Zeile daher in `BSB bus(68,69);` geändert werden!  
    - Bei BSB-LAN-Versionen **ab v2.x** ist in der Datei *BSB_LAN_config.h* eine automatische Erkennung der verwendeten Pins voreingestellt. Somit wird automatisch erkannt, ob ein Mega (= software serial) oder ein Due (= hardware serial) zum Einsatz kommt.    
   
- ***Warum gibt es überhaupt einen Umstieg auf den Due?***  
Der Mega 2560 bot einfach nicht mehr genügend Speicher, um auch in Zukunft das stetig wachsende BSB-LAN zu beherbergen! ;)  

- ***Warum gibt es überhaupt einen neuen Adapter v3/v4?***  
Das war nötig, da der bisherige Adapter v2 aus verschiedenen Gründen nicht kompatibel mit dem Due ist.  
  
- ***Kann ich den Adapter v2 an einem Due weiterverwenden?***  
    Nein! Der Grund dafür liegt primär darin, dass sowohl der Adapter v2 als auch der Due kein EEPROM aufweist, was für BSB-LAN 
jedoch notwendig ist.  
    Möchtest du also auch in Zukunft von den neuen Funktionen von BSB-LAN profitieren, musst du dir einen Adapter v3 besorgen oder 
selbst herstellen und ihn mit einem Arduino Due verwenden. 

- ***Kann ich den Adapter v2 zu einem Adapter v3/v4 'umbauen'?***  
Nein! Der primäre Grund hierfür liegt (neben weiteren Gründen) auch wieder im fehlenden EEPROM des Due.  

- ***Kann ich den Adapter v3/v4 mit meinem bisherigen Mega 2560 weiterverwenden?***  
Nein! Auch wenn es vielleicht nach gewissen Änderungen am Adapter v3/v4 möglich wäre, so würde es keinerlei Mehrwert gegenüber 
dem Adapter v2 bieten. Neue Funktionen von BSB-LAN würden aufgrund des mangelnden Speicher des Mega 2560 trotzdem nicht 
genutzt werden können. Wenn du also den neuen Adapter v3/v4 einsetzen möchtest, dann nur in Verbindung mit einem Arduino Due.  

- ***Warum ist auf der Platine v3/v4 ein EEPROM?***  
Der Arduino Due weist kein EEPROM auf, was jedoch für BSB-LAN notwendig ist.  

- ***Kann ich das LAN-Shield bei einem Umstieg auf den Due weiterverwenden?***  
Ja, das ist normalerweise problemlos möglich.  

- ***Kann ich mein bestehendes Gehäuse weiterverwenden?***  
Jein. Der Due weist prinzipiell den gleichen Formfaktor auf wie der Mega 2560, insofern sollte das Gehäuse von den Abmessungen 
her passen. Allerdings musst du vermutlich dein Gehäuse etwas anpassen und einen Ausschnitt oder eine große Bohrung für den 
mittleren USB-Port des Due ('Programming Port') hinzufügen, damit du auch weiterhin bequem das entspr. USB-Kabel anschließen kannst.  

---  
  
[Zurück zum Inhaltsverzeichnis](inhaltsverzeichnis.md)  
  
    
