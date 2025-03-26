# AB API
In diesem Repository werden die Beschreibung der API zur HS Auftragsbearbeitung (YAML-Datei) und der Changelog veröffentlicht.

# Elements UI API
Die Schnittstellenbeschreibung kann auch in der Elements UI API direkt als GitHub-Page angezeigt werden unter folgender URL:
https://hamburger-software.github.io/ab-api/

# Code-Generierung
Für die Code-Generierung aus der YAML-Datei kann z.B. der OpenApi Generator verwendet werden (https://openapi-generator.tech/docs/usage/).
Es werden diverse Programmiersprachen unterstützt. 

Die Code-Generierung kann mit dem Kommandozeilen-Tool "openapi-generator-cli" erfolgen. Für C#-Client-Code kann das z.B. so aussehen:
> java -jar openapi-generator-cli-7.3.0.jar generate -i AB-API.yaml -g csharp --generate-alias-as-model --model-name-suffix Model -c config.json --http-user-agent MySolution -o C:\TEMP\Hs.Ab.RestApi

Sinnvoll ist die Verwendung eines eigenen Namespaces. Dieser kann über eine Config-Datei (z.B. config.json) angegeben werden, z.B.:
### config.json
```
{
  "packageName": "Hs.Ab.RestApi.Generated",
  "targetFramework": "net48"
}
```

Außerdem sollte der HTTP-User-Agent gesetzt werden. Das ist über die Kommandozeile möglich: 
> --http-user-agent MySolution

# Weitere Informationen
Weitere Informationen für Lösungsersteller sind zu finden in der Hilfe der Auftragsbearbeitung im Kapitel "Anleitung & Wissen / Datenaustausch / API (Webservice) / Anwendungswissen".

Zusätzlich befindet sich im Programmverzeichnis der installierten Auftragsbearbeitung (z.B. C:\Program Files (x86)\Hs\Ab) im Unterverzeichnis "Hilfe_Pdfs" die Datei "ab_api_webservice.pdf" mit allen Informationen zur Einrichtung des Webservice.
