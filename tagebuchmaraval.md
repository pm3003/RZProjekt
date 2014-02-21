### Tagebuch Projektarbeit Rechenzentrum  

_________

#### Woche vom 20. Januar bis zum 26. Januar
* Recherchearbeiten Datenbank, Authentification, LDAP
* Konfiguration von Ubuntu Server auf den Desktop PCs

______________

#### Woche vom 27. Januar bis zum 2. Februar
* Konfiguration Ubuntu Server auf Schrotty 
* Recherchearbeiten Datenbank
* Vorbereitung Vortrag
* Test der Installation eines Groupware (Horde) in einer virtuellen Umgebung.
* Freitag, den 31. Januar : **Vortrag Storage and Backup**  : [pdf-Datei](https://github.com/pm3003/RZProjekt/blob/master/01312014_Presentation/01312014_Presentation_Storage.pdf) , 
[Mit notes.](https://github.com/pm3003/RZProjekt/tree/master/01312014_Presentation)

________________ 

#### Woche vom 3. bis zum 9. Februar
* Test eines LDAP Server auf dem Laptop 
* Einrichten eines LDAP server im Rechenzentrum (nicht erfolgreich)
* Einrichten von einem SQL Server auf dbsvr2

_________________

#### Woche vom 10. bis zum 16. Februar
* Einrichten des LDAP-Server auf dbsvr1
* Einrichten einer Konfigurationschnittstelle für den SQL-Server, 
* Test der SQL Konfiguration mit einem Test-Webserver
* Einrichten von Datenbank-Benutzern und Benutzergruppen.
* Konfigurationsarbeiten beim Email Server (Horde)

________________

#### Woche vom 17. bis zum 22. Februar
* Konfiguration des LDAP-Servers
    
      *17.02.2014* : 
      
	*Der LDAP Server läuft jetzt auf 192.168.2.24. Der Verzeichnis ist so gut wie leer.*
	
	*Zu tun : Verzeichnis-Plan entwerfen. Auffüllung des Verzeichnisses mittels PHP webpage (auf den Testwebserver) oder manuell mit JXplorer (oder manuell mit command-line). Sicherheit des Verzeichnisses überprüfen (Access-control regeln überprüfen). Integration von Horde mit LDAP und SQL. Zentrale Authentifizierungsschnittstelle im PHP entwerfen (angefangen auf denm Testwebserver).*

      *19.02.2014* : 
      
	*Access-Control-Regeln wurden angepasst. Thomas Bouchard schreibt die Authentifizierungsschnittstelle (basische Funktionen wie Suche und Prüfung von Benutzernamen hat er schon geschrieben) in der Form von PHP-Snippets. Integration von LDAP und mySQL mit Horde ist erfolgreich, aber nicht abgeschlossen.*
	
	*Zu tun : Access-Control-Regeln für bestimmte Benutzer besser anpassen und vielleicht erweitern. *
* Aufbau des Verzeichnisses im LDAP-Server
* Einrichten eines Webmail-Servers mit Horde, Dovecot (IMAP), Postfix (SMTP) auf 192.168.2.27 :

      *19.02.2014* :

	*Die Authentifizierung mit dem LDAP server funktioniert, aber nur  Benutzername und Passwort sind in der struktur registriert. Email Daten und Einstellungen können bis jetzt nur Separat gespeichert werden, wahrscheinlich wegen eines Fehlers in der LDAP-Server Konfiguration. Zur Lösung dieses Problem würde eine Überführung der Konfiguration von der Config Datei slapd.conf in den LDAP-Verzeichnis wahrscheinlich helfen. Addieren von User und Einteilung in Gruppen funktioniert, auch innerhalb von Horde.*

	*Zu tun : Erweiterung der Benutzerverwaltung-Möglichkeiten durch Benutzung von anderen Benutzerkonto-Typen und Gruppen-Typen. Dafür ist eine Änderung der LDAP-Konfiguration durch Intgration von neuen Schemas (isb horde.schema) notwendig, die aber nicht klappt. Überführung der Konfiguration von der Config Datei slapd.conf in den LDAP-Verzeichnis probieren. Einstellungen des aktuellen Webmail-Servers mit Team Mailserver absprechen und anpassen. Eventuell weitere Module von Horde an die Datenbanken anknüpfen. Patching der Horde php-Dateien (Hooks) absprechen. Proxy-Einstellungen ?*
	
      *21.02.2014* :

	*Einstellungen des Mailservers müssen noch geklärt werden, insbesondere die Frage des Zugriffs von außen und eventuell die Einrichtung eines DNS-Servers bzw. die Festlegung einer bestimmten DNS Konfiguration. Die Virtuelle Maschinen sind down, keine weitere Arbeiten in diesem Bereich möglich.
	Der SMTP Server Sendmail (zu kompliziert zu konfigurieren) wurde durch Postfix als SMTP-Komponent ersetzt. Zwei Hauptprobleme bei der Authentifizierung wurden identifiziert: php Session Handling (von der email team gelöst), und schlechte Einstellung von IMP (webmail-Komponent von Horde), (Lösung gefunden aber noch nicht angewendet).*
      
 
* Einrichten eines weiteren Datenbank-Servers auf dbsvr3 zum Testen von Sicherheitsfunktionen.

      *19.02.2014* :

	*Zu tun bzw zu untersuchen : Replikation (tutorial auf Youtube), Backup, Notfallplan beim Versagen der Horde-Installation oder des Datenbanks -> mit Team Backup bearbeiten. Proxy-Einstellungen ?*
	
* Weiteren Datenbank für den Webserver erstellt. Weiterer user (webauftritt) mit dementsprechende Rechte auf den Webserver-Datenbanken. Wikiseite aktualisiert.
* Beginn einer Wiki-Howto-Seite für LDAP
      
      *21.02.2014* *: Teil 1 (LDAP Übersicht) ist fertig, mit einem praktischen Beispiel aus dem HSU-Verzeichnis. Teil 2 und 3 müssen noch geschrieben werden.*
      

**Zu tun : Zusammenfassung** : 

*LDAP Konfiguration :* Überführung der Konfiguration von der Config Datei slapd.conf in den LDAP-Verzeichnis; Erweiterte Access-Control Regeln (optional); neue Benutzerkontos einrichten;

*Mailserver :* Konfiguration absprechen. Verknüpfung von Dovecot(IMAP), Postfix (SMTP), und IMP/Horde, insbesondere in der Benutzerverwaltung. LDAP-Integration von Postfix und Dovecot (erfordet einen zusätzlichen LDAP Server)?

*Datenbank-Server :* Sicherheit-Einstellungen und Backup-Plan mit Team Security und Team Backup absprechen. Dementsprechend Replikation und TLS/https/Zertificate recherchieren.

*Howto-Wikiseite:* Fertig schreiben, sobald Thomas mit den php-snippets fertig ist. Ähnliche Seite für SQL?

#### Woche vom 22. bis zum 28. Februar

