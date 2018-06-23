---
title: Oturum geri aramalar bir konuşma öğrenen uygulaması - Microsoft Bilişsel hizmetler ile kullanma | Microsoft Docs
titleSuffix: Azure
description: Oturum geri aramalar bir konuşma öğrenen uygulaması ile kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f8970620c1f0f87ccae13d031092a048144ffb19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354017"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-application"></a>Oturum geri aramalar bir konuşma öğrenen uygulaması ile kullanma

Bu öğretici onSessionStart ve onSessionEnd geri aramalar gösterilmektedir.

## <a name="requirements"></a>Gereksinimler
Bu öğretici "tutorialSessionCallbacks.ts" bot çalışıyor olması gerekir.

    npm run tutorial-session-callbacks

## <a name="details"></a>Ayrıntılar
Bu öğretici bir oturum, oturumlar varsayılan olarak nasıl işlendiğini ve nasıl bu davranışı geçersiz kılabilirsiniz kavramı kapsar.

Bir konuşma bot oturumdur. Birden çok kapatır olabilir ancak (örneğin, 30 dakika) konuşmada uzun kesme yok vardır.  Varsayılan oturum zaman aşımı süresi için "sınırları" üzerinde yardım sayfasına bakın.

Uzun kesintiler varsa, bot sonraki oturumuna geçer.  Yeni bir oturum açması yinelenen sinir ağı ilk durumuna yerleştirir.  Bu davranış (aşağıda Resimli) değiştirilebilir ancak varsayılan olarak, bu da tüm varlık değerleri temizler.

### <a name="open-the-demo"></a>Tanıtım açın

Uygulama listesinden Öğreticisi-11-SessionCallbacks'ı tıklatın. 

### <a name="entities"></a>Varlıklar

Biz dört varlıklar uygulamada tanımladınız.

![](../media/tutorial11_entities.PNG)

Not etmek için bir şey BotName programlı bir varlık olmasıdır.  Bu oturum başlangıç zamanında bot tarafından ayarlanır.

### <a name="actions"></a>Eylemler

Dört eylem oluşturduk. 

![](../media/tutorial11_actions.PNG)

İlk olarak, Bu öğretici varlık değerleri--oturumunun başlangıcında denetleme Örneğin, kullanıcının herhangi bir şey diyor önce BotName varlık ayarı gösterir.

İkinci olarak, Bu öğretici, bir oturum değerlerinden sonrakine kalıcı hale getirmek nasıl yapacağınızı gösterir.  Konumlarını değiştirebilirsiniz, ancak bu öğreticide, kullanıcının adı ve telefon numarası bir sonraki bir oturuma aynı kalır varsayıyoruz.  Biz bu nedenle adı ve telefon numarası oturumları, ancak Temizle kullanıcı konumu kalıcı olmasını sağlar.

### <a name="train-dialog"></a>Tren iletişim

Burada, örnek bir iletişim kutusu verilmiştir. Bu bir oturum değil - diğer bir deyişle, vardır uzun sonları olmadığından bu iletişim kutusunda.

![](../media/tutorial11_traindialog.PNG)

### <a name="code-for-the-callbacks"></a>Geri aramalar için kodu

Geri arama yöntemleri için kod dosyasındadır: c:\<installedpath > \src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial11_code.PNG)

Bu yöntemlerin her ikisi de isteğe bağlıdır.

- OnSessionStartCallback: Bu yöntem BotName varlık ayarlar.
- OnSessionEndCallback: temizlemek istediğiniz belirtebilirsiniz. Bu kullanıcı adı ve kullanıcı telefonu dışındaki tüm varlıklar temizler.

### <a name="try-the-bot"></a>Bot deneyin

Web kullanıcı Arabirimi için geçin ve günlük iletişimleri'ı tıklatın.

1. 'Hello' girin.
2. Sistem: ' Merhaba, Botty ben. Adınız nedir?' hangi OnSessionStartCallback adı Botty gelen sahiptir.
3. 'Jason' girin.
4. Sistem: ' Hi jason. Telefon numaranız nedir?'
5. Girin ' 555-555-5555'.
6. Sistem: ', Botty jason konumunuz anlayabilirsiniz?'
7. 'Redmond' yazın.

Bu, bir oturum değil. Yeni bir oturum başlatmak için bu oturumunu sona erdirmek ihtiyacımız. 

1. Oturum zaman aşımı'ı tıklatın. Bu sonraki oturuma taşınır.
    - "Oturum zaman aşımı" düğmesi hata ayıklama amacıyla sağlanır.  Gerçek bir oturumda oluşmasına yaklaşık olarak 30 dakikadan uzun bir duraklama gerekir.  Varsayılan oturum zaman aşımı süresi için "sınırları" üzerinde yardım sayfasına bakın.
1. 'Merhaba' girin.
2. Sistem: ', Botty jason konumunuz anlayabilirsiniz?'
    - Sistem adı ve telefon numarası hatırlanan.
2. Yeni bir konum girin: 'Seattle'.
3. Sistem: ', jason Seattle olduğunuz'.
4. Done sınama'ı tıklatın.

Şimdi geri günlük iletişim kutuları için geçin. Her günlük iletişim için bir oturum karşılık olmadığından son konuşma ikiye bölündü dikkat edin.  

![](../media/tutorial11_splitdialogs.PNG)

- İlk etkileşim Botty ayarlanır, ancak ad ve telefon numarası değildir.
- İkinci etkileşim adını ve telefon numarasını gösterir.

Şimdi, oturumlar varsayılan olarak nasıl işlendiğini ve nasıl varsayılan davranışı geçersiz kılabilirsiniz gördünüz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API çağrıları](./12-api-calls.md)
