---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/30/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 7907504401f4b47aafe6032ea895d9647e6c303c
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420762"
---
Azure ilkesi için her nesne türü için en fazla bir sayısı yoktur. _Kapsam_, aboneliği veya [yönetim grubunu](../articles/governance/management-groups/overview.md) ifade eder.

| Konum | Nesne | Maksimum sayı |
|---|---|---|
| `Scope` | İlke tanımları | 250 |
| `Scope` | Girişim tanımları | 100 |
| Kiracı | Girişim tanımları | 1000 |
| `Scope` | İlke veya girişim atamaları | 100 |
| İlke tanımı | Parametreler | 20 |
| Girişim tanımı | İlkeler | 100 |
| Girişim tanımı | Parametreler | 100 |
| İlke veya girişim atamaları | Özel durumlar (notScopes) | 400 |
| İlke kuralı | İç içe geçmiş koşullar | 512 |
