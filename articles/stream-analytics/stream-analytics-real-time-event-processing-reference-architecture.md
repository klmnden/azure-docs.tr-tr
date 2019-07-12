---
title: Azure Stream Analytics olay işlemeyi kullanarak gerçek zamanlı Olay işleme
description: Bu makalede, gerçek zamanlı Olay işleme ve Azure Stream Analytics'i kullanarak Analiz elde etmek için başvuru mimarisini açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: ab1ca89427f332151120420c3c087902584706f4
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621775"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Başvuru mimarisi: Gerçek zamanlı Olay işleme ile Microsoft Azure Stream Analytics
Başvuru mimarisi için gerçek zamanlı Olay işleme Azure Stream Analytics ile gerçek zamanlı platformu Microsoft Azure ile hizmet (PaaS) akış işleme çözümü olarak dağıtmak için genel bir şema sağlamak amacını taşımaktadır.

## <a name="summary"></a>Özet
Geleneksel olarak, analiz çözümleri ETL (ayıklama, dönüştürme ve yükleme) ve veri ambarı, gibi analiz öncesinde verilerin depolandığı özelliklerine dayanan. Değişen gereksinimlerini, daha hızlı bir şekilde gelen veriler dahil olmak üzere var olan bu modeli sınırla zorlayan. Yeni bir özellik olmamasına karşın, yaklaşım yaygın dikey tüm sektör yelpazesini benimsenmiştir değil ve veri taşıma depolama önce akışları içinde analiz etmenizi bir çözümüdür. 

Microsoft Azure, bir dizi farklı çözüm senaryolarına ve gereksinimlerine destekleyebildiğini analytics teknolojilerinin kapsamlı bir katalog sunar. Bir uçtan uca çözümü dağıtmak için hangi Azure hizmetlerinin seçilmesi, teklifleri kapsamını verilen bir mücadele haline gelebilir. Bu yazıda, birlikte çalışabilirlik olay akışı çözümünü destekleyen çeşitli Azure Hizmetleri ve özellikleri tanımlamak için tasarlanmıştır. Ayrıca bazı müşteriler bu tür bir yaklaşım yararlanabilir senaryolar açıklanmaktadır.

## <a name="contents"></a>İçindekiler
* Yönetici Özeti
* Gerçek zamanlı analiz giriş
* Gerçek zamanlı verilerin azure'da değer önerisi
* Gerçek zamanlı analiz için ortak senaryolar
* Mimarisi ve bileşenleri
  * Veri Kaynakları
  * Veri tümleştirme katman
  * Gerçek zamanlı analiz katmanı
  * Veri depolama katmanı
  * Sunu / tüketim katman
* Sonuç

**Yazar:** Charles Feddersen, çözüm Mimarı, veri öngörüleri merkezi memnuniyeti, Microsoft Corporation

**Yayım Tarihi:** Ocak 2015

**Düzeltme:** 1.0

**İndirin:** [Gerçek zamanlı Olay işleme ile Microsoft Azure Stream Analytics](https://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için deneyin [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

