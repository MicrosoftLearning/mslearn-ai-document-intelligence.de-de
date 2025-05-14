---
lab:
  title: Analysieren von Inhalten mit Azure KI Content Understanding
  module: Multimodal analysis with Content Understanding
---

# Analysieren von Inhalten mit Azure KI Content Understanding

In dieser Übung erstellen Sie mithilfe des Azure AI Foundry-Portals ein Projekt zum Verstehen von Inhalten, mit dem Informationen aus Rechnungen extrahiert werden können. Anschließend testen Sie Ihr Inhaltsanalysetool im Azure KI Foundry-Portal und nutzen sie über die REST-Schnittstelle für Content Understanding.

Diese Übung dauert ca. **30** Minuten.

## Erstellen eines Azure KI Foundry-Projekts

Beginnen wir mit dem Erstellen eines Azure AI Foundry-Projekts.

1. Öffnen Sie in einem Webbrowser unter `https://ai.azure.com` das [Azure KI Foundry-Portal](https://ai.azure.com) und melden Sie sich mit Ihren Azure-Anmeldeinformationen an. Schließen Sie alle Tipps oder Schnellstartbereiche, die beim ersten Anmelden geöffnet werden, und verwenden Sie bei Bedarf das **Azure AI Foundry-Logo** oben links, um zur Startseite zu navigieren, die ähnlich wie die folgende Abbildung aussieht:

    ![Screenshot des Azure KI Foundry-Portals.](../media/ai-foundry-portal.png)

1. Wählen Sie auf der Startseite **+ Projekt erstellen**.
1. Geben Sie im Assistenten **Projekt erstellen** einen gültigen Projektnamen ein und wählen Sie, falls ein vorhandener Hub vorgeschlagen wird, die Option zum Erstellen eines neuen. Überprüfen Sie dann die Azure-Ressourcen, die automatisch erstellt werden, um Ihren Hub und Ihr Projekt zu unterstützen.
1. Wählen Sie **Anpassen** aus und legen Sie die folgenden Einstellungen für Ihren Hub fest:
    - **Hubname**: *Geben Sie einen gültigen Namen für Ihren Hub an*
    - **Abonnement:** *Geben Sie Ihr Azure-Abonnement an.*
    - **Ressourcengruppe**: *Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine Ressourcengruppe aus*.
    - **Standort**: Wählen Sie eine der folgenden Regionen\*
        - USA (Westen)
        - Schweden, Mitte
        - Australien (Osten)
    - A**zure KI Services oder Azure OpenAI verbinden**: *Wählen Sie Neuen KI-Dienst erstellen aus*
    - **Verbinden von Azure KI-Suche**: *Erstellen Sie eine neue Ressource von Azure KI-Suche mit einem eindeutigen Namen*

    > \*Zum Zeitpunkt des Schreibens ist das Verständnis von Azure AI-Inhalten nur in diesen Regionen verfügbar.

1. Klicken Sie auf **Weiter**, um Ihre Konfiguration zu überprüfen. Klicken Sie auf **Erstellen** und warten Sie, bis der Vorgang abgeschlossen ist.
1. Sobald Ihr Projekt erstellt wurde, schließen Sie alle angezeigten Tipps und überprüfen Sie die Projektseite im Azure AI Foundry-Portal, die in etwa wie in der folgenden Abbildung aussehen sollte:

    ![Screenshot eines Azure KI-Projekts im Azure AI Foundry-Portal.](../media/ai-foundry-project.png)

## Erstellen eines Analysetools für Content Understanding

Sie werden einen Analysator erstellen, der Informationen aus Rechnungen extrahieren kann. Sie beginnen mit der Definition eines Schemas auf der Grundlage einer Beispielrechnung.

1. Öffnen Sie einen neuen Browser-Tab, laden Sie das Musterformular [invoice-1234.pdf](https://github.com/microsoftlearning/mslearn-ai-document-intelligence/raw/main/Labfiles/05-content-understanding/forms/invoice-1234.pdf) von `https://github.com/microsoftlearning/mslearn-ai-document-intelligence/raw/main/Labfiles/05-content-understanding/forms/invoice-1234.pdf` herunter und speichern Sie es in einem lokalen Ordner.
1. Kehren Sie zur Registerkarte mit der Startseite Ihres Azure AI Foundry-Projekts zurück und wählen Sie im Navigationsbereich auf der linken Seite **Inhaltsverständnis** aus.
1. Wählen Sie auf der Seite **Inhaltsverständnis** oben die Registerkarte **Benutzerdefinierter Analysator**.
1. Wählen Sie auf der Seite „Benutzerdefinierter Analysator für Inhaltsverständnis“ die Option **+ Erstellen** und erstellen Sie eine Aufgabe mit den folgenden Einstellungen:
    - **Vorgangsname**: Rechnungsanalyse
    - **Beschreibung**: Extrahieren von Daten aus einer Rechnung
    - **Azure KI-Dienstverbindung**: *die Azure KI-Dienstressource in Ihrem Azure AI Foundry-Hub*
    - **Azure Blob Storage-Konto**: *Das Standardspeicherkonto in Ihrem Azure AI Foundry-Hub*
1. Bitte warten Sie, bis die Vorgang erstellt wurde.

    > **Tipp**: Wenn beim Zugriff auf den Speicher ein Fehler auftritt, warten Sie bitte eine Minute und versuchen Sie es erneut.

1. Laden Sie auf der Seite **Schema definieren** die soeben heruntergeladene Datei **Rechnung-1234.pdf** hoch.
1. Wählen Sie die Vorlage **Rechnungsanalyse** und anschließend **Erstellen**.

    Die Vorlage *Rechnungsanalyse* enthält gängige Felder, die in Rechnungen zu finden sind. Mit dem Schema-Editor können Sie alle vorgeschlagenen Felder, die Sie nicht benötigen, löschen und beliebige benutzerdefinierte Felder hinzufügen.

1. Wählen Sie in der Liste der vorgeschlagenen Felder **BillingAddress** aus. Dieses Feld wird für das von Ihnen hochgeladene Rechnungsformat nicht benötigt. Verwenden Sie daher das Symbol **Feld löschen** (**&#128465;**), um es zu löschen.
1. Löschen Sie nun die folgenden vorgeschlagenen Felder, die nicht benötigt werden:
    - BillingAddressRecipient
    - CustomerAddressRecipient
    - CustomerId
    - CustomerTaxId
    - DueDate
    - InvoiceTotal
    - PaymentTerm
    - PreviousUnpaidBalance
    - PurchaseOrder
    - RemittanceAddress
    - RemittanceAddressRecipient
    - ServiceAddress
    - ServiceAddressRecipient
    - ShippingAddress
    - ShippingAddressRecipient
    - TotalDiscount
    - VendorAddressRecipient
    - VendorTaxId
    - TaxDetails
1. Verwenden Sie die Schaltfläche **+ Neues Feld hinzufügen**, um die folgenden Felder hinzuzufügen:

    | Feldname | Feldbeschreibung | Werttyp | Methode |
    |--|--|--|--|
    | `VendorPhone` | `Vendor telephone number` | String | Extract |
    | `ShippingFee` | `Fee for shipping` | Anzahl | Extract |

1. Überprüfen Sie, ob Ihr abgeschlossenes Schema wie folgt aussieht, und wählen Sie **Speichern**.
    ![Screenshot eines Schemas für eine Rechnung.](../media/invoice-schema.png)

1. Falls die Analyse nicht automatisch beginnt, wählen Sie auf der Seite **Analysetool testen** den Befehl **Analyse ausführen** aus. Warten Sie anschließend, bis die Analyse abgeschlossen ist, und überprüfen Sie die Textwerte auf der Rechnung, die als mit den Feldern im Schema übereinstimmend identifiziert wurden.
1. Überprüfen Sie die Analyseergebnisse, die in etwa wie folgt aussehen sollten:

    ![Screenshot der Testergebnisse für das Analysetool.](../media/analysis-results.png)

1. Zeigen Sie die Details der Felder an, die im Bereich **Felder** identifiziert wurden, und zeigen Sie dann die Registerkarte **Ergebnis** an, um die JSON-Darstellung anzuzeigen.

## Erstellen und Testen eines Analysetools

Nachdem Sie ein Modell zum Extrahieren von Feldern aus Rechnungen trainiert haben, können Sie einen Analysator erstellen, der mit ähnlichen Formularen verwendet werden kann.

1. Wählen Sie die Seite **Build Analyzer** aus, klicken Sie auf **+ Build Analyzer** und erstellen Sie einen neuen Analyzer mit den folgenden Eigenschaften (genau wie hier angegeben):
    - **Name**: `contoso-invoice-analyzer`
    - **Beschreibung:** `Contoso invoice analyzer`
1. Warten Sie, bis das neue Analysetool bereit ist (Sie können dies mit der Schaltfläche **Aktualisieren** überprüfen).
1. Laden Sie die Datei [invoice-1235.pdf](https://github.com/microsoftlearning/mslearn-ai-document-intelligence/raw/main/Labfiles/05-content-understanding/forms/invoice-1235.pdf) von `https://github.com/microsoftlearning/mslearn-ai-document-intelligence/raw/main/Labfiles/05-content-understanding/forms/invoice-1235.pdf` herunter und speichern Sie sie in einem lokalen Ordner.
1. Kehren Sie zur Seite **Build Analyzer** zurück und wählen Sie den Link **contoso-invoice-analyzer** aus. Die im Schema des Analysetools definierten Felder werden angezeigt.
1. Wählen Sie auf der Seite **contoso-invoice-analyzer** die Registerkarte **Test**.
1. Verwenden Sie die Schaltfläche **+ Testdateien hochladen**, um **Rechnung-1235.pdf** hochzuladen und die Analyse auszuführen, um Felddaten aus dem Testformular zu extrahieren.
1. Überprüfen Sie die Ergebnisse des Tests und stellen Sie sicher, dass das Analysegerät die richtigen Felder aus der Testrechnung extrahiert hat.
1. Schließen Sie die Seite **contoso-invoice-analyzer***.

## Verwenden der REST-API für Inhaltsverständnis

Nachdem Sie nun ein Analysetool erstellt haben, können Sie es über die Content Understanding-REST-API in einer Clientanwendung nutzen.

1. Beachten Sie im Bereich **Projektdetails** die **Projektverbindungszeichenfolge**. Sie verwenden diese Verbindungszeichenfolge, um eine Verbindung mit Ihrem Projekt in einer Clientanwendung herzustellen.
1. Öffnen Sie eine neue Browserregisterkarte (wobei das Azure AI Foundry-Portal auf der vorhandenen Registerkarte geöffnet bleibt). Wechseln Sie dann in der neuen Registerkarte zum [Azure-Portal](https://portal.azure.com) unter `https://portal.azure.com` und melden Sie sich mit Ihren Azure-Anmeldedaten an, wenn Sie dazu aufgefordert werden.

    Schließen Sie alle Willkommensbenachrichtigungen, um die Startseite des Azure-Portals anzuzeigen.

1. Verwenden Sie die Schaltfläche **[\>_]** rechts neben der Suchleiste oben auf der Seite, um eine neue Cloud-Shell im Azure-Portal zu erstellen, und wählen Sie eine ***PowerShell***-Umgebung ohne Speicher in Ihrem Abonnement.

    Die Cloud Shell bietet eine Befehlszeilenschnittstelle in einem Fenster am unteren Rand des Azure-Portals. Sie können die Größe dieses Bereichs ändern oder maximieren, um die Arbeit zu vereinfachen.

    > **Hinweis**: Wenn Sie zuvor eine Cloud-Shell erstellt haben, die eine *Bash*-Umgebung verwendet, wechseln Sie zu ***PowerShell***.

1. Wählen Sie in der Cloud Shell-Symbolleiste im Menü **Einstellungen** das Menüelement **Zur klassischen Version wechseln** aus (dies ist für die Verwendung des Code-Editors erforderlich).

    **<font color="red">Stellen Sie sicher, dass Sie zur klassischen Version der Cloud Shell gewechselt haben, bevor Sie fortfahren.</font>**

1. Geben Sie im Cloud Shell-Bereich die folgenden Befehle ein, um das GitHub-Repository zu klonen, das die Codedateien für diese Übung enthält (geben Sie den Befehl ein, oder kopieren Sie ihn in die Zwischenablage, und klicken Sie dann mit der rechten Maustaste in die Befehlszeile, und fügen Sie ihn als Nur-Text ein):

    ```
   rm -r mslearn-ai-foundry -f
   git clone https://github.com/microsoftlearning/mslearn-ai-document-intelligence mslearn-ai-doc
    ```

    > **Tipp**: Wenn Sie Befehle in die Cloud Shell einfügen, kann die Ausgabe einen großen Teil des Bildschirmpuffers in Anspruch nehmen. Sie können den Bildschirm löschen, indem Sie den Befehl `cls` eingeben, um sich besser auf die einzelnen Aufgaben konzentrieren zu können.

1. Nachdem das Repo geklont wurde, navigieren Sie zu dem Ordner, der die Codedateien für Ihre App enthält:

    ```
   cd mslearn-ai-doc/Labfiles/05-content-understanding/code
   ls -a -l
    ```

1. Geben Sie im Befehlszeilenfenster der Cloud Shell den folgenden Befehl ein, um die zu verwendenden Bibliotheken zu installieren:

    ```
   python -m venv labenv
   ./labenv/bin/Activate.ps1
   pip install dotenv azure-identity azure-ai-projects
    ```

1. Geben Sie den folgenden Befehl ein, um die bereitgestellte Konfigurationsdatei zu bearbeiten:

    ```
   code .env
    ```

    Die Datei wird in einem Code-Editor geöffnet.

1. Ersetzen Sie in der Codedatei den Platzhalter **YOUR_PROJECT_CONNECTION_STRING** durch die Verbindungszeichenfolge für Ihr Projekt (kopiert von der Seite **Übersicht** des Azure AI Foundry-Portals) und stellen Sie sicher, dass **ANALYSETOOL** auf den Namen gesetzt ist, den Sie Ihrem Analysetool zugewiesen haben (das sollte *contoso-invoice-analyzer* sein).
1. Nachdem Sie die Platzhalter ersetzt haben, verwenden Sie im Code-Editor den Befehl **STRG+S**, um Ihre Änderungen zu speichern und dann den Befehl **STRG+Q**, um den Code-Editor zu schließen, während die Befehlszeile der Cloud Shell geöffnet bleibt.

1. Geben Sie in der Befehlszeile der Cloud Shell den folgenden Befehl ein, um die bereitgestellte Python-Code-Datei **analyze_invoice.py** zu bearbeiten:

    ```
    code analyze_invoice.py
    ```
    Die Python-Codedatei wird in einem Code-Editor geöffnet:

1. Überprüfen Sie den Code, der:
    - Identifiziert die zu analysierende Rechnungsdatei mit dem Standardwert **Rechnung-1236.pdf**.
    - Ruft das Endgerät und den Schlüssel für Ihre Azure KI Services-Ressource aus dem Projekt ab.
    - Übermittelt eine HTTP POST-Anfrage an Ihr Content Understanding Endgerät und weist dieses an, das Bild zu analysieren.
    - die Antwort des POST-Vorgangs überprüft, um eine ID für den Analysevorgang abzurufen.
    - wiederholt eine HTTP GET-Anforderung an Ihren Content Understanding-Service sendet, bis der Vorgang nicht mehr ausgeführt wird.
    - Wenn der Vorgang erfolgreich war, parst er die JSON-Antwort und zeigt die abgerufenen Werte an
1. Verwenden Sie den Tastaturbefehl **STRG+Q**, um den Code-Editor zu schließen, während die Befehlszeile der Cloud Shell geöffnet bleibt.
1. Geben Sie in der Cloud Shell-Befehlszeile den folgenden Befehl ein, um den Python-Code auszuführen:

    ```
    python analyze_invoice.py invoice-1236.pdf
    ```

1. Überprüfen Sie die Ausgabe des Programms.
1. Verwenden Sie den folgenden Befehl, um das Programm mit einer anderen Rechnung auszuführen:

    ```
    python analyze_invoice.py invoice-1235.pdf
    ```

    > **Tipp**: Im Code-Ordner befinden sich drei Rechnungsdateien, die Sie verwenden können (Rechnung-1234.pdf, Rechnung-1235.pdf und Rechnung-1236.pdf) 

## Bereinigen

Wenn Sie Arbeit mit dem Content Understanding-Service abgeschlossen haben, sollten Sie die in dieser Übung erstellten Ressourcen löschen, um unnötige Azure-Kosten zu vermeiden.

1. Navigieren Sie im Azure KI Foundry-Portal zum Projekt **Reiseversicherung** und löschen Sie es.
1. Löschen Sie im Azure-Portal die Ressourcengruppe, die Sie in dieser Übung erstellt haben.

