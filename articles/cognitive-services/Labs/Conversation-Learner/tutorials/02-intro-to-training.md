---
title: Microsoft Bilişsel hizmetler konuşma Öğrenici modeli - giriş | Microsoft Docs
titleSuffix: Azure
description: Dallanma ve konuşma Öğrenici aracılığıyla önceki eğitim düzenleme gibi bir model eğitip öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1e6dea521207ec00efe948a879c705bd28a75d57
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60707640"
---
# <a name="introduction-to-training"></a>Eğitim giriş

Bu öğreticide, bir önceki eğitim ve değiştirmek için robot yanıt düzenleme göre yeni bir eğitim kapalı dallanma modeli, temelleri gösterilir.

## <a name="video"></a>Video

[![Öğretici Önizleme Eğitim giriş](https://aka.ms/cl_Tutorial_v3_IntroTraining_Preview)](https://aka.ms/cl_Tutorial_v3_IntroTraining)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

- Eylemler: Kullanıcı girişi için robot yanıtı.
- Eğitimi: Yol, biz kullanıcı girişine yanıt vermek için bir Bot öğretin.
- Dal oluşturma: Yeni bir eğitimi iletişim kutusu oluşturma amacıyla kaydedilmiş bir eğitimi iletişim içinde bir kullanıcı girişi değiştirilmesini özgün aynı çıkış başlatmaz, bunun farklı bir yönde konuşma alır.

## <a name="steps"></a>Adımlar

### <a name="create-a-new-model"></a>Yeni model oluştur

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. "Adı", "Bot ilham verin" yazın. Oluştur'a tıklayın.

### <a name="create-an-action"></a>Bir eylem oluşturun

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt" alanına, "Merhaba! Bugün ilham istiyor musunuz? ".
    - Diğer tüm alanlar ve onay kutuları, varsayılan ayarı bırakın.
3. Oluştur’a tıklayın.

### <a name="first-training-and-creating-another-action-while-training"></a>İlk eğitim ve eğitim sırasında başka bir eylem oluşturma

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "hello" yazın. 
    - Bu, konuşma kullanıcı tarafı benzetimini yapar.
3. "Eylemler puan"'a tıklayın.
4. "Merhaba! yanıt seçin Bugün ilham istiyor musunuz? ".
5. Bir kullanıcı olarak, "Evet" yanıt.
6. "Eylemler puan"'a tıklayın.
7. Tıklayın "+ eylem" düğmesi. 
    - Bu bilinen "bir Oluştur eylemi" iletişim kutusuna götürür.
8. "Harika işiniz,!"olarak Botun yanıt olarak yazın.
9. Oluştur’a tıklayın.
10. Bot hemen yanıt verdiğini dikkat edin.
11. "Kaydet" düğmesine tıklayın.

### <a name="branch-a-second-training-off-of-the-first-training"></a>Dal ilk eğitim dışına ikinci bir eğitim
1. İlk eğitim özetleyen kılavuz satırına tıklayın. 
    - Bu, görüntülemek ve var olan eğitim düzenlemenize olanak sağlar.
2. "Evet" Kullanıcı yanıtı üzerinde tıklayın. 
    - Bu düzenleme denetimleri açığa çıkarır.
3. Dal simgesine tıklayın. 
    - Bu yeni konuşma için farklı bir kullanıcı bir giriş için bir istem çıkarır.
4. Tür "Hayır" isabet girin veya "Oluştur" düğmesine tıklayın. 
    - Bu noktada Train iletişim yeni bir örneğini gerekir, ilkinin değişmeden kalır.
5. "Eylemler puan"'a tıklayın.
6. Yalnızca görünen Botun yanlış yanıtta tıklayın.
7. Tıklayın "+ eylem" düğmesi 
    - Böylece Bot ile yanıt vermesi için yeni bir eylem oluşturabiliriz.
8. "Olarak, sorun! Botun yanıtta yazın Harika bir günü olan:!"
9. Oluştur’a tıklayın
10. Bot hemen yanıt verdiğini dikkat edin.
11. "Kaydet" düğmesine tıklayın.

### <a name="test-the-trainings"></a>Test eğitimleri
1. Sol panelde, "Günlük iletişim kutuları" sonra "Yeni günlük iletişim kutusu" tıklayın.
2. İletide, "hi" yazın. 
3. Bot biz eğitilmiş şekilde otomatik olarak yanıt verir dikkat edin.
4. Kullanıcı yanıtı içinde "yes" yazın.
5. Bot yanıt fark ilk eğitim çalışıp çalışmadığını gösterir.
6. "Oturum zaman aşımı" düğmesine tıklayın. Bu konuşma Öğrenici yeniden başlamasını istiyoruz yalnızca gerçekleşen damıtarak konuşma bağlamında kullanılabilen kapatır yoksayılıyor bildirir.
7. İletide, "hi" yazın. 
8. Bot biz eğitilmiş şekilde otomatik olarak yanıt verir dikkat edin.
9. Kullanıcı yanıtı "Hayır" yazın.
10. Bot yanıt fark ikinci eğitim çalışıp çalışmadığını gösterir.
11. "Test Bitti" düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bekleyin ve eylemlerin-wait](./03-wait-vs-nonwait-actions.md)
