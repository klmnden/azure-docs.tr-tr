---
title: "Azure arama Blob depolama alanına ekleme | Microsoft Docs"
description: "Azure Search HTTP REST API'sini kullanarak kod içinde bir dizin oluşturun."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
ms.service: search
ms.topic: article
ms.date: 05/04/2017
ms.author: ashmaka
ms.openlocfilehash: 15469e8a2d28bdf00d6e8d8c9f823c51975ee90e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="searching-blob-storage-with-azure-search"></a>Azure Search ile Blob deposu arama

Azure Blob depolamada depolanan içerik türleri çeşitli genelinde arama çözmek için zor bir sorun olabilir. Ancak, dizin ve Azure Search kullanarak yalnızca birkaç tıklama ile Bloblarınızın içeriğini arayın. BLOB storage arama Azure Search Hizmeti sağlanması gerekir. Çeşitli hizmet sınırları ve Azure Search'ün fiyatlandırma katmanlarına bulunabilir [fiyatlandırma sayfası](https://aka.ms/azspricing).

## <a name="what-is-azure-search"></a>Azure Search nedir?
[Azure arama](https://aka.ms/whatisazsearch) güçlü tam metin arama deneyimleri web ve mobil uygulamalar eklemek geliştiriciler için kolaylaştıran bir arama hizmetidir. Bir hizmet olarak Azure arama sunumu sırasında herhangi bir arama altyapı yönetme ihtiyacını ortadan kaldırır. bir [% 99,9 çalışma süresi SLA](https://aka.ms/azuresearchsla).

## <a name="index-and-search-enterprise-document-formats"></a>Dizin ve arama Kurumsal Belge biçimleri
Desteğiyle [belge ayıklama](https://aka.ms/azsblobindexer) Azure Blob depolama alanına aşağıdaki içeriğe dizin oluşturabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

Bu dosya türlerinden metin ve meta verileri çıkartarak, tek bir sorgu ile birden çok dosya biçimleri arasında arama yapabilirsiniz. 

## <a name="search-through-your-blob-metadata"></a>, Blob meta veri arama
Özel meta verileri ve Sistem özellikleri her bir blob için dizin için herhangi bir içerik türüne BLOB'lar sıralama kolaylaştırır yaygın bir senaryo değil. Bu şekilde, belge türü ne olursa olsun tüm BLOB'lar için bilgi dizine alınır. Ardından tüm Blob Depolama içeriği arasında sıralama, filtre ve model geçebilirsiniz.

[Meta veriler blob dizin oluşturma hakkında daha fazla bilgi edinin.](https://aka.ms/azsblobmetadataindexing)

## <a name="image-search"></a>Görüntü arama
Azure Search'ün tam metin araması, çok yönlü gezinme ve sıralama yeteneklerini görüntülerinin blob'larda depolanan meta veriler artık uygulanabilir.

Bu görüntüleri kullanarak önceden işlenirse [bilgisayar görme API](https://www.microsoft.com/cognitive-services/computer-vision-api) Microsoft'un Bilişsel hizmetlerinden sonra OCR ve el yazısı tanıma dahil olmak üzere her görüntüde bulunan görsel içerik dizin oluşturmak mümkündür. OCR eklemek için çalışıyoruz ve bu özelliklere ilgileniyorsanız diğer görüntü işleme özelliklerini doğrudan Azure Search üzerinde bir istek göndermek bizim [UserVoice](https://aka.ms/azsuv) veya [bize e-posta](mailto:azscustquestions@microsoft.com).

## <a name="index-and-search-through-json-blobs"></a>Dizin ve JSON BLOB'ları ile arama
Azure arama, yapılandırılmış içerik JSON içeren BLOB'ları bulundu ayıklamak için yapılandırılabilir. Azure arama, JSON BLOB'ları okuyun ve bir Azure Search belge uygun alanlara yapılandırılmış içeriği ayrıştırılamıyor. Azure Search, her öğe ayrı bir Azure Search belgeye eşleyin ve bir dizi JSON nesnesi içeren BLOB'ları da alabilir.

JSON ayrıştırma Portalı aracılığıyla şu anda yapılandırılabilir değildir. [Azure Search'te JSON ayrıştırma hakkında daha fazla bilgi edinin.](https://aka.ms/azsjsonblobindexing)

## <a name="quick-start"></a>Hızlı başlangıç
Azure arama BLOB'larını doğrudan Blob Depolama portal sayfasından eklenebilir.

![](./media/search-blob-storage-integration/blob-blade.png)

Tıklatın **Azure arama Ekle** , var olan bir Azure Search hizmetini seçin veya yeni bir hizmet Oluştur bir akış başlatmak için. Yeni bir hizmet oluşturursanız, depolama hesabınızın portal deneyimi dışında gittiğinizde. Depolama portal sayfasına geri gidin ve yeniden seçin **Azure arama Ekle** varolan hizmeti seçebileceğiniz seçeneği.

### <a name="next-steps"></a>Sonraki Adımlar
Tam Azure arama Blob oluşturucuda hakkında daha fazla bilgi [belgelerine](https://aka.ms/azsblobindexer).
