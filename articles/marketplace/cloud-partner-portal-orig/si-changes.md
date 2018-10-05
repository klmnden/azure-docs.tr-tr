---
title: Satıcı Insights sürüm notları | Microsoft Docs
description: Satıcı öngörü özelliği değişiklikler hakkında bilgi sağlar.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ec3784a91f8aeb7f0fedd13c9ab86a844832a578
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811279"
---
<a name="seller-insights-release-notes"></a>Satıcı Insights sürüm notları 
===============================
(Yayın Tarihi: 28 Temmuz 2018)

Bu makale, satıcı Insights özelliği değişiklikler hakkında bilgi sağlar. [bulut iş ortağı portalı](https://cloudpartner.azure.com/#insights).


<a name="release-highlights"></a>Yayının öne çıkan noktaları
------------------

-   *Fiyat tahmini* müşteri ücretleri görünümünü para birimi dönüştürme uygulamalarını sağlar.
-   *Tahmini ödemeler* olası ödemeler önceki bir görünüm sağlar.
-  *Kullanım başvuru tanımlayıcıları* müşteri kullanımını ve siparişler arasındaki verilerin aslına uygunluğunu ödemeler ile sağlayın
-   *Günlük dilimde gösterimi kullanımı* daha fazla ayrıntı ve daha iyi müşteri kullanımını Öngörüler sağlar.


<a name="changes-to-data-structure-and-taxonomy"></a>Değişiklikleri veri yapısı ve sınıflandırma
--------------------------------------

Aşağıdaki tabloda, eklenen veya önemli ölçüde bu sürümle birlikte değişen ölçümlerin listeler. 

| **Yeni terim**                   |    **Tanım**                                                             |
|--------------------------------|  ---------------------------------------------------------------------------- |
| Fiyat (CC)                     | Kullanım (müşterinin para birimi içinde) belirtilen bir SKU için bir birim fiyatı.       |
| Tahmini genişletilmiş ücreti (CC) | Genişletilmiş birim kullanım miktarını ücreti (müşterinin para birimi içinde) belirtilen bir SKU için tahmini. Bu değer, yuvarlama veya kesme hataları nedeniyle tam olmayabilir.   |
| Yayımcı para birimi (PC)        | Para birimi olarak doğrulanmak için yayımcı tarafından tercih edilir.                               |
| Tahmini Fiyat (PC)           | Tahmini fiyat Döviz dönüştürme tarihi kullanımına dayalı belirli bir SKU kullanımının bir birim için (yayımcının para biriminde) olarak hesaplanır. Bu değer, yuvarlama veya kesme hataları nedeniyle tam olmayabilir.   |
| Tahmini genişletilmiş ücreti (PC) | Birim kullanım miktarını genişletilmiş tahmini ücreti Döviz dönüştürme tarihi kullanımına dayalı belirli bir SKU (yayımcının para biriminde) olarak hesaplanır. Bu değer, yuvarlama veya kesme hataları nedeniyle tam olmayabilir. |
| Tahmini ödeme (PC)          | Tahmini ödeme birimlerinin miktarı için kullanım için belirli bir SKU Döviz dönüştürme kullanım (yayımcının para biriminde) hesaplanır tarihi temel. Bu değer, yuvarlama veya kesme hataları nedeniyle tam olmayabilir.   |
| Kullanım başvurusu                | Bir veya daha fazla günlük müşteri kullanım için ödeme rapordaki bir girişi ile ilişkili belirtilen bir SKU tanımlayıcısı. |
|  |  |
