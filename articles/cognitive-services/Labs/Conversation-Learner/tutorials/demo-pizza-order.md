---
title: Tanıtım konuşma Öğrenici modeli, pizza sipariş - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Bir tanıtım konuşma Öğrenici modeli oluşturmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 052ef249f3367a562e5598b90533c0e52ed75df4
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171393"
---
# <a name="demo-pizza-order"></a>Demo: Pizza sırası
Bu Tanıtım bir pizza Robotu sıralama gösterir. Bu tek bir pizza bu işlevle sıralama destekler:

- pizza toppings içinde kullanıcı konuşma tanıma
- pizza toppings stoktaki veya stok dışında ise denetlemek ve uygun şekilde yanıt
- önceki bir sipariş pizza toppings anımsama ve yeni bir sıra ile aynı toppings Başlat - teklifi

## <a name="video"></a>Video

[![Tanıtım Pizza Önizleme](http://aka.ms/cl-demo-pizza-preview)](http://aka.ms/blis-demo-pizza)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, pizza sipariş bot çalışıyor olması gerekir

    npm run demo-pizza

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi modeli listesinde TutorialDemo Pizza siparişine tıklayın. 

## <a name="entities"></a>Varlıklar

Üç varlığı oluşturdunuz.

- Toppings: Bu varlık için kullanıcı sorulan toppings birikir. Stokta bulunan geçerli toppings içerir. Bir belirtti içine veya dışına hisse senedi olup olmadığını denetler.
- OutofStock: Bu varlık, seçili belirtti stokta olmadığını kullanıcıya geri iletişim kurmak için kullanılır.
- LastToppings: sipariş yerleştirildikten sonra kullanıcıya, sipariş toppings listesini sunmak için bu varlık kullanılır.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Eylemler

Ne eklemiş şu ana kadar vb. belirten bir dizi eylemi kendi pizza üzerinde istediğini anın dahil olmak üzere oluşturdunuz.

İki API çağrıları vardır:

- FinalizeOrder: pizza sırasının yerleştirmek için
- UseLastToppings: önceki adımların sırası izlenecek toppings geçirmek için 

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Eğitim iletişim kutuları
Eğitim iletişim kutuları birkaç tanımladınız. 

![](../media/tutorial_pizza_dialogs.PNG)

Örneğin, öğretim oturumu deneyelim.

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
1. 'Bir pizza sipariş' girin.
2. Puan eylemini tıklatın.
3. İçin seçin 'ne, pizza üzerinde ister misiniz?' tıklayın
4. 'Mushrooms ve peynirlerine ayırıyor' girin.
    - LUIS her ikisi de Toppings olarak etiketlenmiş dikkat edin. Doğru değildi, vurgulamak için tıklayın ve ardından düzeltmek.
    - Varlığın yanındaki '+' işaretine toppings kümesine eklenmekte anlamına gelir.
5. Puan eylemleri tıklayın.
    - Bildirim `mushrooms` ve `cheese` Toppings bellekte değildir.
3. 'Üzerinde pizza $Toppings sahip' seçmek için tıklayın
    - Sonraki eylem için robot ister bu olduğundan bekleme olmayan eylem dikkat edin.
6. 'Başka bir şey ister misiniz?' seçin
7. 'Mushrooms kaldırın ve ekmeye Ekle' girin.
    - Bildirim `mushroom` sahip bir '-', kaldırılacak yanındaki işareti. Ve `peppers` sahip 'için toppings eklemek için bir +' işaretine yanındaki.
2. Puan eylemini tıklatın.
    - Bildirim `peppers` içinde kalın olarak yeni artık. Ve `mushrooms` çaprazlandı.
8. 'Üzerinde pizza $Toppings sahip' seçmek için tıklayın
6. 'Başka bir şey ister misiniz?' seçin
7. 'Add Bezelye' girin.
    - `Peas` Hisse Senedi dışında olan bir belirtti örneğidir. Hala belirtti olarak etiketlenir.
2. Puan eylemini tıklatın.
    - `Peas` OutOfStock gösterilir.
    - Bunun nasıl olduğunu görmek için koda göz açın `C:\<\installedpath>\src\demos\demoPizzaOrder.ts`. At EntityDetectionCallback yöntemi arayın. Bu yöntem, stok olup olmadığını görmek için her belirtti sonra çağrılır. Aksi halde toppings kümesinden temizler ve OutOfStock varlığa ekler. Stok toppings listesi olan bu yöntem inStock değişkeni tanımlanır.
6. 'Biz $OutOfStock yoksa' seçin.
7. 'Başka bir şey ister misiniz?' seçin
8. 'Hayır' girin.
9. Puan eylemini tıklatın.
10. 'FinalizeOrder' API çağrısı seçin. 
    - Bu kod içinde tanımlanan 'FinalizeOrder' işlevi çağırır. Bu toppings temizler ve 'siparişinizi yolda olduğunu' döndürür. 
2. 'Başka bir sipariş' girin. Yeni bir sipariş başlıyoruz.
9. Puan eylemini tıklatın.
    - 'peynirlerine ayırıyor' ve 'ekmeye' son sipariş toppings olarak bellekte olan.
1. '$LastToppings istediğinizi' seçin.
2. 'Evet' girin
3. Puan eylemini tıklatın.
    - Bot UseLastToppings işlem yapmasını istemektedir. İki geri çağırma yöntemlerine ikinci olmasıdır. Bu son siparişin toppings toppings kopyalayın ve son toppings temizleyin. Bu son siparişin anımsama ve kullanıcının başka bir pizza istedikleri diyorsa seçenek olarak bu toppings sağlayan bir yoludur.
2. 'Üzerinde pizza $Toppings sahip' seçmek için tıklayın.
3. 'Başka bir şey ister misiniz?' seçin
8. 'Hayır' girin.
4. Öğretim Bitti'ye tıklayın.

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Demo - VR uygulama Başlatıcısı](./demo-vr-app-launcher.md)
