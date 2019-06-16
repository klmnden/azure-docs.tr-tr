---
title: 'Azure Blob Depolama: Azure arama için tam metin araması Ekle'
description: Azure Search HTTP REST API'leri kullanarak kod içinde dizin için Azure Blob Depolama alanında gezinme metin içeriği.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/01/2019
author: mgottein
manager: cgronlun
ms.author: magottei
ms.custom: seodec2018
ms.openlocfilehash: b6bb70e4c56adb162006d2597d301c73b12d2a8a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540882"
---
# <a name="searching-blob-storage-with-azure-search"></a>Azure Search ile Blob deposu arama

Azure Blob depolamada depolanan içerik türleri çeşitli arama çözmek için zor bir sorun olabilir. Ancak, dizin ve Azure Search kullanarak Bloblarınızın içeriğini yalnızca birkaç tıklamayla arayın. BLOB deposu arama, bir Azure Search hizmet sağlanması gerekir. Çeşitli hizmet sınırları ve Azure Search'ün fiyatlandırma katmanları bulunabilir [fiyatlandırma sayfası](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Azure Search nedir?
[Azure Search'ü](https://aka.ms/whatisazsearch) geliştiricilerin web ve mobil uygulamalarınıza ekleyin, güçlü tam metin arama deneyimleri sağlayan bir arama hizmetidir. Bir hizmet olarak Azure Search teklifi sırasında herhangi bir arama altyapısını yönetme ihtiyacını ortadan kaldırır. bir [% 99,9 çalışma süresi SLA'sı](https://aka.ms/azuresearchsla).

## <a name="index-and-search-enterprise-document-formats"></a>Dizin ve arama Kurumsal Belge biçimleri
Desteğiyle [belge ayıklama](https://aka.ms/azsblobindexer) Azure Blob Depolama alanında aşağıdaki içeriğe dizin oluşturabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

Bu dosya türlerinden metin ve meta verilerini ayıklayarak tek bir sorgu ile birden çok dosya biçimleri arasında arama yapabilirsiniz. 

## <a name="search-through-your-blob-metadata"></a>Blob meta verilerinizi arayın
Herhangi bir içerik türü bir blob'un üzerinden sıralamak kolaylaştıran sık karşılaşılan bir senaryodur özel meta veriler hem Sistem özellikleri her blob için dizin sağlamaktır. Bu şekilde, belge türü ne olursa olsun tüm bloblar için bilgi dizine alınır. Ardından tüm Blob Depolama içeriği arasında sıralama, filtreleme ve modeli geçebilirsiniz.

[Meta veriler blob dizin oluşturma hakkında daha fazla bilgi edinin.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Resim arama
Azure Search'ün tam metin araması, çok yönlü gezinme ve sıralama özellikleri artık meta veri blob'larda depolanan görüntülerin uygulanabilir.

Bilişsel arama içeren görüntü işleme becerileri gibi [optik karakter tanıma (OCR)](cognitive-search-skill-ocr.md) tanımlama ve [görsel özellikleri](cognitive-search-skill-image-analysis.md) olun, her bulunan görsel içerik dizinleme olanağı görüntü.

## <a name="index-and-search-through-json-blobs"></a>Dizin ve JSON BLOB'ları ile arama
Azure Search, JSON içeren bloblar bulundu yapılandırılmış içeriği ayıklamak için yapılandırılabilir. Azure Search, JSON BLOB'ları okuyun ve bir Azure Search belgesinin uygun alanlara yapılandırılmış içeriği ayrıştırılamıyor. Azure arama, bir JSON nesne dizisi içerir ve her öğe için ayrı bir Azure Search belge harita blobları da yararlanabilirsiniz.

JSON ayrıştırma şu anda portal üzerinden yapılandırılabilir değildir. [Azure Search'te JSON ayrıştırma hakkında daha fazla bilgi edinin.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Hızlı başlangıç
Azure arama, doğrudan Blob Depolama portal sayfasında blob'lara eklenebilir.

![](./media/search-blob-storage-integration/blob-blade.png)

Tıklayın **Azure Search Ekle** burada mevcut bir Azure Search hizmeti seçin veya yeni bir hizmet oluşturun, bir akışı başlatmak için. Yeni bir hizmet oluşturun, depolama hesabınızın portal deneyimi gitme. Geri depolama portal sayfasına gidin ve yeniden seçin **Azure Search Ekle** hizmetiniz seçebileceğiniz seçeneği.

## <a name="next-steps"></a>Sonraki Adımlar
Azure Search Blob dizin oluşturucu tam hakkında daha fazla bilgi [belgeleri](https://aka.ms/azsblobindexer).
