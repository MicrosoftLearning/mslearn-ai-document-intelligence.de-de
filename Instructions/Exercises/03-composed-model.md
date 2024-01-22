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
1. Warten Sie, während zusätzliche Dateien zur Unterstützung der C#-Codeprojekte im Repository installiert werden.

    > **Hinweis**: Wenn Sie aufgefordert werden, erforderliche Ressourcen zum Erstellen und Debuggen hinzuzufügen, wählen Sie **Not now** (Jetzt nicht) aus.

1. Klicken Sie mit der rechten Maustaste auf das Verzeichnis **03-composed-model**, öffnen Sie das integrierte Terminal, und führen Sie das Setupskript aus:

   ``` bash
   bash setup.sh
   ```

## Erstellen des benutzerdefinierten Modells „1040 Forms“

Zum Erstellen eines zusammengesetzten Modells müssen Sie zunächst zwei oder mehr benutzerdefinierte Modelle erstellen. So erstellen Sie das erste benutzerdefinierte Modell:

1. Starten Sie [Azure KI Dokument Intelligenz Studio](https://formrecognizer.appliedai.azure.com/studio) in einer neuen Browserregisterkarte.
1. Scrollen Sie nach unten, und wählen Sie dann unter **Benutzerdefiniertes Modell** die Option **Benutzerdefiniertes Modell** aus.
1. Wenn Sie aufgefordert werden, sich bei Ihrem Konto anzumelden, geben Sie Ihre Azure-Anmeldeinformationen an.
1. Wenn Sie gefragt werden, welche Azure KI Dokument Intelligenz-Ressource verwendet werden soll, wählen Sie das Abonnement und den Ressourcennamen aus, die Sie beim Erstellen der Azure KI Dokument Intelligenz-Ressource verwendet haben.
1. Wählen Sie unter **Meine Projekte** die Option **+ Projekt erstellen** aus.
1. Geben Sie im Textfeld **Projektname** den Eintrag **1040 Forms** ein, und wählen Sie dann **Weiter** aus.
1. Wählen Sie auf der Seite **Configure service resource** (Dienstressource konfigurieren) in der Dropdownliste **Abonnement** Ihr Azure-Abonnement aus.
1. Wählen Sie in der Dropdownliste **Ressourcengruppe** die Option **DocumentIntelligenceResources** aus.
1. Wählen Sie in der Dropdownliste **Azure KI Dokument Intelligenz- oder Azure KI Services-Ressource** die Option **DocumentIntelligence** aus.
1. Vergewissern Sie sich in der Dropdownliste **API-Version**, dass **2022-06-30-preview** ausgewählt ist, und wählen Sie dann **Weiter** aus.

    :::image type="content" source="../media/4-configure-service-resources.png" alt-text="Screenshot: Seite zum Konfigurieren von Dienstressourcen im Azure KI Dokument Intelligenz Studio-Assistenten für benutzerdefinierte Modelle" lightbox="../media/4-configure-service-resources.png":::

1. Wählen Sie auf der Seite **Configure training data source** (Trainingsdatenquelle konfigurieren) in der Dropdownliste **Abonnement** Ihr Azure-Abonnement aus.
1. Wählen Sie in der Dropdownliste **Ressourcengruppe** die Option **DocumentIntelligenceResources** aus.
1. Wählen Sie in der Dropdownliste **Speicherkonto** das einzige Speicherkonto aus, das aufgeführt wird.
1. Wählen Sie in der Dropdownliste **BLOB-Container** die Option **1040examples** und dann **Weiter** aus.

    :::image type="content" source="../media/4-connect-training-data-source.png" alt-text="Screenshot: Seite zum Verbinden der Trainingsdatenquelle im Azure KI Dokument Intelligenz Studio-Assistenten für benutzerdefinierte Modelle" lightbox="../media/4-connect-training-data-source.png":::

1. Wählen Sie auf der Seite **Überprüfen und erstellen** die Option **Projekt erstellen** aus.

## Beschriften des benutzerdefinierten Modells „1040 Forms“

Jetzt beschriften Sie die Felder in den Beispielformularen:

1. Wählen Sie auf der Seite **Bezeichnungsdaten** oben rechts auf der Seite die Option **+** und dann **Feld** aus.

    :::image type="content" source="../media/4-add-label.png" alt-text="Screenshot: Hinzufügen einer neuen Bezeichnung in Azure KI Dokument Intelligenz Studio" lightbox="../media/4-add-label.png":::

1. Geben Sie **FirstName** ein, und drücken Sie dann die <kbd>EINGABETASTE</kbd>.
1. Wählen Sie im Dokument **John** aus, und wählen Sie dann **FirstName** aus.

    :::image type="content" source="../media/4-label-first-name.png" alt-text="Screenshot: Abschließen einer neuen Bezeichnung in Azure KI Dokument Intelligenz Studio" lightbox="../media/4-label-first-name.png":::

1. Wählen Sie oben rechts auf der Seite **+** und dann **Feld** aus.
1. Geben Sie **LastName** ein, und drücken Sie dann die <kbd>EINGABETASTE</kbd>.
1. Wählen Sie im Dokument **Doe** und dann **LastName** aus.
1. Wählen Sie oben rechts auf der Seite **+** und dann **Feld** aus.
1. Geben Sie **City** ein, und drücken Sie dann die <kbd>EINGABETASTE</kbd>.
1. Wählen Sie im Dokument **Los Angeles** aus, und wählen Sie dann **City** aus.
1. Wählen Sie oben rechts auf der Seite **+** und dann **Feld** aus.
1. Geben Sie **State** ein, und drücken Sie dann die <kbd>EINGABETASTE</kbd>.
1. Wählen Sie im Dokument **CA** aus, und wählen Sie dann **State** aus.
1. Wiederholen Sie den Beschriftungsprozess für die verbleibenden Formulare in der Liste auf der linken Seite. Beschriften Sie dieselben vier Felder: *FirstName*, *LastName*, *City* und *State*.

> [!IMPORTANT]
> Für diese Übung werden nur fünf Beispielformulare verwendet und nur vier Felder beschriftet. In Ihren in der Praxis verwendeten Modellen sollten Sie so viele Beispiele wie möglich verwenden, um die Genauigkeit und Vertrauenswürdigkeit Ihrer Vorhersagen zu maximieren. Sie sollten auch alle verfügbaren Felder in den Formularen und nicht nur vier Felder beschriften.

## Trainieren des benutzerdefinierten Modells „1040 Forms“

Nachdem die Beispielformulare jetzt beschriftet sind, kann das erste benutzerdefinierte Modell trainiert werden:

1. Wählen Sie in Azure KI Dokument Intelligenz Studio die Option **Trainieren** aus.
1. Geben Sie im Dialogfeld **Train a new model** (Neues Modell trainieren) im Textfeld **Modell-ID** den Eintrag **1040FormsModel** ein.
1. Wählen Sie in der Dropdownliste **Build mode** (Buildmodus) die Option **Vorlage** aus, und wählen Sie dann **Trainieren** aus. 
1. Wählen Sie im Dialogfeld **Training in Progress** (Training wird durchgeführt) die Option **Go to Models** (Zu den Modellen) aus.

## Erstellen des benutzerdefinierten Modells „1099 Forms“

Jetzt müssen Sie ein zweites Modell erstellen, das Sie am Beispiel von 1099-Steuerformularen trainieren werden:

1. Wählen Sie in Azure KI Dokument Intelligenz Studio die Option **Benutzerdefiniertes Modell** aus.
1. Wählen Sie unter **Meine Projekte** die Option **+ Projekt erstellen** aus.
1. Geben Sie im Textfeld **Projektname** den Eintrag **1099 Forms** ein, und wählen Sie dann **Weiter** aus.
1. Wählen Sie auf der Seite **Configure service resource** (Dienstressource konfigurieren) in der Dropdownliste **Abonnement** Ihr Azure-Abonnement aus.
1. Wählen Sie in der Dropdownliste **Ressourcengruppe** die Option **DocumentIntelligenceResources** aus.
1. Wählen Sie in der Dropdownliste **Azure KI Dokument Intelligenz- oder Azure KI Services-Ressource** die Option **DocumentIntelligence** aus.
1. Vergewissern Sie sich in der Dropdownliste **API-Version**, dass **2022-06-30-preview** ausgewählt ist, und wählen Sie dann **Weiter** aus.

    :::image type="content" source="../media/4-configure-service-resources.png" alt-text="Screenshot: Seite zum Konfigurieren von Dienstressourcen im Azure KI Dokument Intelligenz Studio-Assistenten für benutzerdefinierte Modelle" lightbox="../media/4-configure-service-resources.png":::

1. Wählen Sie auf der Seite **Configure training data source** (Trainingsdatenquelle konfigurieren) in der Dropdownliste **Abonnement** Ihr Azure-Abonnement aus.
1. Wählen Sie in der Dropdownliste **Ressourcengruppe** die Option **DocumentIntelligenceResources** aus.
1. Wählen Sie in der Dropdownliste **Speicherkonto** das einzige Speicherkonto aus, das aufgeführt wird.
1. Wählen Sie in der Dropdownliste **BLOB-Container** die Option **1099examples** und dann **Weiter** aus.
1. Wählen Sie auf der Seite **Überprüfen und erstellen** die Option **Projekt erstellen** aus.

## Beschriften des benutzerdefinierten Modells „1099 Forms“

Beschriften Sie nun die Beispielformulare mit einigen Feldern:

1. Wählen Sie auf der Seite **Bezeichnungsdaten** oben rechts auf der Seite die Option **+** und dann **Feld** aus.
1. Geben Sie **FirstName** ein, und drücken Sie dann die <kbd>EINGABETASTE</kbd>.
1. Wählen Sie im Dokument **John** aus, und wählen Sie dann **FirstName** aus.
1. Wählen Sie oben rechts auf der Seite **+** und dann **Feld** aus.
1. Geben Sie **LastName** ein, und drücken Sie dann die <kbd>EINGABETASTE</kbd>.
1. Wählen Sie im Dokument **Doe** und dann **LastName** aus.
1. Wählen Sie oben rechts auf der Seite **+** und dann **Feld** aus.
1. Geben Sie **City** ein, und drücken Sie dann die <kbd>EINGABETASTE</kbd>.
1. Wählen Sie im Dokument die Option **New Haven** und dann **City** aus.
1. Wählen Sie oben rechts auf der Seite **+** und dann **Feld** aus.
1. Geben Sie **State** ein, und drücken Sie dann die <kbd>EINGABETASTE</kbd>.
1. Wählen Sie im Dokument die Option **CT** aus, und wählen Sie dann **State** aus.
1. Wiederholen Sie den Beschriftungsprozess für die verbleibenden Formulare in der Liste auf der linken Seite. Beschriften Sie dieselben vier Felder: *FirstName*, *LastName*, *City* und *State*.

## Trainieren des benutzerdefinierten Modells „1099 Forms“

Sie können jetzt das zweite benutzerdefinierte Modell trainieren:

1. Wählen Sie in Azure KI Dokument Intelligenz Studio die Option **Trainieren** aus.
1. Geben Sie im Dialogfeld **Train a new model** (Neues Modell trainieren) im Textfeld **Modell-ID** den Eintrag **1099FormsModel** ein.
1. Wählen Sie in der Dropdownliste **Build mode** (Buildmodus) die Option **Vorlage** aus, und wählen Sie dann **Trainieren** aus. 
1. Wählen Sie im Dialogfeld **Training in Progress** (Training wird durchgeführt) die Option **Go to Models** (Zu den Modellen) aus.
1. Der Trainingsprozess kann einige Minuten dauern. Aktualisieren Sie den Browser gelegentlich, bis für beide Modelle der Status **erfolgreich** angezeigt wird.

## Erstellen und Zusammenführen eines zusammengesetzten Modells

Die beiden benutzerdefinierten Modelle, von denen 1040- und 1099-Steuerformulare analysiert werden, sind jetzt fertig. Sie können jetzt damit fortfahren, das zusammengesetzte Modell zu erstellen:

1. Wählen Sie auf der Azure KI Dokument Intelligenz-Seite „Modelle“ die Optionen **1040FormsModel** und **1099FormsModel** aus.
1. Wählen Sie oben in der Liste der Modelle die Option **Compose** (Zusammenstellen) aus.

    :::image type="content" source="../media/4-start-compose-model.png" alt-text="Screenshot: Starten des Modellerstellungsvorgangs in Azure KI Dokument Intelligenz Studio" lightbox="../media/4-start-compose-model.png":::

1. Geben Sie im Dialogfeld **Compose a new model** (Neues Modell zusammenstellen) im Textfeld **Modell-ID** den Eintrag **TaxFormsModel** ein, und wählen Sie dann **Compose** (Zusammenstellen) aus. Das zusammengesetzte Modell wird von Azure KI Dokument Intelligenz erstellt und in der Liste der benutzerdefinierten Modelle angezeigt:

## Verwenden des zusammengesetzten Modells

Nachdem das zusammengesetzte Modell jetzt fertig ist, testen Sie es mit einem Beispielformular:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) die Option **Alle Ressourcen** aus, und wählen Sie dann das Speicherkonto **formsrecstorage&lt;xxxxx&gt;** aus, wobei &lt;xxxxx&gt; eine Zufallszahl ist.
1. Wählen Sie unter **Datenspeicher** die Option **Container** aus, und wählen Sie dann **TestDoc** aus.
1. Wählen Sie rechts neben **f1040_7.pdf** die Option **...** aus, und wählen Sie dann **Herunterladen** aus.
1. Speichern Sie das PDF-Dokument auf dem lokalen Computer, und notieren Sie sich den gewählten Speicherort.
1. Wählen Sie in Azure KI Dokument Intelligenz Studio die Option **TaxFormsModel** aus, und klicken Sie dann auf **Testen**.
1. Wählen Sie **+ Hinzufügen** aus, und navigieren Sie dann an den Speicherort, an dem Sie das PDF-Dokument gespeichert haben.
1. Wählen Sie **f1040_7.pdf**aus, und wählen Sie dann **Öffnen** aus.
1. Wählen Sie **Analysieren** aus. Azure KI Dokument Intelligenz analysiert das Formular mithilfe des zusammengesetzten Modells.

    :::image type="content" source="../media/4-composed-model-analysis.png" alt-text="Screenshot: Verwenden eines zusammengesetzten Modells in Azure KI Dokument Intelligenz Studio" lightbox="../media/4-composed-model-analysis.png":::

1. Das von Ihnen analysierte Dokument ist ein Beispiel für das Steuerformular 1040. Überprüfen Sie die Eigenschaft **DocType**, um festzustellen, ob das richtige benutzerdefinierte Modell verwendet wurde. Überprüfen Sie auch die vom Modell identifizierten Werte **FirstName**, **LastName**, **City** und **State**.

## Bereinigen der Übungsressourcen

Nachdem Sie gesehen haben, wie zusammengesetzte Modelle funktionieren, entfernen Sie die Ressourcen, die Sie in Ihrem Azure-Abonnement erstellt haben.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) die Option **Ressourcengruppen** aus.
1. Wählen Sie in der Liste der **Ressourcengruppen** die Option **DocumentIntelligenceResources** aus, und klicken Sie dann auf **Ressourcengruppe löschen**. 
1. Geben Sie im Textfeld **RESSOURCENGRUPPENNAME EINGEBEN****DocumentIntelligenceResources** ein, und klicken Sie dann auf **Löschen**, um die Dokument Intelligenz-Ressource und das Speicherkonto zu löschen.

## Weitere Informationen

- [Zusammensetzen benutzerdefinierter Modelle](/azure/ai-services/document-intelligence/concept-composed-models)
- [Erstellen Ihres Trainingsdatasets für ein benutzerdefiniertes Modell](/azure/applied-ai-services/form-recognizer/how-to-guides/build-custom-model-v3)
