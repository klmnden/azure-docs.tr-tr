---
title: Azure Time Series Insights (Önizleme) veri sorgulama | Microsoft Docs
description: Azure Time Series Insights (Önizleme) verileri Sorgulama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/04/2018
ms.openlocfilehash: 00ef6eed23d1645320c28123d6670230cdd725c9
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964926"
---
# <a name="data-querying"></a>Veri sorgulama

Azure zaman serisi öngörüleri (TSI) olayları ve genel yüzey API'leri aracılığıyla ortam içinde depolanan meta verileri Sorgulama sağlar. Bu API'ler ayrıca kullanılan [TSI Gezgini](./time-series-insights-update-explorer.md).

Azure TSI kullanılabilir üç birincil API kategorisi vardır:

* TSI ortam sorgulama ortam API'leri etkinleştirin, ortamların listesi gibi arayan, ortam meta veriler, vb. erişebilir.

* Zaman serisi modeli sorgu (TSM-Q) API'leri etkinleştir oluşturma, okuma, güncelleştirme ve silme işlemleri zaman serisi modeli bir ortam parçası içinde depolanan meta veriler. Örnekler, türleri, hiyerarşileri, vb. gibi.

* Zaman serisi sorgu (TSQ) API'leri, kaynak Sağlayıcısı'ndan kaydedilmiş veya işlemleri dönüştürme, birleştirmek ve zaman serisi verileri üzerinde hesaplamalar gerçekleştirebilirsiniz olayları veri alımını etkinleştirin.

[Zaman serisi ifade dili](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) (TSX), güçlü, dördüncü, kategori. Zaman serisi modelleri (TSM), oluşumunu Gelişmiş hesaplama etkinleştirmek için kullanır.

## <a name="azure-time-series-insights-core-apis"></a>Azure Time Series Insights core API'leri

Çekirdek destekliyoruz API'leri aşağıdadır.

![tsq][1]

### <a name="the-environment-apis"></a>Ortam API'leri

Mevcut ortam API'lere şunlardır:

* [Ortam API'sini kullanmaya başlamak](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environments-api): ortam listesi çağırana erişme yetkisi verir.
* [Ortam kullanılabilirlik API'sini kullanmaya başlamak](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environment-availability-api): olay zaman damgası üzerinde olay sayısı dağılımını döndürür `$ts`. Bu API, herhangi bir olayı, olay sayısı döndürerek zaman damgası var olup olmadığını yardımcı olur.
* [Olay şeması API'sini kullanmaya başlamak](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-event-schema-api): Belirtilen arama yayılma için olay şema meta verileri döndürür. Bu API tüm meta veri/mevcut özelliklerin şemasında belirtilen, arama aralığının yardımcı olur.

### <a name="time-series-model-query-tsm-q-apis"></a>Zaman serisi modeli-sorgu (TSM-Q) API'leri

Zaman serisi modeli sorgu API'leri kullanılabilir şunlardır:

* [Model ayarları API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): alın ve düzeltme eki varsayılan türünü ve model adını ortamın sağlar.
* [API türleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): zaman serisi türlerine ve bunların ilişkili değişkenler CRUD sağlar.
* [Hiyerarşiler API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): zaman serisi hiyerarşileri ve ilişkili alan yollarına CRUD sağlar.
* [API örnekleri](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): zaman serisi örnekleri ve bunların ilişkili örnek alanları CRUD sağlar.

### <a name="the-time-series-query-tsq-apis"></a>Zaman serisi sorgu (TSQ) API'ları

Zaman serisi sorgu API'leri kullanılabilir şunlardır:

* [Olayları API'sini kullanmaya başlamak](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-events-api): alma olayları API'si, sorgu ve olayları TSI verileri alma Azure TSI kaynak Sağlayıcısı'ndan kayıtlı sağlar.

* [Seri API'sini kullanmaya başlamak](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-series-api): Sorgu sağlar ve Azure TSI verileri değişkenleri kullanarak kablo kaydedilen veri yararlanarak yakalanan olayları alma model veya sağlanan satır içinde tanımlayın.

    >[!NOTE]
    > Toplama yan tümcesi bir modelde belirtilen bile göz ardı veya satır içi sağlanan.

  Sağlanan temel her zaman aralığı için her bir değişken için alma serisi API (zaman serisi değeri, TSI kullandığı için sorgu JSON çıktısını bir biçimde) bir TSV döndürür **zaman serisi kimliği** ve sağlanan değişkenleri kümesi.

* [Toplama serisi API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#aggregate-series-api): kayıtlı veri sorgu etkinleştirir ve örnekleme ve toplama tarafından yakalanan olayları TSI verileri alma.

  Sağlanan temel her zaman aralığı için her değişken için bir TSV toplama serisi API döndürür **zaman serisi kimliği** ve sağlanan değişkenleri kümesi. Toplama serisi API TSM içinde depolanan veya satır içi aggregate veya örnek verilere sağlanan değişkenleri yararlanarak azaltma ulaşır.

  Toplama türleri desteklenir: `Min`, `Max`, `Sum`, `Count`, `Average`

## <a name="next-steps"></a>Sonraki adımlar

Okuma [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Hakkında bilgi edinin [veri modelleme](./time-series-insights-update-tsm.md).

Hakkında bilgi edinin [iyi bir zaman serisi kimliği seçerken](./time-series-insights-update-how-to-id.md).

<!-- Images -->
[1]: media/v2-update-tsq/tsq.png
