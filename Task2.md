Powershell ist eine Windows Scripting Sprache und eine Shell Umgebung die das .NET Framework nutzt.

Das heißt, Powershell kann .NET Funktionen direkt innerhalb seiner Shell ausführen. Die meisten Powershell Kommandos, die sogenannten ***cmdlets***, sind in .NET geschrieben.
Anders als bei anderen Scripting Sprachen und Shell Umgebungen sind die Ausgaben dieser ***cmdlets*** sogenannte Objekte. Das bedeutet Powershell ist Objekt-Orientiert.

Was bedeutet das? Im Prinzip kann man also auf den Output dieser cmdlets, also auf die Objekte, bestimmte Aktionen ausführen. Es ist also auch möglich, den Output eines cmdlets einem anderen cmdlet zu übergeben. Normalerweise ist das Format eines cmdlets immer so aufgebaut, dass es ein Verb mit einem Nomen verbindet. Zum Beispiel das cmdlet um alle Befehle auszugeben lautet ***Get-Command***. In dem Fall ist Get das Verb, und Command das Nomen.

Gängige Verben die genutzt werden sind:

- Get
- Start
- Stop 
- Read
- Write
- New
- Out

Wenn du gerne alle verfügbaren Verben suchst, schau mal [hier](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7)


Answer the questions below
---
What ist the command to ***get*** a ***new*** object?

**get-new**




