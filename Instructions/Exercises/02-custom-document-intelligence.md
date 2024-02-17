---
lab:
  title: Extrahieren von Daten aus Formularen
  module: Module 6 - Document Intelligence
---

# Extrahieren von Daten aus Formularen

Angenommen, in einem Unternehmen müssen Mitarbeiter*innen die Daten von Bestellungen manuell in eine Datenbank eingeben. Das Unternehmen möchte KI-Dienste nutzen, um den Dateneingabeprozess zu verbessern. Sie planen, ein Machine Learning-Modell zu erstellen, das das Formular liest und strukturierte Daten erzeugt, die zum automatischen Aktualisieren einer Datenbank verwendet werden können.

**Azure KI Dokument Intelligenz** ist ein Azure KI-Dienst, mit dem Benutzer*innen Software zur automatisierten Datenverarbeitung erstellen können. Diese Software kann Text, Schlüssel-Wert-Paare und Tabellen mithilfe der optischen Zeichenerkennung (Optical Character Recognition, OCR) aus Formulardokumenten extrahieren. Azure KI Dokument Intelligenz verfügt über vordefinierte Modelle zur Erkennung von Rechnungen, Quittungen und Visitenkarten. Der Dienst bietet auch die Funktionalität zum Trainieren von benutzerdefinierten Modellen. In dieser Übung konzentrieren wir uns auf das Erstellen benutzerdefinierter Modelle.

## Vorbereiten der Entwicklung einer App in Visual Studio Code

Nun verwenden wir das Dienst-SDK zum Entwickeln einer App mit Visual Studio Code. Die Codedateien für Ihre App wurden in einem GitHub-Repository bereitgestellt.

> **Tipp**: Wenn Sie das Repository **mslearn-ai-document-intelligence** bereits geklont haben, öffnen Sie es in Visual Studio Code. Führen Sie andernfalls die folgenden Schritte aus, um es in Ihrer Entwicklungsumgebung zu klonen.

1. Starten Sie Visual Studio Code.
1. Öffnen Sie die Palette (UMSCHALT+STRG+P), und führen Sie einen **Git: Clone**-Befehl aus, um das Repository `https://github.com/MicrosoftLearning/mslearn-ai-document-intelligence` in einen lokalen Ordner zu klonen (der Ordner ist beliebig).
1. Nachdem das Repository geklont wurde, öffnen Sie den Ordner in Visual Studio Code.
1. Warten Sie, während zusätzliche Dateien zur Unterstützung der C#-Codeprojekte im Repository installiert werden.

    > **Hinweis**: Wenn Sie aufgefordert werden, erforderliche Ressourcen zum Erstellen und Debuggen hinzuzufügen, wählen Sie **Not now** (Jetzt nicht) aus. Wenn die Meldung *Azure Functions-Projekt im Ordner erkannt* angezeigt wird, können Sie diese Meldung einfach schließen.

## Erstellen einer Ressource für Azure KI Dokument Intelligenz

Um den Dienst „Azure KI Dokument Intelligenz“ zu nutzen, benötigen Sie eine Azure KI Dokument Intelligenz- oder Azure KI Services-Ressource in Ihrem Azure-Abonnement. Über das Azure-Portal erstellen Sie eine Ressource.

1. Öffnen Sie auf einer Browserregisterkarte das Azure-Portal unter `https://portal.azure.com`, und melden Sie sich mit dem Microsoft-Konto an, das Ihrem Azure-Abonnement zugeordnet ist.
1. Navigieren Sie im Azure-Portal auf der Homepage zum oberen Suchfeld, geben Sie **Dokumentintelligenz** ein, und drücken Sie dann die **Eingabetaste**.
1. Wählen Sie auf der Seite **Dokumentintelligenz** die Option **Erstellen** aus.
1. Verwenden Sie auf der Seite **Dokumentintelligenz erstellen** Folgendes, um Ihre Ressource zu konfigurieren:
    - **Abonnement**: Ihr Azure-Abonnement.
    - **Ressourcengruppe**: Wählen Sie eine Ressourcengruppe aus oder erstellen Sie eine mit einem eindeutigen Namen wie *DocIntelligenceResources*.
    - **Region**: Wählen Sie eine Region in Ihrer Nähe aus.
    - **Name**: Geben Sie einen global eindeutigen Namen ein.
    - **Tarif**: Wählen Sie **Kostenlos F0** aus (wenn Sie über keinen Free-Tarif verfügen, wählen Sie **Standard S0** aus).
1. Wählen Sie **Überprüfen + erstellen** und dann **Erstellen** aus. Warten Sie, während Azure die Azure KI Dokument Intelligenz-Ressource bereitstellt.
1. Wählen Sie nach Abschluss der Bereitstellung **Zu Ressource wechseln** aus, um die Seite **Übersicht** der Ressource anzuzeigen. 

## Sammeln von Dokumenten für das Training

Sie verwenden Beispielformulare wie dieses, um ein Modell zu trainieren: 

![Abbildung einer in diesem Projekt verwendeten Rechnung.](../media/Form_1.jpg)

1. Kehren Sie zu **Visual Studio Code** zurück. Öffnen Sie im Bereich *Explorer* den Ordner **Labfiles/02-custom-document-intelligence**, und erweitern Sie den Ordner **sample-forms**. Beachten Sie, dass der Ordner Dateien enthält, deren Namen auf **.json** und **.jpg** enden.

    Sie verwenden die **JPG**-Dateien, um Ihr Modell zu trainieren.  

    Die **JSON**-Dateien wurden automatisch generiert und enthalten Bezeichnungsinformationen. Die Dateien werden zusammen mit den Formularen in Ihren Blob Storage-Container hochgeladen.

    Sie können die Bilder anzeigen, die wir im Ordner *sample-forms* verwenden, indem Sie sie in Visual Studio Code auswählen.

1. Kehren Sie zum **Azure-Portal** zurück, und navigieren Sie zur Seite **Übersicht** Ihrer Ressource, wenn sie noch nicht angezeigt wird. Zeigen Sie im Abschnitt *Essentials* die **Ressourcengruppe** an, in der Sie die Dokument Intelligenz-Ressource erstellt haben.

1. Notieren Sie sich auf der Seite **Übersicht** für Ihre Ressourcengruppe die **Abonnement-ID** und den **Speicherort**. Sie benötigen diese Werte zusammen mit dem Namen der **Ressourcengruppe** in den nachfolgenden Schritten.

    ![Ein Beispiel für die Ressourcengruppenseite.](../media/resource_group_variables.png)

1. Wechseln Sie in Visual Studio Code im Bereich *Explorer* zum Ordner **Labfiles/02-custom-document-intelligence**, und erweitern Sie je nach Ihrer bevorzugten Sprache den Ordner **CSharp** oder **Python**. Jeder Ordner enthält die sprachspezifischen Dateien für eine App, in die Sie Azure OpenAI-Funktionen integrieren möchten.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **CSharp** oder **Python**, der Ihre Codedateien enthält, und wählen Sie die Option zum **Öffnen eines integrierten Terminals** aus. 

1. Führen Sie den folgenden Befehl im Terminal aus, um sich bei Azure anzumelden: Der Befehl **az login** öffnet den Microsoft Edge-Browser. Melden Sie sich mit dem gleichen Konto an, das Sie zum Erstellen der Azure AI Dokument Intelligenz-Ressource verwendet haben. Sobald Sie angemeldet sind, schließen Sie das Browserfenster.

    ```powershell
    az login
    ```

1. Kehren Sie zu Visual Studio Code zurück. Führen Sie im Terminalbereich den folgenden Befehl aus, um die Azure-Standorte aufzulisten.

    ```powershell
    az account list-locations -o table
    ```

1. Suchen Sie in der Ausgabe nach dem Wert **Name**, der dem Standort Ihrer Ressourcengruppe entspricht (für *USA, Osten* ist der entsprechende Name beispielsweise *eastus).*

    > **Wichtig**: Zeichnen Sie den Wert **Name** auf, und verwenden Sie ihn in Schritt 11.

1. Wählen Sie in Visual Studio Code im Ordner **Labfiles/02-custom-document-intelligence** das Skript **setup.cmd** aus. Sie verwenden dieses Skript, um die Befehle der Azure-Befehlszeilenschnittstelle (CLI) auszuführen, die zum Erstellen der anderen Azure-Ressourcen erforderlich sind.

1. Überprüfen Sie im Skript **setup.cmd** die Befehle. Das Programm erledigt folgende Aufgaben:
    - Erstellen eines Speicherkontos in Ihrer Azure-Ressourcengruppe
    - Hochladen von Dateien aus Ihrem lokalen Ordner *sampleforms* in den Container *sampleforms* im Speicherkonto
    - Drucken eines SAS-URIs (Shared Access Signature)

1. Ändern Sie die Variablendeklarationen **subscription_id**, **resource_group** und **location** mit den entsprechenden Werten für das Abonnement, die Ressourcengruppe und den Standort, an dem Sie die Dokument Intelligenz-Ressource bereitgestellt haben.
**Speichern** Sie dann Ihre Änderungen.

    Behalten Sie die Variable **expiry_date** für die Übung unverändert bei. Diese Variable wird beim Generieren des SAS-URIs (Shared Access Signature) verwendet. In der Praxis sollten Sie ein geeignetes Ablaufdatum für Ihre SAS festlegen. Weitere Informationen zu SAS finden Sie [hier](https://docs.microsoft.com/azure/storage/common/storage-sas-overview#how-a-shared-access-signature-works).  

1. Geben Sie im Terminal für den Ordner **Labfiles/02-custom-document-intelligence** den folgenden Befehl ein, um das Skript auszuführen:

    ```PowerShell
    $currentdir=(Get-Item .).FullName
    cd ..
    ./setup.cmd
    cd $currentdir

    ```

1. Überprüfen Sie nach Abschluss des Skripts die angezeigte Ausgabe.

1. Aktualisieren Sie im Azure-Portal Ihre Ressourcengruppe, und vergewissern Sie sich, dass sie das soeben erstellte Azure-Speicherkonto enthält. Öffnen Sie das Speicherkonto, und klicken Sie im Bereich auf der linken Seite auf den **Speicherbrowser**. Erweitern Sie dann im Speicherbrowser den Eintrag **Blobcontainer**, und wählen Sie den Container **sampleforms** aus, um zu überprüfen, ob die Dateien aus Ihrem lokalen Ordner **02-custom-document-intelligence/sample-forms** hochgeladen wurden.

## Trainieren des Modells mit Dokument Intelligenz Studio

Jetzt trainieren Sie das Modell mithilfe der Dateien, die in das Speicherkonto hochgeladen wurden.

1. Navigieren Sie in Ihrem Browser zu Dokument Intelligenz Studio unter `https://documentintelligence.ai.azure.com/studio`.
1. Scrollen Sie nach unten zum Abschnitt **Benutzerdefinierte Modelle**, und wählen Sie die Kachel **Benutzerdefiniertes Extraktionsmodell** aus.
1. Wenn Sie aufgefordert werden, sich bei Ihrem Konto anzumelden, geben Sie Ihre Azure-Anmeldeinformationen an.
1. Wenn Sie gefragt werden, welche Azure KI Dokument Intelligenz-Ressource verwendet werden soll, wählen Sie das Abonnement und den Ressourcennamen aus, die Sie beim Erstellen der Azure KI Dokument Intelligenz-Ressource verwendet haben.
1. Wählen Sie unter **Meine Projekte** die Option **+ Projekt erstellen** aus. Verwenden Sie die folgenden Konfigurationen:

    - **Projektname**: Geben Sie einen eindeutigen Namen ein.
        - Wählen Sie *Continue* (Weiter) aus.
    - **Dienstressource konfigurieren**: Wählen Sie das Abonnement, die Ressourcengruppen und die Dokument Intelligenz-Ressource aus, die Sie zuvor in diesem Lab erstellt haben. Aktivieren Sie das Kontrollkästchen *Als Standard festlegen*. Behalten Sie die Standard-API-Version bei. 
        - Wählen Sie *Continue* (Weiter) aus.
    - **Verbindung mit der Trainingsdatenquelle herstellen**: Wählen Sie das Abonnement, die Ressourcengruppe und das Speicherkonto aus, das vom Setupskript erstellt wurde. Aktivieren Sie das Kontrollkästchen *Als Standard festlegen*. Wählen Sie den Blobcontainer `sampleforms` aus, und lassen Sie den Ordnerpfad leer.
        - Wählen Sie *Continue* (Weiter) aus.
    - Wählen Sie *Projekt erstellen* aus.

1. Wenn Ihr Projekt erstellt wurde, wählen Sie **Trainieren** aus, um Ihr Modell zu trainieren. Verwenden Sie die folgenden Konfigurationen:
    - **Modell-ID**: *Geben Sie einen global eindeutigen Namen an (im nächsten Schritt benötigen Sie den Modell-ID-Namen).* 
    - **Erstellungsmodus**: Wählen Sie „Vorlage“ aus.
1. Das Training kann einige Zeit dauern. Sie erhalten eine Benachrichtigung, wenn der Vorgang abgeschlossen ist.

## Testen Ihres benutzerdefinierten Dokument Intelligenz-Modells

1. Kehren Sie zu Visual Studio Code zurück. Installieren Sie im Terminal das Dokument Intelligenz-Paket, indem Sie den entsprechenden Befehl für Ihre bevorzugte Sprache ausführen:

    **C#:**

    ```powershell
    dotnet add package Azure.AI.FormRecognizer --version 4.1.0
    ```

    **Python**:

    ```powershell
    pip install azure-ai-formrecognizer==3.3.0
    ```

1. Wählen Sie in Visual Studio Code im Ordner **Labfiles/02-custom-document-intelligence** die von Ihnen verwendete Sprache aus. Bearbeiten Sie die Konfigurationsdatei (**appsettings.json** oder **.env**, je nach Ihrer Spracheinstellung) mit den folgenden Werten:
    - Ihrem Dokument Intelligenz-Endpunkt.
    - Ihr Dokument Intelligenz-Schlüssel.
    - Der Modell-ID, die Sie beim Trainieren Ihres Modells bereitgestellt haben. Diese finden Sie auf der Seite **Modelle** in Dokument Intelligenz Studio. **Speichern** Sie die Änderungen.

1. Öffnen Sie in Visual Studio Code die Codedatei für Ihre Clientanwendung (*Program.cs* für C#, *test-model.py* für Python), und überprüfen Sie den darin enthaltenen Code, insbesondere, ob das Bild in der URL auf die Datei in diesem GitHub-Repository im Web verweist.

1. Kehren Sie zum Terminalfenster zurück, und geben Sie den folgenden Befehl ein, um das Programm auszuführen:

    **C#**

    ```powershell
    dotnet build
    dotnet run
    ```

    **Python**

    ```powershell
    python test-model.py
    ```

1. Sehen Sie sich die Ausgabe an. Sie erkennen dabei, wie die Ausgabe für das Modell Feldnamen wie `Merchant` und `CompanyPhoneNumber` bereitstellt.

## Bereinigung

Wenn Sie mit Ihrer Azure-Ressource fertig sind, denken Sie daran, die Ressource im [Azure-Portal](https://portal.azure.com/?azure-portal=true) zu löschen, um zukünftige Gebühren zu vermeiden.

## Weitere Informationen

Weitere Informationen zum Dokument Intelligenz-Dienst finden Sie in der [Dokumentation zu Dokument Intelligenz](https://learn.microsoft.com/azure/ai-services/document-intelligence/?azure-portal=true).
