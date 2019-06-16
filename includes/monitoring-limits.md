---
title: include dosyası
description: include dosyası
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 050d3314345e64e3d69a07367a0e9acc318fa106
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66271498"
---
| Resource | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Otomatik ölçeklendirme ayarları |Her Abonelikteki bölge başına 100. | Varsayılan ile aynıdır. |
| Ölçüm uyarıları (Klasik) |Abonelik başına 100 etkin uyarı kuralları. | Desteği arayın. |
| Ölçüm uyarıları |1000 etkin uyarı kuralları her abonelikte (genel Bulutlar) ve Azure China ve Azure kamu, abonelik başına 100 etkin uyarı kuralları. | Desteği arayın. |
| Etkinlik günlüğü uyarıları | Abonelik başına 100 etkin uyarı kuralları. | Varsayılan ile aynıdır. |
| Günlük uyarıları | 512 | Desteği arayın. |
| Eylem grupları |Abonelik başına 2.000 Eylem grupları. | Desteği arayın. |

**Eylem grubu özgü sınırları**

| Resource | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Azure uygulama iletimi | Eylem grubu başına 10 azure uygulama eylemleri. | Desteği arayın. |
| Email | bir eylem grubu 1. 000'e-posta eylemleri. Ayrıca bkz: [bilgileri sınırlama oranı](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Desteği arayın. |
| ITSM | Bir eylem grubu 10 ITSM eylemleri. | Desteği arayın. | 
| Mantıksal uygulama | bir eylem grubu 10 mantıksal uygulama eylemleri. | Desteği arayın. |
| Runbook | bir eylem grubu 10 runbook eylemleri. | Desteği arayın. |
| SMS | Bir eylem grubu 10 SMS eylemler. Ayrıca bkz: [bilgileri sınırlama oranı](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Desteği arayın. |
| Ses | bir eylem grubu 10 ses eylemler. Ayrıca bkz: [bilgileri sınırlama oranı](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Desteği arayın. |
| Web Kancası | bir eylem grubu 10 Web kancası eylemleri.  Web kancası çağrıları, en fazla 1500 abonelik başına dakika başına verilir. Diğer limitler kullanılabilir [eylem özgü bilgileri](../articles/azure-monitor/platform/action-groups.md#action-specific-information).  | Desteği arayın. |
