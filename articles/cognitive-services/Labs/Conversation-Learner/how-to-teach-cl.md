---
title: Konuşma öğrenen - Microsoft Bilişsel hizmetler öğretmeyi nasıl | Microsoft Docs
titleSuffix: Azure
description: Konuşma öğrenen ile öğretmek öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 639fea64fc8eeb2c1f6e6240c4eb26efc68febbd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353969"
---
# <a name="how-to-teach-with-conversation-learner"></a>Konuşma öğrenen ile öğretmeyi nasıl 

Bu belgede ne konuşma öğrenen farkındadır ve nasıl öğrenir açıklar sinyalleri açıklanmaktadır.  

Öğretme iki ayrı adımları ayrılmış: Varlık ayıklama ve eylem seçimi.

## <a name="entity-extraction"></a>Varlık ayıklama

Konuşma öğrenen perde arkasında kullanan [HALUK](https://www.luis.ai) varlık ayıklama için.  Konuşma öğrenen varlık ayıklama deneyimini uygulandığı HALUK, varsa.

Varlık ayıklama modelleri farkında *içerik* ve *bağlamı* kullanıcı utterance içinde.  Örneğin, "Seattle" word bir utterance şehirde olarak gibi etiketli "Seattle hava durumu nedir?", varlık ayıklama içeriği ("Seattle") "Popülasyon, Seattle", gibi başka bir utterance şehirde olarak algılamayı yeteneğine sahip olsa bile utterances çok farklıdır.  "Francis sayılabilir ancak" varsa, yeni bir önceden görünmeyen adı "Kümesiyle bir görüşme deneme" gibi benzer bir bağlamda tanınan sonra buna karşılık, adı "Zamanlaması toplantı olan Francis sayılabilir ancak" olarak kabul edilmemiş.  Machine learning oluşturur içeriği, içerik veya her ikisini de katılmak ne zaman eğitim örnekleri üzerinde temel.

Şu anda, varlık ayıklama yalnızca geçerli utterance içeriğini bilmez.  (Aşağıda) Eylem Seçimi önceki sistem açar, önceki kullanıcı etkinleştirir ya da daha önce tanınan varlıklar gibi iletişim geçmişi farkında değil.  Sonuç olarak, varlık ayıklama davranışını "tüm utterances arasında paylaşılıyor".  Örneğin, "Apple istiyorum" kullanıcı utterance "bir kullanıcı utterance varlık türü"Ulaşılacak durumlardır"olarak etiketlenmiş Apple" varsa, varlık ayıklama modeli ("Apple istiyorum") bu utterance "Apple"ulaşılacak durumlardır"etiketli" her zaman gerekir bekler.

Varlık ayıklama beklendiği gibi çalışmaz, olası çözümler aşağıda verilmiştir:

- Daha fazla eğitim örnekleri--özellikle tipik varlık bağlamı (sözcükler çevreleyen) veya özel durumları ortaya örnekler eklemek için denemek için ilk şey.
- Bir eylem için bir "Beklenen varlık" özelliğini eklemeyi uygunsa düşünün.  Öğretici beklenen varlıkların daha ayrıntılı bilgi için bkz.
- El ile işleme eklemek mümkün olmakla birlikte `EntityExtractionCallback` , sisteminizin geliştikçe makine öğrenme içinde geliştirmeleri faydalanacağı değil çünkü kodu kullanarak varlıkları ayıklamak için bu az önerilen yaklaşımdır.

## <a name="action-selection"></a>Eylem Seçimi

Eylem Seçimi konuşma geçmişi giriş olarak tüm alan yinelenen bir sinir ağı kullanır.  Bu nedenle, eylem seçimi, önceki kullanıcı utterances, varlık değerleri ve sistem utterances farkındadır durum bilgisi olan bir işlemdir.  

Bazı sinyalleri doğal olarak learning işlem tarafından tercih edilen.  Diğer bir deyişle, konuşma öğrenen daha "tercih edilen" sinyalleri kullanarak bir eylem seçimi kararı açıklayabilir, çıkarır; başaramazsa, daha az "tercih edilen" sinyalleri kullanır.

Konuşma öğrenen ve hangilerinin eylem seçimi tarafından kullanılan tüm sinyaller gösteren bir tablo aşağıda verilmiştir.  Kullanıcı utterances sipariş word Not göz ardı edilir.

Sinyal | Tercih (1 en çok tercih edilen =) | Notlar
--- | --- | --- 
Önceki ardından sistem eylemi | 1 | 
Geçerli sırayla varlıklar sunma | 1 | 
Bu ilk Aç olup olmadığını | 1 |
Geçerli kullanıcı utterance sözcüklerin tam eşleşme | 2 | 
Geçerli kullanıcı utterance benzer anlamı sözcükleri | 3 | 
Önceki önce sistem işlemleri Aç | 4 |
Geçerli Aç önce tasarrufludur varlıklar var | 4 | 
Geçerli önce kullanıcı utterances Aç | 5 | 

Eylem Seçimi sistem işlemleri--metin, kart içeriği veya API adı veya davranışı--yalnızca sistem eylemi kimliğini içeriğini öncelikli dikkat edin.  Sonuç olarak, bir eylem içeriğini değiştirme eylemi seçimi modeli davranışını değiştirmez.

Ayrıca, varlık içeriği/değerleri Not kullanılır--yalnızca varlığı/roller.

Eylem Seçimi beklendiği gibi çalışmaz, olası çözümler aşağıda verilmiştir:

- Daha fazla tren iletişim kutuları, özellikle eylem seçimi için dikkat ödeme hangi sinyalleri göstermeye iletişim kutuları ekleyin.  Eylem Seçimi bir sinyal başka bir tercih, örneğin, tercih edilen sinyal aynı durumda olan ve çeşitli diğer sinyaller gösteren örnekleri verin.  Bazı sıraları öğrenmek için eğitim iletişim kutuları sayıda sürebilir.
- "Gerekli" ve "eylemi tanımları varlıklara adayını" ekleyin.  Bu sınırları eylemleri kullanılabilir ve hızlı iş kuralları ve bazı sağduyunuzu desenler için yararlı olabilir. 

## <a name="updates-to-models"></a>Modelleri güncelleştirmeleri

Eklemek veya bir varlık, eylem veya tren iletişim kullanıcı arabiriminde, düzenlemek istediğiniz zaman bu yeniden varlık ayıklama modeli ve eylem seçimi modeli eğitmek için bir istek oluşturur.  Bu istek bir sırasına yerleştirilir ve yeniden eğitim zaman uyumsuz olarak yapılır.  Yeni bir model kullanılabilir olduğunda, bu noktadan başlayarak varlık ayıklama ve eylem seçimi için kullanılır.  Yeniden eğitim bu işlem genellikle yaklaşık 5 saniye sürer ancak model karmaşıksa ya da yük eğitim hizmeti üzerinde yüksek ise daha uzun olabilir.

Eğitim zaman uyumsuz olarak yapıldığından, yaptığınız düzenlemeler hemen yansıtılmıyor mümkündür.  Varlık ayıklama veya eylem seçimi son 5-10 saniye içinde yaptığınız değişikliklere göre beklendiği gibi çalışmaz, bu neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varsayılan değerler ve sınırlar](./cl-values-and-boundaries.md)
