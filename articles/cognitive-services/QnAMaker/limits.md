---
title: Sınırları ve sınır - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu arasında sınırları kapsamlı bir listesi.
services: cognitive-services
author: tulasim88
manager: cjgronlund
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: c48a460e180d6c493083624236fd39f6324c5f12
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995627"
---
# <a name="qna-maker-limits"></a>Soru-cevap Oluşturucu sınırlar
Soru-cevap Oluşturucu arasında sınırları kapsamlı bir listesi.

## <a name="knowledge-bases"></a>Bilgi bankaları

* Bilgi bankaları en fazla sayısını temel alarak [Azure arama katmanı sınırları](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Azure arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Yayımlanan bilgi bankalarından sayısı izin verilen (en fazla dizin--1 (test için ayrılmış)|2|14|49|199|199|2999|

## <a name="extraction-limits"></a>Ayıklama sınırları
* En fazla ayıklanabileceği dosya sayısını ve en büyük dosya boyutu: bkz [QnAMaker fiyatlandırması](https://azure.microsoft.com/en-in/pricing/details/cognitive-services/qna-maker/)
* En fazla SSS HTML sayfaları için ayıklama bankalarıyla gezinilebilen ayrıntılı bağlantı sayısı: 20

## <a name="metadata-limits"></a>Meta veri sınırları
* Meta veri alanları Bilgi Bankası başına en fazla sayısını temel alarak [Azure arama katmanı sınırları](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Azure arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Soru-cevap Oluşturucu hizmeti (genelinde tüm KB'leri) başına en fazla meta veri alanları|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Bilgi Bankası içerik sınırları
Bilgi Bankası'nda içeriği genel sınırlamaları:
* Yanıt metnin uzunluğunu: 250000
* Soru metnin uzunluğunu: 1000
* Meta verileri anahtar/değer metnin uzunluğunu: 100
* Karakterler için meta veri adı desteklenen: harfler, rakamlar ve _  
* Meta veri değeri için karakter desteklenir: hariç: ve | 
* Dosya adının uzunluğu: 200
* Desteklenen dosya biçimleri: ".tsv", ".pdf", ".txt", ".docx", ".xlsx".
* Alternatif soru sayısı: 100
* Soru-cevap çiftlerini sayısı: bağımlı [Azure arama katmanı](https://docs.microsoft.com/en-in/azure/search/search-limits-quotas-capacity#document-limits) seçildi 

## <a name="create-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları oluşturun:
Bu temsil Bilgi Bankası eylem her sınırları oluşturmak; diğer bir deyişle, tıklayıp *oluşturma KB* veya CreateKnowledgeBase API'ye çağrı yapma.
* Diğer sorular yanıt başına en fazla sayısı: 100
* URL'leri sayısı: 10
* En fazla dosya sayısı: 10

## <a name="update-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları güncelleştir
Bunlar, her güncelleştirme eylemi sınırlarını temsil eder; diğer bir deyişle, tıklayıp *kaydedin ve eğitme* veya UpdateKnowledgeBase API'ye çağrı yapma.
* Her kaynak adının uzunluğu: 300
* Eklendiğinde veya silindiğinde alternatif soru sayısı: 100
* Meta veri alanları en fazla sayısını eklendiğinde veya silindiğinde: 10
* Yenilenebilir URL'leri sayısı: 5
