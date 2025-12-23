# AB API ![🇩🇪 Deutsch](https://img.shields.io/badge/Deutsch-DE-blue?logo=de&logoColor=white)
English version: [README.en.md](README.en.md)  [![🇬🇧 English](https://img.shields.io/badge/English-EN-brightgreen?logo=gb)](README.en.md) 

In diesem Repository werden die Beschreibung der API zur HS Auftragsbearbeitung (YAML-Datei) und der Changelog veröffentlicht.

## Nutzungsbedingungen
Voraussetzung für die Verwendung der API in einer Zusatzlösung ist der Abschluss eines Vertrags zur Nutzung der API mit HS – Hamburger Software GmbH & Co. KG. 

Die Nutzung ist kostenpflichtig und orientert sich u.a. am Umfang der gewünschten Nutzung (Anzahl und Typ der benötigten Endpunktgruppen, Umfang der Zugriffsrechte, Intensität / Volumen der Nutzung) sowie dem Einsatzszenario.
Für weitere Informationen wenden Sie sich an vertrieb@hamburger-software.de.

# Funktionsumfang
Die API wird mit der Anwendung zur Verfügung gestellt. Die Implementierung der API entspricht dem OpenApi 3.0-Standard
Als Technologien werden **REST** (**Re**presentational **St**ate **T**ransfer) und **JSON** (**J**ava**S**cript **O**bject **N**otation)
verwendet.

## Sicherheit
Die API wird als Backend-API bereitgestellt. Für eine Absicherung steht grundsätzlich HTTPS mit generierten und eigenen Zertifikaten zur Verfügung. 

Der Aufrufer muss sich authentifizieren. Dazu werden die Credentials eines Benutzers der HS Auftragsbearbeitung im Authentifizierungsverfahren Basic oder OAuth2 verwendet. In diesem Zuge werden auch die fachlichen Berechtigungen des Benutzers überprüft.

Für die Absicherung der API gegen unberechtigte Zugriffe (Firewall, API-Gateway etc.) ist darüber hinaus der Ersteller einer Zusatzlösung sowie der Betreiber der API (z.B. HS Kunde, Hoster) verantwortlich.

## Autorisierung
Um den Zugriff einer Zusatzlösung abzusichern, werden zur Autorisierung der Lösungen API-Keys verwendet. Jede Lösung erhält dazu einen eigenen API-Key, der ausschließlich für diese Lösung verwendet werden darf. Der API-Key wird nach Abschluss des Vertrags zur Nutzung der HS API zugesendet. Die Weitergabe eines API-Keys ist nicht erlaubt.

Die Lizenz der HS Anwendung, bei der die Zusatzlösung eingesetzt wird, muss entsprechend aktualisiert werden, damit der Lizenzschlüssel den API-Key enthält. (Arbeitsgebiet LizenzCenter - Lizenz aktualisieren).

Der API-Key ist entsprechend in der Zusatzlösung zu implementieren. API-Zugriffe ohne API-Key werden als "Unauthorized" abgewiesen. Ein API-Key ist an die Endpunktgruppen gebunden. Das heißt, mit einem APIKey kann nur auf Endpunkte innerhalb der vertraglich vereinbarten Endpunktgruppen zugriffen werden.

Im Arbeitsgebiet **LizenzCenter - Registrierte Lösungen** sind die registrierten Zusatzlösung gelistet, die per API auf die HS Anwendung zugreifen dürfen. Per Doppelklick auf eine Lösung erhält man zusätzlich die Informationen, für welche Endpunktgruppen der Zugriff erlaubt ist und mit welchen Rechten (Lesen/Vollzugriff) diese freigeschaltet sind.

### Beispieldaten / Schulungsversion
Die Verwendung der API für Erkundungs- sowie Testzwecke ist in den Beispieldaten der HS Anwendung und in der Schulungsversion grundsätzlich möglich. Der für diese Zwecke zu verwendende API-Key ist in der YAML im Bereich **securitySchemes** bei **ApiKeyAuth** aufgeführt.

# API einrichten
Die Verwendung der API erfordert folgende Schritte.
- Installation der Anwendung
- Lizenzierung der API
- Einrichtung und Konfiguration des Webservice

Alle Informationen hierzu finden Sie in der Hilfe zu HS Auftragsbearbeitung unter „Anleitungen & Wissen / Datenaustausch / API (Webservice)“.

# Nutzung der API
In der Auftragsbearbeitung wird ein Webservice (Dienst) gestartet, der auf verschiedene Endpunkte (URLs) reagiert.

Der Webservice wird pro Datenbank/Mandant einmal gestartet, d.h. der Webservice ist mandantenbezogen. Wenn mehrere Datenbanken/Mandanten verwendet werden, muss für jeden Datenbestand/Mandant eine Instanz des Webservice gestartet werden. Werden die Instanzen auf einem
Rechner betrieben, unterscheidet sich der Aufruf nur im Port.

## Dokumentation
Die API ist in der Auszeichnungssprache YAML dokumentiert.

Die Schnittstellenbeschreibung kann auch in der Elements UI API direkt als GitHub-Page angezeigt werden unter folgender URL:
https://hamburger-software.github.io/ab-api/

Änderungen an der API (Changelog), die zu neuen Programmständen erfolgen, werden in der Datei https://hamburger-software.github.io/ab-api/changelog.txt protokolliert.

# Code-Generierung
Für die Code-Generierung aus der YAML-Datei kann z.B. der OpenApi Generator verwendet werden (https://openapi-generator.tech/docs/usage/).
Es werden diverse Programmiersprachen unterstützt. 

Die Code-Generierung kann mit dem Kommandozeilen-Tool "openapi-generator-cli" erfolgen. Für C#-Client-Code kann das z.B. so aussehen:
```bat
java -jar openapi-generator-cli-7.12.0.jar generate -i AB-API.yaml -g csharp --generate-alias-as-model --model-name-suffix Model -c config.json --http-user-agent MySolution -o C:\TEMP\Hs.Ab.RestApi
```

Sinnvoll ist die Verwendung eines eigenen Namespaces. Dieser kann über eine Config-Datei (z.B. config.json) angegeben werden, z.B.:
### config.json
```json
{
  "packageName": "Hs.Ab.RestApi.Generated",
  "targetFramework": "net48",
  "library": "restsharp"
}
```

Außerdem sollte der HTTP-User-Agent gesetzt werden. Das ist über die Kommandozeile möglich: 
```bat
--http-user-agent MySolution
```

### Generierter Code
Es wird der Programmcode und entsprechende UnitTest generiert. Der Programmcode ist in drei Bereiche
aufgeteilt.
- API: Enthält die Methoden für den Zugriff auf die konkreten Endpunkte
- Client: Implementierung des hSp-Clients etc.
- Model: Enthält die Models

# Besonderheiten zu Endpunktgruppen und Endpunkten
Hinweise zur Verwendung, der fachlichen Gegebenheiten und zu Besonderheiten sind umfassend in der Beschreibungsdatei enthalten. Verwenden Sie ein Tool wie z.B. Swagger Editor, um die Möglichkeiten optimal zu nutzen.

# Programmierbeispiel

## Aufruf Endpunkt „/appinfo“
Bei Nutzung der mit openapi-codegen-cli generierten C#-Klassen:
```csharp
var apiInstance = new Api.AppInfoApi("https://ab-server:9001/ab-api");
try
{
  var result = apiInstance.AppinfoGetWithHttpInfo();
}
catch (ApiException ex)
{
  // Fehlerbehandlung
}
```

## API-Key
Der API-Key muss beim Request im Header übermittelt werden. Unter Verwendung der mit openapi-codegen-cli generierten C#-Klassen kann man den API-Key bei der Konfiguration wie folgt setzen:
```csharp
const string apiKeyIdentifier = "X-API-KEY";
const string apiKeyValue = "ABCDEFGH-HIJKL-1234-5678-90MNOPQRSTUV";
configuration.AddApiKey(apiKeyIdentifier, apiKeyValue);
```

# Troubleshooting
## Protokollierung der API
Der Webservice schreibt auf dem Rechner, auf dem er gestartet wurde, ein Protokoll der API in das Verzeichnis %TEMP%\HsAbWebservice.

Wenn der Webservice als Dienst eingerichtet ist, wird das Protokoll der API unter %PROGRAMDATA%\Hs\Ab\Trace\Webservice gespeichert.

Bei Bedarf kann für jeden Webservice ein weiteres Protokoll aktiviert werden. In diesem werden dann die Daten protokolliert, die je nach Request übertragen werden.

Informationen hierzu finden Sie in der Hilfe „Anleitungen & Wissen / API (Webservice) / Protokollierung der Vorgänge“

## Test der API im Browser
Mit Hilfe eines Internet-Browser kann die Erreichbarkeit des Webservice geprüft werden, indem man die URL des Endpunkts „/appinfo/“ (z.B. https://localhost:9001/ab-api/appinfo) in die Adresszeile einträgt.

Wenn Sie kein „vorhandenes Zertifikat“ verwenden, können Sie den Hinweis auf eine unsichere Verbindung bestätigen und den Hinweis auf ein ungültiges Zertifikat ignorieren. Weitere Informationen dazu finden Sie in der Hilfe der Anwendung.

## Registrierung der Zusatzlösung
Im Arbeitsgebiet **LizenzCenter - Registrierte Lösungen** sind die registrierten Zusatzlösungen gelistet, die per API auf die HS Anwendung zugreifen dürfen. Per Doppelklick auf eine Lösung erhält man zusätzlich die Informationen, für welche Endpunktgruppen der Zugriff erlaubt ist und mit welchen Rechten (Lesen/Vollzugriff) diese freigeschaltet sind.

Ist die erwartete Zusatzlösung dort nicht aufgeführt, oder sind erwartete Rechte nicht freigeschaltet, ist die Lizenz zu aktualisieren oder ggf. die vertraglichen Vereinbarungen zur Nutzung der API mit HS – Hamburger Software GmbH & Co. KG zu prüfen. Für weitere Informationen wenden Sie sich an vertrieb@hamburger-software.de.

# Weitere Informationen
Weitere Informationen für Lösungsersteller gibt es
- in der Hilfe der Auftragsbearbeitung im Kapitel "Anleitung & Wissen / Datenaustausch / API (Webservice) / Anwendungswissen"
- in der Datei "ab_api_webservice.pdf" im Unterverzeichnis "Hilfe_Pdfs" des Programmverzeichnisses der installierten Auftragsbearbeitung (z.B. C:\Program Files (x86)\Hs\Ab) 
