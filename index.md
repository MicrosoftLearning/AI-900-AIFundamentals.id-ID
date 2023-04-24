---
title: Petunjuk Host Online
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Latihan Dasar-Dasar AI Azure

Latihan langsung ini dirancang untuk mendukung konten pelatihan di [Microsoft Learn](https://docs.microsoft.com/training/).

Untuk menyelesaikan latihan ini, Anda harus berlangganan Microsoft Azure Anda dapat mendaftar untuk uji coba gratis di [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="labs"></a>Lab

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions'" %}
| Modul | Laboratorium |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}