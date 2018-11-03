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
ms.openlocfilehash: 0a54dfdb810ea578c1e7c8fcc7ca0343e72164ae
ms.sourcegitcommit: 3dcb1a3993e51963954194ba2a5e42260d0be258
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50964648"
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
| İlke/Girişim Atamaları | Özel durumlar (notScopes) | 250 |
| İlke Kuralı | İç İçe Geçmiş Koşullar | 512 |
