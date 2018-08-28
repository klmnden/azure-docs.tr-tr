---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/16/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 0e5d1214a8e1af8299cb40d1a3b31ff6eafc8c5c
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40184823"
---
Azure İlkesi'nde her nesne türü için bir maksimum sayı vardır. _Kapsam_, aboneliği veya [yönetim grubunu](../articles/azure-resource-manager/management-groups-overview.md) ifade eder.

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
