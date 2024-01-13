---
title: Instruksi yang Dihosting Online
permalink: index.html
layout: home
---

# Latihan Dasar-Dasar Azure AI

Latihan langsung ini dirancang untuk mendukung konten pelatihan di [Microsoft Learn](https://docs.microsoft.com/training/).

Untuk menyelesaikan latihan ini, Anda memerlukan langganan Microsoft Azure. Anda dapat mendaftar untuk uji coba gratis di [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Latihan |
| ------- | 
{% untuk aktivitas di lab %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
