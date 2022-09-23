---
title: Petunjuk Host Online
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Latihan Dasar-Dasar AI Azure

Latihan langsung ini dirancang untuk mendukung konten pelatihan di [Microsoft Learn](https://docs.microsoft.com/training/).

To complete these exercises, you'll need a Microsoft Azure subscription. You can sign up for a free trial at <bpt id="p1">[</bpt><ph id="ph1">https://azure.microsoft.com</ph><ept id="p1">](https://azure.microsoft.com)</ept>.

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Latihan |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
