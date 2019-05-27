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
ms.openlocfilehash: 57cec39bde460c6079091490acf541761c61e003
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66119293"
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
| İlke veya girişim atamaları | Özel durumlar (notScopes) | 250 |
| İlke kuralı | İç içe geçmiş koşullar | 512 |
