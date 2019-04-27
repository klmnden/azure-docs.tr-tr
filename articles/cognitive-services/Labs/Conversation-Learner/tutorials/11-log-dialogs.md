---
title: Nasıl iletişim kutuları log bir konuşma Öğrenici modeli - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: İletişim kutuları bir konuşma Öğrenici modelde günlüğe kaydetme hakkında bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e3a84bfa643ebe74983bcef0d0ea72c701ffa589
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707531"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>İletişim kutuları bir konuşma Öğrenici modelde günlüğe kaydetme hakkında

Bu öğreticide nasıl günlük iletişim kutuları daha iyi gerçek dünyada kullanıcıların kayıtlı etkileşim konuşma Öğrenici modelleri eğitmek için çalışan gösterilmektedir.

## <a name="video"></a>Video

[![Günlük iletişim kutuları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_LogDialogs_Preview)](https://aka.ms/cl_Tutorial_v3_LogDialogs)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

ve önceki öğreticilerdeki oluşturulan hava durumu modeli.

## <a name="details"></a>Ayrıntılar
Günlük, son kullanıcılar, botun etkileşime kaydedilen günlükleri kutularıdır. Bu günlük iletişim kutuları yararlanılarak varlık etiketleri ve modelin performans ve genel sistem performansını geliştirmek için eylem seçimleri düzeltebilirsiniz.

## <a name="steps"></a>Adımlar

Web kullanıcı Arabirimi "İçeri aktarma eğitimler" tıklayın ve "Öğreticisi-11-LogDialogs" adlı modelini seçin.

Eylemler vazgeçilmez hava durumu, şehirde hakkında yanıt vermek için tasarlanmış ve bu modeli "Şehir" adlı bir varlık içeriyor. Eğitim iki iletişim kutuları, performans beklentilerini biraz düşük olacak şekilde modeli eğitmek için kullanıldı. Ek eğitim ve gerçek hayatta Kullanıcı etkileşimlerine maruz kalma riskinizi modeli geliştirmek.

### <a name="create-a-new-conversation"></a>Yeni bir konuşma oluşturma

1. Sol panelde, "Günlük iletişim kutuları" sonra "Yeni günlük iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Austin hava durumu tahminini" yazın
3. "Test Bitti" düğmesine tıklayın.
4. Listeden "Austin hava durumu tahminini" günlük iletişim tıklayın.
5. Sohbet panelinde "Austin hava durumu tahminini" utterance tıklayın.
6. "Austin" ı "city" varlık listesinden tıklatın.
7. "Değişiklikleri Gönder" düğmesine tıklayın.
    - Biz yeni varlık değerlerini belleğe sahip olduğundan varlık değeri bu değişikliğe neden konuşma aşağı akış değişiklikler. Sonraki eylemler büyük olasılıkla geçersiz hale özellikle olanları "city" varlığı içeren.
8. Tıklayın "City hangi?" Sohbet panelinde utterance.
9. Yanıtı seçin, "Austin, hava durumu, büyük olasılıkla güneşli sağlıyor."
10. "Kaydet olarak Train iletişim" düğmesine tıklayın.
    - Eğitim hemen başlatılır

![](../media/T11_logdialog.png)

Son unutmayın. İş gereksinimlerinize bağlı olarak konuşma günlüğe kaydetme özelliğinin ayarlarını ve seçimini kaldırarak "günlük konuşmaları." giderek kapatılabilir

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlık algılama geri çağırma](./12-entity-detection-callback.md)
