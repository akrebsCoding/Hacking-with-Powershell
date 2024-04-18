Okay einzelne Powershell Befehle kennen wir jetzt, also lass uns das Scripten anschauen.

In dieser Aufgabe nutzen wie Powershell ISE, ein Powershell Text Editor. Schauen wir uns mal ein kleines Szenario an wie man ein Script nutzen könnte. Gegeben ist eine Liste von Ports die wir nutzen möchten um zu sehen, ob ein Lokaler Port auf Listening steht. Wir öffnen auf der Maschine das listening-ports.ps1 Script auf dem Desktop mit Powershell ISE. Powershell Scripte haben normalerweise immer die Dateiendung *.ps1.

```powershell
$system_ports = Get-NetTCPConnection -State Listen

$text_port = Get-Content -Path C:\Users\Administrator\Desktop\ports.txt

foreach($port in $text_port){

    if($port -in $system_ports.LocalPort){
        echo $port
     }

}
```

In der ersten Zeile speichern wir alle Ports die auf Listening sind in der Variable ***$system_ports***. Das machen wir mit dem *cmdlet* `Get-NetTCPConnection`

Das Format zum erzeugen einer Variablen ist also:

`$variable_name = value`


In der zweiten Zeile speichern wir dann den Inhalt einer Datei, die eine Liste mit Ports enthält, in der Variable `$text_port`. Das machen wir mit dem *cmdlet* `Get-Content`. 
Daraufhin wird in der nächsten Zeile wird durch diese Liste iteriert:

```ps
foreach($new_var in $existing_var){}
```

Die einzelne Code Block dient dazu, durch eine Anzahl an Objekten zu loopen. Dabei vergleichen wir jeden Port mit den Ports die lokal auf Listening stehen. Anstatt eine weitere *for* Schleife zu verwenden, nutzen wir ein *if* Statement mit dem `-in` Operator um zu schauen ob der Port sich in der `LocalPort` Eigenschaft (bzw. Spalte) eines Objektes befindet. Ein komplette Liste der *if* Statement Vergleichsoperatoren findest du [hier](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-6). Wir starten das Script indem wir oben in der Menüleiste den grünen Play-Button drücken.

Wir werden jetzt versuchen, ein eigenes Powershell Script zu schreiben. Dazu befindet sich auf dem Desktop ein Ordner mit dem Namen Emails. Darin befinden sich Kopien von Emails von John, Martha und Mary, die sie sich gegenseitig geschickt haben. Mit dem Script beantworten wir folgende Fragen:

Answer the questions below
---

1. What file contains the password?

    **doc3m**

    ```ps

    Mit diesem Script kann ich sämtliche Ordner und Dateien durchsuchen.


    $ordnerpfad = "C:\Users\Administrator\Desktop\emails"
    $suchwort = "password"

    Get-ChildItem -Path $ordnerpfad -recurse -file | ForEach-Object {

        $gefunden = Get-Content -path $_.FullName | Select-String -Pattern $suchwort -quit

        if ($gefunden) {
            Write-Host "Datei: $($_.FullName)"
            Get-Conent -path $_.FullName | Select-String -Pattern $suchwort
            Write-Host "---------------------"
        }

    }
    ```

2. What is the password?

    **johnisalegend99**

3. What files contains an HTTPS link?

    **Doc2Mary**

    <span style="background-color: grey; color: black;">Einfach das <span style="background-color:grey; color:cyan;">$suchwort</span> oben im Script von `password` in `http` ändern</span>
