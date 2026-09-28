# Labbdokumentation

**Namn:** Azeez Wali

**GitHub:** azeezwali-arch

**Kurs:** IT infrastructure secure cloud - 26

**Datum:** 2026-09-15

## Innehåll
1. Del 1: Git och versionshantering
2. Del 2: Virtuell labbmiljö och nätverk
3. Del 3: Kommandoradsarbete och Felsökning
4. Del 4: AI-stöd och Kritisk Utvärdering


## Del 1: Git och versionshantering

### Installation
Jag installerade Git på min MacBook via terminalen, med hjälp av AI. Jag installerade
även Homebrew och GitHub CLI och loggade in på mitt GitHub-konto
(azeezwali-arch). 

### Arbetsgång
1. Jag skapade en ny mapp för uppgiften 
2. Jag initierade mappen som ett Git-repository med "git init".
3. Jag skapade ett repo på GitHub och kopplade det till min lokala mapp
   med "git remote add origin".
4. Jag öppnade mappen i VS Code och skapade Markdown filen
   "Labbdokumentation.md".
5. Jag committade och pushade ändringarna till GitHub.


## Del 2: Virtuell labbmiljö och nätverk

### 2.1 Miljö
Jag laddade ner VirtualBox på min MacBook och skapade två virtuella
maskiner: en Linux-maskin och en Windows 11-maskin. Jag hade inte
mycket förkunskaper, så jag tog hjälp av AI under arbetet och lärde mig
genom att prova mig fram.

Båda maskinerna sattes till "Internal Network" med namnet LabNet under
Nätverk i VirtualBox, så att de kan kommunicera med varandra men inte
med internet.

### 2.2 Miljötabell
| Hostname | Operativsystem | IP-adress | Subnätmask | Standard Gateway |
|---|---|---|---|---|
| azeez-virtualbox | Linux, Lubuntu | 192.168.1.50 | 255.255.255.0 (/24) | 192.168.1.1 |
| windows11 | Windows 11 | 192.168.1.51 | 255.255.255.0 (/24) | 192.168.1.1 |

### 2.3 Konfiguration
**Windows:** Statisk IP sattes i Inställningar → Nätverk → Ethernet:
192.168.1.51, mask 255.255.255.0, gateway 192.168.1.1.

![Ip konfiguration-windwos](bilder/ip-windows.webp)

**Linux:** Det var mycket krångligare på Linux, och jag fick mycket
hjälp av AI. IP-adressen 192.168.1.50/24 sattes först med kommandot
"ip addr add 192.168.1.50/24 dev enp0s3". Anslutningen hade ingen
sparad konfigurationsfil och "nmcli" gav felet "Message recipient
disconnected from message bus", så jag testade flera kommandon.
Till slut gav AI mig intruktionerna att skapa filen "/etc/cron.d/labip" med en rad som sätter
adressen automatiskt vid varje uppstart.

![Ip konfiguration-linux](bilder/ip-linux.webp)

### 2.4 Test
Ping från Windows till Linux (192.168.1.50) och från Linux till
Windows (192.168.1.51) gav svar utan paketförlust.

![ping linux](bilder/ping%20linux%20till%20widows.webp)
![ping windows](bilder/ping%20widows%20to%20linux.webp)

### 2.5 Felsökning
- Ping från Linux till Windows misslyckades först, eftersom
  Windows-brandväggen blockerar ping som standard. Jag lade till en
  brandväggsregel som tillåter ICMP. AI hjälpte till med "New-NetFirewallRule -DisplayName "Allow ICMPv4 Ping" -Protocol ICMPv4 -IcmpType 8 -Action Allow"
  
  ![Windows-brandväggen blockerar](bilder/powershell%20blockerar.webp)

- Min första Windows-fil var för ARM-processorer och fungerade inte på
  min Intel-Mac. Jag laddade ner x64-versionen.
  