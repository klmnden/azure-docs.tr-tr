---
title: Sürüm oluşturma ve bir konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler ile etiketleme kullanmayı | Microsoft Docs
titleSuffix: Azure
description: Sürüm oluşturma ve konuşma Öğrenici modeliyle etiketleme kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c7f23d989cbfa0ece9e404a0fe0feb68cf5fddb2
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170554"
---
# <a name="how-to-use-versioning-and-tagging"></a>Sürüm oluşturma ve etiketlemeyi nasıl kullanılacağını

Bu öğreticide, hangi sürümün "canlı" konuşma Öğrenici modelinizi sürümleri etiketleme ve ayarlamak nasıl gösterir.  

## <a name="requirements"></a>Gereksinimler
Bu öğretici, günlük iletişim kutuları, olmayan günlük iletişim Web kullanıcı arabirimini oluşturmak için robot öykünücüyü gerektirir.  

Bu öğreticide, genel öğretici bot çalışıyor olması gerekir:

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Düzenlerken, her zaman "ana" olarak adlandırılan etiketi düzenlemekte olduğunuz--etiketli sürümleri (Bu aslında ana anlık görüntüsünü alın) yöneticisinden oluşturabilirsiniz, ancak etiketlenmiş sürümlerinde düzenleyemezsiniz.

## <a name="steps"></a>Adımlar

### <a name="install-the-bot-framework-emulator"></a>Bot framework öykünücüsü'nü yükleyin

- [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator) kısmına gidin.
- İndirin ve öykünücüyü yükleyin.

### <a name="create-an-model"></a>Bir model oluşturma

1. Yeni bir modele tıklayın
2. Öğretici-16-Versioning ad alanında girin
3. Oluştur’a tıklayın 
4. Ayarları
5. Model kimliği kopyalayın

### <a name="configure-the-emulator"></a>Öykünücü yapılandırın

- Konuşma Öğrenici kök klasöründe .env dosyasını açın.
- Model kimliği CONVERSATION_LEARNER_MODEL_ID değeri olarak yapıştırın.
- Komut İstemi'nden çıkmadan ve yeniden çalıştırılması, konuşma Öğrenici hizmeti yeniden başlatın:
 
    npm çalıştırma Öğreticisi-genel 

### <a name="actions"></a>Eylemler

Bir eylem oluşturalım:

1. Web kullanıcı Arabirimine geçin.
1. Eylemler ve ardından yeni bir eylem tıklayın.
2. Yanıt olarak girin "Merhaba vardır (sürüm 1)".
3. Kaydet’e tıklayın.


![](../media/tutorial16_action1.PNG)

Yeni bir etiket oluşturun:

3. "Ayarları"'a tıklayın ve yeni bir "etiket" oluşturun.
    - "Sürüm 1" çağrısı
4. "" Canlı"olmasını sürüm 1" olarak ayarlayın.  
    - Bu botunu kullanarak kanalları kullanacağını "sürüm 1" Canlı etikete ayarlamanın etkisi olan "sürüm 1" etiketi.
    - Etiketli sürümlerini modelleri düzenlemelerini (Eylemler, varlıklar train iletişim kutuları ekleme, değiştirme) tarafından etkilenmez.  
    - (Eylemler, varlıkları değiştirme, ekleme train iletişim kutuları) bir Modeli'ne düzenlemeler, her zaman "ana" etiket üzerinde yapılır.  Diğer bir deyişle, "ana" değiştirebilecek tek etikettir; diğer etiketler anlık görüntüleri sabittir.
    - İletişim kutuları konuşma Öğrenici kullanıcı Arabiriminde her zaman ana (Canlı etiketi değil) oturum açın.

![](../media/tutorial16_v1_create.PNG)

Sürüm ayarlarında oluşturuldu:

![](../media/tutorial16_settings.PNG)

İkinci bir eylem ekleyelim:

1. Eylemler ve ardından yeni bir eylem tıklayın.
2. Yanıt olarak, "bye bye (sürüm 2)" girin.

İlk eylemi Düzenle:

1. Eylemler tıklayın.
2. Eylemler altında tıklayın "Merhaba vardır (sürüm 1)".
3. Yanıta değiştirme "Merhaba vardır (sürüm 2)".

![](../media/tutorial16_hi_there_v2.PNG)

### <a name="switch-to-the-bot-emulator"></a>Bot öykünücüye geçin

1. Bot UI "goodbye ifadesini" girin.
2. Bot ile yanıt veren "Merhaba vardır (sürüm 1)".
    - Bu sürüm 1 "canlı" gösterir. 

![](../media/tutorial16_bf_response.PNG)

### <a name="switch-to-the-web-ui"></a>Web kullanıcı Arabirimine

1. Günlük iletişim kutuları (Yenile düğmesini tıklatın, tüm iletişim kutularını görmüyorsanız) tıklayın.
2. Tıklayın "Merhaba vardır (sürüm 2)"

> [!NOTE]
> Şu anda kullanılabilen tüm eylemler seçerek düzeltmeleri yapabiliyoruz. Bu düzenlemeler, ana dala yapılır.

Artık, sürüm oluşturma nasıl çalıştığını ve Bot framework öykünücüsü'nü bot ile nasıl etkileşim kurabileceğine gördünüz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Demo - parola sıfırlama](./demo-password-reset.md)
