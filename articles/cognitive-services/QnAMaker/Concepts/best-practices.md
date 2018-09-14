---
title: En iyi uygulamalar - soru-cevap Oluşturucu
titlesuffix: Azure Cognitive Services
description: Bilgi bankanızı artırmak ve uygulama/sohbet Robotu ait son kullanıcılara daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: c82c117d149da39fba7b9a243aebb3e127540881
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542942"
---
# <a name="best-practices"></a>En iyi uygulamalar
[Bilgi Bankası geliştirme yaşam döngüsü](../Concepts/development-lifecycle-knowledge-base.md) KB uçtan uca yönetme konusunda size yol gösterir. Bilgi bankanızı artırmak ve uygulama/sohbet Robotu ait son kullanıcılara daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.

## <a name="extraction"></a>Ayıklama
Soru-cevap Oluşturucu sürekli olarak Bankalarıyla içeriği ayıklamak algoritmaları geliştirme ve dosya ve HTML sayfası desteklenen biçimler listesini genişleterek. İzleyin [yönergeleri](../Concepts/data-sources-supported.md) ayıklama, kullandığınız belgenin türüne göre için ayıklama. 

Genel olarak, SSS sayfaları, tek başına ve diğer bilgilerle birleşik olması gerekir. Ürün kılavuzlarını, NET başlıklar ve tercihen dizin sayfası olması gerekir. 

## <a name="rankingmatching"></a>Derecelendirme ve eşleştirme
Soru-cevap Oluşturucu desteklediği derecelendirme özellikleri en iyi kullanımı kuran emin olun. Bunun yapılması olasılığına artıracak belirli kullanıcı sorgusu ile uygun bir yanıt yanıtlandığı.

### <a name="add-alternate-questions"></a>Diğer sorular ekleyin
[Diğer sorular](../How-To/edit-knowledge-base.md) bir kullanıcı sorgu ile bir eşleşme olasılığını artırın. Diğer sorular, aynı soruyu istenebilir birden çok yol olduğunda yararlıdır. Bu değişiklikler cümle yapısında içerebilir (örneğin, *"park kullanılabilir mi?"* karşı *"araba park gerekiyor?"* ) veya word-style ve argo kullanımlar değişiklikler (örneğin, *"Hi"* karşı *"Yo"*, *"Var. Merhaba!"* ).

### <a name="use-metadata-filters"></a>Meta veri filtresini kullanma
[Meta veri](../How-To/edit-knowledge-base.md) filtrelere göre kullanıcı sorgunun sonuçlarını daraltmak için özelliği ekler. Sorgu aynı olsa bile, Bilgi Bankası yanıt meta veri etiketine göre değişebilir. Örneğin, *"bulunan park olduğu"* Restoran dalın konumu farklıdır - diğer bir deyişle, meta veriler, farklı bir yanıt olabilir *konumu: Seattle* karşı *konumu: Redmond*.)

### <a name="use-synonyms"></a>Eş anlamlıları kullanma
Eş Anlamlılar İngilizce dilinde bazı desteği olsa da, kullanın [word değişiklikleri](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) farklı biçiminde anahtar sözcükleri eş anlamlılar eklemek için (örnek: *satın* -> *satın alın*  veya *netbanking* -> *net bankacılık*. Eş Anlamlılar soru-cevap Oluşturucu hizmet düzeyinde eklendi ve hizmette tüm bilgi bankalarından tarafından paylaşılan.

### <a name="use-distinct-words-to-differentiate-questions"></a>Sorular ayırt etmek için farklı sözcükler kullanın
Farklı ihtiyaçları her soruyu ele Bilgi Bankası'nda soru kullanıcı sorguyla eşleşen soru-cevap Oluşturucu eşleşme derecesi algoritmaları en iyi şekilde çalışır. Yineleme sorular arasında ayarlanmış aynı kelimenin doğru yanıt sözcükleri ile verilen kullanıcı sorgusu için seçilen olasılığını azaltır.

## <a name="collaborate"></a>İşbirliği yapın
Soru-cevap Oluşturucu, kullanıcılara sağlayan [işbirliği](../How-to/collaborate-knowledge-base.md) Bilgi Bankası üzerinde. Kullanıcılar, bilgi bankalarından erişmek için soru-cevap Oluşturucu Azure kaynak grubuna erişim gerektirir. Bazı kuruluşlar Bilgi Bankası düzenleme ve Bakım dış ve Azure kaynaklarına erişim hala koruyabilmesini isteyebilirsiniz. Bu düzenleyici onaylayan modeli iki özdeş ayarlayarak gerçekleştirilebilir [soru-cevap Oluşturucu Hizmetleri](../How-to/set-up-qnamaker-service-azure.md) farklı Aboneliklerde ve bir düzenleme test döngüsü için belirleme. İle sınama aşaması tamamlandıktan sonra Bilgi Bankası içerikleri aktarılabilir bir [içeri / dışarı aktarma](../Tutorials/migrate-knowledge-base.md) son Bilgi Bankası yayımlama ve uç noktayı güncelleştirmek onaylayanın soru-cevap Oluşturucu hizmeti için işlem.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-to/edit-knowledge-base.md)

