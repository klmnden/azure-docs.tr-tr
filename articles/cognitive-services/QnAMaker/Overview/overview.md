---
title: Soru-Cevap Oluşturma nedir?
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma, kullanıcı tarafından yöneltilen doğal dil sorularına en iyi yanıtı vermek için özel makine öğrenimi zekası uygulayan bulut tabanlı bir API hizmetidir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: overview
ms.date: 10/09/2018
ms.author: tulasim
ms.openlocfilehash: bd859183a13e0f8a21cdd2eabb464b718e949464
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212225"
---
# <a name="what-is-qna-maker"></a>Soru-Cevap Oluşturma nedir?

Soru-Cevap Oluşturma, verilerinizi temel alan bir sohbet ve soru-cevap katmanı oluşturan bulut tabanlı bir API hizmetidir. 

Soru-Cevap Oluşturma, Sık Sorulan Sorular (SSS) URL'leri, ürün kılavuzları, destek belgeleri ve özel soru-cevaplar gibi yarı yapılandırılmış içeriklerinizden bilgi bankası (KB) oluşturmanızı sağlar. Soru-Cevap Oluşturma hizmeti, kullanıcılarınızın doğal dil sorularını bilgi bankanızda bulunan en iyi cevapla eşleştirerek cevap verir.

Bu kullanımı kolay [web portalı](https://qnamaker.ai), geliştirici deneyimine sahip olmadan hizmetinizi oluşturmanızı, yönetmenizi, eğitmenizi ve yayımlamanızı sağlar. Hizmet bir uç noktada yayımlandıktan sonra sohbet botu gibi bir istemci uygulaması kullanıcıyla iletişim kurarak soruları alabilir ve cevap verebilir. 

![Genel Bakış](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Temel Soru-Cevap Oluşturma işlemleri

Soru-Cevap Oluşturma, verileriniz için iki temel hizmet sunar:

* **Ayıklama**: SSS ve ürün kılavuzları gibi yarı yapılandırılmış ve yarı yapılandırılmış [veri kaynaklarından](../Concepts/data-sources-supported.md) yapılandırılmış soru-cevap verileri ayıklanır. Bu ayıklama işlemi, KB [oluşturma](https://aka.ms/qnamaker-docs-createkb) sırasında veya daha sonra düzenleme işlemi sırasında gerçekleştirilebilir.

* **Eşleştirme**: Bilgi bankanız [eğitildikten ve test edildikten](https://aka.ms/qnamaker-docs-trainkb) sonra [yayımlamanız](https://aka.ms/qnamaker-docs-publishkb) gerekir. Bu sayede Soru-Cevap Oluşturma bilgi bankanızda, botunuzda veya istemci uygulamanızda kullanabileceğiniz bir uç nokta etkinleştirilir. Bu uç nokta kullanıcı sorularını kabul eder ve bilgi bankasındaki en iyi yanıtı verip eşleşme güvenilirlik puanını ekler.

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

1. **Soru-Cevap Oluşturma yönetim hizmetleri**: İlk oluşturma, güncelleştirme, eğitim ve yayımlama adımlarından oluşan Soru-Cevap Oluşturma bilgi bankası yönetim deneyimi. Bu etkinlikler [portal](https://qnamaker.ai) veya [yönetim API'leri](https://aka.ms/qnamaker-v4-apis) üzerinden gerçekleştirilebilir. 

2. **Soru-Cevap Oluşturma verileri ve çalışma zamanı**: Belirlediğiniz bölgedeki Azure aboneliğinize dağıtılır. KB içeriğiniz [Azure Search](https://azure.microsoft.com/services/search/) içinde depolanır ve uç nokta bir [App service](https://azure.microsoft.com/services/app-service/) olarak dağıtılır. Analiz için [Application Insights](https://azure.microsoft.com/services/application-insights/) kaynağı olarak dağıtmayı da seçebilirsiniz.

![Mimari](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Hizmetle ilgili önemli noktalar

- **Kod yazma** deneyimine ihtiyaç duymadan [SSS botu oluşturma](https://aka.ms/qnamaker-docs-create-faqbot).
- **Tahminler için ağ kapasitesi azaltma gerçekleştirilmez**. İşlem sayısı için değil barındırma ve hizmet için ödeme yapın. Daha ayrıntılı bilgi edinmek için [fiyatlandırma sayfasına](https://aka.ms/qnamaker-docs-pricing) bakın.
- **İstediğinizde ölçeklendirebilirsiniz**. Senaryonuza uygun bileşenlerin ilgili SKU'larını seçin. Soru-Cevap Oluşturma hizmetinizi için [kapasiteyi nasıl belirleyeceğinizi](https://aka.ms/qnamaker-docs-capacity) öğrenin.
- **Eksiksiz veri uyumluluğu**. Tahmin hizmeti bileşenleri, Azure aboneliğinizde ve uyumluluk sınırları çerçevesinde dağıtılır.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma hizmeti oluşturma](../how-to/set-up-qnamaker-service-azure.md)
