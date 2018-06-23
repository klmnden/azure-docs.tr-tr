---
title: Desteklenen - veri kaynakları Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Desteklenen veri kaynakları
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: b888846056fd60f37cdb1da85904fa14ffe79a39
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354101"
---
# <a name="data-sources"></a>Veri kaynakları 
QnA Maker SSS ve ürün kılavuzları gibi ortak yarı yapılandırılmış içerik biçimlerden soru-yanıt çiftleri otomatik olarak ayıklayabilirsiniz. İçerik, yapılandırılmış dosyalarından Bilgi Bankası'na da eklenebilir.

## <a name="plain-faq-pages"></a>Düz SSS sayfaları
Bu SSS sayfası, yanıtları aynı sayfa sorulara hemen izleyin, en yaygın türüdür. 

![Düz SSS sayfası](../media/qnamaker-concepts-datasources/plain-faq.png) 

 

## <a name="faq-pages-with-section-links"></a>SSS bölümüne bağlantılar sayfalarıyla 
Bu tür bir SSS sayfası, sorular birlikte toplanır ve aynı sayfada farklı bölümlerde bulunan yanıtlar bağlantılı.

 ![Bölüm bağlantı SSS sayfası](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


## <a name="faq-pages-with-links-to-different-pages"></a>Farklı sayfalara SSS sayfaları 
Bu tür SSS sayfası, başka bir sayfaya bağlantıları yönlendirme dışında bir bölüm bağlı SSS sayfasına benzerdir. QnA Maker yanıtlarını ayıklamak için bağlantılı tüm sayfaları gezinir.

 ![Ayrıntılı bağlantı SSS sayfası](../media/qnamaker-concepts-datasources/deeplink-faq.png) 


## <a name="product-manuals"></a>Ürün kılavuzları

El ile normal bir ürünle birlikte verilen yönergeleri malzeme olur. Bu kullanıcıya ayarlamak, kullanma, korumanıza ve ürünle ilgili sorunları giderme yardımcı olur. QnA Maker el ile işlediğinde, başlıkları ve başlıklar olarak sorular ve yanıtlar olarak sonraki içeriği ayıklar. Bir örnek görmek [burada](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

> [!NOTE]
> Ayıklama İçindekiler ve/veya dizin sayfası ve açık bir yapı hiyerarşik başlıkları ile birlikte bir tablonuz kılavuzları en iyi şekilde çalışır.


## <a name="structured-data-format-through-file-upload"></a>Karşıya dosya yükleme aracılığıyla yapılandırılmış veri biçimi

Yapılandırılmış dosyalar .tsv, biçimlendirilmiş sütunlarla .xlsx gibi de QnA Maker Bilgi Bankası oluşturma sırasında karşıya yüklenebilir. Dosyaları karşıya yükleyebilir **ayarları** Bilgi Bankası sekmesi

| Soru  | Yanıt  | Meta Veriler                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | `Key1:Value1\|Key2:Value2` |
| Question2 | Answer2 |      `Key:Value`           |
Kaynak dosyasında ek sütunlar göz ardı edilir.

## <a name="structured-data-format-through-import"></a>İçeri aktarma yoluyla yapılandırılmış veri biçimi
Bilgi Bankası içeri aktarma varolan Bilgi Bankası içeriğini değiştirir. İçeri aktarma veri kaynağı bilgilerini içeren bir yapılandırılmış .tsv dosyası gerektirir. Bu bilgiler QnA soru-yanıt çiftleri gruplandırmak ve belirli bir veri kaynağı için öznitelik Maker yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta Veriler                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1|`Key1:Value1\|Key2:Value2` |
| Question2 | Answer2 | Düzeltme sınıfı|    `Key:Value`       |

## <a name="editorial"></a>Düzeltme sınıfı
Bilgi Bankası doldurmak için önceden var olan içerik yoksa, ayrıca bunları düzenleme şeklinde QnA Maker bilgi temel ekleyebilirsiniz. Bilgi Bankası güncelleştirme öğrenin [burada](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnA Maker hizmetini kurma](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Ayrıca bkz. 

[QnA Maker genel bakış](../Overview/overview.md)