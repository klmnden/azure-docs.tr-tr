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
ms.date: 05/08/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 2c596b49d5587b07fe75cefde72e897478dc3dc8
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472065"
---
# <a name="test-your-knowledge-base-interactively-in-qna-maker"></a>Bilgi bankanızı etkileşimli soru-cevap Oluşturucu test edin

Soru-cevap Oluşturucu bankanızı test etme, verilen yanıtları doğruluğunu artırmak için bir süreçtir önemli bir parçasıdır. Bilgi Bankası düzenlemeler yapmak da sağlayan gelişmiş sohbet arabiriminden test edebilirsiniz.

## <a name="test-answer-matching"></a>Test yanıt eşleştirme

1. Adını seçerek bilgi bankanızı erişim **My bilgi bankalarından** sayfası.
1. Test slayt genişletme paneline erişmek için seçin **Test** uygulamanızın üst panelinde.
1. Metin kutusuna bir sorgu girin ve Enter tuşuna basın.
1. Bilgi Bankası'nden en iyi eşleşen yanıtlama yanıt döndürülür.

## <a name="clear-test-panel"></a>Açık test paneli

Tüm girilen test sorgular ve sonuçları test konsolundan temizlemek için seçin **baştan** Test panelinin sol üst köşesinde.

## <a name="close-test-panel"></a>Kapat test paneli

Test paneli kapatmak için seçin **Test** düğmesini tekrar. Bilgi Bankası içerikleri, Test panel açıkken düzenleyemezsiniz.

## <a name="inspect-score"></a>Puan inceleyin

İnceleyin panelinde test sonucunun ayrıntılarını inceleyin.

1.  Test slayt çıkış panelini Aç seçin **inceleyin** bu yanıtı hakkında daha fazla bilgi.

    ![Yanıtları](../media/qnamaker-how-to-test-kb/inspect.png)

2.  İnceleme paneli görüntülenir. Panelin amacını ve bunun yanı sıra tanımlanan herhangi bir varlık Puanlama üst içerir. Paneli, seçilen utterance sonucunu gösterir.

## <a name="correct-the-top-scoring-answer"></a>Yanıt Puanlama üst düzeltin

Yanıt Puanlama üst yanlışsa, doğru yanıtı listesi ve select seçin **kaydedin ve eğitme**.

![Yanıt Puanlama üst düzeltin](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Diğer sorular ekleyin

Belirli bir yanıt soru diğer formlara ekleyebilirsiniz. Diğer tıklayın ve metin kutusu içinde yanıt türü bunları eklemek için enter. Seçin **kaydedin ve eğitme** güncelleştirmeleri depolamak için.

![Diğer sorular ekleyin](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Yeni bir yanıt Ekle

Yanıt Bilgi Bankası (KB iyi eşleşme bulundu) mevcut değil veya eşleştirilmiş olan mevcut yanıtlar yanlış olan, yeni bir yanıt ekleyebilirsiniz. 

Yanıtlar listenin altındaki metin kutusuna yeni bir yanıt girmeye kullanın ve eklemek için enter tuşuna basın. 

Seçin **kaydedin ve eğitme** bu yanıt kalıcı hale getirmek için. Yeni bir soru-cevap çifti şimdi Bilgi Bankası'na eklendi. 

> [!NOTE]
> Tuşuna bastığınızda tüm düzenlemeleri bilgi bankanızı yalnızca kaydedileceği **kaydedin ve eğitme** düğmesi.

## <a name="test-the-published-knowledge-base"></a>Yayımlanan Bilgi Bankası test

Test Bölmesi'nde Bilgi Bankası yayımlanan sürümünü test edebilirsiniz. KB yayımladıktan sonra seçin **yayımlanan KB** kutusu ve yayımlanan KB sonuçları elde etmek için bir sorgu gönderin.

![Yayımlanan bir KB karşı test etme](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası yayımlama](./publish-knowledge-base.md)
