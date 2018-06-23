---
title: Bilgi Bankası - QnA Maker - Azure Bilişsel hizmetler test etme | Microsoft Docs
description: Bilgi Bankası yayımlamadan önce test edin.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: cffb63666edab25e1b3b0739d0e0f2f828600f3a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354095"
---
# <a name="test-your-knowledge-base"></a>Bilgi Bankası test

QnA Maker Bilgi Bankası sınama, döndürülen yanıtları doğruluğunu artırmak için yinelemeli süreç önemli bir parçasıdır. Bilgi Bankası'de düzenlemeleri yapın sağlar bir Gelişmiş sohbet arabirim üzerinden test edebilirsiniz.

## <a name="test-answer-matching"></a>Test yanıt eşleştirme

1.  Bilgi Bankası üzerinde adını seçerek erişim **My Bilgi Bankası** sayfası.
2.  Test slayt çıkış paneli erişmek için seçin **Test** uygulamanızın üst panelinde.

    ![Erişim sınaması](../media/qnamaker-how-to-test-kb/access-test.png)

3.  Metin kutusuna bir sorgu girin ve Enter seçin.

4.  Bilgi Bankası'nden en iyi eşleşen yanıtlama yanıt olarak döndürülür.

## <a name="clear-test-panel"></a>Clear test paneli

Tüm girilen test sorgular ve sonuçları test konsolundan temizlemek için seçin **baştan başlamak** Test bölmenin sol üst köşesindeki.

## <a name="close-test-panel"></a>Kapat test paneli

Test paneli kapatmaya seçin **Test** yeniden düğmesine tıklayın. Test paneli açık olsa da, Bilgi Bankası içeriğini düzenleyemezsiniz.

## <a name="inspect-score"></a>Puan inceleyin.

Test sonucu İncele panelinde ayrıntılarını inceleyin.

1.  Test slayt çıkış paneli açık seçin **incele** bu yanıtı hakkında daha fazla ayrıntı için.

    ![Yanıtları inceleyin.](../media/qnamaker-how-to-test-kb/inspect.png)

2.  İncelemesini panelinde görüntülenir. Bölmenin hedefi olarak tanımlanan herhangi bir varlık Puanlama üst içerir. Bölmenin seçili utterance sonucunu gösterir.

## <a name="correct-the-top-scoring-answer"></a>Yanıt Puanlama üst düzeltin

Yanıt Puanlama üst yanlış ise, doğru yanıt listesi ve select seçin **kaydedin ve eğitmek**.

![Erişim sınaması](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Diğer sorular ekleyin

Belirli bir yanıt bir soru alternatif forms ekleyebilirsiniz. Bunları eklemek için diğer'i tıklatın ve metin kutusu içinde yanıt türü girin. Seçin **kaydedin ve eğitmek** güncelleştirmeleri depolamak için.

![Erişim sınaması](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Yeni bir yanıt Ekle

Yanıt Bilgi Bankası (KB cinsinden iyi eşleşme bulundu) mevcut değil ya da eşleştirildiklerinden varolan yanıtlar yanlış olan yeni bir yanıt ekleyebilirsiniz. Geçerli soru yeni yanıt metin kutusuna girin ve eklemek için enter tuşuna basın. 

Seçin **kaydedin ve eğitmek** bu yanıt kalıcı hale getirmek için. Yeni bir soru-yanıt çifti şimdi Bilgi Bankası'na eklendi.

![Erişim sınaması](../media/qnamaker-how-to-test-kb/add-answer.png)

> [!NOTE]
> Tuşuna bastığınızda, Bilgi Bankası yapılan düzenlemelerin yalnızca kaydedileceği **kaydedin ve eğitmek** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası yayımlama](./publish-knowledge-base.md)
