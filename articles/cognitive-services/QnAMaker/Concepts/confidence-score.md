---
title: Güvenilirlik puanı - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Güvenilirlik puanı güvenle yanıt verilen kullanıcı sorgusu için doğru eşleşme olduğunu gösterir.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/05/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 4fb5d1e20c4c857dedcec2dc4695f82fccd9269d
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792741"
---
# <a name="confidence-score-of-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası güvenilirlik puanı
Kullanıcı sorgusu karşı Bilgi Bankası eşleştiğinde, soru-cevap Oluşturucu bir güven puanı yanı sıra ilgili yanıt verir. Bu puanı güvenle yanıt verilen kullanıcı sorgusu için doğru eşleşme olduğunu gösterir. 

Güvenilirlik puanı, 0 ile 100 arasında bir sayıdır. Bir puan 100 olasılıkla eşleşen hiç yanıt bulunamadı 0 anlamına gelir, bir puan sırasında tam bir eşleşme var. Yüksek puan - yanıtında kendilerinden daha emin. Belirli bir sorgu için birden çok yanıt döndürdü olabilir. Bu durumda, yanıtları güvenilirlik puanı azalan sırayla döndürülür.

Aşağıdaki örnekte, 2 sorularla bir soru-cevap varlık görebilirsiniz. 


![Soru-cevap çifti örneği](../media/qnamaker-concepts-confidencescore/ranker-example-qna.png)

Yukarıdaki örnek - için puanları örnek puanı aralık aşağıdaki-kullanıcı sorgularına farklı türleri için gibi bekleyebilirsiniz:


![Derecelendiricisini uygulama puanı aralığı](../media/qnamaker-concepts-confidencescore/ranker-score-range.png)


Aşağıdaki tabloda tipik güvenilirlik için belirli bir puan ilişkili gösterir.

|Puanı değeri|Puan anlama|Örnek sorgu|
|--|--|--|
|90 - 100|Bir kullanıcı sorgu KB soru ve tam eşleşme yakın|"Değişikliklerimi KB güncelleştirmeyen yayımlama sonrasında"|
|70 >|Yüksek güvenle - tamamen kullanıcının sorgu yanıt veren genellikle iyi bir cevap|"BB'mi yayımladım ancak değil güncelleştirildi"|
|50 - 70|Orta güven - genellikle bir kullanıcı sorgusu ana amacı yanıt oldukça iyi yanıt|"I BB'mi yayımlamadan önce güncelleştirmelerim kaydetmeliyim?"|
|30 - 50|Düşük güven - kullanıcının amacı kısmen yanıtladığı genellikle bir ilgili yanıt|"Kaydet ve eğitme ne yapar?"|
|< 30|Çok düşük güven - genellikle kullanıcının sorgu yanıt vermezse, ancak bazı eşleşen sözcük ve tümcecikleri sahiptir |"Nerede eş anlamlılar için BB'mi ekleyebilirim"|
|0|Yanıt alınmadı için hiç eşleşme.|"Hizmet maliyeti"|

## <a name="choose-a-score-threshold"></a>Bir puan eşiği seçin
Yukarıdaki tabloda, çoğu KB'leri üzerinde beklenen puanları gösterilmektedir. Her KB farklı olduğunu ve farklı türde bir sözcük olduğundan, ancak amaçlar ve hedefler-test etme ve eşiği seçin öneririz. Bu sizin için en uygun. Böylece tüm olası yanıt döndürülür varsayılan Eşik 0 olarak ayarlanır. Çoğu KB'leri için çalışmalıdır önerilen eşiğin **50**.

Eşiğine seçerken, doğruluk ve kapsamı arasındaki dengeyi göz önünde bulundurun ve gereksinimlerinize göre eşiğine ince ayar.

- Varsa **doğruluğu** (veya duyarlık) senaryonuz için daha önemlidir ve ardından, eşiğini yükseltin. Bu şekilde bir yanıt döndürür her zaman büyük/küçük harf ve yanıt kullanıcılar aradığınız olması olası çok daha fazlasını bir kişiye olacaktır. Bu durumda, daha fazla yanıtlanmamış soruları bırakarak yukarı bitiş. *Örneğin:* eşiği yaparsanız **70**, "nedir kaydedin ve eğitme?" bazı belirsiz örnekler beğenilerin kaçırabilirsiniz.

- Varsa **kapsamı** (veya geri çağırma) daha önemli olduğu ve yalnızca kısmi bir ilişkisi için kullanıcının soru - olsa bile kadar fazla soruyu mümkün olduğunca ardından alt olarak eşiği yanıt istiyorsanız. Bu gösterir, burada yanıt kullanıcının gerçek sorgu yanıt vermezse, ancak bazı diğer biraz ilgili yanıt verir daha fazla durumda olabilir. *Örneğin:* eşiği yaparsanız **30**, ister sorgular için yanıt verebilir "Nerede miyim düzenleyebilir BB'mi?"

> [!NOTE]
> Soru-cevap Oluşturucu daha yeni sürümlerini Puanlama mantığı için geliştirmeler içerir ve eşiğine etkileyebilir. İstediğiniz zaman hizmet güncelleştirmesi, test edin ve gerekiyorsa eşik ince emin olun. Soru-cevap hizmet sürümü denetleyebilirsiniz [burada](https://www.qnamaker.ai/UserSettings)ve son gelişmeleri öğrenin [burada](../How-To/troubleshooting-runtime.md).

## <a name="improve-confidence-scores"></a>Güven puanlarını geliştirmeleri
Belirli bir kullanıcı sorgu yanıt güvenilirlik puanı geliştirmek için alternatif bir soru, yanıt olarak Bilgi Bankası'na kullanıcı sorgusu ekleyebilirsiniz. Ayrıca büyük/küçük harfe kullanabilirsiniz [word değişiklikleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) , KB anahtar sözcükleri eş anlamlılar eklemek için.


## <a name="similar-confidence-scores"></a>Benzer güven puanları
Birden çok yanıtı benzer bir güvenilirlik puanı varsa, sorgu çok geneldir ve bu nedenle, birden çok yanıtlarla eşit olasılığını ile eşleşen olasıdır. Soru-cevap her varlık, farklı bir hedefi sahip olacak şekilde, Bankalarıyla daha iyi yapısı deneyin.


## <a name="confidence-score-differences"></a>Güvenilirlik puanı farkları
İçeriği aynı olsa bile bir yanıt güvenilirlik puanı negligibly test ve Bilgi Bankası yayımlanmış sürümü arasında değişebilir. Test ve yayımlanan Bilgi Bankası içeriğini bulunur farklı Azure Search dizinlerini olmasıdır. Bilgi Bankası yayımladığınızda, bilgi bankanızı soru ve yanıt içeriğini taşır test dizini Azure Search'te bir üretim dizini. Bkz. nasıl [yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) işlemi çalışır.

Bilgi Bankası farklı bölgelerde varsa, her bölgede Azure Search dizinini kullanır. Farklı bir dizin kullanıldığından, puanları tam olarak aynı olmayacaktır. 


## <a name="no-match-found"></a>Eşleşme bulunamadı
Derecelendiricisini tarafından iyi bir eşleşme bulunduğunda 0.0 ya da "None" güven puanı döndürülür ve "iyi eşleşme KB bulunamadı" varsayılan yanıttır. Bu geçersiz kılma [varsayılan yanıt](#change-default-answer) bot veya uygulamanın koddaki uç noktasını çağırma. Alternatif olarak, geçersiz kılma yanıt Azure'da ayarlayabilirsiniz ve bu belirli bir soru-cevap Oluşturucu hizmeti dağıtılan tüm bilgi bankaları için varsayılan değiştirir.

## <a name="change-default-answer"></a>Varsayılan yanıt değiştirme

1. Git [Azure portalında](https://portal.azure.com) ve oluşturduğunuz soru-cevap Oluşturucu hizmetini temsil eder kaynak grubuna gidin.

2. Açmak için tıklayın **App Service**.

    ![Azure portalında App service için soru-cevap Oluşturucu erişim](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Tıklayarak **uygulama ayarları** ve düzenleme **DefaultAnswer** istenen varsayılan yanıt alanı. **Kaydet**’e tıklayın.

    ![Uygulama ayarlarını seçin ve ardından DefaultAnswer için soru-cevap Oluşturucu düzenleyin](../media/qnamaker-concepts-confidencescore/change-response.png)

4. App service'ı yeniden başlatın

    ![Soru-cevap Oluşturucu appservice DefaultAnswer değiştirdikten sonra yeniden başlatın](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Desteklenen veri kaynakları](./data-sources-supported.md)

