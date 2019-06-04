---
title: Yaklaşım analizi
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, pozitif, negatif ve nötr yaklaşım konuşma alma gösteren bir uygulama oluşturun. Yaklaşım konuşmanın tamamından belirlenir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: diberry
ms.openlocfilehash: 1408e29793fdac77b89e3f0cc0a7be525f7fa1d2
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479786"
---
# <a name="tutorial--get-sentiment-of-utterance"></a>Öğretici:  Utterance duyarlılığını Al

Bu öğreticide, pozitif, negatif ve nötr elde edilen konuşma duyarlılığı belirlemek nasıl erişileceğini gösteren bir uygulama oluşturun. Yaklaşım konuşmanın tamamından belirlenir.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Yeni bir uygulama oluşturma
> * Yaklaşım analizini yayımlama ayarı olarak ekleyin
> * Uygulamayı eğitme
> * Uygulama yayımlama
> * Konuşmanın yaklaşımını uç noktasından alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="sentiment-analysis-is-a-publish-setting"></a>Yaklaşım analizi Yayımla ayardır

Aşağıdaki konuşmalarda yaklaşım örnekleri gösterilmektedir:

|Yaklaşım|Puan|İfade|
|:--|:--|:--|
|pozitif|0,91 |John W. Smith did a great job on the presentation in Paris. (John W. Smith, Paris'teki sunumda harika bir iş çıkardı.)|
|pozitif|0,84 |Seattle mühendislerin Parker satış konuşmasından harika işlere vermedi.|

Yaklaşım analizi, tüm konuşmalar için geçerli olan bir yayımlama ayarıdır. Yaklaşımını gerçek utterance belirten sözcükleri bulmak ve bunları işaretleyin gerekmez. 

Yayımlama ayarı olduğu için amaçlar veya varlıklar sayfalarında görmezsiniz. [Etkileşimli test](luis-interactive-test.md#view-sentiment-results) bölmesinde veya uç nokta URL'sinde test yaparken görebilirsiniz. 


## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="add-personname-prebuilt-entity"></a>PersonName ekleme önceden oluşturulmuş varlık 

1. Seçin **derleme** Gezinti menüsünde.

1. Sol gezinti menüsünden **Entities** (Varlıklar) öğesini seçin.

1. **Add prebuilt entity** (Önceden oluşturulan varlık ekle) düğmesini seçin.

1. Aşağıdaki varlık önceden oluşturulmuş varlıklarla listesinden ardından seçin **Bitti**:

   * **[PersonName](luis-reference-prebuilt-person.md)** 

     ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/add-personname-prebuilt-entity.png)

## <a name="create-an-intent-to-determine-employee-feedback"></a>Çalışan geri bildirim belirlemek için bir hedefi oluşturma

Şirket üyelerinin çalışanlar hakkındaki geri bildirimini yakalamak için yeni bir amaç ekleyin. 

1. Sol panelden **Intents** (Amaçlar) öğesini seçin.

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin.

1. Yeni amaca `EmployeeFeedback` adını verin.

    ![EmployeeFeedback adı görünen Create new intent (Yeni amaç oluştur) iletişim kutusu](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent-ddl.png)

4. Bir çalışanın iyi yaptığı bir işi veya geliştirilmesi gereken bir alanı belirten birden fazla konuşma ekleyin:

    |Konuşmalar|
    |--|
    |John Smith açısında Karşılama arka Doğum ayrılma yapan bir iş arkadaşınız vermedi|
    |Jill Jones, üzüntüyü, her zaman içinde bir iş arkadaşınız comforting için harika bir iş vermedi.|
    |Bob Barnes belgeler için gerekli tüm fatura yoktu.|
    |Todd Thomas gerekli formları ayda hiçbir imzalarla geç açık.|
    |Önemli pazarlama şirket dışı toplantıya Katherine Kelly hale gelmedi.|
    |Gamze Dillard toplantı Haziran incelemeleri için eksik.|
    |Satış konuşmasından Harvard adresindeki işareti Mathews rocked|
    |Harika bir iş üzerinde Stanford sunuyu Walter Williams vermedi|

    Seçin **görüntüleme seçenekleri**seçin **varlık değerlerini göster** adlarını görmek için.

    [![Örnek konuşma EmployeeFeedback hedefi olarak ile ekran görüntüsü, LUIS uygulaması](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png#lightbox)

## <a name="add-example-utterances-to-the-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulama hedefi değişiklikleri test edilebilir şekilde eğitme 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Uygulamayı yaklaşım analizi içerecek şekilde yapılandırma

1. Sağ üst gezinti bölmesinden **Yönet**'i seçin, sonra sol menüden **Yayımlama ayarları**'nı seçin.

1. Seçin **yaklaşım analizi, bir kullanıcının utterance pozitif, negatif veya nötr olup olmadığını belirlemek için kullanın.** Bu ayarı etkinleştirmek için. 

    ![Yayımlama ayarı olarak üzerinde yaklaşım analizi Aç](./media/luis-quickstart-intent-and-sentiment-analysis/turn-on-sentiment-analysis-as-publish-setting.png)

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitilen modelin uç noktasından sorgulanabilir, bu nedenle, uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-the-sentiment-of-an-utterance-from-the-endpoint"></a>Uç noktasından bir utterance duyarlılığını Al

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Adres URL'SİNİN sonuna gidin ve aşağıdaki utterance girin:

    `Jill Jones work with the media team on the public portal was amazing` 

    Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `EmployeeFeedback` amacını yaklaşım analizi ayıklanmış şekilde döndürmelidir.
    
    ```json
    {
      "query": "Jill Jones work with the media team on the public portal was amazing",
      "topScoringIntent": {
        "intent": "EmployeeFeedback",
        "score": 0.9616192
      },
      "intents": [
        {
          "intent": "EmployeeFeedback",
          "score": 0.9616192
        },
        {
          "intent": "None",
          "score": 0.09347677
        }
      ],
      "entities": [
        {
          "entity": "jill jones",
          "type": "builtin.personName",
          "startIndex": 0,
          "endIndex": 9
        }
      ],
      "sentimentAnalysis": {
        "label": "positive",
        "score": 0.8694164
      }
    }
    ```

    % 86'lık bir puan sentimentAnalysis pozitiftir. 

    Başka bir utterance değeri kaldırarak deneyin `q` tarayıcının adres çubuğuna: `William Jones did a terrible job presenting his ideas.` Düşük bir puan döndürerek yaklaşım puanını negatif yaklaşımı gösterir `0.18597582`.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* Yaklaşım analizi Bilişsel hizmet tarafından sağlanan [metin analizi](../Text-Analytics/index.yml). Metin analizi için sınırlı bir özelliktir [desteklenen diller](luis-language-support.md##languages-supported).
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUIS portalında test etme](luis-interactive-test.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğretici, bir bütün olarak konuşmadan yaklaşım değerleri ayıklamak için bir yayımlama ayarı olarak yaklaşım analizi ekler.

> [!div class="nextstepaction"] 
> [İK uygulamasında uç nokta konuşmasını gözden geçirme](luis-tutorial-review-endpoint-utterances.md) 

