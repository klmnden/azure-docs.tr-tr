---
title: Dal oluşturma ve geri alma işlemleri bir konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler ile nasıl kullanılacağını | Microsoft Docs
titleSuffix: Azure
description: Dal oluşturma ve geri alma işlemleri bir konuşma Öğrenici modeliyle nasıl kullanılacağını öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 6ffa0881df07e453c8beb175b8580deebbfc1ec9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389884"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Dal oluşturma ve geri alma işlemleri kullanma
Bu öğreticide, geri alma ve dal oluşturma işlemleri alacağız.


## <a name="details"></a>Ayrıntılar
### <a name="undo"></a>Geri alma
"Son kullanıcı girişi veya eylem seçimi geri almak" Geliştirici sağlar. Planda, "geri" gerçekten yeni bir iletişim kutusu oluşturur ve en fazla önceki adımdan başlayarak yeniden oynatılır.  Varlık algılama geri çağırma ve API çağrıları iletişim kutusunda Bunun anlamı tekrar çağrılır.

### <a name="branch"></a>Şube
Aynı şekilde, mevcut bir eğitim iletişim – bu efor kaydeder gibi başlayan yeni bir train iletişim oluşturur el ile yeniden girildiğinde iletişim kutusu açar. Planda, "dal" yeni bir iletişim kutusu oluşturur ve mevcut eğitme iletişim seçili adımın kadar başlayarak yeniden oynatılır.  Varlık algılama geri çağırma ve API çağrıları iletişim kutusunda Bunun anlamı tekrar çağrılır.


## <a name="requirements"></a>Gereksinimler
Bu öğreticide, pizza siparişleri alır Bot çalışıyor olması gerekir:

    npm run demo-pizza

### <a name="open-or-import-the-demo"></a>Açın veya tanıtım içeri aktarma

Pizza sıralama öğreticide çalıştıysanız, sonra bu modeli web kullanıcı Arabirimi listeden açmanız yeterlidir. Aksi takdirde "İçeri aktarma eğitimler"'a tıklayın ve "Demo-PizzaOrder" adlı bir model seçmek gerekir.

## <a name="undo"></a>Geri alma

İşte bir örnek görmek nasıl `Undo` eylemi özelliği:

### <a name="training-dialogs"></a>Eğitim iletişim kutuları
1. Sol paneldeki "Train iletişim kutuları"'a tıklayın ve ardından `New Train Dialog` düğmesi.
2. "Bir pizza sipariş" yazın.
3. `Score Actions` düğmesine tıklayın.
4. "Ne üzerinde pizza ister misiniz?" seçeneğini seçin tıklayın
5. "Şey" yazın.
6. `Undo` düğmesine tıklayın.
    - En son giriş, "Ne üzerinde pizza ister misiniz?", son Bot yanıt bırakarak kaldırılır

## <a name="branch"></a>Şube

Bu Tanıtım için biz varolan bir Train iletişim kutusunu açın ve dallara ayırma tarafından yeni bir eğitimi iletişim kutusu oluşturmayı.

1. Sol panelde "Train iletişim kutuları"'a tıklayın.
2. Kılavuz dikkat edin, eğitim tek başlar "ile yeni sipariş" görmeniz gerekir.
3. "Yeni sipariş" kılavuzunda tıklayın mevcut eğitme iletişim kutusunu açın.
4. Son iletişim kutusunda "Hayır" tıklayın.
5. "Dal" simgesine tıklayın, bunu bu resimdeki kırmızı daire içine alınmıştır:
    - ![](../media/tutorial15_branch.PNG)
    - Yeni bir eğitimi iletişim kutusuna "Hayır" önce tüm Train iletişim kopyalanır.
    - Bu noktadan itibaren önceki yeniden girmeye kapatır yeni konuşma "dal" keşfetmek için bu kaydeder.
6. "Yes" yazın, isabet girin.
7. `Score Actions` düğmesine tıklayın.
    - Bu noktada Bot yanıt otomatik olarak seçer, ancak değiştirmek kullanacağız şekilde yanıt gönderilmesini yok.
8. Son Bot yanıtta tıklayın.
    - Bu, farklı bir yanıt bize seçecektir.
9. "UseLastToppings" seçin.
10. `Score Actions` düğmesine tıklayın.
    - Yeniden Bot otomatik olarak bir yanıt alır. Bunu varsayalım, "sausage ve peynirlerine ayırıyor mushrooms üzerinde pizza.". 
    - Bu süre, saklayacağız şekilde yanıt istiyoruz.
11. `Score Actions` düğmesine tıklayın.
    - Yeniden Bot otomatik olarak bir yanıt olmalıdır, "başka bir şey ister misiniz?" seçer
12. "Hayır" yazın.
13. `Save Branch` düğmesine tıklayın.
14. Kılavuz artık "Yeni sırası" ile başlar iki eğitimleri olduğuna dikkat edin.
    - Bunlardan biri, dal için kullanılan paroladır.
    - Ve diğer bir yeni kaydettiğiniz dallı sürümüdür.
    - Her birinin bu beklentileri doğrulamak için tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturma ve etiketleme](./18-version-tag.md)
