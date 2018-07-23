---
title: Konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler oturumu geri çağırmaları kullanmayı | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle oturum geri çağırmaları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 0f51b232470e4e4da3f25d40d025dd3b09dd1204
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171924"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Konuşma Öğrenici modeliyle oturum geri çağırmaları kullanma

Bu öğreticide onSessionStart ve onSessionEnd geri çağırmaları gösterilmektedir.

## <a name="video"></a>Video

[![Öğretici 11 Önizleme](http://aka.ms/cl-tutorial-11-preview)](http://aka.ms/blis-tutorial-11)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide gerektiren `tutorialSessionCallbacks` bot çalışıyor.

    npm run tutorial-session-callbacks

## <a name="details"></a>Ayrıntılar
Bu öğretici, bir oturum, oturumlar varsayılan olarak nasıl işleneceğini ve bu davranışı nasıl geçersiz kavramını kapsar.

Bot ile bir konuşma oturumdur. Birden çok kapatır olabilir ancak (örneğin, 30 dakika) konuşmada uzun kesme yok vardır.  Varsayılan oturum zaman aşımı süresi için "sınırlar" Yardım sayfasında bakın.

Uzun sonları varsa, bot, bir sonraki oturum için gidin.  Yeni bir oturum başlatmayı yinelenen sinir ağı, ilk duruma koyar.  Bu davranış (aşağıda Resimli) değiştirilebilir ancak varsayılan olarak, bu da tüm varlık değerlerini temizler.

### <a name="open-the-demo"></a>Tanıtım açın

Öğretici-11-SessionCallbacks modeli listesinde tıklayın. 

### <a name="entities"></a>Varlıklar

Dört varlıkları modelde tanımlanmıştır.

![](../media/tutorial11_entities.PNG)

Dikkat edilecek bir şey BotName programlı bir varlık olmasıdır.  Bu varlık oturum başlangıç saati robot tarafından ayarlanır.

### <a name="actions"></a>Eylemler

Dört eylem modelde tanımlanmıştır.

![](../media/tutorial11_actions.PNG)

İlk olarak, bu öğreticide--oturumunun başlangıcında varlık değerlerini denetlemek Örneğin, kullanıcının herhangi bir şey diyor önce BotName varlık ayarı gösterilmektedir.

İkinci olarak, bu öğreticinin sonraki bir oturumdan değerler kalır yapmayı gösterir.  Konumlarını değiştirebilirsiniz, ancak bu öğreticide, bir oturumundan diğerine aynı kalır, bu kullanıcı adını ve telefon numarasını varsayıyoruz.  Biz bu nedenle adını ve telefon numarasını oturumları, ancak NET kullanıcı konumu arasında kalıcı hale getirin.

### <a name="train-dialog"></a>Train iletişim

Örnek bir iletişim kutusu aşağıda verilmiştir. Bu bir oturum değil - diğer bir deyişle, vardır uzun sonları olmadığından bu iletişim kutusunda.

![](../media/tutorial11_traindialog.PNG)

### <a name="code-for-the-callbacks"></a>Kod için geri çağırmaları

Geri çağırma yöntemleri için kod dosyasıdır: c:\<installedpath > \src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial11_code.PNG)

Bu yöntemlerin ikisi de isteğe bağlıdır.

- OnSessionStartCallback: Bu yöntem BotName varlık ayarlar.
- OnSessionEndCallback: korumak istediğiniz belirtebilirsiniz. Bu kullanıcı adı ve kullanıcı telefonu hariç tüm varlıklar temizler.

### <a name="try-the-bot"></a>Bot deneyin

Geçiş için Web UI ve günlük iletişim kutuları üzerinde tıklayın.

1. 'Hello' girin.
2. Sistem: ' Merhaba, Ben Botty. Adınız ne?' OnSessionStartCallback adı Botty gelen sahip.
3. 'Jason' girin.
4. Sistem: ' Merhaba jason. Telefon numaranız nedir?'
5. Girin ' 555-555-5555'.
6. Sistem: ', Botty jason konumunuz söyleyebilirsiniz?'
7. 'Redmond' yazın.

Bu, bir oturum değil. Yeni bir oturum başlatmak için bu oturumu sonlandırmak ihtiyacımız var. 

1. Oturum zaman aşımı'a tıklayın. Bu oturuma İleri taşınır.
    - "Oturum zaman aşımı" düğmesi, hata ayıklama amacıyla sağlanır.  Gerçek bir oturumda uzun bir duraklama gerçekleşir, yaklaşık 30 dakika gerekir.  Varsayılan oturum zaman aşımı süresi için "sınırlar" Yardım sayfasında bakın.
1. 'Merhaba' girin.
2. Sistem: ', Botty jason konumunuz söyleyebilirsiniz?'
    - Sistem adını ve telefon numarasını anımsanacak.
2. Yeni bir konum girin: 'Seattle'.
3. Sistem: 'Bu nedenle, jason Seattle olduğunuz'.
4. Yapılan test tıklayın.

Şimdi geri günlük iletişim kutuları için geçiş yapın. Her günlük iletişim karşılık gelen bir oturumla olduğundan son konuşma ikiye ayırma olduğuna dikkat edin.  

![](../media/tutorial11_splitdialogs.PNG)

- İlk etkileşim Botty ayarlanır, ancak ad ve telefon numarası değildir.
- İkinci etkileşimi adını ve telefon numarasını gösterir.

Şimdi, oturumlar varsayılan olarak nasıl işleneceğini ve varsayılan davranışını nasıl geçersiz gördünüz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API çağrıları](./12-api-calls.md)
