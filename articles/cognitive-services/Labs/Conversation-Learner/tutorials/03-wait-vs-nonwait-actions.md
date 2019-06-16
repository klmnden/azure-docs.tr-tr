---
title: Konuşma Öğrenici modeliyle - Microsoft Bilişsel hizmetler bekleyin ve bekleme olmayan eylemlerini kullanma | Microsoft Docs
titleSuffix: Azure
description: Konuşma Öğrenici modeliyle bekleyin ve bekleme olmayan Eylemler kullanmayı öğrenin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ms.openlocfilehash: 57a58b008792130c82a86ff4a5801f4bb39aabad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66387901"
---
# <a name="wait-and-non-wait-actions"></a>Bekleyin ve eylemlerin-wait

Bu öğretici konuşma Öğrenici bekleme Eylemler ve bekleme olmayan Eylemler arasındaki farkı gösterir.

## <a name="video"></a>Video

[![VS Non-Wait öğretici Önizleme bekleyin](https://aka.ms/cl_Tutorial_v3_WaitnonWait_Preview)](https://aka.ms/cl_Tutorial_v3_WaitnonWait)

## <a name="requirements"></a>Gereksinimler
Bu öğreticide, genel öğretici bot çalışıyor olması gerekir

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

- Eylem bekleyin: Sistem "wait" eylemi gerçekleştirdikten sonra Eylemler almayı durdurmak ve kullanıcı girişini bekleme.
- Non-wait eylem: Sistem "olmayan-wait" eylemi gerçekleştirdikten sonra (kullanıcı girişi bekleniyor olmadan) bunu hemen başka bir eylem seçersiniz.

## <a name="steps"></a>Adımlar

### <a name="create-a-new-model"></a>Yeni model oluştur

1. Web kullanıcı Arabiriminde, yeni bir modele tıklayın
2. "Name" alanında "bekleyin Non-Wait" yazın, isabet girin ya da "Oluştur" düğmesine tıklayın.

### <a name="create-the-first-two-wait-actions"></a>İlk iki bekleyin eylem oluşturma

1. Sol panelde, "Eylemler" sonra "Yeni Eylem" düğmesine tıklayın.
2. "Botun yanıt..." alanı, "hangi pizza ister misiniz?" yazın.
    - Bu bir bekleme eylemi, bu nedenle "Yanıt için beklemek" kutusunun işaretini.
3. "Oluştur" düğmesine tıklayın.
4. Bu adımları yineleyerek, "Pizza şekilde!" ile başka bir eylem oluşturun Botun yanıt.

### <a name="train-using-those-wait-actions"></a>Bu bekleyin eylemlerini kullanma eğitimi

1. Sol panelde, "İletişim kutuları eğitme" ve ardından "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. Sohbet panelinde nerede yazacaktır "Yazın, iletinizi...", "Hi" yazın. 
    - Bu, konuşma kullanıcı tarafı benzetimini yapar.
3. "Puan Eylemler" düğmesine tıklayın.
4. "Hangi pizza istiyor musunuz?" yanıt'ı seçin.
5. "Margherita," kullanıcı olarak yanıtlayın.
6. "Puan Eylemler" düğmesine tıklayın.
7. Yanıt, "Pizza şekilde!"'i seçin.
8. "Kaydet" düğmesine tıklayın.

### <a name="create-a-non-wait-action-while-training"></a>Eğitim sırasında bir bekleme olmayan Eylem oluştur
Daha önce yaptığınız gibi olmayan-Wait eylem oluşturabilirsiniz ancak ondan bir eğitim oturumu içinde de oluşturabilirsiniz.
1. "Yeni Train iletişim kutusu" düğmesine tıklayın.
2. İçinde "Hello" kullanıcı türü olarak.
3. "Puan Eylemler" düğmesine tıklayın.
4. Tıklayın "+ eylem" düğmesi. 
    - Bu bilinen "bir Oluştur eylemi" iletişim kutusuna götürür.
5. "Pizza Robotu hoşgeldiniz!" olarak Botun yanıt yazın
6. Kaldırma-"Yanıt için beklemek" onay kutusu denetimi.
7. "Oluştur" düğmesine tıklayın.
    - Hemen ile Bot yanıt "Hoş Geldiniz, Pizza Robota!" dikkat edin. ve yeniden başladığınız için başka bir Bot yanıt istenir. Yeni oluşturduğumuz Non-Wait eylem Botun yanıt: olmasıdır.
9. "Hangi pizza istiyor musunuz?" yanıt'ı seçin.
10. "Margherita," kullanıcı olarak yanıtlayın.
11. "Puan Eylemler" düğmesine tıklayın.
12. Yanıt, "Pizza şekilde!"'i seçin.
13. "Kaydet" düğmesine tıklayın.

> [!NOTE]
> Bot yanıtları bakımından bekleyin ve bekleme olmayan Eylemler dizisi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Varlıkları giriş](./04-introduction-to-entities.md)
