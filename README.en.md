# AB API
German: [README.de.md](README.de.md)

In this repository, the description of the API for HS Auftragsbearbeitung (YAML file) and the changelog are published.

## Terms of Use
A prerequisite for using the API in an add-on solution is the conclusion of a contract for API usage with HS – Hamburger Software GmbH & Co. KG.

Usage is subject to a fee and is based in part on the scope of the desired usage (number and type of required endpoint groups, scope of access rights, intensity/volume of usage) as well as the deployment scenario. 
For further information, please contact vertrieb@hamburger-software.de.

# Scope of Functions
The API is provided with the application. The API implementation complies with the OpenAPI 3.0 standard.

**REST** (**Re**presentational **St**ate **T**ransfer) and **JSON** (**J**ava**S**cript **O**bject **N**otation) are used as technologies.

## Security
The API is provided as a Backend API. For securing it, HTTPS with generated and custom certificates is fundamentally available.

The caller must authenticate using the credentials of an HS Auftragsbearbeitung user via Basic or OAuth2 authentication. During this process, the user's functional permissions are also verified.

Additionally, the creator of an add-on solution and the API operator (e.g., HS customer, hoster) are responsible for securing the API against unauthorized access (firewall, API gateway, etc.).

## Authorization
To secure access for an add-on solution, API keys are used for authorization of the solutions. Each solution receives its own API key, which may only be used exclusively for that solution. The API key is sent after the contract for using the HS API has been concluded. Passing on an API key is not permitted.

The license of the HS application in which the add-on solution is deployed must be updated accordingly so that the license key contains the API key (Work area LizenzCenter - Lizenz aktualisieren).

The API key must be implemented accordingly in the add-on solution. API accesses without an API key are rejected as "Unauthorized". An API key is bound to endpoint groups. This means that with an API key, access is only possible to endpoints within the contractually agreed endpoint groups.

In the work area **LizenzCenter - Registrierte Lösungen** , the registered add-on solutions that are allowed to access the HS application via API are listed. By double-clicking on a solution, additional information is provided on which endpoint groups access is permitted and with which rights (Read/Full access) they are enabled.

### Demo Data / Training Version
Using the API for exploration and testing purposes is fundamentally possible in the sample data of the HS application and in the training version.
The API key to be used for these purposes is listed in the YAML under **securitySchemes** at **ApiKeyAuth**.

# API Setup
Using the API requires the following steps:
- Installation of the application
- Licensing of the API
- Setup and configuration of the webservice

All information on this can be found in the HS order processing help under  „Anleitungen & Wissen / Datenaustausch / API (Webservice)“.

# Usage of the API
In HS Auftragsbearbeitung, a webservice (Windows-service) is started that responds to various endpoints (URLs).

The webservice is started once per database/tenant, meaning it is tenant-specific. If multiple databases/tenants are used, an instance of the webservice must be started for each database/tenant. If the instances are operated on one computer, the call differs only in the port.

## Documentation
The API is documented in the YAML markup language.

The interface description can also be displayed directly as a GitHub Page in the Elements UI API at the following URL: https://hamburger-software.github.io/ab-api/.

Changes to the API (changelog) that occur with new program versions are logged in the file https://hamburger-software.github.io/ab-api/changelog.txt.

# Code-Generation
For code generation from the YAML file, the OpenAPI Generator can be used, for example (https://openapi-generator.tech/docs/usage/).
Various programming languages are supported.
​
Code generation can be performed using the command-line tool "openapi-generator-cli". For C# client code, it might look like this
```bat
java -jar openapi-generator-cli-7.12.0.jar generate -i AB-API.yaml -g csharp --generate-alias-as-model --model-name-suffix Model -c config.json --http-user-agent MySolution -o C:\TEMP\Hs.Ab.RestApi
```

Using a custom namespace is recommended. This can be specified via a config file (e.g., config.json), for example:
### config.json
```json
{
  "packageName": "Hs.Ab.RestApi.Generated",
  "targetFramework": "net48",
  "library": "restsharp"
}
```

Additionally, the HTTP User-Agent should be set. This is possible via the command line:
```bat
--http-user-agent MySolution
```

### Generated Code
The generated program code and corresponding unit tests are divided into three areas:
- API: Contains the methods for accessing the specific endpoints
- Client: Implementation of the hSp client, etc.
- Model: Contains the models

# Special Features of Endpoint Groups and Endpoints
Notes on usage, business requirements, and special features are comprehensively included in the description file. Use a tool like Swagger Editor to optimally utilize the possibilities.

# Code example

## Calling endpoint „/appinfo“
When using the C# classes generated with openapi-generator-cli:
```csharp
var apiInstance = new Api.AppInfoApi("https://ab-server:9001/ab-api");
try
{
  var result = apiInstance.AppinfoGetWithHttpInfo();
}
catch (ApiException ex)
{
  // Error handling
}
```

## API-Key
The API key must be transmitted in the request header. When using the C# classes generated with openapi-generator-cli, the API key can be set in the configuration as follows:
```csharp
const string apiKeyIdentifier = "X-API-KEY";
const string apiKeyValue = "ABCDEFGH-HIJKL-1234-5678-90MNOPQRSTUV";
configuration.AddApiKey(apiKeyIdentifier, apiKeyValue);
```

# Troubleshooting
## API Logging
The webservice writes an API log on the computer where it was started to the directory %TEMP%\HsAbWebservice.

If the webservice is set up as a service, the API log is saved under %PROGRAMDATA%\Hs\Ab\Trace\Webservice.

If needed, an additional log can be activated for each webservice, which records the data transmitted depending on the request. 

Information on this can be found in the help. „Anleitungen & Wissen / API (Webservice) / Protokollierung der Vorgänge“

## Testing the API in the Browser
The reachability of the webservice can be checked using an internet browser by entering the URL of the "/appinfo/" endpoint (e.g. https://localhost:9001/ab-api/appinfo) into the address bar.

If no "existing certificate" is shown, the warning about an insecure connection can be confirmed and the notice about an invalid certificate can be ignored. Further information can be found in the application help.

## Registration of the Add-on Solution
In the work area **LizenzCenter - Registrierte Lösungen**, the registered add-on solutions that are permitted to access the HS application via API are listed. By double-clicking on a solution, additional information is provided on which endpoint groups access is allowed and with which rights (Read/Full access) they are enabled.

If the expected add-on solution is not listed there, or if expected rights are not enabled, the license must be updated or the contractual agreements for API usage with HS – Hamburger Software GmbH & Co. KG must be checked. For further information, contact vertrieb@hamburger-software.de.

#Further Information
Further information for solution developers is available
- in the help of HS Auftragsbearbeitung under the chapter "Anleitung & Wissen / Datenaustausch / API (Webservice) / Anwendungswissen"
- in the file "ab_api_webservice.pdf" in the "Hilfe_Pdfs" subfolder of the installed order processing program directory (e.g. C:\Program Files (x86)\Hs\Ab)
