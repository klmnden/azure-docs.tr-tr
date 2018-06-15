---
title: En iyi yöntemler - QnA Maker - Azure Bilişsel hizmetler | Microsoft Docs
description: Bilgi Bankası geliştirmek ve uygulama/sohbet bot's son kullanıcılar için daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 7a85ebbc3892a90e98e73a73425c1f8ec1de0b35
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355925"
---
# <a name="best-practices"></a>En iyi uygulamalar
[Bilgi Bankası geliştirme yaşam döngüsü](../Concepts/development-lifecycle-knowledge-base.md) KB uçtan uca yönetme konusunda size yol gösterir. Bilgi Bankası geliştirmek ve uygulama/sohbet bot's son kullanıcılar için daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.

## <a name="extraction"></a>Ayıklama
QnA Maker sürekli QnAs içeriği Ayıkla algoritmaları artırma ve dosya ve desteklenen HTML sayfası biçimleri listesini genişletme. İzleyin [yönergeleri](../Concepts/data-sources-supported.md) ayıklama belge türüne olduğunuz tabanlı için ayıklama. 

Genel olarak, SSS sayfaları, tek başına ve diğer bilgilerle birleşik olması gerekir. Ürün kılavuzlarının Temizle başlıkları ve tercihen dizin sayfası olması gerekir. 

## <a name="rankingmatching"></a>Derecelendirme ve eşleştirme
QnA Maker desteklediği derecelendirme özellikleri en iyi kullanımı kuran emin olun. Bunun yapılması olasılığını artırmak uygun bir yanıt ile belirtilen kullanıcının sorguda yanıtlanan.

### <a name="add-alternate-questions"></a>Diğer sorular ekleyin
[Diğer sorular](../How-To/edit-knowledge-base.md) bir kullanıcı sorgu ile bir eşleşme olasılığını artırmak. Diğer sorular, aynı soruyu istenebilir birden çok yol olduğunda faydalıdır. Bu değişiklikleri cümle yapısında içerebilir (örneğin, *"park kullanılabilir mi?"* karşı *"araba park var mı?"* ) veya word-style ve argo değişiklikler (örneğin, *"Hi"* karşı *"Yo"*, *"Var. Hey!"* ).

### <a name="use-metadata-filters"></a>Meta veri filtreleri kullanın
[Meta veri](../How-To/edit-knowledge-base.md) filtrelere göre kullanıcı sorgunun sonuçlarını daraltmak için yeteneği ekler. Sorgu aynı olsa bile Bilgi Bankası yanıt meta veri etiketi göre değişebilir. Örneğin, *"bulunan park olduğu"* farklı bir yanıt içerip içeremeyeceğini Restoran Şube konumunu farklı - diğer bir deyişle, meta veriler *konumu: Seattle* karşı *konumu: Redmond*.)

### <a name="use-synonyms"></a>Eş anlamlıları kullanın
Eş anlamlıları İngilizce dilinde bazı desteği olsa da, kullanmak [word değişiklikleri](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) farklı biçiminde anahtar sözcükleri eş anlamlıları eklemek için (örnek: *satın* -> *satın alma*  veya *netbanking* -> *net bankacılık*. Eş anlamlıları QnA Maker hizmet düzeyinde eklenir ve tüm Bilgi Bankası tarafından hizmetinde paylaşılan.

### <a name="use-distinct-words-to-differentiate-questions"></a>Sorular ayırt etmek için farklı sözcükler kullanın
Her soru farklı gereksinimlerine yönelik Bilgi Bankası'nda bir soru kullanıcı sorguyla eşleşen QnA Maker eşleşme derecesi algoritmaları en iyi şekilde çalışır. Yineleme arasında soruları Ayarla aynı word'ün sağ yanıt sözcükleri ile verilen kullanıcı sorgu için seçilen olasılığını azaltır.

## <a name="collaborate"></a>İşbirliği yapın
QnA Maker verir kullanıcılara [işbirliği](../How-to/collaborate-knowledge-base.md) Bilgi Bankası üzerinde. Kullanıcılar, Bilgi Bankası erişmek için Azure QnA Maker kaynak grubuna erişim gerektirir. Bazı kuruluşlar Bilgi Bankası düzenleme ve Bakım dış ve hala Azure kaynaklarına erişimi koruyabilmek için isteyebilirsiniz. Bu düzenleyici onaylayan model iki özdeş ayarlayarak gerçekleştirilebilir [QnA Maker Hizmetleri](../How-to/set-up-qnamaker-service-azure.md) farklı Aboneliklerde ve düzenleme test döngüsü için bir tane belirleme. İle test tamamlandıktan sonra Bilgi Bankası içeriği aktarılabilir bir [içeri dışarı aktarma](../Tutorials/migrate-knowledge-base.md) son olarak Bilgi Bankası yayımlama ve uç noktasını güncelleyin onaylayanın QnA Maker hizmetine işlem.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-to/edit-knowledge-base.md)

