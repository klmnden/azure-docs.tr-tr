---
title: Bilgi Bankası - soru-cevap Oluşturucu test etme
titlesuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu bankanızı test etme, verilen yanıtları doğruluğunu artırmak için bir süreçtir önemli bir parçasıdır. Bilgi Bankası düzenlemeler yapmak da sağlayan gelişmiş sohbet arabiriminden test edebilirsiniz.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/17/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 20ebcb502e03f2d817fe18624d8c790e920c667f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61372043"
---
# <a name="test-your-knowledge-base-interactively-in-qna-maker"></a>Bilgi bankanızı etkileşimli soru-cevap Oluşturucu test edin

Soru-cevap Oluşturucu bankanızı test etme, verilen yanıtları doğruluğunu artırmak için bir süreçtir önemli bir parçasıdır. Bilgi Bankası düzenlemeler yapmak da sağlayan gelişmiş sohbet arabiriminden test edebilirsiniz.

## <a name="test-answer-matching"></a>Test yanıt eşleştirme

1.  Adını seçerek bilgi bankanızı erişim **My bilgi bankalarından** sayfası.
2.  Test slayt genişletme paneline erişmek için seçin **Test** uygulamanızın üst panelinde.

    ![Erişim Test paneli](../media/qnamaker-how-to-test-kb/access-test.png)

3.  Metin kutusuna bir sorgu girin ve Enter tuşuna basın.

4.  Bilgi Bankası'nden en iyi eşleşen yanıtlama yanıt döndürülür.

## <a name="clear-test-panel"></a>Açık test paneli

Tüm girilen test sorgular ve sonuçları test konsolundan temizlemek için seçin **baştan** Test panelinin sol üst köşesinde.

## <a name="close-test-panel"></a>Kapat test paneli

Test paneli kapatmak için seçin **Test** düğmesini tekrar. Bilgi Bankası içerikleri, Test panel açıkken düzenleyemezsiniz.

## <a name="inspect-score"></a>Puan inceleyin

İnceleyin panelinde test sonucunun ayrıntılarını inceleyin.

1.  Test slayt çıkış panelini Aç seçin **inceleyin** bu yanıtı hakkında daha fazla bilgi.

    ![Yanıtları](../media/qnamaker-how-to-test-kb/inspect.png)

2.  İnceleme paneli görüntülenir. Panelin amacını ve bunun yanı sıra tanımlanan herhangi bir varlık Puanlama üst içerir. Paneli, seçilen utterance sonucunu gösterir.

## <a name="correct-the-top-scoring-answer"></a>Yanıt Puanlama üst düzeltin

Yanıt Puanlama üst yanlışsa, doğru yanıtı listesi ve select seçin **kaydedin ve eğitme**.

![Yanıt Puanlama üst düzeltin](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Diğer sorular ekleyin

Belirli bir yanıt soru diğer formlara ekleyebilirsiniz. Diğer tıklayın ve metin kutusu içinde yanıt türü bunları eklemek için enter. Seçin **kaydedin ve eğitme** güncelleştirmeleri depolamak için.

![Diğer sorular ekleyin](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Yeni bir yanıt Ekle

Yanıt Bilgi Bankası (KB iyi eşleşme bulundu) mevcut değil veya eşleştirilmiş olan mevcut yanıtlar yanlış olan, yeni bir yanıt ekleyebilirsiniz. Yeni geçerli sorusuna yanıt metin kutusuna girin ve eklemek için enter tuşuna basın. 

Seçin **kaydedin ve eğitme** bu yanıt kalıcı hale getirmek için. Yeni bir soru-cevap çifti şimdi Bilgi Bankası'na eklendi.

![Yeni bir soru ve yanıt çifti Ekle](../media/qnamaker-how-to-test-kb/add-answer.png)

> [!NOTE]
> Tuşuna bastığınızda tüm düzenlemeleri bilgi bankanızı yalnızca kaydedileceği **kaydedin ve eğitme** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası yayımlama](./publish-knowledge-base.md)
