# Heartbleed Attack

----
## Was ist der Heartbleed Attack?
see [Wikipedia](https://de.wikipedia.org/wiki/Heartbleed)

> Der Heartbleed-Bug ist ein schwerwiegender Programmfehler in älteren Versionen der Open-Source-Bibliothek OpenSSL, durch den über verschlüsselte TLS-Verbindungen private Daten von Clients und Servern ausgelesen werden können. Der Fehler betrifft die OpenSSL-Versionen 1.0.1 bis 1.0.1f und wurde mit Version 1.0.1g am 7. April 2014 behoben. Ein großer Teil der Online-Dienste, darunter auch namhafte Websites wie auch VoIP-Telefone, Router und Netzwerkdrucker waren dadurch für Angriffe anfällig.

## Begründung zur Auswahl der Sicherheitslücke
Wir fanden es interessant dass wichtige System die unsere Daten beinhalten, durch irgendwelche Lücken, ganz einfach geheime Daten freigeben. Das Interesse ist eher allgemein, da solch ein Phenomen auch auf andere Sicherheitslücken zutreffen kann.

----
## Grösserer Vorfall
Der Diebstahl von 4,5 Millionen Patientendaten bei der US-Krankenhauskette Community Health Systems (CHS) war durch die Heartbleed-Lücke in OpenSSL möglich.
Die Heartbleed-Lücke hatten das finnische Unternehmen Codenomicon und Neel Mehta von Google Security entdeckt. Angreifer können damit an zufällige Daten aus dem Speicher eines Servers gelangen, unter denen sich auch Verschlüsselungszertifikate oder andere kritische Informationen befinden können. Mit einem solchen Zertifikat wäre es beispielsweise möglich, sich gegenüber Dritten als der angegriffene Server auszugeben.

----
[mehr](http://www.zdnet.de/88202827/bericht-gehackte-us-krankenhauskette-war-heartbleed-opfer/?inf_by=5a096546671db8a02d8b4b25)

## Die Sicherheitslücke
### Entdeckung

Am 7. April 2014 hat das OpenSSL-Team einen Sicherheitshinweis des Inhalts herausgegeben, dass die OpenSSL-Versionen 1.0.1 bis einschließlich 1.0.1f, sowie 1.0.2-beta bis einschließlich 1.0.2-beta1 vom sogenannten Heartbleed-Bug betroffen seien. Es ist jedoch möglich, OpenSSL ohne die fehlerhafte Heartbeat-Komponente zu kompilieren 

    (-DOPENSSL_NO_HEARTBEATS)
eine so kompilierte 
Version war dadurch auch gegen einen Heartbleed-Angriff immun.

Die Sicherheitslücke bestand insgesamt 27 Monate, bis sie unabhängig voneinander von Antti Karjalainen (Codenomicon, Oulu, Finnland) und Neel Mehta (Google Security) entdeckt und behoben wurde. Bereits vor dem öffentlichen Bekanntwerden des Fehlers waren einige Anbieter informiert, die daraufhin bereits ihre Systeme absicherten.

[mehr](https://de.wikipedia.org/wiki/Heartbleed#Entdeckung)

### Erklärung
Der Fehler befindet sich in der OpenSSL-Implementierung der Heartbeat-Erweiterung für die Verschlüsselungsprotokolle TLS und DTLS. Die Heartbeat-Erweiterung sieht vor, dass ein Kommunikationsteilnehmer eine bis zu 16 kByte große Menge an beliebigen Daten (Payload und Padding) an die Gegenseite schickt, die anschließend den Payload-Teil unverändert zurücksendet, womit periodisch abgeprüft werden kann, ob die Verbindung zum Server noch besteht.

Compiler der für OpenSSL verwendeten Programmiersprache C erzeugen standardmäßig keinen Code zur automatischen Überprüfung der Datenlänge. Sofern der Programmierer solche Fehler nicht explizit durch zusätzlichen Programmcode abfängt, wird daher auch nicht überprüft, ob die angegebene Länge der Daten mit der tatsächlichen Länge der mitgelieferten Daten übereinstimmt. Ist die angegebene Länge größer als die tatsächliche Länge, so kopiert die OpenSSL-Implementierung über das Ende des Eingabepuffers hinaus Daten aus dem Heap in den Ausgabepuffer. Aufgrund der fehlenden Überprüfung kann ein Angreifer mit einer Anfrage bis zu 64 kByte des Arbeitsspeichers der Gegenstelle auslesen. Der Angriff kann mehrmals hintereinander durchgeführt werden, um mehr Speicher auszulesen. In Tests gelang es damit unter anderem den privaten Schlüssel des Serverzertifikats, Benutzernamen und Passwörter von Servern auszulesen.

### Interessante Links

**Sehr gute Demo**
[https://www.youtube.com/watch?v=EUzRvsd8zdM](https://www.youtube.com/watch?v=EUzRvsd8zdM)

**Computerphile** [https://www.youtube.com/watch?v=1dOCHwf8zVQ](https://www.youtube.com/watch?v=1dOCHwf8zVQ)

**Artikel von Spiegel**
[http://www.spiegel.de/netzwelt/web/heartbleed-openssl-fehler-verraet-passwoerter-a-963381.html](http://www.spiegel.de/netzwelt/web/heartbleed-openssl-fehler-verraet-passwoerter-a-963381.html)
----

### 5.1.4

**Informationssicherheit**

- Verfügbarkeit: Server Backups und paralell laufende backup Systeme, die bei einem total ausfall zum laufen kommen
- Integrität: Mit Checksums (Beispiel md5 hash)
- Vertraulichkeit: Sessions, Sockets, Private key
- Nicht-Abstreitbarkeit (Verbindlichkeit): durch logging, machine readable
- Authentizität: Mit Private public keys, 2 faktor authentifizierung, cookies
- Privatsphäre: Muss mit Erlaubnis eingeholt werden, beispiel: Darf nicht benutzer ohne erlaubnis erstellen, kaufverhalten eines users dürfen in cookies nur festgehalten falls vom user erlaubt wird.

### 5.1.5
 
 **Host- und Netzwerksicherheit**

- No open ports
- Always use the newest updates
- Datacenter connections should always use Ethernet
- Don't use PHP in production
- Shutdown unused SSH daemons
- Don't log into your servers as root
- Use usergroups
- Use containers if there are more than one application per machine
- Whitelist ips that are allowed to connect to the database
- Hardcode static and known DNS addresses inside /etc/hosts
 
 
### changelog
* 13-Nov-2017 Creation, planning
* 19-Nov-2017 Finish
