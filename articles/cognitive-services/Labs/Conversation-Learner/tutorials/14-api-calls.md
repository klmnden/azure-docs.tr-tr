---
title: API'SİNİN nasıl kullanılacağı çağıran bir konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle API çağrıları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: aba3c2eb925370704ea52364891502a7a09cc9ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60635758"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-model"></a>Konuşma Öğrenici modeli yapılan API çağrılarının ekleme

Bu öğreticide, API çağrıları, bir Modeli'ne Ekle gösterilmektedir. API çağrılarıdır tanımlayın ve yazma Botunuzun, İşlevler ve konuşma Öğrenici çağırabilirsiniz.

## <a name="video"></a>Video

[![API çağrıları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_APICalls_Preview)](https://aka.ms/cl_Tutorial_v3_APICalls)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, "tutorialAPICalls.ts" bot çalışıyor olması gerekir.

    npm run tutorial-api-calls

## <a name="details"></a>Ayrıntılar

- API çağrıları, okuma ve varlıkları işlemek.
- API çağrıları, bellek yöneticisi nesnesine erişebilir.
- API çağrıları, bağımsız değişken alabilir.

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi "İçeri aktarma eğitimler" tıklayın ve "Öğreticisi-14-APICalls" adlı modelini seçin.

### <a name="entities"></a>Varlıklar

Bir varlık olarak adlandırılan modelde tanımladığınız `number`.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>API çağrıları
Bu API çağrısı için kod tanımlanan dosya: `C:\<installedpath>\src\demos\tutorialAPICalls.ts`.

![](../media/tutorial12_apicalls.PNG)

- `RandomGreeting` Geri döndürür içinde tanımlanan rastgele bir karşılama `greeting` dizisi.
- `Multiply` Geri çağırma ile çarpın çağırır ve kullanıcı Arabiriminde işlenebilecek bir sonuç döndüren eylem tarafından geçirilen iki sayı.
    - Bu bellek yöneticisi ilk bağımsız değişken olduğuna dikkat edin. 
    - API geri çağırmaları çoklu giriş, bu durumda sürebilir fark `num1string` ve `num2string`.
- `ClearEntities` Geri çağırma, böylece kullanıcı başka bir sayı girebilirsiniz sayı varlık temizler. 
    - API çağrıları varlıkları nasıl işleyebileceğiniz gösterilmektedir.

### <a name="actions"></a>Eylemler
Dört eylem oluşturduk. Üç tanesi "Non-Wait" API eylemlerini, dördüncü kullanıcı ne diğer öğreticileri, gördük için benzer bir soru soran bir "Metin" eylemdir. Her nasıl oluşturulduğuna görmek için aşağıdakileri yapın:
1. Sol panelde, "Eylemler", ardından bir kılavuzda listelenen dört eylem tıklatın.
2. Açılır form üzerindeki her bir alan değerlerine dikkat edin.
3. Bildirim `Refresh` API alanın yanındaki düğmesi.
    - Bot durdurun ve UI sayfasında, ederken tıklatabilirsiniz API'lerine değiştirme olsaydık `Refresh` en son değişiklikleri almak için düğme.

![](../media/tutorial12_actions.PNG)

#### <a name="clearentities-multiply-and-randomgreeting"></a>ClearEntities, çarpma ve RandomGreeting
Bu eylemler üç API türüdür. Bunların hepsi, bazı işleri gerçekleştirin ve büyük olasılıkla kullanıcıya sunulacak bir değer döndürmek için API geri arama işlevlerine bağlıdır.

#### <a name="what-number-do-you-want-to-multiply-by-12"></a>"Hangi numarasını 12 ile çarp istiyorsunuz"
Bu "Metin" eylemdir ve yalnızca kullanıcının soru sorar. Bu eylem gerçekten API geri çağırmaları biri ile etkileşime girmez sırada API geri çağırmaları birini "Çarp" eylemi tarafından kullanılabilecek bir varlığın belleğe geçer bir sayı ile yanıt vermek için kullanıcı ister.


### <a name="train-dialog"></a>Train iletişim

"Eğitim iletişim" atalım.

1. Sol panelde tıklayın `Train Dialogs`, ardından `New Train Dialog` düğmesi.
2. "Hello" yazın.
3. `Score Actions` düğmesine tıklayın.
4. `RandomGreeting` öğesini seçin. 
    - Bu rastgele Karşılama API çağrısı yürütülür.
    - Bu, bir kullanıcıdan yanıt beklemez.
5. `What number to do you want to multiply by 12?` seçeneğini belirleyin
6. Bir sayı, herhangi bir sayı ve yalnızca bir sayı yazın.
    - Numaranızı otomatik olarak etiketlendi bildirimi `number` varlık.
7. `Score Actions` düğmesine tıklayın.
8. Seçin `Multiply` eylem.
    - Çarpma işleminin sonucu 12 dikkat edin.
    - Uyarı, bellek için girdiğiniz değer yine de içerir. `number`
9. Seçin `ClearEntities` eylem.
    - Varlık değeri için bildirim `number` bellekten temizlendi.
10. `Save` düğmesine tıklayın.

Artık API geri aramaları, kaydetme gördünüz kendi ortak desenler ve bağımsız değişkenleri tanımlayın ve değerleri ve bunları varlıklarda ilişkilendirin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kartları bölüm 1](./15-cards.md)
