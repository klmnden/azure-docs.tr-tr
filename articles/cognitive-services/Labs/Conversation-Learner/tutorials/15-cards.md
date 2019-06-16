---
title: Konuşma Öğrenici modeliyle kartları kullanma hakkında bölüm 1 - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle kartları kullanmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: d0b87597c506aca5c3b4f6f3815f58656203ac3c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389513"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Kart (Kısım 1 / 2) kullanma

Bu öğreticide, ekleyebilir ve basit bir kart botunuzun içinde gösterilir.

> [!NOTE]
> Konuşma Öğrenici şu anda Bot başlatıldığı dizinde mevcut "kartları" adlı bir dizinde bulunmasını kart tanım dosyalarınızı bekliyor.

## <a name="video"></a>Video

[![Kartları öğretici Önizleme](https://aka.ms/cl_Tutorial_v3_Cards_Preview)](https://aka.ms/cl_Tutorial_v3_Cards)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Konuşmada bir seçenek belirlemek kullanıcının olanak tanıyan kullanıcı Arabirimi öğeleri kartlardır. 

### <a name="open-the-demo"></a>Tanıtım açın

Web kullanıcı Arabirimi "İçeri aktarma eğitimler" tıklayın ve "Öğreticisi-15-kartları" adlı modelini seçin.

### <a name="the-card"></a>Kart

Kart tanımıdır şu konumda: C:\<installedpath\>\src\cards\prompt.json.

Bu "kartları" dizininde kart tanımlarınızı bulmak sistem bekliyor.

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> "TextBlock" ve "{{Soru}}" yer tutucu metin alanına yazın gövdesi dikkat edin.
> İki gönderme düğmeler ve her biri için gönderilen metin vardır.

### <a name="actions"></a>Eylemler

Üç eylem oluşturduk. Aşağıda görebileceğiniz gibi bir kart olduğunda ilk eylem.

![](../media/tutorial13_actions.PNG)

Diyelim ki kart eylem türünü nasıl oluşturulduğuna bakın:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> Kartta üç farklı parametreler - soru giriş, düğme 1 ve 2 düğmesi bulunur. Bu kart şablonu başvurular soru ve yanıtlarının girebileceğiniz öğeleridir. Ayrıca, başvuru ve varlıkları ya da metin ve varlıkların bir karışımını kullanabilirsiniz.

Simgelerine kart nasıl göründüğünü gösterir.

### <a name="practicing-creating-card-actions"></a>Oluşturma kartı eylemleri uygulayan

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Kart" seçin "Eylem türü."
3. "Sor", "Şablon" listeden seçin.
4. "Soru" alanını "sola veya sağa Git" yazın.
5. "Button1" alanını "sol" yazın
6. "Sağ" "button2" alanına yazın
7. "İptal" düğmesine tıklayın.

### <a name="train-dialog-using-an-adaptive-card"></a>Uyarlamalı kartlar kullanarak iletişim eğitin

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Burada yazacaktır "Yazın, iletinizi...", sohbet panelinde türü "hi"
3. "Puan Eylemler" düğmesine tıklayın.
4. Yanıtı seçin "istemi: Soru: Sola veya sağa Git?"
    - Simgelerine kart Önizleme için kullanılabilir
5. Sohbet panelinde işlenmiş istemi "Sol" düğmesine tıklayın.
6. "Puan Eylemler" düğmesine tıklayın.
7. Yanıt, "Sol" seçin
8. "Kaydet" düğmesine tıklayın.
9. Yanıtı seçin "istemi: Soru: Sola veya sağa Git?"
10. Sohbet panelinde işlenmiş istemi "Sağ" düğmesine tıklayın.
11. "Puan Eylemler" düğmesine tıklayın.
12. Yanıt, "Sağ" seçin

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Karma Botlar](./16-hybrid-bots.md)
