---
title: Petunjuk Host Online
permalink: index.html
layout: home
ms.openlocfilehash: b85af520a10e63a2f9a5696db03bfd946aff968f
ms.sourcegitcommit: 1ef64e3008a439d0d0bb3d93a27d3df68d3d64a9
ms.translationtype: HT
ms.contentlocale: id-ID
ms.lasthandoff: 03/17/2022
ms.locfileid: "140688692"
---
# <a name="azure-ai-fundamentals-exercises"></a>Latihan Dasar-Dasar AI Azure

Gunakan tautan di bawah untuk menyelesaikan latihan lab langsung untuk kursus Microsoft [AI-900 *Dasar-Dasar Microsoft Azure AI*](https://docs.microsoft.com/learn/certifications/courses/ai-900t00).

Untuk menyelesaikan latihan ini, Anda harus berlangganan Microsoft Azure Jika instruktur Anda belum menyediakannya, Anda dapat mendaftar untuk coba gratis di [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Latihan |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
