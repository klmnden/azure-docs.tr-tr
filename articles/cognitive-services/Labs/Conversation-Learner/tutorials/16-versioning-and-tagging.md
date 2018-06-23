---
title: Sürüm oluşturma ve konuşma öğrenen uygulamayla - Microsoft Bilişsel hizmetler etiketleme kullanmayı | Microsoft Docs
titleSuffix: Azure
description: Sürüm oluşturma ve bir konuşma öğrenen uygulamayla etiketleme kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ea013db078ff33f8597b0e15a8fc951e8ae320e8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354041"
---
# <a name="how-to-use-versioning-and-tagging"></a>Sürüm oluşturma ve etiketleme kullanma

Bu öğretici, hangi sürümün "canlı" konuşma öğrenen uygulamanızı sürümlerini etiketine ve ayarlamak nasıl gösterilmektedir.  

## <a name="requirements"></a>Gereksinimler
Bu öğretici, günlük iletişim kutuları, değil günlük iletişim Web kullanıcı arabirimini oluşturmak için bot öykünücüsü kullanarak gerektirir.  

Bu öğretici, genel öğretici bot çalışıyor olması gerekir:

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Düzenlerken, her zaman "ana" adlı etiket düzenlemekte olduğunuz--(hangi temelde ana bir anlık görüntüsünü) yöneticisinden etiketli sürümler oluşturabilirsiniz, ancak etiketli sürümleri düzenleyemezsiniz.

## <a name="steps"></a>Adımlar

### <a name="install-the-bot-framework-emulator"></a>Bot framework öykünücüyü yükleyin

- [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator) kısmına gidin.
- Öykünücü yükleyip yeniden açın.

### <a name="create-an-app"></a>Bir uygulama oluşturun

1. Yeni uygulama'ı tıklatın
2. Öğretici-16-Versioning ad alanına girin
3. Oluştur’a tıklayın 
4. Ayarlar'ı tıklatın
5. Uygulama Kimliğini kopyalayın

### <a name="configure-the-emulator"></a>Öykünücü yapılandırın

- Konuşma öğrenen kök klasöründe .env dosyasını açın.
- Uygulama Kimliği CONVERSATION_LEARNER_APP_ID değeri olarak Yapıştır
- Komut isteminden çıkmak ve yeniden çalıştırılması konuşma öğrenen hizmetini yeniden başlatın:
 
    npm Öğreticisi-genel çalıştırın 

### <a name="actions"></a>Eylemler

Bir eylem oluşturalım:

1. Web kullanıcı Arabirimi geçin.
1. Eylemler, yeni bir eylem tıklatın.
2. Yanıt olarak girin "Merhaba vardır (sürüm 1)".
3. Kaydet'i tıklatın.


![](../media/tutorial16_action1.PNG)

Yeni bir etiket oluşturun:

3. "Ayarlar" üzerinde'ı tıklatın ve yeni bir "etiket" oluşturun.
    - "Sürüm 1" çağırın
4. "" Canlı"olarak sürüm 1" olarak ayarlayın.  
    - Canlı etiketi sürümüne"1" ayarı bu bot kullanarak kanallar kullanacağını etkisidir "sürüm 1" etiketi.
    - Uygulamaların etiketli sürümleri (Eylemler, varlıklar tren iletişim kutuları ekleme, değiştirme) düzenlemeleri tarafından etkilenmez.  
    - (Eylemler, varlıkları değiştirme, tren iletişim kutuları ekleme), bir uygulamaya düzenlemeler her zaman "ana" etiketinde yapılır.  Diğer bir deyişle, "ana" değiştirebilirsiniz yalnızca etiketi olur; diğer etiketleri anlık görüntüleri düzeltilmiştir.
    - İletişim kutuları konuşma öğrenen Arabiriminde her zaman ana (dinamik etiketi değil) oturum açın.

![](../media/tutorial16_v1_create.PNG)

Not sürüm ayarlarında oluşturuldu:

![](../media/tutorial16_settings.PNG)

İkinci bir eylemi ekleyelim:

1. Eylemler, yeni bir eylem tıklatın.
2. Yanıtta, "bye bye (sürüm 2)" girin.

İlk eylem düzenleyin:

1. Eylemler'i tıklatın.
2. Eylemler altında tıklayın "Merhaba vardır (sürüm 1)".
3. Yanıta değiştirme "Merhaba vardır (sürüm 2)".

![](../media/tutorial16_hi_there_v2.PNG)

### <a name="switch-to-the-bot-emulator"></a>Bot öykünücüsü geçiş

1. Bot UI "goodbye" girin.
2. Bot yanıt ile Not "Merhaba vardır (sürüm 1)".
    - Bu sürüm 1 "canlı" olduğunu gösterir. 

![](../media/tutorial16_bf_response.PNG)

### <a name="switch-to-the-web-ui"></a>Web kullanıcı Arabirimi geçiş

1. Günlük iletişim kutuları (uygulama Yenile tüm iletişim kutularını görmüyorsanız) tıklayın.
2. Tıklayın "Merhaba vardır (sürüm 2)"

Şu anda kullanılabilir tüm eylemler seçme düzeltmeler yapabilirsiniz unutmayın. Bu düzenlemeler asıl yapılacaktır.

Sürüm oluşturma nasıl çalıştığını ve Bot framework öykünücüsü kullanarak bot ile nasıl etkileşim şimdi gördünüz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Gösteri - parola sıfırlama](./demo-password-reset.md)
