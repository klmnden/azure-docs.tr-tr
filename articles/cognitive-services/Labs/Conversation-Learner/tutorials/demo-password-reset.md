---
title: Gösteri konuşma öğrenen uygulama, parola sıfırlama - Microsoft Bilişsel hizmetler | Microsoft Docs
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
ms.openlocfilehash: 24d61787a79ee1a1a9737c417aa966cc8fd75930
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353993"
---
# <a name="demo-password-reset"></a>Gösteri: Parola sıfırlama
Bu demo parola sıfırlama ile yardımcı olacak basit teknik destek bot gösterilmektedir. 

Bu, konuşma öğrenen Önemsiz olmayan iletişim akışlar, çok Aç dizileri, etki sınıfı dahil olmak üzere nasıl öğrenebilirsiniz gösterir. Bu Tanıtım herhangi bir kod veya varlıklar kullanmaz.

## <a name="requirements"></a>Gereksinimler
Bu öğretici, parola sıfırlama bot çalışıyor olması gerekir

    npm run demo-password

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi uygulama listesi, öğretici Demo parola sıfırlama'ı tıklatın. 

### <a name="actions"></a>Eylemler

Kullanıcı parolalarını çözümleri dahil olmak üzere ile Yardım için burada arıyor Eylemler oluşturduk.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Eğitim iletişim kutuları

Eğitim iletişim kutuları mevcuttur. Etki alanı sınıfı--dışı gösterileri 'yol tarifi' dışında etki alanı gibi örneğin, kullanıcı isteklerini vardır; bot birkaç etki alanı istekleri dışında örnekleri verilen ve 'I ile yardımcı olamaz.' yanıt verebilirsiniz

![](../media/tutorial_pw_reset_entities.PNG)

Örnek olarak, bir öğretme oturumu deneyelim.

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
1. 'I parolamı kayıp' girin.
2. Puan eylemini tıklatın.
3. ', Yerel hesap veya Microsoft hesabı için mi?' seçmek için tıklatın
4. 'Yerel hesap' girin.
5. Puan Eylemler'i tıklatın.
3. 'Windows sürümünü kullanıyorsunuz?' seçmek için tıklatın
4. Girin ' Windows 8'.
5. Puan Eylemler'i tıklatın.
6. Seç ' çözüm: Windows 8 parola sıfırlama.'
4. Öğretme Bitti'yi tıklatın.

Bot etki sınıfı nasıl öğrenebilirsiniz deneyelim.

1. Tren iletişim kutuları, ardından yeni tren iletişim'ı tıklatın.
1. 'Web araması' girin.
    - Bu, etki sınıfı örneğidir. 
2. Puan eylemini tıklatın.
3. 'Ne yazık ki ile Yardım olamaz.' seçmek için tıklatın
    - Bu seçenek puanını şu anda düşük olduğuna dikkat edin. Ancak biraz daha öğretme sonra puanı daha yüksek alırsınız.
4. Öğretme Bitti'yi tıklatın.

Şimdi, temel teknik destek Tanıtımı oluşturmak nasıl ve ne çözümleri sağlamak ve ayrıca dışında örnek sorguları işlemek öğrenebilirsiniz gördünüz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Gösteri - pizza sırası](./demo-pizza-order.md)
