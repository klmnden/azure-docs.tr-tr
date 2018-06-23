---
title: QnA Maker Dağıtımınız - Microsoft Bilişsel hizmetler için kapasite seçme | Microsoft Docs
titleSuffix: Azure
description: QnA Maker dağıtımınız için kapasite seçmek için bir kılavuz
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: b0219b9f7dbbee52406dab9d808134fa2e2a689d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354064"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Kapasite QnA Maker dağıtımınız için seçme

QnA Maker hizmeti üç Azure kaynaklarına bağımlılık alır:
1.  Uygulama Hizmeti (çalışma zamanı)
2.  (QnAs depolamak için) azure arama
3.  App Insights (chatlogs ve telemetri depolamak için isteğe bağlı)

QnA Maker hizmetinizi oluşturmadan önce hangi katmanları yukarıdaki Hizmetleri sizin için uygun olduğuna karar. 

Genellikle göz önünde bulundurmanız gereken üç parametre vardır:
1. **Verimlilik hizmetinden gereksinim**: uygun seçin [uygulama planı](https://azure.microsoft.com/en-in/pricing/details/app-service/plans/) uygulama hizmetiniz için gereksinimlerinize göre. Yapabilecekleriniz [ölçeği](https://docs.microsoft.com/en-us/azure/app-service/web-sites-scale) veya uygulama aşağı. Bkz. daha ayrıntılı bu Azure arama SKU seçiminiz ayrıca etkilemelidir [burada](https://docs.microsoft.com/en-us/azure/search/search-sku-tier).

2. **Boyutu ve Bilgi Bankası sayısı**: uygun seçin [Azure arama SKU](https://azure.microsoft.com/en-in/pricing/details/search/) senaryonuz için. N katmanda izin verilen en büyük dizinler, belirli bir katmanına N-1 Bilgi Bankası yayımlayabilirsiniz. Ayrıca en büyük boyutuna ve katman izin verilen belge sayısını denetleyin.

3. **Belge kaynakları olarak sayısı**: QnA Maker Yönetim hizmetinin ücretsiz SKU belgeleri portal ve 3 (boyutunun 1 MB her) API'leri aracılığıyla yönetme sayısını sınırlar. Standart SKU yönetebileceğiniz belge sayısı herhangi bir sınırı vardır. Daha fazla ayrıntı görmek [burada](https://aka.ms/qnamaker-pricing).

Aşağıdaki tabloda bazı üst düzey yönergeler verilmektedir.

|                        | QnA Maker Yönetimi | App Service | Azure Search | Sınırlamalar                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Deneme        | Ücretsiz SKU             | Ücretsiz Katmanı   | Ücretsiz Katmanı    | En çok 2 KB, 50 MB boyutu yayımlama  |
| Geliştirme ve Test Ortamı   | Standart SKU         | Paylaşılan      | Temel        | En fazla 4 KB, 2 GB boyutunu yayımlama    |
| Üretim ortamı | Standart SKU         | Temel       | Standart     | En fazla 49 KB, 25 GB boyutunu yayımlama |

QnA Maker yığın yükseltmek için bkz: [QnA Maker hizmetinizi yükseltme](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnA Maker hizmetinizi yükseltme](../How-To/upgrade-qnamaker-service.md)
