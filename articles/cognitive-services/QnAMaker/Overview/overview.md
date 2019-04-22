---
title: Soru-Cevap Oluşturma nedir?
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma, kullanıcı tarafından yöneltilen doğal dil sorularına en iyi yanıtı vermek için özel makine öğrenimi zekası uygulayan bulut tabanlı bir API hizmetidir.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: overview
ms.date: 04/05/2019
ms.author: tulasim
ms.openlocfilehash: bafc39e7d9237fc7dd8469e5f9e97adb30355c8f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59257238"
---
# <a name="what-is-qna-maker"></a>Soru-Cevap Oluşturma nedir?

Soru-Cevap Oluşturma, verilerinizi temel alan bir sohbet ve soru-cevap katmanı oluşturan bulut tabanlı bir API hizmetidir. 

Soru-Cevap Oluşturma, Sık Sorulan Sorular (SSS) URL'leri, ürün kılavuzları, destek belgeleri ve özel soru-cevaplar gibi yarı yapılandırılmış içeriklerinizden bilgi bankası (KB) oluşturmanızı sağlar. Soru-Cevap Oluşturma hizmeti, kullanıcılarınızın doğal dil sorularını bilgi bankanızda bulunan en iyi cevapla eşleştirerek cevap verir.

Bu kullanımı kolay [web portalı](https://qnamaker.ai), geliştirici deneyimine sahip olmadan hizmetinizi oluşturmanızı, yönetmenizi, eğitmenizi ve yayımlamanızı sağlar. Hizmet bir uç noktada yayımlandıktan sonra sohbet botu gibi bir istemci uygulaması kullanıcıyla iletişim kurarak soruları alabilir ve cevap verebilir. 

![Genel Bakış](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Temel Soru-Cevap Oluşturma işlemleri

Soru-Cevap Oluşturma, verileriniz için iki temel hizmet sunar:

* **Ayıklama**: Yapılandırılmış soru-cevap verilerin yapılandırılmış & yarı yapılandırılmış [veri kaynakları](../Concepts/data-sources-supported.md) SSS ve ürün kılavuzlarını ister. Bu ayıklama işlemi, KB [oluşturma](https://aka.ms/qnamaker-docs-createkb) sırasında veya daha sonra düzenleme işlemi sırasında gerçekleştirilebilir.

* **Eşleşen**: Bilgi bankanızı silindikten sonra [eğitim ve test](https://aka.ms/qnamaker-docs-trainkb), size [yayımlama](https://aka.ms/qnamaker-docs-publishkb) bu. Bu sayede Soru-Cevap Oluşturma bilgi bankanızda, botunuzda veya istemci uygulamanızda kullanabileceğiniz bir uç nokta etkinleştirilir. Bu uç nokta kullanıcı sorularını kabul eder ve bilgi bankasındaki en iyi yanıtı verip eşleşme güvenilirlik puanını ekler.

```JSON
{
    "answers": [
        {
            "questions": [
                "How do I share a knowledge base with other?"
            ],
            "answer": "Sharing works at the level of a QnA Maker service, i.e. all knowledge bases in the services will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base)how to collaborate on a knowledge base.",
            "score": 70.95,
            "id": 4,
            "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
            "metadata": []
        }
    ]
}

```

## <a name="qna-maker-architecture"></a>Soru-Cevap Oluşturma mimarisi

Soru-Cevap Oluşturma mimarisi aşağıdaki iki bileşenden oluşur:

1. **Soru-cevap Oluşturucu Yönetim Hizmetleri**: Soru-cevap Oluşturucu güncelleştiriliyor, eğitim ve yayımlama ilk oluşturma içeren Bilgi Bankası için yönetim deneyimini yaşayın. Bu etkinlikler [portal](https://qnamaker.ai) veya [yönetim API'leri](https://aka.ms/qnamaker-v4-apis) üzerinden gerçekleştirilebilir. 

2. **Soru-cevap Oluşturucu veri ve çalışma zamanı**: Bu, Azure aboneliğinizdeki, belirtilen bölgede dağıtılır. KB içeriğiniz [Azure Search](https://azure.microsoft.com/services/search/) içinde depolanır ve uç nokta bir [App service](https://azure.microsoft.com/services/app-service/) olarak dağıtılır. Analiz için [Application Insights](https://azure.microsoft.com/services/application-insights/) kaynağı olarak dağıtmayı da seçebilirsiniz.

![Mimari](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Hizmetle ilgili önemli noktalar

- Eksiksiz bir **Kodsuz** için deneyimi [bir bot oluşturulabilir](../Quickstarts/create-publish-knowledge-base.md#create-a-bot) gelen Bilgi Bankası.
- **Tahminler için ağ kapasitesi azaltma gerçekleştirilmez**. İşlem sayısı için değil barındırma ve hizmet için ödeme yapın. Daha ayrıntılı bilgi edinmek için [fiyatlandırma sayfasına](https://aka.ms/qnamaker-docs-pricing) bakın.
- **İstediğinizde ölçeklendirebilirsiniz**. Senaryonuza uygun bileşenlerin ilgili SKU'larını seçin. Soru-Cevap Oluşturma hizmetinizi için [kapasiteyi nasıl belirleyeceğinizi](https://aka.ms/qnamaker-docs-capacity) öğrenin.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma hizmeti oluşturma](../how-to/set-up-qnamaker-service-azure.md)
