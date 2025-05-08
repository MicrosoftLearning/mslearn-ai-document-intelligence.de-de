---
title: Azure KI Dokument Intelligenz-Übungen
permalink: index.html
layout: home
---

Die folgenden Übungen bieten Ihnen praktische Erfahrungen, in denen Sie typische Aufgaben kennenlernen, die Fachkräfte in der Entwicklung beim Erstellen von Dokumentenintelligenzlösungen in Microsoft Azure ausführen.

> **Hinweis**: Um die Übungen durchzuführen, benötigen Sie ein Azure-Abonnement mit ausreichenden Berechtigungen und Kontingenten, um die erforderlichen Azure-Ressourcen bereitzustellen. Wenn Sie noch kein Konto haben, können Sie sich für ein [Azure-Konto](https://azure.microsoft.com/free) anmelden. Für neue Benutzende gibt es dort eine kostenlose Testoption, die Guthaben für die ersten 30 Tage beinhaltet.

## Übungen

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions'" %} {% for activity in labs  %} {% if activity.url contains 'ai-foundry' %} {% continue %} {% endif %}
<hr>
### [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }})

{{activity.lab.description}}

{% endfor %}

> **Hinweis**: Diese Übungen können zwar eigenständig absolviert werden, sie sind jedoch als Ergänzung zu den Modulen auf [Microsoft Learn](https://learn.microsoft.com/training/paths/extract-data-from-forms-document-intelligence/) gedacht, in denen Sie tiefer in einige der zugrunde liegenden Konzepte eintauchen können, auf denen diese Übungen basieren.
