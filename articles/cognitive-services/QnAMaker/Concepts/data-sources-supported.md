---
title: Destekleniyor - veri kaynakları soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu SSS ve ürün kılavuzlarını gibi ortak yarı yapılandırılmış içerik biçimlerden soru-cevap çiftlerini otomatik olarak ayıklayabilir. İçeriği, yapılandırılmış dosyalarından Bilgi Bankası'na da eklenebilir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/24/2018
ms.author: tulasim
ms.openlocfilehash: b9214d44061edad967212a1f904c2cdb6687dba2
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039063"
---
# <a name="data-sources"></a>Veri kaynakları 
Soru-cevap Oluşturucu SSS ve ürün kılavuzlarını gibi ortak yarı yapılandırılmış içerik biçimlerden soru-cevap çiftlerini otomatik olarak ayıklayabilir. İçeriği, yapılandırılmış dosyalarından Bilgi Bankası'na da eklenebilir.

## <a name="plain-faq-pages"></a>Düz SSS sayfaları
Bu SSS sayfası, yanıtları aynı sayfada sorular hemen izleyin, en yaygın türüdür. 

![Düz SSS sayfası](../media/qnamaker-concepts-datasources/plain-faq.png) 

 

## <a name="faq-pages-with-section-links"></a>Bölüm bağlantılarla SSS sayfaları 
Bu tür bir SSS sayfası, sorular araya toplanır ve farklı bölümleri aynı sayfada bulunan yanıtlar bağlanır.

 ![Bölüm bağlantı SSS sayfasını](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


## <a name="faq-pages-with-links-to-different-pages"></a>Farklı sayfalara SSS sayfaları 
Bu tür SSS sayfası, başka bir sayfaya bağlantıları yeniden yönlendirin dışında bir bölüm bağlı SSS sayfasına benzer. Soru-cevap Oluşturucu, karşılık gelen yanıtları ayıklamak için tüm bağlı sayfalarda gezinir.

 ![Ayrıntılı bağlantı SSS sayfası](../media/qnamaker-concepts-datasources/deeplink-faq.png) 


## <a name="product-manuals"></a>Ürün kılavuzlarını

El ile genellikle bir ürünle birlikte verilen yönergeleri malzeme oluşur. Bunu ayarlamak, kullanma, korumanıza ve ürünle ilgili sorunları giderme kullanıcıya yardımcı olur. Soru-cevap Oluşturucu el ile işlediğinde, başlıklar ve başlıklar olarak sorular ve yanıtlar olarak sonraki içeriği ayıklar. Bir örnek görmek [burada](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

> [!NOTE]
> Ayıklama içeriğini ve/veya dizin sayfası ve açık bir yapı hiyerarşik başlıklara sahip bir tablosu kılavuzları en iyi şekilde çalışır.


## <a name="structured-data-format-through-file-upload"></a>Yapılandırılmış veri biçimi aracılığıyla karşıya dosya yükleme

.Tsv, biçimlendirilmiş sütunlar içeren .xlsx gibi yapılandırılmış dosyaları için soru-cevap Oluşturucu Bilgi Bankası oluşturma sırasında da yüklenebilir. Dosyaları da karşıya yükleyebilirsiniz **ayarları** Bilgi Bankası sekmesi

| Soru  | Yanıt  | Meta Veriler                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | ' Key1:Value1|Key2:Value2' |
| Question2 | Answer2 |      `Key:Value`           |
Kaynak dosyadaki ek sütunlar göz ardı edilir.

## <a name="structured-data-format-through-import"></a>İçeri aktarma ile yapılandırılmış veri biçimi
Bilgi Bankası içeri aktarma, var olan bir Bilgi Bankası içeriğini değiştirir. İçeri aktarma, veri kaynağı bilgilerini içeren bir yapılandırılmış .tsv dosyası gerektirir. Bu bilgiler soru-cevap soru-cevap çiftlerini gruplandırabilir ve bunları belirli bir veri kaynağı için öznitelik oluşturucu yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta Veriler                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1|' Key1:Value1|Key2:Value2' |
| Question2 | Answer2 | Düzenleme|    `Key:Value`       |

## <a name="editorial"></a>Düzenleme
Bilgi Bankası doldurmak için önceden var olan içerik yoksa, ayrıca bunları bilgi bankanızı düzenleyerek soru-cevap Oluşturucu bilgi temel ekleyebilirsiniz. Bilgi bankanızı güncelleştirmeyi öğrenebilirsiniz [burada](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kişilik chit sohbet kişilikler yanıtlarla ekleyin](../How-To/chit-chat-knowledge-base.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
