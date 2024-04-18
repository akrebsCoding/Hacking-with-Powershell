Da wir nun das Scripten können (okay mit Tools wie ChatGPT ist es echt machbar) können wir etwas interessanteres schreiben.
Wir haben nicht immer so Dinge wie Nmap oder Python zur Verfügung um rudimentäre Aufgaben auszuführen. 

Wieso also nicht einfach einen Port Scanner in Powershell schreiben? Wie gehen vor?

- Lege über eine Benutzereingabe ein IP Bereich fest, der gescannt werden soll.
- Lege über eine Benutzereingabe einen Portbereich fest, der gescannt werden soll.
- Lege über eine Benutzereingabe fest, welche Typ von Scan durchgeführt werden soll. In unserem Fall ein einfacher TCP Scan.

```powershell
# PowerShell Port Scanner 

# Funktion zum Scannen der Ports
function Scan-Ports {
    param(
        [string]$targetIP,
        [int]$startPort,
        [int]$endPort,
        [string]$scanType
    )

    # Erstellen eines leeren Arrays zum Speichern der Scanergebnisse
    $results = @()

    # Loop durch den Portbereich und Scannen jedes Ports
    for ($port = $startPort; $port -le $endPort; $port++) {
        # Versuche, den Port zu öffnen
        $tcpClient = New-Object System.Net.Sockets.TcpClient
        $portResult = $tcpClient.BeginConnect($targetIP, $port, $null, $null)
        $portResult.AsyncWaitHandle.WaitOne(100, $false)
        
        # Überprüfe, ob der Port geöffnet ist
        if ($tcpClient.Connected) {
            $results += "Der Port $($port) ist offen"
        } else {
            $results += "Der Port $($port) ist geschlossen"
        }
        $tcpClient.Close()
    }

    # Ergebnisse anzeigen
    Write-Host "`nScan Results:`n"
    $results | ForEach-Object { Write-Host $_ }
}

# Hauptprogramm
function Main {
    # Willkommensnachricht
    Write-Host "Willkommen zum PowerShell-Port-Scanner!`n"

    # Benutzereingabe für Ziel-IP-Adresse
    $targetIP = Read-Host "1. Geben Sie die Ziel-IP-Adresse ein, die gescannt werden soll:"

    # Benutzereingabe für Portbereich
    $startPort = Read-Host "2. Geben Sie den Start-Port des zu scannenden Bereichs ein:"
    $endPort = Read-Host "   Geben Sie den End-Port des zu scannenden Bereichs ein:"

    # Benutzereingabe für Scantyp (nur TCP-Scan wird unterstützt)
    $scanType = "TCP"

    # Benutzereingabe, um den Scan zu starten
    $startPrompt = Read-Host "Möchten Sie den Scan starten? (ja/nein)"

    # Überprüfe, ob der Benutzer den Scan starten möchte
    if ($startPrompt -eq "ja") {
        # Scan starten
        Scan-Ports -targetIP $targetIP -startPort $startPort -endPort $endPort -scanType $scanType
    } else {
        Write-Host "Scan abgebrochen."
    }
}

# Programm ausführen
$continue = $true
while ($continue) {
    $prompt = Read-Host "Bist du bereit, das Programm zu starten? Gib 'start' ein, um fortzufahren, oder 'exit', um das Programm zu beenden."
    switch ($prompt) {
        "start" { Main }
        "exit" { $continue = $false }
        default { Write-Host "Ungültige Eingabe. Bitte gib 'start' oder 'exit' ein." }
    }
}
```