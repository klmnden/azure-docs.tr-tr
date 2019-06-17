---
title: Konuşma Öğrenici - Microsoft Bilişsel hizmetler ile öğretmeyi nasıl | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici ile öğretin öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 9e5e6594a74219a22d67af18827bfe2b7cbd0fb8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66385268"
---
# <a name="how-to-teach-with-conversation-learner"></a>Konuşma Öğrenici ile öğretme 

Bu belge, ne konuşma Öğrenici farkındadır ve nasıl öğrenir açıklar sinyalleri açıklar.  

Öğretim iki ayrı adımlar ayrılmış: Varlık ayıklama ve eylem seçimi.

## <a name="entity-extraction"></a>Varlık ayıklama

Konuşma Öğrenici, perde kullanan [LUIS](https://www.luis.ai) varlık ayıklama için.  Konuşma Öğrenici, varlık ayıklama deneyimi LUIS ile biliyorsanız uygular.

Varlık ayıklama modelleri haberdar *içeriği* ve *bağlam* kullanıcı utterance içinde.  Örneğin, "Seattle" sözcüğü bir utterance şehirde olarak gibi etiketli "Seattle hava durumu nedir", varlık ayıklama aynı içeriğin ("Seattle") "Popülasyon Seattle", gibi başka bir utterance şehirde olarak tanıma özellikli bile Konuşma çok farklıdır.  "Francis" varsa yeni daha önce görünmeyen bir adı "Bir toplantı ayarlama ile bir kez deneme" gibi benzer bir bağlamda tanınan sonra buna karşılık, adı "Schedule bir toplantı Francis ile" olarak kabul edilmemiş.  Machine learning çıkarsar olduğunda içeriği, içerik veya her ikisi de katılmak eğitim örnekleri üzerinde temel.

Şu anda, varlık ayıklama yalnızca geçerli utterance içeriğini farkındadır.  (Aşağıda) Eylem Seçimi önceki sistem kapatır, önceki kullanıcı etkinleştirir ya da daha önce tanınan varlıklar gibi iletişim geçmişi farkında değil.  Sonuç olarak, varlık ayıklama davranışını "tüm konuşma arasında paylaşılan".  Örneğin, "Apple istiyorum" kullanıcı utterance "Apple" bir kullanıcı utterance varlık türünde "Meyve" olarak etiketlenmiş varsa, varlık ayıklama modeli bu utterance ("Apple istiyorum") "Apple"Meyve"etiketlenmiş" her zaman olmalıdır bekler.

Varlık ayıklama beklendiği gibi davranmıyorsa, olası çözümler aşağıda verilmiştir:

- Denemeniz gereken ilk şey daha fazla eğitim örnek--özellikle tipik varlık bağlamı (sözcük çevreleyen) veya özel durumları Göster örnekler eklemektir.
- Uygun durumlarda bir eylem için bir "Beklenen varlık" özelliği eklemeyi göz önünde bulundurun.  Öğretici beklenen varlıklarda daha fazla ayrıntı için bkz.
- El ile işleme eklemek mümkünken `EntityExtractionCallback` Bu, sistem geliştikçe machine learning'e yönelik geliştirmeler faydalanacağı değil çünkü kod kullanarak varlıkları ayıklamak için bu en az önerilen yaklaşımdır.

## <a name="action-selection"></a>Eylem Seçimi

Eylem Seçimi giriş olarak konuşma geçmişinizi tüm alan yinelenen bir sinir ağı kullanır.  Bu nedenle, eylem seçimi, önceki kullanıcı konuşma, varlık değerlerini ve sistem konuşma kullanan bir durum bilgisi olan bir işlemdir.  

Bazı sinyaller doğal olarak learning işlem tarafından tercih edilir.  Diğer bir deyişle, konuşma Öğrenici daha "tercih edilen" sinyalleri kullanarak bir eylem seçimi kararı açıklayabilir ise çalışır; erişilemiyorsa, daha az "tercih edilen" sinyaller kullanır.

Konuşma Öğrenici ve hangilerinin eylem seçimi tarafından kullanılan tüm sinyaller gösteren bir tablo aşağıda verilmiştir.  Sipariş içinde kullanıcı konuşma word Not göz ardı edilir.

Sinyal | Tercih (1 en çok tercih edilen =) | Notlar
--- | --- | --- 
Önceki sırayla sistem eylemi | 1 | 
Geçerli sırayla varlıklar sunar | 1 | 
Bu ilk Aç olup olmadığını | 1 |
Geçerli kullanıcı utterance sözcükleri tam eşleşme | 2 | 
Geçerli kullanıcı utterance benzer anlamı sözcükleri | 3 | 
Sistem işlemleri önce önceki Aç | 4 |
Geçerli bırakma önce kapatır varlıkları sunmak | 4 | 
Geçerli bırakma önce kullanıcı konuşma | 5 | 

> [!NOTE]
> Eylem Seçimi, sistem işlemleri--metin, kart içeriği veya API adı veya davranış--kimlik sistemi eylemin yalnızca içerik almaz.  Sonuç olarak, bir eylem içeriğini değiştirme eylemi seçim modeli davranışını değiştirmez.
>
> Ayrıca, varlık içeriği/değerleri olmadığına kullanılır--yalnızca varlık/roller.

Eylem Seçimi beklendiği gibi davranmıyorsa, olası çözümler aşağıda verilmiştir:

- Daha fazla train iletişim kutuları, özellikle hangi sinyalleri eylem seçimi için dikkat ödeme gösteren iletişim kutularını ekleyin.  Eylem Seçimi bir sinyal diğerine tercih, örneğin, aynı durumda olmasından tercih edilen sinyal ve değişen diğer sinyaller gösteren örnekler verin.  Bazı dizileri öğrenmek için eğitim iletişim kutuları birkaç alabilir.
- "Required" ve "eylem tanımları varlıklara eleyerek" ekleyin.  Bu sınırları eylemleri mevcuttur ve hızlı iş kuralları ve bazı sağduyunuzu desenleri için yararlı olabilir. 

## <a name="updates-to-models"></a>Modelleri güncelleştirmeleri

Ekleme veya bir varlık, eylem veya train iletişim kullanıcı arabiriminde, düzenlemek istediğiniz zaman bu hem varlık ayıklama modeli ve eylem seçimi modeli yeniden eğitme isteği oluşturur.  Bu isteği bir kuyruğa yerleştirilir ve yeniden eğitim zaman uyumsuz olarak gerçekleştirilir.  Yeni bir modeli kullanılabilir olduğunda, o noktadan itibaren başlayarak varlık ayıklama ve eylem seçimi için kullanılır.  Bu yeniden eğitim işlemi genellikle yaklaşık 5 saniye sürer, ancak modelin karmaşık olup olmadığını veya eğitim hizmet üzerindeki yükü yüksekse uzun olabilir.

Eğitim zaman uyumsuz olarak yapıldığı için yaptığınız düzenlemeleri hemen yansıtılmıyor mümkündür.  Varlık ayıklama ya da eylem seçimi son 5-10 saniye içinde yaptığınız değişikliklere göre beklendiği gibi davranmıyorsa, bu neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varsayılan değerleri ve sınırlar](./cl-values-and-boundaries.md)
