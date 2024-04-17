Der erste Schritt den man machen sollte, wenn man Zugang zu einem System erhält ist Enumeration. 

Wir enumerieren folgendes:

- Benutzer
- Grundlegende Netzwerk Informationen
- Datei Berechtigungen
- Registry Berechtigungen
- Geplante und Laufende Tasks
- Unsichere Dateien

Wie beantworten folgende Fragen:

1. How many Users are there on the machine?

    **5**
    >PS > Get-LocalUser

2. Which local user does this SID(S-1-5-21-1394777289-3961777894-1791813945-501) belong to?

    >PS > Get-LocalUser | Select-Object Name, SID

3. How many users have their password required values set to False?

    **4**

    ```bash
    PS > Get-LocalUser | Where-Object PasswordRequired -eq $False | Measure-Object
    ```

4. How many local groups exist?

    **24**
    >PS > Get-LocalGroup | Measure-Object

5. What command did you use to get the IP address info?

    **Get-NetIPAdress**

6. How many ports are listed as listening?

    **20**
    >PS > Get-NetTCPConnection | Where-Object state -eq Listen | Measure-Object

7. What is the remote address of the local port listening on port 445?

    **::**
    >PS > Get-NetTCPConnection | Where-Object LocalPort -eq 445

8. How many patches have been applied?

    **20**
    >PS > Get-HotFix | Measure-Object

9. When was the patch with ID KB4023834 installed?

    **6/15/2017 12:00:00 AM**
    >PS > Get-HotFix | Where-Object HotFixID -eq KB4023834 | Sekect-Object InstalledOn

10. Find the contents of a backup file.

    **backpassflag**
    >PS > Get-ChildItem -Path C:\ -Filter "* .bak *" -Recurse

11. Search for all files containing API_KEY

    **fakekey123**
    >PS > Get-ChildItem C:\* -Recurse | Select-String -pattern API_KEY

12. What command do you do to list all the running processes?

    **Get-Process**

13. What is the path of the scheduled task called new-sched-task?

    **/**
    >PS > Get-ScheduledTask | Where-Object taskname -eq new-sched-task

14. Who is the owner of the C:\

    **nt service\trustedinstaller**
    >PS > Get-ACL -Path c:\

