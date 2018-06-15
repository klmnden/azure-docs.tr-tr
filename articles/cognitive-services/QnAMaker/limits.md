---
title: QnA Maker sınırları - Azure Bilişsel hizmetler | Microsoft Docs
description: QnA Maker sınırları
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 4d2bafad08ffab76743cb802733a5d2f01a97f06
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356029"
---
# <a name="qna-maker-limits"></a>QnA Maker sınırları
Sınırları QnA Maker arasında kapsamlı bir listesi.

## <a name="knowledge-bases"></a>Bilgi Bankası

* Bilgi Bankası sayısını temel alarak [Azure Search katmanı sınırları](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Azure arama katmanı** | **Boş** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Yayımlanan Bilgi Bankası (Max dizinler--1 (test için ayrılmış) verilen|2|14|49|199|199|2999|

## <a name="extraction-limits"></a>Ayıklama sınırları
* En fazla ayıklanabilir dosya sayısını ve maksimum dosya boyutu: bkz [QnAMaker fiyatlandırma](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* Maksimum QnAs ayıklama için SSS HTML sayfaları gezinilebilen derin-bağlantı sayısı: 20

## <a name="metadata-limits"></a>Meta veri sınırları
* Meta veri alanlarını Bilgi Bankası ' nda başına en fazla temel alarak [Azure Search katmanı sınırları](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity)

|**Azure arama katmanı** | **Boş** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|QnA Maker hizmeti (genelinde tüm KB) başına en fazla meta veri alanları|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Bilgi Bankası içerik sınırları
Bilgi Bankası'nda içerik genel sınırlamaları:
* Yanıt metni uzunluğu: 250000
* Soru metnini uzunluğu: 1000
* Meta verileri anahtar/değer metnin uzunluğu: 100
* Karakterler için meta veri adı desteklenen: harfler, rakamlar ve _  
* Meta veri değeri için karakter desteklenen: hariç tümünü: ve | 
* Dosya adının uzunluğu: 200
* Desteklenen dosya biçimleri: ".tsv", ".pdf", ".txt", ".docx", ".xlsx".
* Alternatif soru sayısı: 100
* Soru-yanıt çiftlerinin maksimum sayısı: bağlıdır [Azure Search katmanı](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) seçildi 

## <a name="create-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları oluşturun:
Bu temsil eder, Bilgi Bankası eylem her sınırlarını oluşturun; diğer bir deyişle, tıklatarak *oluşturma KB* veya CreateKnowledgeBase API çağırma.
* Diğer sorular yanıt başına maksimum sayısı: 100
* URL'ler üst sınırını: 10
* En fazla dosya sayısı: 10

## <a name="update-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları güncelleştir
Bunlar, her güncelleştirme eylemini sınırlarını temsil eder; diğer bir deyişle, tıklatarak *kaydedin ve eğitmek* veya UpdateKnowledgeBase API çağırma.
* Her kaynak adın uzunluğu: 300
* Soru maksimum sayısı alternatif eklenmiş veya silinmiş sorular: 100
* Meta veri alanı sayısı eklenmiş veya silinmiş: 10
* Yenilenebilir URL'leri sayısı: 5
