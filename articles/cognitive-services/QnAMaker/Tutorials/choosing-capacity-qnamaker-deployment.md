---
title: Dağıtım - soru-cevap Oluşturucu için kaynak kapasitesi
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu hizmetinizi oluşturmadan önce yukarıdaki hizmetler hangi katmanı size uygun olduğuna karar vermelisiniz.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: a332d263526bb6507e7394c205caa1c4d1f9e3e6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60613268"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Soru-cevap Oluşturucu dağıtımınız için kapasite seçme

Soru-cevap Oluşturucu hizmetini üç Azure kaynaklarında bir bağımlılık alır:
1.  App Service (için çalışma zamanı)
2.  Azure arama (için Bankalarıyla depolama)
3.  App Insights (Sohbet günlükleri ve telemetri depolamak için isteğe bağlı)

Soru-cevap Oluşturucu hizmetinizi oluşturmadan önce yukarıdaki hizmetler hangi katmanı size uygun olduğuna karar vermelisiniz. 

Genellikle dikkate almanız gereken üç parametresi vardır:

1. **Aktarım hızı ihtiyacınız hizmetten**: Uygun seçin [uygulama planı](https://azure.microsoft.com/pricing/details/app-service/plans/) uygulama hizmetiniz için gereksinimlerinize göre. Yapabilecekleriniz [ölçeği](https://docs.microsoft.com/azure/app-service/web-sites-scale) veya uygulama azaltın. Bkz: daha fazla ayrıntı bu Azure arama SKU seçiminiz ayrıca etkilemelidir [burada](https://docs.microsoft.com/azure/search/search-sku-tier).

1. **Boyutuna ve sayısına bilgi bankalarından**: Uygun seçin [Azure arama SKU](https://azure.microsoft.com/pricing/details/search/) senaryonuz için. N katmanında izin verilen en fazla dizin, belirli bir katman içinde N-1 bilgi bankalarından yayımlayabilirsiniz. Ayrıca, en büyük boyutu ve katman izin verilen belge sayısını denetleyin.

    Örneğin, 15 izin verilen dizinler katmanınızı varsa 14 bilgi bankalarından (yayımlanan Bilgi Bankası başına 1 dizini) yayımlayabilirsiniz. On beşinci dizini, geliştirme ve test için tüm bilgi bankaları için kullanılır. 

1. **Kaynağı olarak belge sayısı**: Soru-cevap Oluşturucu management hizmetinin ücretsiz SKU, portalı ve API'leri (1 MB boyut her), 3 aracılığıyla yönetebileceğiniz belge sayısını sınırlar. Standart SKU yönetebileceğiniz belge sayısı için hiçbir sınır vardır. Daha fazla ayrıntı görmek [burada](https://aka.ms/qnamaker-pricing).

Aşağıdaki tabloda bazı üst düzey yönergeler verilmektedir.

|                        | Soru-cevap Oluşturucu Yönetimi | App Service | Azure Search | Sınırlamalar                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Deneme        | Ücretsiz SKU             | Ücretsiz Katmanı   | Ücretsiz Katmanı    | En fazla 2 KB'leri, 50 MB boyutunda yayımlama  |
| Geliştirme/Test Ortamı   | Standart SKU         | Paylaşılan      | Temel        | 14 adede kadar KB'leri, 2 GB boyutunu yayımlama    |
| Üretim ortamı | Standart SKU         | Temel       | Standart     | En fazla 49 KB'leri, 25 GB boyutunu yayımlama |

Soru-cevap Oluşturucu yığın yükseltmek için bkz: [soru-cevap Oluşturucu hizmetini](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu hizmetini](../How-To/upgrade-qnamaker-service.md)
