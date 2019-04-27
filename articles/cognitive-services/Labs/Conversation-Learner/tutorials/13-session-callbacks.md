---
title: Konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler oturumu geri çağırmaları kullanmayı | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle oturum geri çağırmaları kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 246b87e46029c2bf4d7361540939181b3b209acc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60635695"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Konuşma Öğrenici modeliyle oturum geri çağırmaları kullanma

Bu öğretici, oturumlar, nasıl işleneceğini ve konuşma Öğrenici'nın onSessionStart ve onSessionEnd geri çağırmaları tanıtır.

## <a name="video"></a>Video

[![Oturumun geri çağırmaları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_SessionCallbacks_Preview)](https://aka.ms/cl_Tutorial_v3_SessionCallbacks)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, "tutorialSessionCallbacks" bot çalışıyor olması gerekir.

    npm run tutorial-session-callbacks

## <a name="details"></a>Ayrıntılar
Bu öğretici, bir oturum, oturumlar varsayılan olarak nasıl işleneceğini ve bu davranışı nasıl geçersiz kavramını kapsar.

Konuşma Öğrenici içinde bir oturumu bir temsil bot ile kesintisiz etkileşimli exchange. Oturumlarının birden çok kapatır sahip olabilir, ancak eylemsizlik nedeniyle otuz dakikadan daha büyük olması durumunda programlı bir şekilde kapatılmasını geçene.  Bu varsayılan oturum zaman aşımı süresi değiştirilmesi hakkında bilgi için "Sınırlar" Yardım sayfasında bakın.

Bu uzun süre etkin olmayıp, yeni bir oturum oluşturun ve yinelenen sinir ağı başlangıç durumuna sıfırlamak robot neden olur. Varsayılan olarak tüm varlık değerleri temizlenir. Aşağıda görüldüğü gibi varlık değerlerini temizleme, bu varsayılan davranış değiştirilebilir.

### <a name="load-the-demo-model"></a>Tanıtım modeli yüklenemiyor

Web kullanıcı Arabirimi "İçeri aktarma eğitimler" tıklayın ve "Öğreticisi-13-SessionCallbacks" adlı modelini seçin.

### <a name="code-for-the-callbacks"></a>Kod için geri çağırmaları

Bu modelin iki geri çağırmaları bulunabilir için örnek kod: `c:\<installedpath>\src\demos\tutorialSessionCallbacks.ts`.

![](../media/tutorial11_code.PNG)

- OnSessionStartCallback: Bu yöntem BotName varlık ayarlar.
- OnSessionEndCallback: korumak istediğiniz belirtebilirsiniz. Bu kullanıcı adı ve kullanıcı telefonu hariç tüm varlıklar temizler.

Her geri çağırma isteğe bağlıdır.

### <a name="actions"></a>Eylemler

Dört eylem modelde tanımlanmıştır. Var olan eylemler için "Eylemler" ızgara görünümünde görüntülenir

![](../media/tutorial11_actions.PNG)

### <a name="creating-an-end-session-action-for-callback-invocation"></a>Oluşturma bir son oturum eylemi (için geri çağırma)

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "ENDSESSION" seçin "Varlık türü."
3. "Bitti" "Veri..." alanına yazın
4. "Oluştur" düğmesine tıklayın.

### <a name="edit-an-existing-action"></a>Var olan bir eylemi Düzenle

1. "Bu nedenle, $UserName, işiniz $UserLocation içinde"'i seçin kılavuz görünümünden eylem.
2. Kaldırma-"Yanıt için beklemek" onay kutusu denetimi.
3. "Kaydet" düğmesine tıklayın.

### <a name="chaining-actions"></a>Zincirleme eylemleri

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Burada yazacaktır "Yazın, iletinizi...", sohbet panelinde türü "hi."
3. "Puan Eylemler" düğmesine tıklayın.
4. Yanıtı seçin "Merhaba, Botty ortağıyım. Adınız ne?"
5. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Lars" yazın
6. "Merhaba Lars. yanıt seçin Telefon numaranız nedir?"
7. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "555-555-5555" yazın
8. "Puan Eylemler" düğmesine tıklayın.
9. "Lars konumunuz Botty anlayabilirsiniz?" yanıt seçin
10. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Seattle" yazın
11. "Puan Eylemler" düğmesine tıklayın.
12. "Lars, size Seattle'olacak şekilde," yanıt seçin
13. "Bitti" yanıt seçin
14. "Kaydet" düğmesine tıklayın.

### <a name="testing-the-model"></a>Model test etme

1. Sol panelde, "Günlük iletişim kutuları" sonra "Yeni günlük iletişim kutusu" düğmesine tıklayın.
2. Burada yazacaktır "Yazın, iletinizi...", sohbet panelinde türü "hi"
3. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Lars" yazın
4. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "555-555-5555" yazın
5. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Seattle" yazın
    - Bu noktada konumu hariç tüm varlık değerlerini korunurken.
6. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "hello" yazın
7. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Detroit" yazın
8. "Oturum zaman aşımı" düğmesine tıklayın.
    - Bu düğmeye tıklandığında botun yanıt etkin olmadığı dönemler uzun uygular
9. "Tamam" düğmesine tıklayın.
10. "Test Bitti" düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API çağrıları](./14-api-calls.md)
