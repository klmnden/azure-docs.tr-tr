---
title: Azure Stream Analytics olay işleme kullanarak gerçek zamanlı Olay işleme
description: Bu makalede, gerçek zamanlı Olay işleme ve analizi kullanarak Azure Stream Analytics elde etmek için referans mimarisi açıklanmaktadır.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: 8a5d426d67916e010c7fff048eebdc77b93c5c38
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30902591"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Başvuru mimarisi: Gerçek zamanlı Olay Microsoft Azure akış Analizi ile işleme
Gerçek zamanlı Olay Azure akış Analizi ile işleme için başvuru mimarisi, Microsoft Azure ile hizmet (PaaS) akış işleme çözümü olarak gerçek zamanlı bir platform dağıtmak için genel şeması sağlamaya yöneliktir.

## <a name="summary"></a>Özet
Geleneksel olarak, analiz çözümleri analiz öncesinde verilerin depolandığı yetenekleri ETL (ayıklama, dönüştürme ve yükleme) ve veri ambarı, gibi temel. Daha fazla hızlı bir şekilde gelen veriler dahil değişen gereksinimleri, bu varolan model sınırla zorlayan. Yeni bir özellik olmamasına karşın, bir yaklaşım yaygın olarak tüm endüstri verticals benimsemiştir değil ve depolama önce akışları taşıma içinde verileri analiz etme olanağı bir çözümüdür. 

Microsoft Azure bir dizi farklı çözüm senaryoları ve gereksinimleri destekleyebildiğini analytics teknolojilerin kapsamlı bir katalog sağlar. Uçtan uca çözümünü dağıtmak için hangi Azure Hizmetleri seçme teklifleri derecesini verilen zor olabilir. Bu yazı, birlikte çalışabilirlik olay akışı çözümünü destekleyen çeşitli Azure Hizmetleri ve özellikleri açıklamak için tasarlanmıştır. Ayrıca müşteriler bu yaklaşım türünden yararlanabilir senaryolardan bazıları açıklanmaktadır.

## <a name="contents"></a>İçindekiler
* Yönetici Özeti
* Gerçek zamanlı analiz giriş
* Değer teklifinde Azure gerçek zamanlı veri
* Gerçek zamanlı analiz için ortak senaryolar
* Mimarisi ve bileşenleri
  * Veri Kaynakları
  * Veri tümleştirme katmanı
  * Gerçek zamanlı analiz katmanı
  * Veri depolama katmanı
  * Sunu / tüketim katman
* Sonuç

**Yazar:** mükemmel, Microsoft Corporation'ın Charles Feddersen, çözümü Mimarı veri Öngörüler Merkezi

**Yayımlanan:** Ocak 2015

**Düzeltme:** 1.0

**İndirin:** [gerçek zamanlı Olay Microsoft Azure akış Analizi ile işleme](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin [Azure akış analizi Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

