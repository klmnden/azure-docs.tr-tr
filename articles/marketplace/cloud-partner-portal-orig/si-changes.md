---
title: Satıcı Insights sürüm notları
description: Satıcı öngörü özelliği değişiklikler hakkında bilgi sağlar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 3/3/2019
ms.author: pabutler
ms.openlocfilehash: c6e9e4fe672c7e171ed4b1cd60655f9e71a562e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64943116"
---
# <a name="seller-insights-release-notes"></a>Satıcı Insights sürüm notları 

(Yayın Tarihi: 1 Mart 2019)

Bu makale, satıcı Insights özelliği değişiklikler hakkında bilgi sağlar. [bulut iş ortağı portalı](https://cloudpartner.azure.com/#insights).

## <a name="release-highlights-for-march-1-2019"></a>1 Mart 2019 için yayın vurgular

* *Müşteri eğilim* özetine eklendi
* *İlk beş müşteriler* bir müşterinin bulunan tüm Azure abonelikleri ile ilgili özet bilgileri yansıtır
* *Normalleştirilmiş kullanım eğilimi ve etkin siparişler eğilimi* özeti altında taşınmış üzerinde *Aylık Siparişler bir bakışta*
* *Ödeme uzlaştırma raporu* güncelleştirildi
* *İlk beş müşteriler* bir müşterinin bulunan tüm Azure abonelikleri üzerinde ödeme yansıtır
* *Kullanım Raporu* Müşteri Kimliği ile güncelleştirildi
* *Müşteri çalışma süresi* siparişleri & kullanımı bir müşterinin bulunan tüm Azure abonelikleri yansıtır.


(Yayın Tarihi: 28 Temmuz 2018)

## <a name="release-highlights-for-july-28-2018"></a>Yayının öne çıkan noktaları 28 Temmuz 2018 için


-   *Fiyat tahmini* müşteri ücretleri görünümünü para birimi dönüştürme uygulamalarını sağlar.
-   *Tahmini ödemeler* olası ödemeler önceki bir görünüm sağlar.
-  *Kullanım başvuru tanımlayıcıları* müşteri kullanımını ve siparişler arasındaki verilerin aslına uygunluğunu ödemeler ile sağlayın
-   *Günlük dilimde gösterimi kullanımı* daha fazla ayrıntı ve daha iyi müşteri kullanımını Öngörüler sağlar.


### <a name="changes-to-data-structure-and-taxonomy"></a>Değişiklikleri veri yapısı ve sınıflandırma

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
