---
title: Gösteri konuşma öğrenen uygulama, pizza siparişi - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Bir tanıtım konuşma öğrenen uygulama oluşturmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 99cd89c4f4430f2d65ed0963e3092d51a83842d7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354124"
---
# <a name="demo-pizza-order"></a>Gösteri: Pizza sırası
Bu demo bot sıralama pizza gösterilmektedir. Bu işlev ile tek bir pizza sıralama destekliyorsa:

- Kullanıcı utterances içinde pizza toppings tanıma
- pizza toppings stoktaki veya hisse senedi dışında olup olmadığını denetleme ve uygun şekilde yanıt
- önceki bir sipariş pizza toppings anımsama ve yeni bir siparişi ile aynı toppings başlatmak için - sunumu

## <a name="requirements"></a>Gereksinimler
Bu öğretici pizza sipariş bot çalışıyor olması gerekir

    npm run demo-pizza

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi uygulama listesinde TutorialDemo Pizza siparişe tıklayın. 

## <a name="entities"></a>Varlıklar

Üç varlık oluşturduk.

- Toppings: kullanıcı için sorulan toppings toplanacaktır. Stokta olduğunu geçerli toppings içerir. Bir belirtti içinde veya dışında hisse senedi olup olmadığını denetler.
- OutofStock: Bu kullanıcıya geri seçili belirtti stokta olmadığını iletişim kurmak için kullanılır.
- LastToppings: bir siparişin sonra kullanıcıya sıralarına üzerinde toppings listesi sunmak için bu varlık kullanılır.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Eylemler

Bir dizi eylemi istediklerini bunların ne bunlar kadarki eklemiş olduğunuz söyleyen pizza, vb. kullanıcı isteyen dahil olmak üzere oluşturduk.

İki API çağrıları vardır:

- FinalizeOrder: pizza sırası yerleştirmek için
- UseLastToppings: toppings önceki sıra geçirmek için 

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Eğitim iletişim kutuları
Biz eğitim iletişim kutuları sayıda tanımladınız. 

![](../media/tutorial_pizza_dialogs.PNG)

Örnek olarak, bir öğretme oturumu deneyelim.

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
1. 'Bir pizza sipariş' girin.
2. Puan eylemini tıklatın.
3. 'Ne, pizza üzerinde istiyorsunuz?' Select için tıklatın
4. 'Mushrooms ve Peynir' girin.
    - HALUK hem Toppings olarak etiketli dikkat edin. Doğru değildi, vurgulayın, ardından düzeltmek.
    - Varlık yanındaki '+' işaretine toppings kümesine eklenmekte anlamına gelir.
5. Puan Eylemler'i tıklatın.
    - Bildirim mushrooms ve Peynir Toppings için bellekte değildir.
3. 'Üzerinde pizza $Toppings sahip' seçmek için tıklatın
    - Bot için bir sonraki eylem ister şekilde bu bekleme olmayan eylem olduğuna dikkat edin.
6. ', Başka bir şey ister misiniz?' seçin
7. 'Mushrooms kaldırın ve peppers Ekle' girin.
    - Bildirim **mantar** sahip bir '-' yanında işareti de kaldırılacak. Ve peppers için toppings eklemek için ' +'.
2. Puan eylemini tıklatın.
    - Bildirim **peppers** kalın olarak yeni bulunduğu artık. Ve **mushrooms** çaprazlandı.
8. 'Üzerinde pizza $Toppings sahip' seçmek için tıklatın
6. ', Başka bir şey ister misiniz?' seçin
7. 'Bezelye Ekle' girin.
    - Bezelye bir hisse senedi dışında olan bir belirtti örnektir. Bunu hala belirtti olarak etiketli olduğunu unutmayın.
2. Puan eylemini tıklatın.
    - Bezelye OutOfStock gösterilir.
    - Bunun nasıl olduğunu görmek için C: koda açalım\<\installedpath > \src\demos\demoPizzaOrder.ts. Ve EntityDetectionCallback yöntemini not edin. Bu yöntem, stokta olup olmadığını görmek için her belirtti sonra çağrılır. Değilse, toppings kümesinden temizler ve OutOfStock varlığa ekler. İnStock değişkeni stok dışı toppings listesi olan bu yöntem tanımlanır.
6. 'Biz $OutOfStock yok' seçin.
7. ', Başka bir şey ister misiniz?' seçin
8. 'Hayır' girin.
9. Puan eylemini tıklatın.
10. 'FinalizeOrder' API çağrısı seçin. 
    - Bu kod içinde tanımlanan 'FinalizeOrder' işlevi çağırır. Bu toppings temizler ve 'siparişinizi yolda olduğunu' döndürür. 
2. 'Başka bir sipariş' girin. Yeni bir sipariş başlıyor.
9. Puan eylemini tıklatın.
    - Not peynir ve peppers toppings son sipariş olarak bellekte markalarıdır.
1. '$LastToppings ister misiniz' seçin.
2. 'Evet' seçeneğini girin
3. Puan eylemini tıklatın.
    - Bot UseLastToppings eylemde istiyor. İki geri arama yöntemleri ikinci olmasıdır. Bu son siparişin toppings toppings kopyalayın ve son toppings temizleyin. Bu son siparişin anımsama ve kullanıcı başka bir pizza istedikleri diyorsa, bu toppings seçenekler olarak sağlayan bir yoludur.
2. 'Üzerinde pizza $Toppings sahip' seçmek için tıklatın.
3. ', Başka bir şey ister misiniz?' seçin
8. 'Hayır' girin.
4. Öğretme Bitti'yi tıklatın.

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Gösteri - VR uygulama Başlatıcısı](./demo-vr-app-launcher.md)
