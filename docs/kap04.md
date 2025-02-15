[Zurück zum Inhaltsverzeichnis](inhaltsverzeichnis.md)  
[Zurück zu Kapitel 3](kap03.md)    
    
---

# 4. BSB-LAN: Das Webinterface
Die Startseite des Webinterface wird angezeigt, wenn ohne weitere
Parameter auf die URL des Servers zugegriffen wird:  
`http://<IP-Adresse>`

Bei Verwendung eines Passkeys oder weiterer optionaler
Sicherheitsfunktionen muss die URL entsprechend erweitert werden, bei
Passkey-Verwendung bspw.:  
`http://<IP-Adresse>/<passkey>/`

*Bitte den Slash hinter dem Passkey nicht vergessen!*
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/webinterface_startseite.png">
    
Im oberen Bereich des Webinterface sind einige Buttons angeordnet, die einen einfachen und schnellen Zugriff auf bestimmte Funktionen bieten:  
- Heizungsfunktionen  
- Sensoren  
- Ausgabe Logdatei  
- Prüfe auf neue Parameter  
- Einstellungen  
- URL-Befehle  
- Handbuch  
- FAQ  
   
Der Button "Ausgabe Logdatei" wird in schwarzer Schrift dargestellt, wenn die Loggingfunktion nicht aktiviert ist (wie im obigen Screenshot zu sehen). Ist die Logging-Funktion aktiviert, so heißt die Bezeichnung des Buttons "Zeichne Logdatei".
  
Unter dem Headerbereich wird die BSB-LAN-Version angezeigt, die derzeit verwendet wird.  
BSB-LAN kann prüfen, ob eine neuere Version verfügbar ist und zeigt dieses im unteren Bereich der Seite an. Im Falle eines verfügbaren Updates führt der Link zum ZIP-File des Repos, so dass man direkt vom Webinterface heraus die Datei speichern kann:  

<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/neue_version.png">  

| Hinweis |
|:--------|
| Diese Funktion muss aktiviert werden, siehe dazu bitte [Kap. 2.2](kap02.md#22-konfiguration). |  

---  
   
**Heizungsfunktionen (URL-Befehl: /K):**  
Prinzipiell sind alle Parameter in Kategorien zusammengefasst, die den im Display der dargestellten Untermenükategorien entsprechen, wenn auf den Regler des Heizungssystems vom integrierten Bedienteil aus zugegriffen wird.

Ein Klick auf den Menüpunkt „Heizungsfunktionen" zeigt eine vollständige Übersicht der Kategorien, die wiederum ebenfalls anwählbar sind:
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/webinterface_kategorien.png">
    
Ein Klick auf eine der gezeigten Kategorien (bspw. Heizkreis 1) startet eine Komplettabfrage der jeweiligen Kategorie, also aller Parameter, die
in dieser Kategorie verfügbar sind. Nicht verfügbare Parameter (also Parameter, die vom spezifischen Reglermodell nicht unterstützt werden), werden in grauer Schrift mit dem Hinweis "(parameter not supported)" angezeigt:  
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/webinterface_kategorie-HK1.png">
    
| Hinweis |
|:--------|
| Man kann die nicht verfügbaren Parameter ausblenden lassen, bei einer Kategorie- oder Komplettabfrage werden sie dennoch mit abgefragt. Siehe hierzu bitte [Kap. 2.2](kap02.md#22-konfiguration). |    
    
---  
   
**Sensoren (URL-Befehl: /K49):**  
Wenn optionale Sensoren (DS18B20, DHT22, BME280, MAX!) angeschlossen und korrekt konfiguriert sind, dann werden diese hier angezeigt.  

<!-- <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/webinterface_sensoren.png"> -->

---  
   
**Ausgabe/Zeichne Logdatei (URL-Befehle /D und /DG):**  

Eine grafische Darstellung des optional erstellbaren Logfiles (Datei *datalog.txt*) auf einer 
microSD-Karte erfolgt bei Klick auf den Button "Zeichne Logdatei". Ist die Funktion deaktiviert, so wird der deaktivierte Button in schwarzer Schrift angezeigt und die Bezeichnung lautet "Ausgabe Logdatei".
    
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/webinterface_log_graph.jpg">  
    
| Hinweise |
|:---------|
| Für die grafische Anzeige der Logdatei wie im obigen Beispiel dürfen keine JavaScript-Blocker aktiv sein und es muss eine aktive Internetverbindung 
bestehen, da das JavaScript-Framework zur Darstellung von d3js.org geladen wird. |
| Bitte beachte, dass der Arduino nicht multitaskingfähig ist. Eine neue Abfrage kann erst erfolgen, nachdem die vorhergehende Abfrage komplett beendet ist. Speziell die Abfrage mehrerer Parameter, ganzer Kategorien oder auch des Logfiles der microSD-Karte kann u.U. eine längere Zeit in Anspruch nehmen, während dieser der Adapter nicht ‚ansprechbar' ist. |

---  
   
**Prüfe auf neue Parameter (URL-Befehl /Q):**  
Mit dieser Funktion werden sämtliche bekannten Parameter abgefragt und überprüft, ob für den angeschlossenen Regler noch etwaige Parameter freizugeben sind. Siehe auch [Kap. 3.3](kap03.md#33-überprüfen-auf-nicht-freigegebene-reglerspezifische-command-ids).  
   
<img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/webinterface_Q_de.png">  
   
---  
   
**Einstellungen (URL-Befehl: /C):**  
Hier wird eine Übersicht der Konfiguration dargestellt.  
Im oberen Bereich ist das [Webinterface zur Konfiguration](kap02.md#221-konfiguration-mittels-webinterface) verfügbar, im unteren Bereich werden nochmals bestimmte Einstellungen (u.a. die genutzte Version von BSB-LAN, die Uptime, der Bustyp, möglicher Schreib- oder Lesezugriff, die definierten Pins für optional angeschlossene Sensoren, die zu loggenden Parameter etc.) aufgelistet.
   
<!-- <img src="https://raw.githubusercontent.com/1coderookie/BSB-LPB-LAN/master/docs/pics/webinterface_konfig.png">  --> 
   

---  
   
**URL-Befehle:**  
Der Button ist mit diesem Handbuch verknüpft und führt zum Kapitel [URL-Befehle](kap05.md#51-url-befehle), in dem die möglichen Befehle übersichtlich und kurz aufgeführt sind. Internetzugriff wird benötigt.  
   
---  
   
**Handbuch:**  
Der Button ist mit dem [Inhaltsverzeichnis](inhaltsverzeichnis.md) dieses Handbuchs verlinkt. Internetzugriff wird benötigt.  
   
---  
   
**FAQ:**  
Der Button ist mit dem Kapitel [FAQ](kap15.md) dieses Handbuchs verlinkt. Internetzugriff wird benötigt.
    
---
    

     
[Weiter zu Kapitel 5](kap05.md)      
[Zurück zum Inhaltsverzeichnis](inhaltsverzeichnis.md)   
    

