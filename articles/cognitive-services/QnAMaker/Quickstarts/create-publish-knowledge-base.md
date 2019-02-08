---
title: Bilgi bankası oluşturma, eğitme ve yayımlama - Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu örnekteki Soru-Cevap Oluşturma KB BitLocker anahtarı kurtarma sorularının yer aldığı basit bir SSS web sayfasından oluşturulmuştur.
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: diberry
ms.openlocfilehash: 2ac6e6fcd73abddcee668b8f73184b923aeab5d3
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55877006"
---
# <a name="create-train-and-publish-your-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi bankanızı oluşturma, eğitme ve yayımlama

SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu örnekteki Soru-Cevap Oluşturma KB BitLocker anahtarı kurtarma sorularının yer aldığı basit bir SSS web sayfasından oluşturulmuştur.

## <a name="prerequisite"></a>Önkoşul

> [!div class="checklist"]
> * Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-qna-maker-knowledge-base"></a>Soru-Cevap Oluşturma bilgi bankası oluşturma

1. Azure kimlik bilgilerinizi kullanarak QnAMaker.ai adresinde oturum açın.

2. Soru-Cevap Oluşturma web sitesinde **Create a knowledge base** (Bilgi bankası oluştur) öğesini seçin.

   ![Yeni KB oluşturma](../media/qna-maker-create-kb.png)

3. **Create** (Oluştur) sayfasındaki 1. adımda **Create a QnA service** (Soru-Cevap hizmeti oluştur) öğesini seçin. Aboneliğinizde bir Soru-Cevap Oluşturma hizmeti ayarlamak için [Azure portala](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) yönlendirilirsiniz. Azure portal zaman aşımına uğrarsa sitede **Try again** (Yeniden deneyin) öğesini seçin. Bağlantı kurulduktan sonra Azure panonuz görünür.

4. Azure'da yeni bir Soru-Cevap Oluşturma hizmetini başarıyla oluşturduktan sonra qnamaker.ai/create sayfasına geri dönün. 2. adımdaki açılan listelerden Soru-Cevap hizmetinizi seçin. Yeni bir Soru-Cevap hizmeti oluşturduysanız sayfayı yenilemeyi unutmayın.

   ![Soru-Cevap hizmeti KB seçme](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. 3. adımda KB'nize **My Sample QnA KB** adını verin.

6. KB'nize içerik eklemek için üç veri kaynağı türü seçin. 4. adımdaki **Populate your KB** (KB'nizi doldurun) bölümünde **URL** kutusuna [BitLocker Kurtarma Hakkında SSS](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) sayfasının URL'sini girin.

   ![Soru-Cevap hizmeti KB seçme](../media/qnamaker-quickstart-kb/add-datasources.png)

7. 5. adımda **Create your KB** (KB'nizi oluşturun) öğesini seçin.

8. KB oluşturulurken bir açılır pencere görüntülenir. Ayıklama işleminin HTML sayfasını okuyup soruları ve yanıtları tanımlaması birkaç dakika sürer.

9. KB başarıyla oluşturulduktan sonra **Knowledge base** (Bilgi bankası) sayfası açılır. Bu sayfada KB içeriğini düzenleyebilirsiniz.

10. Sağ üst köşeden **Add QnA pair** (Soru-Cevap çifti ekle) öğesine tıklayarak KB'nin **Editorial** (Düzenleme) bölümüne yeni bir satır ekleyin. **Question** (Soru) bölümüne **Hi** (Merhaba) yazın. **Answer** (Cevap) bölümüne **Hello. Ask me bitlocker questions.** (Merhaba. Bana BitLocker ile ilgili sorular sor.) yazın.

   ![Soru-Cevap çifti ekleme](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. Sağ üst köşeden **Save and train** (Kaydet ve eğit) öğesine tıklayarak yaptığınız düzenlemeleri kaydedin ve Soru-Cevap Oluşturma modelinizi eğitin. Kaydedilmeyen düzenlemeler silinir.

12. Sağ üst köşeden **Test**'i seçerek yaptığınız değişikliklerin geçerli olup olmadığını test edin. ENTER `hi there` kutusunda ve Enter'ı seçin. Yanıt olarak oluşturduğunuz cevabı almanız gerekir.

13. Yanıtı daha ayrıntılı bir şekilde incelemek için **Inspect** (Denetle) öğesini seçin. Test penceresini kullanarak KB'yi yayımlamadan önce yaptığınız değişiklikleri test edebilirsiniz.

   ![Test Paneli](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. **Test**'i tekrar seçerek **Test** penceresini kapatın.

15. **Edit** (Düzenle) öğesinin yanındaki menüde **Publish** (Yayımla) öğesini seçin. Ardından onaylamak için sayfadaki **Publish** (Yayımla) öğesini seçin.

16. Soru-Cevap Oluşturma hizmeti başarıyla yayımlandı. Uç noktayı uygulamanızda veya bot kodunuzda kullanabilirsiniz.

   ![Yayımlama](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](../How-To/create-knowledge-base.md)
