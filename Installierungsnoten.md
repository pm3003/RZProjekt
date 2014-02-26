### Noten : Installation von postfix;

##### Installiert: postfix, postfix-mysql, postfix-ldap, bsd-mailx

##### postfix-ldap:
"Adding ldap map entry to /etc/postfix/dynamicmaps.cf"

main.cf :

>alias_maps = hash:/etc/aliases, ldap:/etc/postfix/ldap/ldap->aliases.cf
>alias_database = hash:/etc/aliases
>
>virtual_alias_maps = ldap:/etc/postfix/ldap/virtual_aliases.cf
>virtual_mailbox_domains = >ldap:/etc/postfix/ldap/virtual_domains.cf
>virtual_mailbox_maps = >ldap:/etc/postfix/ldap/virtual_mailboxes.cf
>virtual_mailbox_base = /
>virtual_uid_maps = ldap:/etc/postfix/ldap/virtual_uid.cf
>virtual_gid_maps = ldap:/etc/postfix/ldap/virtual_gid.cf
>
>transport_maps = ldap:/etc/postfix/ldap/virtual_transport.cf
>relay_domains = ldap:/etc/postfix/ldap/virtual_relay.cf

Erstellung der obengenannten Dateien

"To enable saslauthd, edit /etc/default/saslauthd and set START=yes"

edit /etc/default/saslauthd

include qmail.schema ? (in den Dateien verwendet, sonst nicht nötig)

/etc/init.d/postfix restart
/etc/init.d/saslauthd restart
### Aus whisperedshouts.de


#### Konfiguration der LDAP Anbindung
Modifikation der /etc/postfix/main.cf

Folgender Abschnitt muss in der main.cf erstellt werden. Einzelheiten zu diesen Optionen erfahren Sie im nächsten Abschnitt, der sich mit der Erstellung der angegebenen Dateien befasst, oder natürlich auf der ensprechenden MAN Page.

>virtual_alias_maps = ldap:/etc/postfix/ldap/virtual_aliases.cf
>virtual_mailbox_domains = >ldap:/etc/postfix/ldap/virtual_domains.cf
>virtual_mailbox_maps = >ldap:/etc/postfix/ldap/virtual_mailboxes.cf
>virtual_mailbox_base = /
>virtual_uid_maps = ldap:/etc/postfix/ldap/virtual_uid.cf
>virtual_gid_maps = ldap:/etc/postfix/ldap/virtual_gid.cf
>
>#TRANSPORT
>transport_maps = ldap:/etc/postfix/ldap/virtual_transport.cf
>relay_domains = ldap:/etc/postfix/ldap/virtual_relay.cf

#### Erstellung der Konfigurationen für den LDAP Zugriff

Nun müssen die Dateien erstellt werden die letztlich die Konfiguration bzw. die Lookups beschreiben. Hierzu legen wir einen Ordner “ldap” unter dem Standardkonfigurationsverzeichnis “/etc/postfix” an.
/etc/postfix/ldap/virtual_aliases.cf

##### Beschreibung

Zuständig für Weiterleitungsadressen der Benutzer.

##### Zum Ablauf:
Sucht nach einem Eintrag, dessen Attribut mail oder mailAlternateAdress der Mailadresse entspricht, an die eine Mail gesendet wurde
Gibt den Wert des Attributs mailForwardingAddress zurück 
Ist der Wert leer wird die Mail nicht weitergeleitet
Ist der Wert nicht leer wird die Mail entsprechend weitergeleitet

##### Inhalt der Datei

virtual_aliases.cf
>server_host = localhost
>server_port = 389
>search_base = dc=testlab,dc=local
>query_filter = (|(mail=%s)(mailAlternateAddress=%s))
>result_attribute = mailForwardingAddress
>/etc/postfix/ldap/virtual_domains.cf

##### Beschreibung

Zuständig für den Lookup valider Domains, für die Postfix Mails annehmen soll.
 Zum Ablauf:
Sucht unterhalb der ou Domains, ob es einen Eintrag gibt, dessen Attribut associatedDomain der Empfängerdomain entspricht
Gibt den Wert des Attributs zurück 
Ist der Wert nicht leer akzeptiert Postfix Mails für diese Domäne

##### Inhalt der Datei

virtual_domains.cf
>server_host = localhost
>server_port = 389
>search_base = ou=Domains,dc=testlab,dc=local
>query_filter = (associatedDomain=%s)
>result_attribute = associatedDomain
>/etc/postfix/ldap/virtual_mailboxes.cf

##### Beschreibung

Sucht den Maildir Pfad zu einer Mailadresse. Dies wird benötigt, falls Postfix lokale Mailboxen beliefern soll.
 Zum Ablauf:
Sucht nach einem Eintrag, dessen Attribut mail oder mailAlternateAdress der Mailadresse entspricht, an die eine Mail gesendet wurde
Gibt den Wert des Attributs mailMessageStore zurück 
Ist der Wert nicht leer
und wurde keine mailForwardingAddress gesetzt wird der Wert des Attributs als Pfad zum Maildir verwendet. 

##### Inhalt der Datei

virtual_mailboxes.cf
>server_host = localhost
>server_port = 389
>search_base = dc=testlab,dc=local
>query_filter = (|(mail=%s)(mailAlternateAddress=%s))
>result_attribute = mailMessageStore
>/etc/postfix/ldap/virtual_uid.cf

##### Beschreibung

Zuständig für den Lookup der UID zu einer Mailadresse. Da in unserem Setup stets die gleiche UID verwendet wird ist dieser Lookup eigentlich unnötig, ich möchte mir die Möglichkeit aber offen lassen ohne konfigurative Änderung am Postfix damit zu arbeiten.

##### Zum Ablauf:
Sucht nach einem Eintrag, dessen Attribute mail oder MailAlternateAddress mit der übergebenen Mailadresse übereinstimmt
Gibt den Wert des Attributs qmailUID zurück

##### Inhalt der Datei

virtual_uid.cf
>server_host = localhost
>server_port = 389
>search_base = dc=testlab,dc=local
>query_filter = (|(mail=%s)(mailAlternateAddress=%s))
>result_attribute = qmailUID
>/etc/postfix/ldap/virtual_gid.cf

##### Beschreibung

Zuständig für den Lookup der GID zu einer Mailadresse. Da in unserem Setup stets die gleiche GID verwendet wird ist dieser Lookup eigentlich unnötig, ich möchte mir die Möglichkeit aber offen lassen ohne konfigurative Änderung am Postfix damit zu arbeiten.
 
 
##### Zum Ablauf:

Sucht nach einem Eintrag, dessen Attribute mail oder MailAlternateAddress mit der übergebenen Mailadresse übereinstimmt
Gibt den Wert des Attributs qmailGID zurück

##### Inhalt der Datei

virtual_gid.cf
>server_host = localhost
>server_port = 389
>search_base = dc=testlab,dc=local
>query_filter = (|(mail=%s)(mailAlternateAddress=%s))
>result_attribute = qmailGID
>/etc/postfix/ldap/virtual_transport.cf

##### Beschreibung

Dieser Lookup wird verwendet um zu prüfen, ob Postfix Mails für eine bestimmte Domain an einen dedizierten Server senden soll. Dies ist z.B. in einem Multi-Domain Setup interessant, oder aber auch bei der Kommunikation von externen Adressen zu internen Adressen (als Beispiel sei ein Exchange Server genannt, der im Unternehmensnetz eine lokale Domäne bedient, aber nicht direkt ans Netz angeschlossen werden soll).

##### Zum Ablauf:

Sucht unterhalb der ou forwardDomains, ob es einen Eintrag gibt, dessen Attribut associatedDomain der Empfängerdomain entspricht
Gibt den Wert des Attributs destinationIndicator zurück 
Ist der Wert nicht leer verwendet Postfix diese Information für sein Routing.

##### Inhalt der Datei

virtual_transport
>server_host = localhost
>server_port = 389
>search_base = ou=forwardDomains,dc=testlab,dc=local
>query_filter = (associatedDomain=%s)
>result_attribute = destinationIndicator
>/etc/postfix/ldap/virtual_relay.cf

##### Beschreibung

Dieser Lookup unterscheidet sich geringfügig vom vorherigen Lookup und wird verwendet, um Postfix relaying für eine Domäne zu befehlen ohne eine feste Destination anzugeben. Postfix wird also entsprechend MX Lookups ausführen und die Mail dann relayen.

##### Zum Ablauf:

Sucht unterhalb der ou forwardDomains, ob es einen Eintrag gibt, dessen Attribut associatedDomain der Empfängerdomain entspricht
Gibt den Wert des Attributs associatedDomain zurück

##### Inhalt der Datei

virtual_relay.cf
>server_host = localhost
>server_port = 389
>search_base = ou=forwardDomains,dc=testlab,dc=local
>query_filter = (associatedDomain=%s)
>result_attribute = associatedDomain


#### Abschließende Tätigkeiten
Konfiguration prüfen und laden

Wurden alle Dateien angelegt muss Postfix neu gestartet bzw. dessen Konfiguration neu geladen werden.
 Zunächst prüfen wir die Konfiguration auf syntaktische Fehler
/etc/init.d/postfix check

und falls keine Probleme gemeldet werden direkt noch ein
/etc/init.d/postfix reload

hinterher. Prüfen Sie auch den Inhalt der /var/log/syslog auf Postfix Fehlermeldungen. Wenn alles gut aussieht ist es Zeit die Zustellung zu testen.
Test der Mailfunktionalität

Zum Testen bietet es sich natürlich an eine Mail über den Server zu versenden. Zum Debugging können sie mit telnet oder netcat eine Verbindung zum Mailserver aufnehmen und die Kommandos manuell absetzen. Im Folgenden senden wir eine Mail an die Alternativadresse von Herrn Max Mustermann (vgl. Part 2) Die fett gedruckten Befehle müssen Sie eingeben, die kursiv gedruckten Meldungen sind Antworten von Postfix und können leicht von den abgedruckten Informationen abweichen.
nc localhost 25
220 mail.example.com ESMTP Postfix (Debian/GNU rulez!)
helo localhost
250 mail.example.com
mail from:<testmessage@gmail.com>
250 2.1.0 Ok
rcpt to:<postmaster@example.com>
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
From: testmessage@gmail.com
To: postmaster@example.com
Subject: Testmessage

Dies ist eine Testnachricht. Wenn alles funktioniert sollte diese zugestellt werden.
.
250 2.0.0 Ok: queued as 428828A0076
quit
221 2.0.0 Bye

Wenn alles geklappt hat sollte eine Mail versendet worden sein. In unserem Beispieldatensatz von Herrn Mustermann ist eine Weiterleitungsadresse angegeben, entsprechend sollte sich in dieser Mailbox eine Mail befinden. Ein Blick ins syslog kann weitere Informationen beinhalten
Apr 10 16:05:41 uvhsb30 postfix/smtp[21402]: 428828A0076: to=<eine.adresse@mailprovider.com>, orig_to=<postmaster@example.com>, relay=gmail-smtp-in.l.google.com[173.194.70.26]:25, delay=118, delays=117/0.01/0.1/0.36, dsn=2.0.0, status=sent (250 2.0.0 OK 1365602700 h2si144053eeg.57 - gsmtp)
Ausblick

Gratulation, Postfix empfängt nun Mails! In unserem nächsten Artikel beschäftigen wir uns mit Authentifizierung gegen den LDAP Server, um unseren Benutzern das Senden von Mails zu gestatten, sowie Zertifikaten und SSL, um die Kommunikation mit Postfix zu verschlüsseln. Dies können Sie im Artikel Postfix und LDAP Part 4 – saslauthd und SSL nachlesen.