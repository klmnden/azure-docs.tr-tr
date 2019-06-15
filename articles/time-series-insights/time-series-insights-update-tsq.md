---
title: Azure zaman serisi öngörüleri Önizleme veri sorgulama | Microsoft Docs
description: Azure zaman serisi öngörüleri Önizleme veri sorgulama.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: seodec18
ms.openlocfilehash: bbf682df2df7a8cdc9fedb36aa4244fc5c0e9488
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244011"
---
# <a name="data-querying"></a>Veri sorgulama

Azure zaman serisi öngörüleri Önizleme, olayları ve genel yüzey API'leri aracılığıyla ortam içinde depolanan meta verileri Sorgulama sağlar. Bu API'ler ayrıca kullanılan [zaman serisi öngörüleri Önizleme Gezgini](./time-series-insights-update-explorer.md).

Üç birincil API kategorileri, zaman serisi Öngörülerinde sağlanır:

* **Ortam API'leri**: Sorguları zaman serisi görüşleri ortamı sağlar. Çağıranın erişimi olan ortamların listesini ve ortam meta veri sorguları örnekleridir.

* **Zaman serisi modeli-sorgu (TSM-Q) API'leri**: Etkinleştirir oluşturma, okuma, güncelleştirme ve silme işlemleri zaman serisi modeli bir ortam parçası içinde depolanan meta veriler. Örnekler, türleri ve Hiyerarşiler örnektir.

* **Zaman serisi sorgu (TSQ) API'leri**: Kaynak Sağlayıcısı'ndan kaydedilirken olayları veri alımını sağlar. Bu API'ler, dönüştürme, birleştirmek ve zaman serisi verileri üzerinde hesaplamalar işlemleri gerçekleştirebilir.

[Zaman serisi ifade (TSX) dil](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) güçlü dördüncü kategori. Zaman serisi modelleri, oluşumunu Gelişmiş hesaplama etkinleştirmek için kullanır.

## <a name="azure-time-series-insights-preview-core-apis"></a>Azure zaman serisi öngörüleri Önizleme core API'leri

Aşağıdaki Çekirdek API'leri desteklenir.

[![Zaman serisi sorgu genel bakış](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>Ortam API'leri

Aşağıdaki ortam API'leri kullanılabilir:

* [API ortamını Al](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environments-api): Ortam listesi, çağırana erişme yetkisi verir.
* [Ortam kullanılabilirlik API alma](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environment-availability-api): Olay zaman damgası üzerinde olay sayısı dağılımını döndürür `$ts`. Bu API, özel olarak varsa meydana gelen olayları zaman damgasının, olayların sayısı döndürerek varsa belirlenmesine yardımcı olur.
* [Olay şeması API get](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-event-schema-api): Belirtilen arama aralığının olay şema meta verilerini döndürür. Bu API tüm meta veriler ve özellikler kullanılabilir şemasında belirtilen, arama aralığının yardımcı olur.

## <a name="time-series-model-query-tsm-q-apis"></a>Zaman serisi modeli-sorgu (TSM-Q) API'leri

Aşağıdaki zaman serisi modeli sorgu API'leri kullanılabilir:

* [Model ayarları API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): Alma ve varsayılan türünü ve model adını ortamın düzeltme sağlar.
* [API türleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): CRUD, zaman serisi türleri ve bunların ilişkili değişkenler sağlar.
* [Hiyerarşiler API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): CRUD, zaman serisi hiyerarşileri ve onların ilişkilendirilmiş alan yollarını sağlar.
* [API örnekleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): CRUD, zaman serisi örnekler ve bunların ilişkili örnek alanları sağlar.

## <a name="time-series-query-tsq-apis"></a>Zaman serisi sorgu (TSQ) API'leri

Aşağıdaki zaman serisi sorgu API'leri kullanılabilir:

* [Olayları API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-events-api): Zaman serisi Öngörülerinde kaynak Sağlayıcısı'ndan kayıtlı sorgu ve olayları zaman serisi öngörüleri verilerini alınmasını sağlar.

* [API serisi alma](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-series-api): Sorgu etkinleştirir ve kablo kaydedilen veriler kullanılarak yakalanan olayları zaman serisi görüşleri verileri alma. Döndürülen değerler modelde tanımlı veya satır içi sağlanan değişkenlere temel alır.

    >[!NOTE]
    > Bir modelde belirtilen veya satır içi sağlanan bile toplama yan tümcesi göz ardı edilir.

  Alma serisi API her aralık için her bir değişken için bir zaman serisi değer döndürür. Zaman serisi değeri JSON çıktısını bir sorgu için Time Series Insights'ı kullanan bir biçimidir. Döndürülen değerler, zaman serisi kimliği ve sağlanan değişkenler kümesini temel alır.

* [API serisi toplama](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#aggregate-series-api): Sorgu etkinleştirir ve örnekleme ve toplama tarafından yakalanan olayları zaman serisi görüşleri verileri alma, veri kaydedilmiş.

  Toplama serisi API her aralık için her bir değişken için bir zaman serisi değer döndürür. Değerler, zaman serisi kimliği ve sağlanan değişkenler kümesini temel alır. Toplama serisi API'si azaltma zaman serisi modelde tutulan veya satır içi aggregate veya örnek verilere sağlanan değişkenlerini kullanarak elde eder.

  Toplama türleri desteklenir: `Min`, `Max`, `Sum`, `Count`, `Average`

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [depolama ve giriş](./time-series-insights-update-storage-ingress.md) Azure zaman serisi öngörüleri önizlemede.

- Zaman serisi öngörüleri Önizleme okuma [veri modelleme](./time-series-insights-update-tsm.md) makalesi.

- Bulma [en iyi uygulamalar bir zaman serisi kimliği seçerken](./time-series-insights-update-how-to-id.md).
