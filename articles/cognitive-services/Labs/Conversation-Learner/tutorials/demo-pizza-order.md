---
title: Tanıtım konuşma Öğrenici modeli, pizza sipariş - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Bir tanıtım konuşma Öğrenici modeli oluşturmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3fe11bef6c505771ee1e3f2e12e647eafc7c45d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60430998"
---
# <a name="demo-pizza-order"></a>Tanıtım: Pizza sırası
Bu Tanıtım destekleyen göre sıralama tek pizza Robotu, sıralama bir pizza gösterilmektedir:

- pizza toppings gelen kullanıcı konuşma tanıma
- uygun şekilde yanıt vermesini ve toppings envanteri Yönetimi
- Önceki siparişleri anımsayarak ve özdeş bir pizza yeniden sıralama hızlandırma

## <a name="video"></a>Video

[![Tanıtım Pizza Önizleme](https://aka.ms/cl_Tutorial_v3_DemoPizzaOrder_Preview)](https://aka.ms/cl_Tutorial_v3_DemoPizzaOrder)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, pizza sipariş bot çalışıyor olması gerekir

    npm run demo-pizza

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi modeli listesinde TutorialDemo Pizza siparişine tıklayın. 

## <a name="entities"></a>Varlıklar

Modelin üç varlıklar içerir:

- "Toppings" Kullanıcının belirtilen toppings varsa toplanır.
- "OutofStock" Seçili belirtti sağlanamıyor olduğunu kullanıcıya bildirir.
- Önceki düzenlerine gelen geçmiş toppings "LastToppings" içerir

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Eylemler

Model belirtti seçimi, birikmiş toppings ve daha fazla kullanıcının soran bir eylemler kümesi içerir.

İki API çağrıları de sağlanır:

- Siparişi yerine getirme "FinalizeOrder" işleme
- "UseLastToppings" geçmiş toppings bilgileri işler.

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Eğitim iletişim kutuları

Çeşitli eğitim iletişim kutuları modelde bulunamadı.

![](../media/tutorial_pizza_dialogs.PNG)

Şimdi başka bir eğitim iletişim oluşturarak biraz daha fazla modeli eğitme.

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Bir pizza peynirlerine ayırıyor ile sipariş" yazın
    - "Peynirlerine ayırıyor" sözcüğü, varlık ayıklayıcı tarafından ayıklandı.
3. "Puan Eylemler" düğmesine tıklayın.
4. Yanıtı seçin, ", pizza peynirlerine ayırıyor sahibiz."
5. "Başka bir şey ister misiniz?" yanıt seçin
6. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "mushrooms ve ekmeye Ekle" türü
7. "Puan Eylemler" düğmesine tıklayın.
8. "Peynirlerine ayırıyor ve mushrooms ekmeye, pizza üzerinde sizde." yanıt seçin
9. "Başka bir şey ister misiniz?" yanıt seçin
10. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "ekmeye kaldırın ve sausage Ekle" türü
11. "Puan Eylemler" düğmesine tıklayın.
12. Yanıtı seçin, "Peynirlerine ayırıyor ve mushrooms sausage üzerinde pizza sizde."
13. "Başka bir şey ister misiniz?" yanıt seçin
14. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "yam Ekle" türü
15. "Puan Eylemler" düğmesine tıklayın.
    - Metnin desteklenen tüm malzemeleri eşleşmediğinden "yam" değeri "OutofStock" için varlık algılama geri çağırma kodu tarafından eklendi.
16. Yanıt, "OutOfStock" seçin
17. "Başka bir şey ister misiniz?" yanıt seçin
18. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Hayır" yazın
    - "Hayır" olarak her türlü amacı işaretlenir. Bunun yerine, biz geçerli bağlam temelinde uygun eylemi ayrılmayın.
19. "Puan Eylemler" düğmesine tıklayın.
20. Yanıt, "FinalizeOrder" seçin
    - Müşteri'nin geçerli toppings "LastToppings" varlığı ve "Toppings" varlığın silinmesini FinalizeOrder geri çağırma kod tarafından kaydedilen, bu eylem seçildikten sonuçlandı.
21. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "başka bir sipariş" yazın
22. "Puan Eylemler" düğmesine tıklayın.
23. "Peynirlerine ayırıyor ve mushrooms sausage ister misiniz?" yanıt seçin
    - Bu eylem ayarlanıyor "LastToppings" varlığı nedeniyle artık kullanılabilir.
24. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "yes" yazın
25. "Puan Eylemler" düğmesine tıklayın.
26. Yanıt, "UseLastToppings" seçin
27. Yanıtı seçin, "Peynirlerine ayırıyor ve mushrooms sausage üzerinde pizza sizde."
28. "Başka bir şey ister misiniz?" yanıt seçin

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma Öğrenici bot dağıtma](../deploy-to-bf.md)
