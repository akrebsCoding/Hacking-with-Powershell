Da wir nun wissen, wie cmdlets nutzen können, werden wir mal ein Paar Dinge mit diesen anstellen. Bedenke das ***Get-Command*** und ***Get-Help*** deine besten Freunde sind.

# Using Get-Help

***Get-Help*** zeigt dir Informationen über ein cmdlet an. Um Hilfe zu einem Cmdlet zu erhalten gib folgendes ein:

>Get-Help Command-Name

Um den Befehl eventuell noch besser zu verstehen kannst du dem Get-Help Cmdlet noch ein -Example Argument mitgeben:

```bash
PS C:\Users\Administrator> Get-Help Get-Command -Examples

NAME
    Get-Command

SYNOPSIS
Gets all commands.

Example 1: Get cmdlets, functions, and aliases

PS C:\>Get-Command
```

# Using Get-Command

***Get-Command*** zeigt alle cmdlets an, die sich aktuell auf dem Rechner befinden. Das Tolle an diesem Befehl ist, dass man damit Pattern-Matching machen kann. Also nach bestimmten Kommandos suchen:

>Get-Command Verb-* oder Get-Command *-Noun

Wenn wir ***Get-Command New-**** eingeben, erhalten wir folgenden Output:

```bash
PS C:\Users\Administrator> Get-Command New-*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           New-AWSCredentials                                 3.3.563.1  AWSPowerShell
Alias           New-EC2FlowLogs                                    3.3.563.1  AWSPowerShell
Alias           New-EC2Hosts                                       3.3.563.1  AWSPowerShell
Alias           New-RSTags                                         3.3.563.1  AWSPowerShell
Alias           New-SGTapes                                        3.3.563.1  AWSPowerShell
Function        New-AutologgerConfig                               1.0.0.0    EventTracingManagement
Function        New-DAEntryPointTableItem                          1.0.0.0    DirectAccessClientComponents
Function        New-DscChecksum                                    1.1        PSDesiredStateConfiguration
Function        New-EapConfiguration                               2.0.0.0    VpnClient
Function        New-EtwTraceSession                                1.0.0.0    EventTracingManagement
Function        New-FileShare                                      2.0.0.0    Storage
Function        New-Fixture                                        3.4.0      Pester
Function        New-Guid                                           3.1.0.0    Microsoft.PowerShell.Utility
--cropped for brevity--
```

# Object Manipulation

Wie bereits erwähnt, sind die Ausgaben eines Cmdlets Objekte, auf die wir Aktionen ausführen können. Um das zu tun, müssen wir erstmal paar Dinge verstehen:

- Den Output eines cmdlets an ein anderes cmdlet übergeben
- Wie man spezielle Objekt Cmdlets nutzt, um Informationen zu extrahieren

Mit der Pipeline (  ***|***  ) können wir den Output an andere cmdlets übergeben. Im Gegensatz zu anderen Shells übergibt Powershell ein Objekt an das nächste cmdlet, also keinen Text oder String. Wie bei alle Objekt-Orientierten Frameworks enthält ein Objekt Methoden und Eigenschaften.

Du denkst vielleicht bei Methoden an Funktionen die man auf den Output eines cmdlets anwenden kann, und bei Eigenschaften an Variablen. Um diese Details zu sehen übergib den Output eines cmdlets mal an das ***Get-Member*** cmdlet.
>Verb-Noun | Get-Member

Ein Beispiel mit dem Get-Command:
```bash
PS C:\Users\Administrator> Get-Command | Get-Member -MemberType Method


   TypeName: System.Management.Automation.AliasInfo

Name             MemberType Definition
----             ---------- ----------
Equals           Method     bool Equals(System.Object obj)
GetHashCode      Method     int GetHashCode()
GetType          Method     type GetType()
ResolveParameter Method     System.Management.Automation.ParameterMetadata ResolveParameter(string name)
ToString         Method     string ToString()


   TypeName: System.Management.Automation.FunctionInfo

Name             MemberType Definition
----             ---------- ----------
Equals           Method     bool Equals(System.Object obj)
GetHashCode      Method     int GetHashCode()
GetType          Method     type GetType()
ResolveParameter Method     System.Management.Automation.ParameterMetadata ResolveParameter(string name)
ToString         Method     string ToString()


   TypeName: System.Management.Automation.CmdletInfo

Name             MemberType Definition
----             ---------- ----------
Equals           Method     bool Equals(System.Object obj)
GetHashCode      Method     int GetHashCode()
GetType          Method     type GetType()
ResolveParameter Method     System.Management.Automation.ParameterMetadata ResolveParameter(string name)
ToString         Method     string ToString()


PS C:\Users\Administrator>
```
Wie man in bei der Angabe des Flags -MemberType sehen kann, können wir zwischen Methoden und Eigenschaften wählen.

# Creating Objects From Previous cmdlets

Eine Möglichkeit Objekte zu manipulieren wäre, die Eigenschaften des Outputs eines cmdlets zu nehmen und ein neues Objekt daraus zu erstellen. Das macht man mit dem ***Select-Object*** Cmdlet.

Hier ein Beispiel wie man den Inhalt eines Ordners ausgibt bzw. nur den Mode und den Namen:

```bash
PS C:\Users\Administrator> Get-ChildItem | Select-Object -Property Mode, Name
Mode   Name
----   ----
d-r--- Contacts
d-r--- Desktop
d-r--- Documents
d-r--- Downloads
d-r--- Favorites
d-r--- Links
d-r--- Music
d-r--- Pictures
d-r--- Saved Games
d-r--- Searches
d-r--- Videos

PS C:\Users\Administrator>
```

Du kannst auch folgende Flags nutzen um bestimmte Informationen zu selektieren:

- first - gets the first x object
- last - gets the last x object
- unique - shows the unique objects
- skip - skips x objects

# Filtering Objects

Wir können auch die Objekte filtern, die uns ein cmdlet ausgibt. Mit dem ***Where-Object*** können wir nach dem Wert einer Eigenschaft filtern.

Das genrelle Format für dieses cmdlet ist:

>Verb-Noun | Where-Object -Property PropertyName -operator Value

>Verb-Noun | Where-Object {$_.PropertyName -operator Value}

Die zweite Version mit dem $_ Operator iteriert durch jedes Objekt das dem Whre-Object cmdlet übergeben wurde.

### Powershell ist ein wenig sensibel was die Syntax angeht, daher nutze keine Klammern für diesen Befehl!

Hier ist ***-operator*** eine Liste folgender Operatoren:

- -containts: wenn ein Item in den Eigenschaften dem entsprechenden Wert entspricht
- -EQ: wenn der Wert der Eigenschaft dem entsprechenden Wert entspricht
- -GT: wenn der Wert der Eigenschaft größer als der entsprechende Wert ist

Eine komplette Liste der Operatoren findest du [hier](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-6)

Hier ein Beispiel wie man sich alle gestoppten Prozesse ansehen kann:

```bash
PS C:\Users\Administrator> Get-Service | Where-Object -Property Status -eq Stopped

Status   Name               DisplayName
------   ----               -----------
Stopped  AJRouter           AllJoyn Router Service
Stopped  ALG                Application Layer Gateway Service
Stopped  AppIDSvc           Application Identity
Stopped  AppMgmt            Application Management
Stopped  AppReadiness       App Readiness
Stopped  AppVClient         Microsoft App-V Client
Stopped  AppXSvc            AppX Deployment Service (AppXSVC)
Stopped  AudioEndpointBu... Windows Audio Endpoint Builder
Stopped  Audiosrv           Windows Audio
Stopped  AxInstSV           ActiveX Installer (AxInstSV)
Stopped  BITS               Background Intelligent Transfer Ser...
Stopped  Browser            Computer Browser
Stopped  bthserv            Bluetooth Support Service
-- cropped for brevity--
```

# Sort Object

Wenn man sehr viel Output hat, kann man sich diesen sortieren lassen. Das kannst du, in dem den Output zum ***Sort-Object*** cmdlet pipest. 

>Verb-Noun | Sort-Object

Beispiel für die Sortierung des Inhalts eines Ordners:

```bash
PS C:\Users\Administrator> Get-ChildItem | Sort-Object
    Directory: C:\Users\Administrator
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-r---        10/3/2019   5:11 PM                Contacts
d-r---        10/5/2019   2:38 PM                Desktop
d-r---        10/3/2019  10:55 PM                Documents
d-r---        10/3/2019  11:51 PM                Downloads
d-r---        10/3/2019   5:11 PM                Favorites
d-r---        10/3/2019   5:11 PM                Links
d-r---        10/3/2019   5:11 PM                Music
d-r---        10/3/2019   5:11 PM                Pictures
d-r---        10/3/2019   5:11 PM                Saved Games
d-r---        10/3/2019   5:11 PM                Searches
d-r---        10/3/2019   5:11 PM                Videos
PS C:\Users\Administrator>
```

Da du nun weißt wie man Grundlegend mit Powershell arbeitet, werden wir paar Befehle durchgehen.

---

Answer the questions below:

1. What is the location of the file "interesting-file.txt"

    **c:\program files**

    ```bash
    PS > Get-ChildItem -Path C:\ -Filter "interesting-file.txt.txt" -Recurse
    ```

2. Specify the contents of this file

    **notsointerestingcontent**
    >PS > Get-Content "interesting-file.txt.txt"

3. How many cmdlets are installed on the system(only cmdlets, not functions and aliases)?

    **6638**
    ```bash
    PS > Get-Command | Where-Object -Property commandtype -eq cmdlet | Measure-Object
    ```

4. Get the MD5 hash of interesting-file.txt

    **49A586A2A9[   .....  PLATZHALTER  ......      ]8A1B4CEC6FAB329**

    >PS > Get-FileHash .\interesting-file.txt.txt -Algorithm md5

5. What is the command to get the current working directory?

    **Get-Location**

6. Does the path "C:\Users\Administrator\Documents\Passwords" Exist (Y/N)?

    **N**

    >PS > Test-Path "C:\Users\Administrator\Documents\Passwords"

7. What command would you use to make a request to a web server?

    **Invoke-WebRequest**

8. Base64 decode the file b64.txt on Windows.

    **iho[XXXXXXXXX]windows**

    ```bash

    $dateiPfad = "C:\pfad\zu\b64.txt"
    $base64String = Get-Content -Path $dateiPfad -Raw
    $bytes = [System.Convert]::FromBase64String($base64String)
    $decodierterText = [System.Text.Encoding]::UTF8.GetString($bytes)
    Write-Output $decodierterText

    ```




