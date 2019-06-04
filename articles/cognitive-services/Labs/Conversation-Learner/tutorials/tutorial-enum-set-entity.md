---
title: ENUM varlıkları ve varlık KÜMESİ eylemleri konuşma Öğrenici sahip bir Model - Azure Bilişsel Hizmetler'in kullanıldığı durumlar | Microsoft Docs
titleSuffix: Azure
description: ENUM varlıkları ve varlık KÜMESİ eylemleri konuşma Öğrenici modeliyle kullanmak, uygun olduğunda öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1393d0bd1c31a2c9c24652e260ef7f3182d91367
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476501"
---
# <a name="when-to-use-enum-entities-and-set-entity-actions"></a>ENUM varlıkları ve varlık KÜMESİ Eylemler ne zaman kullanılacağı

ENUM (numaralandırma) varlıkları ve SET_ENTITY eylemlerini kullanmanız gerekir, bu öğreticide açıklanmaktadır.

## <a name="video"></a>Video

[![Varlık öğretici Önizleme ayarlayın](https://aka.ms/cl_Tutorial_v3_SetEntity_Preview)](https://aka.ms/cl_Tutorial_v3_SetEntity)



## <a name="what-is-covered"></a>Kapsanan konular

Bu öğreticide, iki yeni özellik kullanıma sunacak. ENUM (kısaltması numaralandırma) yeni bir varlık türü olarak adlandırılan ve yeni bir eylem türü bu sabit listesi değerlerinin birine başvurabilir ve adından da anlaşılacağı gibi bu değer için varlık kümeleri SET_ENTITY olarak adlandırılır. Aşağıda öğreneceksiniz gibi bu yeni özellikleri birlikte kullanılır ve biz nedir ve bunları aşağıdaki nasıl kullanacağınızı açıklar. Biz ayrıntılarına geçmeden önce bu özellikler çözmenize yardımcı olur. hangi sorunu anlamak önemlidir.

![enum_entity_type.png](../media/tutorial-enum-set-entity/enum_entity_type.png)
![action_set_entity_type.png](../media/tutorial-enum-set-entity/action_set_entity_type.png)

## <a name="problem"></a>Sorun

Burada sözcükler anlamını bağlam üzerinde bağlıdır konuşmalardaki durumlar vardır.  Normalde etiketli anahtar sözcükler öğrenilen ve dil anlama hizmeti kullanarak ayıkladığınız, ancak bu tür durumlarda bu sistemlerin etiketli örneklerini kullanmayı öğrenmek mümkün olmayabilir.

Kişiler arasında bir konuşma bölümü overhear ve yalnızca "Evet" sözcüğü işittiğiniz hayal edin Ne "Yes" kabul bilmez veya sorular önce sorusuna yanıt alamadık beri onaylanıyor. Önce sorulan soru için anlamlı bir yanıt veren, bağlamdır. Benzer şekilde "Yes" bu yana, kullanılamaz öğrenilen örnekleri ile yaptığınız şekilde sağlayarak birçok farklı soruların ortak yanıt [özel eğitilen](04-introduction-to-entities.md) varlıkları çünkü sonra her "Evet"seçeneği, varlık olarak etiketlemek bilgi.

### <a name="example"></a>Örnek

Şimdi aşağıdaki örnek ile daha fazla netleştirin:

Bot: Azure Bilişsel hizmetler beğendiniz mi?
Kullanıcı: Bot Evet: ICE cream beğendiniz mi?
Kullanıcı: Evet

Önceki öğreticilerde, inceledik [özel eğitilen](04-introduction-to-entities.md) varlıkları ve ilk söyleyecekleriniz "likesCogServices" adlı bir varlık oluşturun ve ilk "Evet" olarak bu varlık etiketi olabilir.  Ancak, sistem ayrıca ikinci "Evet" etiket. İkinci "likesIceCream" için "Evet" etiketini düzeltin çalıştı, biz ardından iki aynı girişe bir çakışma anlamı farklı şeyler "Evet" oluşturacak ve takılmış.

Bu sabit listesi varlıkları ve SET_ENTITY eylemlerini kullanmanız gerekir bu tür durumlarda olur.

## <a name="when-to-use-enums-or-setentity-actions"></a>Sabit listeleri kullan ne zaman veya SET_ENTITY eylemleri

ENUM varlıkları ve SET_ENTITY Eylemler ne zaman kullanılacağını öğrenmek için aşağıdaki kurallar kullanın:

- Algılama ayarı varlık bağlama bağlı mı
- Olası değerler sayısı sabit (Evet ve iki değer Hayır olacaktır)

Diğer bir deyişle, bunlar her zaman Evet veya Hayır olarak neden onay sorular gibi herhangi bir kapatma sona erdi istemi için kullanın

> [!NOTE]
> Şu anda enum varlık başına en fazla 5 değeri SORUMLULUĞUN sahibiz. Her değer, geçerli 64 sınır yuvalarda birini kullanır. Bkz: [cl-değerleri-ve-sınırları](../cl-values-and-boundaries.md)

Örnek: Bot: Numaralı Siparişiniz doğru mu?
Kullanıcı: Evet

Olası değerler varlığın açık uçlu ve düzeltilmedi olduğunda, alternatif bir özellik gibi kullanmanız gerekir [beklenen varlık](05-expected-entity.md).

Örnek: Bot: Adınız ne?
Kullanıcı: Matt Bot: En sevdiğiniz renk nedir?
Kullanıcı: Gümüş

Rasgele değerler ile yanıtlanması çünkü bu istemleri açık uçlu olarak kabul edilir.

## <a name="what"></a>Nesne

### <a name="enum-entities"></a>ENUM varlıklar

ENUM varlıklar, yalnızca diğer varlıklar gibi oluşturulur. "Program" varlıklara benzer şekilde, sözcükleri bu varlıklar olarak etiket olamaz. Bunun yerine, kod veya SET_ENTITY eylemleri ayarlanmalıdır.

![enum_entity_creation.png](../media/tutorial-enum-set-entity/enum_entity_creation.png)

### <a name="set-entity-actions"></a>Varlık eylemleri ayarlayın

Yukarıda belirtildiği gibi "Varlık kümesi" eylemleri bir varlık için bir bilinen enum değeri ayarlamanız yeterlidir. Bir API geri çağırma eylemi oluşturarak ve varlık kümesi için bir değer için bellek Yöneticisi'ni kullanarak aynı sonuçları elde. Örneğin `memory.Set(entityName, entityValue)`. Konuşma Öğrenici bu çalışmayı kolaylaştırmak ve bunların ne zaman kullanıldığını bu eylemleri otomatik olarak oluşturmak için özel eylemler vardır. Bu nedenle bu kod yazma ve bu eylemlerin oluşturmak zorunda sıkıcı ve yönetmek - sabit hale gelir. Bunlar bağımsız eylem olarak sahip olmak, bu diğer eylemler veya botunuzun kodda eşleşmiş olmadan oluşturma olanağı korur.

- Eylemler, yalnızca bir enum varlık önce oluşturmanız gerekir enum varlığın değerine başvururken oluşturulabilir varlık kümesi.
- Kümesi varlık ayrıca "olmayan-çıktı görünür olan ve"kullanıcının görebileceği bir wait"eylemi tarafından izlenmesi gereken await" eylemlerdir.
- Varlık eylemleri kümesi oluşturulduktan sonra düzenleyemezsiniz anlamı sabittir.

![action_set_entity_creation.png](../media/tutorial-enum-set-entity/action_set_entity_creation.png)

### <a name="automatic-action-generation"></a>Otomatik eylem oluşturma

Modelinizde bir enum varlık varsa konuşma Öğrenici Eylemler her olası değerler için yer tutucu oluşturun ve eğitim sırasında seçmek kullanılabilir hale. Seçimi eylemi otomatik olarak sizin için oluşturulacak.

Örneğin bir enum varlık değerlerle "Evet" oluşturabilirim ve "Hayır" için:

![enum_entity_order_confirmation.png](../media/tutorial-enum-set-entity/enum_entity_order_confirmation.png)

Hatta açıkça bu yeni sabit listesi için Eylemler oluşturmadan eğitim sırasında kullanılabilir iki yeni eylemler görür:

![action_set_entity_sample.png](../media/tutorial-enum-set-entity/action_set_entity_sample.png)


## <a name="create-a-bot-using-these-new-features"></a>Bu yeni özellikleri kullanarak bir Bot oluşturun

### <a name="requirements"></a>Gereksinimler

Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

Fast food sıralama benzetimini yapmak için bir bot oluşturacağız. Ayrık değerler İçecekler ve (küçük/Orta/büyük) ve boyutları için olması ve onay Evet ile soru / yanıt yok. Bu varlıkların hem de bağlama bağlı yanıtlar ve sabit değerleri, yukarıdaki iki kural karşılar.

### <a name="create-the-model"></a>Modeli oluşturma

1. Web kullanıcı Arabirimi "" Al
2. "Öğreticisi-Enum-kümesi-varlık" adlı öğreticiyi seçin

Bu model yönetimi sayfasına gider.
Model zaten birkaç enum varlık içerdiğine dikkat edin ve varlık eylemleri ayarlayın.

### <a name="create-the-first-dialog"></a>İlk iletişim kutusu oluşturma

1. Sol gezinti bölmesinde, "Train iletişim kutuları"'a tıklayın ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Kullanıcı türü "Merhaba, Lütfen sipariş bir coke ve ve istiyorum".
3. "Puan Eylemler" düğmesine tıklayın

   > Kullanıcı içecek boyutları belirtilen dolmadığından veya ve bu nedenle isteyin seçmeliyiz.

4. Yanıt eylemi seçin: "Hangi boyutu içecek ister misiniz?"
5. Kullanıcı olarak, "large" yazın
6. "Puan Eylemler" düğmesine tıklayın
7. ' % S'eylemini SET_ENTITY - seçin "drinkSize: BÜYÜK"
8. Yanıt eylemi seçin: "Hangi boyut ve istiyor musunuz?"
9. Kullanım türü "Ümmül, bir orta hale.
10. "Puan Eylemler" düğmesine tıklayın
11. ' % S'eylemini SET_ENTITY - seçin "friesSize: ORTA"
12. Yanıt eylemi seçin: "Tüm Çeşniler ister misiniz?"
13. İçinde "Yes" Kullanım türü
14. "Puan Eylemler" düğmesine tıklayın
15. ' % S'eylemini SET_ENTITY - seçin "condimentsConfirmation: EVET "SEÇENEĞİ
16. Yanıt eylemi seçin: "Tamam, büyük bir içecek ve orta ölçekli ve sipariş sahibim. Bu doğru mu?"
17. İçinde "Yes" Kullanım türü
18. "Puan Eylemler" düğmesine tıklayın
19. ' % S'eylemini SET_ENTITY - seçin "orderConfirmation: EVET "SEÇENEĞİ
20. Yanıt eylemi seçin: "Tamam, sipariş numarası 58'dir. Burada üzerinde bekleyin."
21. İletişim kutusunu kapatmak için "Kaydet" tıklayın

Yalnızca ENUM varlıkları ve SET_ENTITY eylemleri kullanarak ilk iletişim oluşturdunuz. Daha fazla çok sayıda kullanıcı kısmi bilgiler belirterek veya diğer Kapat uçlu soruları türleri ile denemeler yapabilirsiniz.

> [!NOTE]
> Eğitim sırasında SET_ENTITY eylemleri için yer tutucular gibi görürsünüz
>
> ![action_set_entity_training.png](../media/tutorial-enum-set-entity/action_set_entity_training.png)
>
> ancak oluşturma oturum açtığınızda iletişim kutuları veya dağıtılan kullanarak botlar kullanıcılar bunları görmez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Diğer girişler](./10-alternative-inputs.md)
