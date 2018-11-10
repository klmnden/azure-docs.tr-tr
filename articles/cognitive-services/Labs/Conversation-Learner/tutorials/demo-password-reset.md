---
title: Tanıtım konuşma Öğrenici modeli, parola sıfırlama - Microsoft Bilişsel hizmetler | Microsoft Docs
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
ms.openlocfilehash: bd0bcd79bb21dc3973b34086f6dad21b47a95c2f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240877"
---
# <a name="demo-password-reset"></a>Demo: Parola sıfırlama
Bu Tanıtım, parola sıfırlama ile yardım edebilecek bir basit teknik destek bot gösterilmektedir. 

Bu, konuşma Öğrenici Önemsiz iletişim akışlar, çıkış, etki alanı sınıfı dahil olmak üzere, çok Aç dizileri nasıl bilgi gösterir. Bu sunum, herhangi bir kod veya varlıkları kullanmaz.

## <a name="video"></a>Video

[![Tanıtım parola Önizleme](https://aka.ms/cl-demo-password-preview)](https://aka.ms/blis-demo-password)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, parola sıfırlama bot çalışıyor olması gerekir

    npm run demo-password

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi Model listesi, öğretici tanıtım parola sıfırlama tıklayın. 

### <a name="actions"></a>Eylemler

Kullanıcı parolalarını ile çözümleri dahil olmak üzere Yardım almak için nereye arıyor eylemleri kümesini oluşturduk.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Eğitim iletişim kutuları

Eğitim iletişim kutuları vardır. Yetersiz alan sınıfının--gösterimlerine 'yol tarifi' etki alanı yetersiz olduğu gibi kullanıcı isteklerini vardır; bot, birkaç etki alanı istekleri dışında örnekleri verilen ve yanıt 'I ile yardımcı olamaz.'

![](../media/tutorial_pw_reset_entities.PNG)

Örneğin, öğretim oturumu deneyelim.

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
1. 'I parolamı kayıp' girin.
2. Puan eylemini tıklatın.
3. 'Yerel hesap veya Microsoft hesabı için olan?' seçmek için tıklayın
4. 'Yerel hesabı' girin.
5. Puan eylemleri tıklayın.
3. 'Windows'ın hangi sürümünü kullanıyorsunuz?' seçmek için tıklayın
4. Girin ' Windows 8'.
5. Puan eylemleri tıklayın.
6. Seç ' çözümü: Windows 8 parolasını nasıl sıfırlayacağımı.'
4. Öğretim Bitti'ye tıklayın.

Bot çıkış, etki alanı sınıfı nasıl bilgi deneyelim.

1. Train iletişim kutuları, ardından yeni Train iletişim tıklayın.
1. 'Web araması' girin.
    - Bu çıkış, etki alanı sınıfı, bir örnektir. 
2. Puan eylemini tıklatın.
3. 'Ne yazık ki ile Yardım olamaz.' seçmek için tıklayın
    - Bu seçenek puanını şu anda düşük olduğuna dikkat edin. Ancak biraz daha fazla öğretim sonra puan daha yüksek elde edersiniz.
4. Öğretim Bitti'ye tıklayın.

Artık, temel teknik destek tanıtım oluşturma ve bu çözümleri sağlayan ve ayrıca dışında örnek sorguları işlemek nasıl edinebilirsiniz gördünüz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Demo - pizza sırası](./demo-pizza-order.md)
