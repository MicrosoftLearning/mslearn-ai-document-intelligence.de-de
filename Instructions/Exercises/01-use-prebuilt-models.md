---
lab:
  title: Verwenden von vorgefertigten Dokument-Intelligenz-Modellen
  module: Module 11 - Reading Text in Images and Documents
---

# Verwenden von vorgefertigten Dokument-Intelligenz-Modellen

In dieser Übung richten Sie eine Ressource für „Azure KI Dokument Intelligenz“ in Ihrem Azure-Abonnement ein. Sie verwenden sowohl Azure KI Dokument Intelligenz-Studio als auch C# oder Python, um Formulare zur Analyse an diese Ressource zu übermitteln.

## Erstellen einer Ressource für „Azure KI Dokument Intelligenz“

Bevor Sie den Dienst „Azure KI Dokument Intelligenz“ aufrufen können, müssen Sie eine Ressource zum Hosten dieses Dienstes in Azure erstellen:

1. Öffnen Sie auf einer Browserregisterkarte das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com?azure-portal=true), und melden Sie sich mit dem Microsoft-Konto an, das Ihrem Azure-Abonnement zugeordnet ist.
1. Navigieren Sie im Azure-Portal auf der Homepage zum oberen Suchfeld, geben Sie **Dokument Intelligenz** ein, und drücken Sie dann die **Eingabetaste**.
1. Wählen Sie auf der Seite **Dokumentintelligenz** die Option **Erstellen** aus.
1. Verwenden Sie auf der Seite **Dokument Intelligenz erstellen** Folgendes, um Ihre Ressource zu konfigurieren:
    - **Abonnement**: Ihr Azure-Abonnement.
    - **Ressourcengruppe**: Wählen Sie eine Ressourcengruppe mit einem eindeutigen Namen wie *DocIntelligenceResources* aus, oder erstellen Sie eine.
    - **Region**: Wählen Sie eine Region in Ihrer Nähe aus.
    - **Name**: Geben Sie einen global eindeutigen Namen ein.
    - **Tarif**: Wählen Sie **Free F0** aus (wenn Sie über keinen Free-Tarif verfügen, wählen Sie **Standard S0** aus).
1. Wählen Sie dann **Überprüfen + erstellen** und **Erstellen** aus. Warten Sie, während Azure die neue Ressource für Azure KI Dokument Intelligenz bereitstellt.
1. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressourcengruppe wechseln**. Halten Sie diese Seite für den Rest dieser Übung geöffnet.

## Verwenden des Lesemodells

Beginnen wir mit **Azure KI Dokument Intelligenz-Studio** und dem Lesemodell, um ein Dokument in mehreren Sprachen zu analysieren. Sie verbinden Azure KI Dokument Intelligenz-Studio mit der Ressource, die Sie gerade erstellt haben, um die Analyse auszuführen:

1. Öffnen Sie eine neue Browserregisterkarte, und wechseln Sie zu **Azure KI Dokument Intelligenz-Studio** unter [https://documentintelligence.ai.azure.com/studio](https://documentintelligence.ai.azure.com/studio).
1. Wählen Sie unter **Dokumentanalyse** die Kachel **Lesen** aus.
1. Wenn Sie aufgefordert werden, sich bei Ihrem Konto anzumelden, geben Sie Ihre Azure-Anmeldeinformationen an.
1. Wenn Sie gefragt werden, welche Azure KI Dokument Intelligenz-Ressource verwendet werden soll, wählen Sie das Abonnement und den Ressourcennamen aus, die Sie beim Erstellen der Azure KI Dokument Intelligenz-Ressource verwendet haben.
1. Wählen Sie links in der Liste der Dokumente **read-german.png** aus.

    ![Screenshot: Seite „Lesen“ in Azure KI Dokument Intelligenz-Studio.](../media/read-german-sample.png#lightbox)

1. Wählen Sie links oben **Analyse ausführen** aus.
1. Nach Abschluss der Analyse wird der aus dem Bild extrahierte Text rechts auf der Registerkarte **Inhalt** angezeigt. Überprüfen Sie diesen Text, und vergleichen Sie ihn mit dem Text im Originalbild auf Richtigkeit.
1. Wählen Sie die Registerkarte **Ergebnis** aus, auf der der extrahierte JSON-Code angezeigt wird. 
1. Scrollen Sie auf der Registerkarte **Ergebnis** im JSON-Code nach unten. Beachten Sie, dass das Modell „Lesen“ die Sprache jedes Spans erkannt hat. Die meisten Spans sind Deutsch (Sprachcode `de`), aber das letzte Span ist Englisch (Sprachcode `en`).

    ![Screenshot: Spracherkennung für zwei Spannen in den Ergebnissen aus dem Lesemodell in Azure KI Dokument Intelligenz-Studio.](../media/language-detection.png#lightbox)

## Vorbereiten der Entwicklung einer App in Visual Studio Code

Sehen wir uns nun die App an, die das Dienst-SDK von Azure KI Dokument Intelligenz verwendet. Sie entwickeln Ihre App mit Visual Studio Code. Die Codedateien für Ihre App wurden in einem GitHub-Repository bereitgestellt.

> **Tipp**: Wenn Sie das Repository **mslearn-ai-document-intelligence** bereits geklont haben, öffnen Sie es in Visual Studio Code. Führen Sie andernfalls die folgenden Schritte aus, um es in Ihrer Entwicklungsumgebung zu klonen.

1. Starten Sie Visual Studio Code.
1. Öffnen Sie die Palette (UMSCHALT+STRG+P), und führen Sie einen **Git: Clone**-Befehl aus, um das Repository `https://github.com/MicrosoftLearning/mslearn-ai-document-intelligence` in einen lokalen Ordner zu klonen (der Ordner ist beliebig).
1. Nachdem das Repository geklont wurde, öffnen Sie den Ordner in Visual Studio Code.
1. Warten Sie, während zusätzliche Dateien zur Unterstützung der C#-Codeprojekte im Repository installiert werden.

    > **Hinweis**: Wenn Sie aufgefordert werden, erforderliche Ressourcen zum Erstellen und Debuggen hinzuzufügen, wählen Sie **Not now** (Jetzt nicht) aus. Wenn Sie die Meldung *Azure Functions-Projekt im Ordner erkannt* angezeigt wird, können Sie diese Meldung einfach schließen.

## Konfigurieren der Anwendung

Es werden Anwendungen für C# und Python bereitgestellt sowie eine Beispiel-PDF-Datei, mit der Sie die Dokument Intelligenz testen können. Beide Apps verfügen über die gleiche Funktionalität. Zuerst vervollständigen Sie einige wichtige Teile der Anwendung, um die Verwendung Ihrer Azure KI Dokument Intelligenz-Ressource zu aktivieren.

1. Sehen Sie sich die folgende Rechnung an und beachten Sie einige der darin enthaltenen Felder und Werte. Dies ist die Rechnung, die Ihr Code analysieren wird.

    ![Screenshot eines Beispielrechnungsdokuments.](../media/sample-invoice.png#lightbox)

1. Wechseln Sie in Visual Studio Code im Bereich **Explorer** zum Ordner **Labfiles/01-prebuild-models**, und erweitern Sie je nach Ihrer bevorzugten Sprache den Ordner **CSharp** oder **Python**. Jeder Ordner enthält die sprachspezifischen Dateien für eine App, in die Sie die Azure KI Dokument Intelligenz-Funktionalität integrieren möchten.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **CSharp** oder **Python**, der Ihre Codedateien enthält, und wählen Sie **Integriertes Terminal öffnen** aus. Installieren Sie das Paket des Azure-Formularerkennungs-SDK (der frühere Name von Dokument Intelligenz), indem Sie den entsprechenden Befehl für Ihre bevorzugte Sprache ausführen:

    **C#:**

    ```powershell
    dotnet add package Azure.AI.FormRecognizer --version 4.1.0
    ```

    **Python**:

    ```powershell
    pip install azure-ai-formrecognizer==3.3.0
    ```

## Hinzufügen von Code zur Verwendung des Azure KI Dokument Intelligenz-Diensts

Jetzt können Sie das SDK verwenden, um die PDF-Datei auszuwerten.

1. Wechseln Sie zu der Browserregisterkarte, auf der die Übersicht über „Azure KI Dokument Intelligenz“ im Azure-Portal angezeigt wird. Wählen Sie im Bereich auf der linken Seite unter *Ressourcenverwaltung* die Option **Schlüssel und Endpunkt** aus. Klicken Sie rechts neben dem Wert **Endpunkt** auf die Schaltfläche **In Zwischenablage kopieren**.
1. Öffnen Sie im Bereich **Explorer** im Ordner **CSharp** oder **Python** die Codedatei für Ihre bevorzugte Sprache, und ersetzen Sie `<Endpoint URL>` durch die Zeichenfolge, die Sie gerade kopiert haben:

    **C#**: ***Program.cs***

    ```csharp
    string endpoint = "<Endpoint URL>";
    ```

    **Python**: ***document-analysis.py***

    ```python
    endpoint = "<Endpoint URL>"
    ```

1. Wechseln Sie zu der Browserregisterkarte, auf der die Option **Schlüssel und Endpunkt** für Azure KI Dokument Intelligenz im Azure-Portal angezeigt wird. Klicken Sie rechts neben dem Wert **KEY 1** auf die Schaltfläche *In Zwischenablage kopieren**.
1. Suchen Sie in der Codedatei in Visual Studio Code diese Zeile, und ersetzen Sie `<API Key>` durch die Zeichenfolge, die Sie gerade kopiert haben:

    **C#**

    ```csharp
    string apiKey = "<API Key>";
    ```

    **Python**

    ```python
    key = "<API Key>"
    ```

1. Suchen Sie den Kommentar `Create the client`. Geben Sie anschließend in neuen Zeilen den folgenden Code ein:

    **C#**

    ```csharp
    var cred = new AzureKeyCredential(apiKey);
    var client = new DocumentAnalysisClient(new Uri(endpoint), cred);
    ```

    **Python**

    ```python
    document_analysis_client = DocumentAnalysisClient(
        endpoint=endpoint, credential=AzureKeyCredential(key)
    )
    ```

1. Suchen Sie den Kommentar `Analyze the invoice`. Geben Sie anschließend in neuen Zeilen den folgenden Code ein:

    **C#**

    ```csharp
    AnalyzeDocumentOperation operation = await client.AnalyzeDocumentFromUriAsync(WaitUntil.Completed, "prebuilt-invoice", fileUri);
    await operation.WaitForCompletionAsync();
    ```

    **Python**

    ```python
    poller = document_analysis_client.begin_analyze_document_from_url(
        fileModelId, fileUri, locale=fileLocale
    )
    ```

1. Suchen Sie den Kommentar `Display invoice information to the user`. Geben Sie anschließend in neuen Zeilen den folgenden Code ein:

    **C#**

    ```csharp
    AnalyzeResult result = operation.Value;
    
    foreach (AnalyzedDocument invoice in result.Documents)
    {
        if (invoice.Fields.TryGetValue("VendorName", out DocumentField? vendorNameField))
        {
            if (vendorNameField.FieldType == DocumentFieldType.String)
            {
                string vendorName = vendorNameField.Value.AsString();
                Console.WriteLine($"Vendor Name: '{vendorName}', with confidence {vendorNameField.Confidence}.");
            }
        }
    ```

    **Python**

    ```python
    receipts = poller.result()
    
    for idx, receipt in enumerate(receipts.documents):
    
        vendor_name = receipt.fields.get("VendorName")
        if vendor_name:
            print(f"\nVendor Name: {vendor_name.value}, with confidence {vendor_name.confidence}.")
    ```

    > [!NOTE]
    > Sie haben Code zum Anzeigen des Namens des Lieferanten hinzugefügt. Das Startprojekt enthält auch Code zur Anzeige des *Kundennamens* und des *Rechnungsbetrags*.

1. Speichern Sie die Änderungen in der Codedatei.

1. Stellen Sie im interaktiven Terminalbereich sicher, dass der Ordnerkontext der Ordner für Ihre bevorzugte Sprache ist. Geben Sie dann den folgenden Befehl ein, um die Anwendung auszuführen.

1. ***Nur C#***: Um Ihr Projekt zu erstellen, geben Sie diesen Befehl ein:

    **C#:**

    ```powershell
    dotnet build
    ```

1. Um Ihr Projekt auszuführen, geben Sie diesen Befehl ein:

    **C#:**

    ```powershell
    dotnet run
    ```

    **Python**:

    ```powershell
    python document-analysis.py
    ```

Das Programm zeigt den Namen des Lieferanten und Kunden sowie die Gesamtsumme der Rechnung mit entsprechendem Konfidenzniveau an. Vergleichen Sie die gemeldeten Werte mit der Beispielrechnung, die Sie am Anfang dieses Abschnitts geöffnet haben.

## Bereinigen

Wenn Sie mit Ihrer Azure-Ressource fertig sind, denken Sie daran, die Ressource im [Azure-Portal](https://portal.azure.com/?azure-portal=true) zu löschen, um zukünftige Gebühren zu vermeiden.

## Weitere Informationen

Weitere Informationen zum Dokument Intelligenz-Dienst finden Sie in der [Dokumentation zur Dokument Intelligenz](https://learn.microsoft.com/azure/ai-services/document-intelligence/?azure-portal=true).
