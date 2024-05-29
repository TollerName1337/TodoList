# TodoList

## Nutzer erstellen

### 1. Verbinden mit raspberrypi
Über den ssh Befehl verbindet man sich mit den Raspberrypi. Es folgt eine Passwort Eingabeaufforderung.

    - ssh pi@192.168.24.140
    
### 2. Netzwerkkonfiguration mit einer statischen IP-Adresse im lokalen Subnetz
Der erste Schritt ist sich alle Verbindungen des Raspberrypi's anzuzeigen.

    - sudo nmcli -p connection show
Danach wählt man die genutzte Verbindung aus und passt diese an.

    - sudo nmcli c mod "Wired connection 1" ipv4.addresses 192.168.24.184 ipv4.method manual

Gateway und DNS einrichten

    - sudo nmcli con mod "Wired connection 1" ipv4.gateway 192.168.24.254
    - sudo nmcli con mod "Wired connection 1" ipv4.dns "192.168.24.254"
    
Am Ende sollte man die Verbindung neustarten, damit die Änderungen übernommen werden.

    - sudo nmcli c down "Wired connection 1" && sudo nmcli c up "Wired connection 1"

### 3. Zwei lokale Benutzer
#### „willi“ – Normaler Benutzer ohne Administratorrechte
Der User willi wir durch diesen Command erstellt. Nach dem Erstellen muss man ein Passwort festlegen.

    - sudo adduser willi 

#### „fernzugriff“ – Benutzer für den Zugriff von außen mittels SSH mit sudo-Rechten
Der User mit Adminrechten wird genauso angelegt, danach muss man allerdings die Rechte vergeben

    - sudo adduser fernzugriff 
    - sudo usermod -aG sudo fernzugriff

## Docker installieren

    - sudo apt install docker.io
    
## Deployment der Web-App „Todo-Listen-Verwaltung“ als Container
```Dockerfile
# Basisimage für Python-Anwendungen herunterladen
FROM python:3.11-alpine

# Benenne Port, der von der Web-App genutzt wird
EXPOSE 5000

# Arbeitsverzeichnis im Container wechseln
WORKDIR /app

# Notwendige Bibliotheken installieren
RUN pip install flask

# Kopiere lokale Datei in das Container-Image
COPY beispiel-server.py /app

# Konfiguriere den Befehl, der im Container ausgeführt werden soll 
# (Anwendung Python + Skriptname als Parameter)
ENTRYPOINT [ "python" ]
CMD ["beispiel-server.py" ]
```
























