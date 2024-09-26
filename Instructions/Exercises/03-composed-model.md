---
lab:
  title: Erstellen eines zusammengesetzten Document Intelligenz-Modells
  module: Module 6 - Document Intelligence
---

# Erstellen eines zusammengesetzten Document Intelligenz-Modells

In dieser Übung erstellen und trainieren Sie zwei benutzerdefinierte Modelle, mit denen verschiedene Steuerformulare analysiert werden. Anschließend erstellen Sie ein zusammengesetztes Modell, das beide benutzerdefinierte Modelle enthält. Sie testen das Modell, indem Sie ein Formular übermitteln und überprüfen, ob der Dokumenttyp und die beschrifteten Felder korrekt erkannt werden.

## Einrichten von Ressourcen

Das Skript wird dazu verwendet, die Azure KI Dokument Intelligenz-Ressource, ein Speicherkonto mit Beispielformularen und eine Ressourcengruppe zu erstellen:

1. Starten Sie Visual Studio Code.
1. Öffnen Sie die Palette (UMSCHALT+STRG+P), und führen Sie einen **Git: Clone**-Befehl aus, um das Repository `https://github.com/MicrosoftLearning/mslearn-ai-document-intelligence` in einen lokalen Ordner zu klonen (der Ordner ist beliebig).
1. Nachdem das Repository geklont wurde, öffnen Sie den Ordner in Visual Studio Code.

    > **Hinweis:** Wenn Visual Studio Code eine Popupnachricht anzeigt, in der Sie aufgefordert werden, dem geöffneten Code zu vertrauen, klicken Sie auf die Option **Ja, ich vertraue den Autoren** im Popupfenster.
    
    > **Hinweis**: Wenn Sie aufgefordert werden, erforderliche Ressourcen zum Erstellen und Debuggen hinzuzufügen, wählen Sie **Not now** (Jetzt nicht) aus. Wenn andere Popups aus Visual Studio Code vorhanden sind, können Sie diese gefahrlos schließen.

1. Erweitern Sie im linken Bereich den Ordner **Labfiles**, und klicken Sie mit der rechten Maustaste auf das Verzeichnis **03-composed-model**. Wählen Sie die Option zum Öffnen im integrierten Terminal aus, und führen Sie das folgende Skript aus:

    ```powershell
    az login --output none
    ```

    > **Hinweis:** Wenn Sie eine Fehlermeldung über keine aktiven Abonnements erhalten und MFA aktiviert haben, müssen Sie sich möglicherweise zunächst im Azure-Portal auf `https://portal.azure.com` anmelden und dann `az login`Vorgang erneut ausführen.

1. Melden Sie sich bei Ihrem Azure-Abonnement an, wenn Sie dazu aufgefordert werden. Kehren Sie dann zu Visual Studio Code zurück, und warten Sie, bis der Anmeldevorgang abgeschlossen ist.
1. Führen Sie im integrierten Terminal den folgenden Befehl aus, um Ressourcen einzurichten:

   ```powershell
   ./setup.ps1
   ```

   > **WICHTIG:** Die letzte Ressource, die vom Skript erstellt wurde, ist Ihr Azure AI Document Intelligence-Dienst. Wenn dieser Befehl aufgrund bereits einer F0-Ebenenressource fehlschlägt, verwenden Sie diese Ressource entweder für dieses Lab, oder erstellen Sie eine manuell mithilfe der S0-Ebene im Azure-Portal.

## Erstellen des benutzerdefinierten Modells „1040 Forms“

Zum Erstellen eines zusammengesetzten Modells müssen Sie zunächst zwei oder mehr benutzerdefinierte Modelle erstellen. So erstellen Sie das erste benutzerdefinierte Modell:

1. Starten Sie **Azure KI Dokument Intelligenz Studio** auf `https://documentintelligence.ai.azure.com/studio`.
1. Scrollen Sie nach unten, und wählen Sie dann unter **Benutzerdefinierte Modelle** die Option **Benutzerdefiniertes Extraktionsmodell** aus.
1. Wenn Sie aufgefordert werden, sich bei Ihrem Konto anzumelden, geben Sie Ihre Azure-Anmeldeinformationen an.
1. Wenn Sie gefragt werden, welche Azure KI Dokument Intelligenz-Ressource verwendet werden soll, wählen Sie das Abonnement und den Ressourcennamen aus, die Sie beim Erstellen der Azure KI Dokument Intelligenz-Ressource verwendet haben.
1. Wählen Sie unter **Meine Projekte** die Option **+ Projekt erstellen** aus.
1. Geben Sie im Textfeld **Projektname** den Eintrag **1040 Forms** ein, und wählen Sie dann **Weiter** aus.
1. Wählen Sie auf der Seite **Configure service resource** (Dienstressource konfigurieren) in der Dropdownliste **Abonnement** Ihr Azure-Abonnement aus.
1. Wählen Sie in der Dropdownliste **Ressourcengruppe** die für Sie erstellte **DocumentIntelligenceResources&lt;xxxx&gt;** aus.
1. Wählen Sie in der Dropdown-Liste **Dokumenten Intelligenz oder Cognitive Service/Ressource** die Option **DocumentIntelligence&lt;xxxx&gt;**.
1. Vergewissern Sie sich, dass in der Dropdownliste **API-Version** der Eintrag **2024-07-31 (Preview)** ausgewählt ist, und wählen Sie dann **Weiter** aus.
1. Wählen Sie auf der Seite **Trainingsdatenquelle verbinden** in der Dropdownliste **Abonnement** Ihr Azure-Abonnement aus.
1. Wählen Sie in der Dropdownliste **Ressourcengruppe** den Eintrag **DocumentIntelligenceResources&lt;xxxx&gt;** aus.
1. Wählen Sie in der Dropdownliste **Speicherkonto** das einzige Speicherkonto aus, das aufgeführt wird. Wenn Sie über mehrere Speicherkonten in Ihrem Abonnement verfügen, wählen Sie das Konto aus, das mit *docintelstorage* beginnt.
1. Wählen Sie in der Dropdownliste **BLOB-Container** die Option **1040examples** und dann **Weiter** aus.
1. Wählen Sie auf der Seite **Überprüfen und erstellen** die Option **Projekt erstellen** aus.
1. Wählen Sie **Jetzt ausführen** unter *Layout ausführen* im Popup-Fenster *Beschriftung jetzt starten* und warten Sie, bis die Analyse abgeschlossen ist.

## Beschriften des benutzerdefinierten Modells „1040 Forms“

Jetzt beschriften Sie die Felder in den Beispielformularen:

1. Wählen Sie auf der Seite **Daten beschriften** oben rechts auf der Seite die Option **+ Feld hinzufügen** und dann **Feld** aus.
1. Geben Sie **FirstName** ein, und drücken Sie dann die *EINGABETASTE*.
1. Wählen Sie das Dokument **f1040_1.pdf** in der linken Liste, dann **John** und dann **FirstName** aus.
1. Wählen Sie rechts oben auf der Seite **+ Feld hinzufügen** und dann **Feld** aus.
1. Geben Sie **LastName** ein, und drücken Sie dann die *EINGABETASTE*.
1. Wählen Sie im Dokument **Doe** und dann **LastName** aus.
1. Wählen Sie rechts oben auf der Seite **+ Feld hinzufügen** und dann **Feld** aus.
1. Geben Sie **City** ein, und drücken Sie dann die *EINGABETASTE*.
1. Wählen Sie im Dokument **Los Angeles** aus, und wählen Sie dann **City** aus.
1. Wählen Sie rechts oben auf der Seite **+ Feld hinzufügen** und dann **Feld** aus.
1. Geben Sie **State** ein, und drücken Sie dann die *EINGABETASTE*.
1. Wählen Sie im Dokument **CA** aus, und wählen Sie dann **State** aus.
1. Wiederholen Sie den Beschriftungsprozess unter Verwendung der von Ihnen erstellten Bezeichnungen für die verbleibenden Formulare in der Liste auf der linken Seite. Beschriften Sie dieselben vier Felder: *FirstName*, *LastName*, *City* und *State*. Beachten Sie, dass eines der Dokumente weder über Orts- noch über Landesdaten verfügt.

> **WICHTIG** Für diese Übung werden nur fünf Beispielformulare verwendet und nur vier Felder beschriftet. In Ihren in der Praxis verwendeten Modellen sollten Sie so viele Beispiele wie möglich verwenden, um die Genauigkeit und Vertrauenswürdigkeit Ihrer Vorhersagen zu maximieren. Sie sollten auch alle verfügbaren Felder in den Formularen und nicht nur vier Felder beschriften.

## Trainieren des benutzerdefinierten Modells „1040 Forms“

Nachdem die Beispielformulare jetzt beschriftet sind, kann das erste benutzerdefinierte Modell trainiert werden:

1. Wählen Sie im Azure KI Dokument Intelligenz Studio oben rechts auf dem Bildschirm **Trainieren**.
1. Geben Sie im Dialogfeld **Train a new model** (Neues Modell trainieren) im Textfeld **Modell-ID** den Eintrag **1040FormsModel** ein.
1. Wählen Sie in der Dropdownliste **Build mode** (Buildmodus) die Option **Vorlage** aus, und wählen Sie dann **Trainieren** aus. 
1. Wählen Sie im Dialogfeld **Training in Progress** (Training wird durchgeführt) die Option **Go to Models** (Zu den Modellen) aus.

## Erstellen des benutzerdefinierten Modells „1099 Forms“

Jetzt müssen Sie ein zweites Modell erstellen, das Sie am Beispiel von 1099-Steuerformularen trainieren werden:

1. Wählen Sie in Azure KI Dokument Intelligenz Studio die Option **Benutzerdefiniertes Extraktionsmodell** aus.
1. Wählen Sie unter **Meine Projekte** die Option **+ Projekt erstellen** aus.
1. Geben Sie im Textfeld **Projektname** den Eintrag **1099 Forms** ein, und wählen Sie dann **Weiter** aus.
1. Wählen Sie auf der Seite **Configure service resource** (Dienstressource konfigurieren) in der Dropdownliste **Abonnement** Ihr Azure-Abonnement aus.
1. Wählen Sie in der Dropdownliste **Ressourcengruppe** den Eintrag **DocumentIntelligenceResources&lt;xxxx&gt;** aus.
1. Wählen Sie in der Dropdown-Liste **Dokumenten Intelligenz oder Cognitive Service/Ressource** die Option **DocumentIntelligence&lt;xxxx&gt;**.
1. Vergewissern Sie sich, dass in der Dropdownliste **API-Version** der Eintrag **2024-07-31 (Preview)** ausgewählt ist, und wählen Sie dann **Weiter** aus.
1. Wählen Sie auf der Seite **Trainingsdatenquelle verbinden** in der Dropdownliste **Abonnement** Ihr Azure-Abonnement aus.
1. Wählen Sie in der Dropdownliste **Ressourcengruppe** den Eintrag **DocumentIntelligenceResources&lt;xxxx&gt;** aus.
1. Wählen Sie in der Dropdownliste **Speicherkonto** das einzige Speicherkonto aus, das aufgeführt wird.
1. Wählen Sie in der Dropdownliste **BLOB-Container** die Option **1099examples** und dann **Weiter** aus.
1. Wählen Sie auf der Seite **Überprüfen und erstellen** die Option **Projekt erstellen** aus.
1. Wählen Sie die Dropdown-Schaltfläche für **Layout ausführen** und wählen Sie **Nicht analysierte Dokumente**.
1. Warten Sie, bis die Analyse abgeschlossen ist.

## Beschriften des benutzerdefinierten Modells „1099 Forms“

Beschriften Sie nun die Beispielformulare mit einigen Feldern:

1. Wählen Sie auf der Seite **Daten beschriften** oben rechts auf der Seite die Option **+ Feld hinzufügen** und dann **Feld** aus.
1. Geben Sie **FirstName** ein, und drücken Sie dann die *EINGABETASTE*.
1. Wählen Sie das Dokument **f1099msc_payer.pdf**, dann **John** und dann **FirstName** aus.
1. Wählen Sie rechts oben auf der Seite **+ Feld hinzufügen** und dann **Feld** aus.
1. Geben Sie **LastName** ein, und drücken Sie dann die *EINGABETASTE*.
1. Wählen Sie im Dokument **Doe** und dann **LastName** aus.
1. Wählen Sie rechts oben auf der Seite **+ Feld hinzufügen** und dann **Feld** aus.
1. Geben Sie **City** ein, und drücken Sie dann die *EINGABETASTE*.
1. Wählen Sie im Dokument die Option **New Haven** und dann **City** aus.
1. Wählen Sie rechts oben auf der Seite **+ Feld hinzufügen** und dann **Feld** aus.
1. Geben Sie **State** ein, und drücken Sie dann die *EINGABETASTE*.
1. Wählen Sie im Dokument die Option **CT** aus, und wählen Sie dann **State** aus.
1. Wiederholen Sie den Beschriftungsprozess für die verbleibenden Formulare in der Liste auf der linken Seite. Beschriften Sie dieselben vier Felder: *FirstName*, *LastName*, *City* und *State*. Beachten Sie, dass zwei der Dokumente über keine Namensdaten verfügen, die beschriftet werden könnten.

## Trainieren des benutzerdefinierten Modells „1099 Forms“

Sie können jetzt das zweite benutzerdefinierte Modell trainieren:

1. Wählen Sie in Azure KI Dokument Intelligenz Studio oben rechts die Option **Trainieren** aus.
1. Geben Sie im Dialogfeld **Train a new model** (Neues Modell trainieren) im Textfeld **Modell-ID** den Eintrag **1099FormsModel** ein.
1. Wählen Sie in der Dropdownliste **Build mode** (Buildmodus) die Option **Vorlage** aus, und wählen Sie dann **Trainieren** aus.
1. Wählen Sie im Dialogfeld **Training in Progress** (Training wird durchgeführt) die Option **Go to Models** (Zu den Modellen) aus.
1. Der Trainingsprozess kann einige Minuten dauern. Aktualisieren Sie den Browser gelegentlich, bis für beide Modelle der Status **erfolgreich** angezeigt wird.

## Verwenden des Modells

Da das Modell nun fertig ist, testen wir es mit einem Beispielformular:

1. Wählen Sie im Azure KI Dokument Intelligenz Studio die Seite **Modelle** und dann das  **1040FormsModel** aus.
1. Klicken Sie auf **Test**.
1. Wählen Sie **Nach Dateien suchen** aus, und navigieren Sie dann zu dem Speicherort, an dem Sie das Repository geklont haben.
1. Wählen Sie die Datei **03-composed-model/trainingdata/TestDoc/f1040_7.pdf** und dann **Öffnen** aus.
1. Wählen Sie **Run Analysis** (Analyse ausführen) aus. Azure KI Dokument Intelligenz analysiert das Formular mithilfe des zusammengesetzten Modells.
1. Das von Ihnen analysierte Dokument ist ein Beispiel für das Steuerformular 1040. Überprüfen Sie die Eigenschaft **DocType**, um festzustellen, ob das richtige benutzerdefinierte Modell verwendet wurde. Überprüfen Sie auch die vom Modell identifizierten Werte **FirstName**, **LastName**, **City** und **State**.

## Bereinigen von Ressourcen

Nachdem Sie gesehen haben, wie zusammengesetzte Modelle funktionieren, entfernen Sie die Ressourcen, die Sie in Ihrem Azure-Abonnement erstellt haben.

1. Wählen Sie im **Azure-Portal** unter `https://portal.azure.com/` die Option **Ressourcengruppen** aus.
1. Wählen Sie in der Liste der **Ressourcengruppen** die von Ihnen erstellte **DocumentIntelligenceResources&lt;xxxx&gt;** aus, und wählen Sie dann **Ressourcengruppe löschen** aus.
1. Geben Sie im Textfeld **NAMEN DER RESSOURCENGRUPPE EINGEBEN** den Namen der Ressourcengruppe ein, und wählen Sie dann **Löschen** aus, um die Dokument Intelligenz-Ressource und das Speicherkonto zu löschen.

## Weitere Informationen

- [Zusammensetzen benutzerdefinierter Modelle](/azure/ai-services/document-intelligence/concept-composed-models)
