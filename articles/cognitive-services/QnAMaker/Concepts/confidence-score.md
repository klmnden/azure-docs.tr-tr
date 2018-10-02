---
title: Güvenilirlik puanı - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Güvenilirlik puanı açıklayan
services: cognitive-services
author: tulasim88
manager: pchoudh
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 09/27/2018
ms.author: tulasim
ms.openlocfilehash: e878da5f6741b1a4c31874af05b7a37f6dee21df
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586232"
---
# <a name="confidence-score"></a>Güvenilirlik puanı
Kullanıcı sorgusu karşı Bilgi Bankası eşleştiğinde, soru-cevap Oluşturucu bir güven puanı yanı sıra ilgili yanıt verir. Bu puanı güvenle yanıt verilen kullanıcı sorgusu için doğru eşleşme olduğunu gösterir. 

Güvenilirlik puanı, 0 ile 100 arasında bir sayıdır. Bir puan 100 olasılıkla eşleşen hiç yanıt bulunamadı 0 anlamına gelir, bir puan sırasında tam bir eşleşme var. Yüksek puan - yanıtında kendilerinden daha emin. Belirli bir sorgu için birden çok yanıt döndürdü olabilir. Bu durumda, yanıtları güvenilirlik puanı azalan sırayla döndürülür.

Aşağıdaki örnekte, 2 sorularla bir soru-cevap varlık görebilirsiniz. 


![Soru-cevap çifti örneği](../media/qnamaker-concepts-confidencescore/ranker-example-qna.png)

Yukarıdaki örnek - için puanları örnek puanı aralık aşağıdaki-kullanıcı sorgularına farklı türleri için gibi bekleyebilirsiniz:


![Derecelendiricisini uygulama puanı aralığı](../media/qnamaker-concepts-confidencescore/ranker-score-range.png)


Aşağıdaki tabloda tipik güvenilirlik için belirli bir puan ilişkili gösterir.

|Puanı değeri|Puan anlama|Örnek sorgu|
|--|--|--|
|90 - 100|Bir kullanıcı sorgu KB soru ve tam eşleşme yakın|"Bilgi Bankası'ndaki Değişikliklerimi güncelleştirmeyen yayımlama sonrasında"|
|70 >|Yüksek güvenle - tamamen kullanıcının sorgu yanıt veren genellikle iyi bir cevap|"BB'mi yayımladım ancak değil güncelleştirildi"|
|50 - 70|Orta güven - genellikle bir kullanıcı sorgusu ana amacı yanıt oldukça iyi yanıt|"I BB'mi yayımlamadan önce güncelleştirmelerim kaydetmeliyim?"|
|30 - 50|Düşük güven - kullanıcının amacı kısmen yanıtladığı genellikle bir ilgili yanıt|"Kaydet ve eğitme ne yapar?"|
|< 30|Çok düşük güven - genellikle kullanıcının sorgu yanıt vermezse, ancak bazı eşleşen sözcük ve tümcecikleri sahiptir |"Nerede eş anlamlılar için BB'mi ekleyebilirim"|
|0|Yanıt alınmadı için hiç eşleşme.|"Hizmet maliyeti"|

## <a name="choose-a-score-threshold"></a>Bir puan eşiği seçin
Yukarıdaki tabloda, çoğu KB'leri üzerinde beklenen puanları gösterilmektedir. Her KB farklı olduğunu ve farklı türde bir sözcük olduğundan, ancak amaçlar ve hedefler-test etme ve eşiği seçin öneririz. Bu sizin için en uygun. Varsayılan ve çoğu KB'leri için çalışmalıdır önerilen eşiği **50**.

Eşiğine seçerken, doğruluk ve kapsamı arasındaki dengeyi göz önünde bulundurun ve gereksinimlerinize göre eşiğine ince ayar.

- Varsa **doğruluğu** (veya duyarlık) senaryonuz için daha önemlidir ve ardından, eşiğini yükseltin. Bu şekilde bir yanıt döndürür her zaman büyük/küçük harf ve yanıt kullanıcılar aradığınız olması olası çok daha fazlasını bir kişiye olacaktır. Bu durumda, daha fazla yanıtlanmamış soruları bırakarak yukarı bitiş. *Örneğin:* eşiği yaparsanız **70**, "nedir kaydedin ve eğitme?" bazı belirsiz örnekler beğenilerin kaçırabilirsiniz.

- Varsa **kapsamı** (veya geri çağırma) daha önemli olduğu ve yalnızca kısmi bir ilişkisi için kullanıcının soru - olsa bile kadar fazla soruyu mümkün olduğunca ardından alt olarak eşiği yanıt istiyorsanız. Bu gösterir, burada yanıt kullanıcının gerçek sorgu yanıt vermezse, ancak bazı diğer biraz ilgili yanıt verir daha fazla durumda olabilir. *Örneğin:* eşiği yaparsanız **30**gibi yukarıdaki örnek ile yanıtlama çok ilgili yanıt verebilir, ister sorgular için "olduğu düzenleyebilirim BB'mi?"


## <a name="improve-confidence-scores"></a>Güven puanlarını geliştirmeleri
Belirli bir kullanıcı sorgu yanıt güvenilirlik puanı geliştirmek için alternatif bir soru, yanıt olarak Bilgi Bankası'na kullanıcı sorgusu ekleyebilirsiniz.


## <a name="similar-confidence-scores"></a>Benzer güven puanları
Birden çok yanıtı benzer bir güvenilirlik puanı varsa, sorgu çok geneldir ve bu nedenle, birden çok yanıtlarla eşit olasılığını ile eşleşen olasıdır. Soru-cevap her varlık, farklı bir hedefi sahip olacak şekilde, Bankalarıyla daha iyi yapısı deneyin.


## <a name="confidence-score-differences"></a>Güvenilirlik puanı farkları
İçeriği aynı olsa bile bir yanıt güvenilirlik puanı negligibly test ve Bilgi Bankası yayımlanmış sürümü arasında değişebilir. Test ve yayımlanan Bilgi Bankası içeriğini bulunur farklı Azure Search dizinlerini olmasıdır.
Burada gördüğünüz nasıl [yayımlama](../How-To/publish-knowledge-base.md) işlemi çalışır.


## <a name="no-match-found"></a>Eşleşme bulunamadı
Derecelendiricisini tarafından iyi bir eşleşme bulunduğunda 0.0 ya da "None" güven puanı döndürülür ve "iyi eşleşme KB bulunamadı" varsayılan yanıttır. Bu varsayılan yanıt uç noktasını çağırma bot veya uygulama kodunda geçersiz kılabilirsiniz. Alternatif olarak, geçersiz kılma yanıt Azure'da ayarlayabilirsiniz ve bu belirli bir soru-cevap Oluşturucu hizmeti dağıtılan tüm bilgi bankaları için varsayılan değiştirir.

1. Git [Azure portalında](http://portal.azure.com) ve oluşturduğunuz soru-cevap Oluşturucu hizmetini temsil eder kaynak grubuna gidin.

2. Açmak için tıklayın **App Service**.

    ![Uygulama hizmetine erişim](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Tıklayarak **uygulama ayarları** ve düzenleme **DefaultAnswer** istenen varsayılan yanıt alanı. **Kaydet**’e tıklayın.

    ![Varsayılan yanıt değiştirme](../media/qnamaker-concepts-confidencescore/change-response.png)

4. App service'ı yeniden başlatın

    ![Soru-cevap Oluşturucu appservice yeniden başlatma](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Desteklenen veri kaynakları](./data-sources-supported.md)
## <a name="see-also"></a>Ayrıca bkz. 
[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
