---
title: En iyi uygulamalar - soru-cevap Oluşturucu
titlesuffix: Azure Cognitive Services
description: Bilgi bankanızı artırmak ve uygulama/sohbet Robotu ait son kullanıcılara daha iyi sonuçlar sağlamak için bu en iyi uygulamaları kullanın.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: dd4f85822a5e6615e7ea6e31b4231c04c9d4e88c
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542847"
---
# <a name="best-practices-of-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası en iyi yöntemleri

[Bilgi Bankası geliştirme yaşam döngüsü](../Concepts/development-lifecycle-knowledge-base.md) başlangıçtan bitişe kadar KB yönetme konusunda size yol gösterir. Bilgi bankanızı artırmak ve istemci uygulamanız için daha iyi sonuçlar sağlamak veya işlemleri sohbet Robotu'nın son kullanıcılar için bu en iyi uygulamaları kullanın.

## <a name="extraction"></a>Ayıklama

Soru-cevap Oluşturucu hizmetini sürekli olarak içerik ve desteklenen dosya ve HTML biçimleri listesini genişleterek Bankalarıyla ayıklamak algoritmaları gelişiyor. İzleyin [yönergeleri](../Concepts/data-sources-supported.md) için belge türüne göre veri ayıklama. 

Genel olarak, SSS sayfaları, tek başına ve diğer bilgilerle birleşik olması gerekir. Ürün kılavuzlarını, NET başlıklar ve tercihen dizin sayfası olması gerekir. 

### <a name="configuring-multi-turn"></a>Yapılandırma çok Aç

Etkin çok Aç ayıklama bilgi bankanızı oluşturun. Bilgi bankanızı vermez veya soru hiyerarşisi desteklemelidir, bu hiyerarşi belgeden ayıklanan veya belge ayıklandıktan sonra oluşturulan. 

<!--is this a global setting that can only be configured at kb creation time? -->

## <a name="creating-good-questions-and-answers"></a>İyi sorularını ve yanıtlarını oluşturma

### <a name="good-questions"></a>İyi sorular

En iyi soruları basittir. Daha sonra bu anahtar sözcük veya tümceciği için basit bir soru oluşturmasını anahtar sözcük veya tümceciği her soru için göz önünde bulundurun. 

Gerekiyor ancak değişiklikleri basit tutmak çok diğer sorular ekleyin. Daha fazla sözcük veya asıl amacı, sorunun bir parçası olmayan sahibiyseniz ekleyerek soru-cevap Oluşturucu bir eşleştirme bulmak yardımcı olmaz. 


### <a name="add-relevant-alternative-questions"></a>İlgili diğer sorular ekleyin

Kullanıcı, metnin bir konuşma stiliyle sorular girebilirsiniz `How do I add a toner cartridge to my printer?` veya gibi arama anahtar sözcüğü `toner cartridge`. Bilgi Bankası doğru şekilde en iyi cevabı döndürmek için her iki stil soruları olması gerekir. Bir müşteri giriyor hangi anahtar sözcükleri emin değilseniz, sorguları analiz etmek için Application Insights verilerini kullanın.

### <a name="good-answers"></a>İyi yanıtları

En iyi cevapları basit yanıtlar olan ancak çok basit. Yanıtları gibi kullanmayın `yes` ve `no`. Yanıtınız bağlamak için diğer kaynakları veya bağlantıları ve ortam ile zengin bir deneyim sağlamak, kullanın [meta verileri etiketleme](./knowledge-base.md#key-knowledge-base-concepts) yanıtları arasında ardından ayırt etmek için [sorgula](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) metaetiketlerle`strictFilters` doğru yanıtı sürümü almak için özellik.

## <a name="chit-chat"></a>Chit sohbet
Botunuzun daha damıtarak konuşma bağlamında kullanılabilen ve ilgi çekici, botunuzun yapmak için chit sohbet ekleme az çaba. Kolayca, KB oluştururken önceden tanımlanmış inancı chit sohbet veri kümeleri ekleme ve bunları dilediğiniz zaman değiştirin. Bilgi edinmek için nasıl [chit sohbet eklemek için KB](../How-To/chit-chat-knowledge-base.md). 

### <a name="choosing-a-personality"></a>Bir kişilik seçme
Chit sohbet için önceden tanımlanmış birkaç inancı desteklenir: 

|Kişilik |Soru-cevap Oluşturucu veri kümesi dosyası |
|---------|-----|
|Profesyonel |[qna_chitchat_professional.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_professional.tsv) |
|Kolay |[qna_chitchat_friendly.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_friendly.tsv) |
|Şakacısın |[qna_chitchat_witty.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_witty.tsv) |
|Caring |[qna_chitchat_caring.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_caring.tsv) |
|Hevesli |[qna_chitchat_enthusiastic.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_enthusiastic.tsv) |

Yanıtları aralıktan biçimsel resmi olmayan ve saygısız. En yakın botunuz için istediğiniz sesi birlikte hizalanır kişilik seçmeniz gerekir. Görüntüleyebileceğiniz [veri kümeleri](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets), botunuza ilişkin temel olarak hizmet veren birini seçin ve sonra yanıtları özelleştirin. 

### <a name="edit-bot-specific-questions"></a>Bot özgü soruları Düzenle
Sohbet chit veri kümesinin parçası olan ve genel yanıtlar oturum doldurulmuş bazı bot özgü soruları vardır. Bu yanıtlar, en iyi bot ayrıntılarınızı yansıtacak şekilde değiştirin. 

Aşağıdaki sohbet chit Bankalarıyla daha belirgin hale öneririz:

* Kimsiniz?
* Ne yapabilirsiniz?
* Kaç yaşındasınız?
* Kim, oluşturduğunuz?
* Merhaba
   
### <a name="adding-custom-chit-chat-with-a-metadata-tag"></a>Özel chit sohbet bir meta veri etiketine sahip ekleme

Kendi chit sohbet soru-cevap çiftlerini eklerseniz, bu nedenle bu yanıtlar döndürülür meta verileri eklemek emin olun. Meta veri adı/değer çiftidir `editorial:chitchat`.

## <a name="searching-for-answers"></a>Yanıtlar aranıyor

Bir kullanıcının sorgu için en iyi cevapları aramak için hem soru ve yanıt GenerateAnswer API kullanır.

### <a name="searching-questions-only-when-answer-is-not-relevant"></a>Yalnızca yanıt ilgili olmadığında arama sorular

Kullanım [ `RankerType=QuestionOnly` ](#choosing-ranker-type) yanıtlar aramak istemiyorsanız. 

Bilgi Bankası tam formlarına yanıt olarak sorular olarak kısaltmalardan kataloğunu olduğunda bu örneğidir. Yanıt değeri için uygun yanıt aramak için yardımcı olmaz.

## <a name="rankingscoring"></a>Derecelendirme ve puanlı
Soru-cevap Oluşturucu desteklediği derecelendirme özellikleri en iyi kullanımı kuran emin olun. Bunun yapılması olasılığına artıracak belirli kullanıcı sorgusu ile uygun bir yanıt yanıtlandığı.

### <a name="choosing-a-threshold"></a>Bir eşiği seçme

Varsayılan [güvenilirlik puanı](confidence-score.md) kullanılan bir eşik değeri 50 olarak ancak [eşiğini değiştirmek](confidence-score.md#set-threshold) gereksinimlerinize göre KB için. Her KB farklı olduğundan, test edin ve en iyi eşiği için KB uygun seçin gerekir. 

### <a name="choosing-ranker-type"></a>Derecelendiricisini uygulama türünü seçme
Varsayılan olarak, soru-cevap Oluşturucu, sorular ve yanıtlar arar. Yalnızca ilgili sorularınızı arama yapmak istiyorsanız, bir yanıt oluşturmak için kullanmak `RankerType=QuestionOnly` GenerateAnswer istek POST gövdesinde.

### <a name="add-alternate-questions"></a>Diğer sorular ekleyin
[Diğer sorular](../How-To/edit-knowledge-base.md) bir kullanıcı sorgu ile bir eşleşme olasılığını artırın. Diğer sorular, aynı soruyu istenebilir birden çok yol olduğunda yararlıdır. Word-style ve cümle yapısı içinde bu değişiklikleri içerebilir.

|Özgün sorgu|Diğer sorgular|Değiştir| 
|--|--|--|
|Kullanılabilir park olan?|Araba park var mı?|cümle yapısı|
 |Merhaba|Yo<br>Hey var!|Word stil veya argo kullanımlar|

<a name="use-metadata-filters"></a>

### <a name="use-metadata-tags-to-filter-questions-and-answers"></a>Filtre sorular ve yanıtlar için meta veri etiketleri kullanma

[Meta veri](../How-To/edit-knowledge-base.md) özelliği bunu bilmek bir istemci uygulaması için görüntüsünü tüm yanıtları ancak bunun yerine tabanlı meta veri etiketleri kullanıcı sorgunun sonuçlarını daraltmak için ekler. Sorgu aynı olsa bile, Bilgi Bankası yanıt meta veri etiketine göre değişebilir. Örneğin, *"bulunan park olduğu"* Restoran dalın konumu farklıdır - diğer bir deyişle, meta veriler, farklı bir yanıt olabilir *konumu: Seattle* karşı *konumu: Redmond*.

### <a name="use-synonyms"></a>Eş anlamlıları kullanma
Eş Anlamlılar İngilizce dilinde bazı desteği olsa da, büyük küçük harf duyarsız word değişiklikleri aracılığıyla kullanmak [değişiklikleri API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) farklı biçiminde anahtar sözcükleri eş anlamlılar eklemek için. Eş Anlamlılar soru-cevap Oluşturucu hizmet düzeyinde eklenir ve tüm bilgi bankalarından tarafından paylaşılan hizmeti.

|Özgün word|Eş anlamlılar|
|--|--|
|Satın alma|satın al<br>NET bankacılık<br>NET bankacılık|

### <a name="use-distinct-words-to-differentiate-questions"></a>Sorular ayırt etmek için farklı sözcükler kullanın
Farklı bir gereksinim her soruyu ele Bilgi Bankası'nda soru kullanıcı sorguyla eşleşen, soru-cevap Oluşturucu'nın Sıralama algoritması en iyi şekilde çalışır. Yineleme sorular arasında ayarlanmış aynı kelimenin doğru yanıt sözcükleri ile verilen kullanıcı sorgusu için seçilen olasılığını azaltır. 

Örneğin, aşağıdaki soruları ile iki ayrı Bankalarıyla olabilir:

|Bankalarıyla|
|--|
|Park olduğu *konumu*|
|ATM olduğu *konumu*|

Bu iki Bankalarıyla çok benzer kelimelerinizle tümcecik oluşturulmuş olduğundan, bu gibi tümcecik oluşturulmuş çok sayıda kullanıcı sorgu çok benzer puanları neden olabilecek *"nerede olduğunu `<x>` konumu"* . Sorgularla gibi açıkça ayırt etmek bunun yerine, deneyin *"park olduğu"* ve *"ATM olduğu"* , "Konum" gibi sözcükleri önleme tarafından KB kadar fazla soruyu içinde olabilir. 

## <a name="collaborate"></a>İşbirliği yapın
Soru-cevap Oluşturucu, kullanıcılara sağlayan [işbirliği](../How-to/collaborate-knowledge-base.md) Bilgi Bankası üzerinde. Kullanıcıların bilgi bankalarından erişmek için soru-cevap Oluşturucu Azure kaynak grubuna erişim gerekir. Bazı kuruluşlar Bilgi Bankası düzenleme ve Bakım dış ve Azure kaynaklarına erişim hala koruyabilmesini isteyebilirsiniz. Bu düzenleyici onaylayan modeli iki özdeş ayarlayarak yapılır [soru-cevap Oluşturucu Hizmetleri](../How-to/set-up-qnamaker-service-azure.md) farklı Aboneliklerde ve bir düzenleme test döngüsü için seçme. Test tamamlandıktan sonra Bilgi Bankası içerikleri ile aktarılır bir [içeri / dışarı aktarma](../Tutorials/migrate-knowledge-base.md) son Bilgi Bankası yayımlama ve uç noktayı güncelleştirmek onaylayanın soru-cevap Oluşturucu hizmeti için işlem.



## <a name="active-learning"></a>Etkin öğrenme

[Etkin öğrenme](../How-to/improve-knowledge-base.md) çok çeşitli kalite ve kullanıcı tabanlı sorgular miktarını sahip olduğunda, diğer sorular önerme en iyi bir iş yapar. Etkin durumda katılmak istemci uygulamalar kullanıcı sorgularına izin ver önemlidir Gerçekliğinde olmadan geri bildirim döngüsü öğrenme. Sorular soru-cevap Oluşturucu Portalı'nda önerilen sonra **[önerileri filtreyle](../How-To/improve-knowledge-base.md#accept-an-active-learning-suggestion-in-the-knowledge-base)** sonra gözden geçirin ve kabul edin veya bu önerilere reddet. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası Düzenle](../How-to/edit-knowledge-base.md)
