---
title: Tanıtım konuşma Öğrenici modeli, parola sıfırlama - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Bir tanıtım konuşma Öğrenici modeli oluşturmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 15aa3a8346087908cf77f1f68db916cc2c184448
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389867"
---
# <a name="demo-password-reset"></a>Tanıtım: Parola sıfırlama
Bu öğreticide, konuşma Learner tarafından desteklenen parola sıfırlama ile yardımcı olabilecek bir basit teknik destek bot gösterir. Önemsiz iletişim akışları ve çok etkinleştirin botun modeli edinebilirsiniz çıkış, etki alanı sınıfları da dahil olmak üzere dizileri. Görev, kod veya varlıkları gerçekleştirilebilir.

## <a name="video"></a>Video

[![Tanıtım parola Önizleme](https://aka.ms/cl_Tutorial_v3_DemoPassword_Preview)](https://aka.ms/cl_Tutorial_v3_DemoPassword)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, parola sıfırlama bot çalışıyor olması gerekir

    npm run demo-password

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi Model listesi, öğretici tanıtım parola sıfırlama tıklayın. 

### <a name="actions"></a>Eylemler

Model bir dizi ortak parola sorunları çözmesine yardımcı olmak için tasarlanan eylemi içerir.

![](../media/tutorial_pw_reset_actions.PNG)

### <a name="training-dialogs"></a>Eğitim iletişim kutuları

Birkaç Train dışında etki alanı sınıfı eğitim gösteren bazı dahil olmak üzere iletişim kutuları, modeli de içerir. Örneğin, yol tarifi isteyebilir kullanıcılar gibi. Örnek bot tanıtım amacıyla birkaç üzerinde geliştirilen ve onu bildiren tarafından yanıt basitçe "ile yardımcı olamaz." Mevcut eğitme iletişim kutuları listesini sol bölmede bulunan "Train iletişim kutuları altında" bulunur.

![](../media/tutorial_pw_reset_entities.PNG)

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "i parolamı kaybolur." türü
3. "Puan Eylemler" düğmesine tıklayın.
4. ", Yerel hesap veya Microsoft hesabı için mi?" yanıt seçin
5. Burada yazacaktır "Yazın, iletinizi...", sohbet panelinde yazın "Lütfen yerel hesap"
6. "Puan Eylemler" düğmesine tıklayın.
7. "Windows hangi sürümünü kullanıyorsunuz?" yanıt seçin
8. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "windows XP'de" türü
9. "Puan Eylemler" düğmesine tıklayın.
10. Tıklayın "+ eylem" düğmesi.
11. "Botun yanıtta..." alanına "çözümü: Windows XP parola sıfırlama nasıl …"
12. "Oluştur" düğmesine tıklayın.

### <a name="training-dialogs-for-out-of-domain-scenarios"></a>Çıkış, etki alanı senaryoları için eğitim iletişim kutuları

1. Sol panelde "İletişim kutuları eğitme" ve ardından mevcut "Oyuncak depoları" tıklayın Train iletişim.
2. Sohbet Masası'ndaki "çocuğunun stores" utterance tıklayın.
3. Web türü "arama" ve isabet "alternatif giriş Ekle" alanına girin.
4. Uçuş türü "kayıt" ve isabet "alternatif giriş Ekle" alanına girin.
5. "Değişiklikleri Kaydet" düğmesine tıklayın.
6. "Kaydet Düzenle" düğmesine tıklayın.
7. Sol panelde, "Günlük iletişim kutuları" sonra "Yeni günlük iletişim kutusu" düğmesine tıklayın.
8. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "i parolamı bulunamıyor" türü
9. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Microsoft hesabı" yazın
10. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "teşekkür ederiz" yazın
11. "Test Bitti" düğmesine tıklayın.
12. "İ parolamı bulunamıyor" günlük iletişim kutusundan kılavuz görünümüne tıklayın.
13. Sohbet panelinde yanlış işlenen tıklayın "çözümü: Bir Microsoft hesabı parola sıfırlama nasıl"yanıtı.
14. Tıklayın "+ eylem" düğmesi.
15. "Botun yanıt..." alanı, "Hoş Geldiniz" yazın
16. "Oluştur" düğmesine tıklayın.
17. "Kaydet olarak Train iletişim" düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Demo - pizza sırası](./demo-pizza-order.md)
