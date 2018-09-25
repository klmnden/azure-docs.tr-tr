---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: c3365450c90c4fda37884e8998fad70f5d164244
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006530"
---
Azure İlkesi'nde her nesne türü için bir maksimum sayı vardır. _Kapsam_, aboneliği veya [yönetim grubunu](../articles/governance/management-groups/overview.md) ifade eder.

| Konum | Nesne | Maksimum sayı |
|---|---|---|
| Kapsam | İlke Tanımları | 250 |
| Kapsam | Girişim Tanımları | 100 |
| Kiracı | Girişim Tanımları | 1000 |
| Kapsam | İlke/Girişim Atamaları | 100 |
| İlke Tanımı | Parametreler | 20 |
| Girişim Tanımı | İlkeler | 100 |
| Girişim Tanımı | Parametreler | 100 |
| İlke/Girişim Atamaları | Özel durumlar (notScopes) | 100 |
| İlke Kuralı | İç İçe Geçmiş Koşullar | 512 |
