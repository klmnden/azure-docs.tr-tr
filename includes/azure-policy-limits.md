---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 06/05/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: ea87e51e66985f860cd6d10595a32facde1dc639
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133511"
---
Azure ilkesi için her nesne türü için en fazla bir sayısı yoktur. _Kapsam_, aboneliği veya [yönetim grubunu](../articles/governance/management-groups/overview.md) ifade eder.

| Konum | Nesne | Maksimum sayı |
|---|---|---|
| `Scope` | İlke tanımları | 500 |
| `Scope` | Girişim tanımları | 100 |
| Kiracı | Girişim tanımları | 1000 |
| `Scope` | İlke veya girişim atamaları | 100 |
| İlke tanımı | Parametreler | 20 |
| Girişim tanımı | İlkeler | 100 |
| Girişim tanımı | Parametreler | 100 |
| İlke veya girişim atamaları | Özel durumlar (notScopes) | 400 |
| İlke kuralı | İç içe geçmiş koşullar | 512 |
