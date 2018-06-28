---
title: Soru-Cevap Oluşturma hakkında bilgi edinin - Microsoft Bilişsel Hizmetler | Microsoft Docs
titleSuffix: Azure
description: Soru-Cevap Oluşturma, SSS belgeleri veya URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: overview
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: f34cec047c18a6db10b5adda82800c51d44c1155
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295154"
---
# <a name="what-is-qna-maker"></a>Soru-Cevap Oluşturma nedir?

[Soru-Cevap Oluşturma](https://qnamaker.ai), SSS (Sık Sorulan Sorular) belgeleri veya URL'ler ve ürün kılavuzları gibi yarı yapılandırılmış içeriklerinizden bir soru cevap hizmeti oluşturmanızı sağlar. Kullanıcı sorguları konusunda esnek, bir botu doğal, konuşma geliştiren bir şekilde eğitmek için kullanacağınız yanıtlar üreten soru ve cevap modeli oluşturabilirsiniz.

Kullanımı kolay grafik kullanıcı arabirimi, geliştirici deneyimine ihtiyaç duymadan hizmetinizi oluşturmanızı, yönetmenizi, eğitmenizi ve kullanıma sunmanızı sağlar.

![Genel Bakış](../media/qnamaker-overview-learnabout/overview.png)

## <a name="highlights"></a>Önemli Noktalar

- **Kod yazma** deneyimine ihtiyaç duymadan [SSS botu oluşturma](https://aka.ms/qnamaker-docs-create-faqbot).
- **Ağ kapasitesini azaltmaya son**. İşlem sayısı için değil barındırma ve hizmet için ödeme yapın. Daha ayrıntılı bilgi edinmek için [fiyatlandırma sayfasına](https://aka.ms/qnamaker-docs-pricing) bakın.
- **Gereksinimlerinize göre ölçeklendirin**. Senaryonuza uygun bileşenlerin ilgili SKU'larını seçin. Soru-Cevap Oluşturma hizmetinizi için [kapasiteyi nasıl belirleyeceğinizi](https://aka.ms/qnamaker-docs-capacity) öğrenin.
- **Eksiksiz veri uyumluluğu**. Veri ve çalışma zamanı bileşenleri, kullanıcının Azure aboneliğinde ve uyumluluk sınırları çerçevesinde dağıtılır. Daha ayrıntılı bilgi edinmek için aşağıya göz atın.

## <a name="key-qna-maker-processes"></a>Temel Soru-Cevap Oluşturma işlemleri

Soru-Cevap Oluşturma, verileriniz için iki temel hizmet sunar:

* **Ayıklama**: SSS ve ürün kılavuzları gibi yarı yapılandırılmış veri kaynaklarından yapılandırılmış soru-cevap verileri ayıklanır. Ayıklama işlemi, bilgi bankası oluşturma sırasında gerçekleştirilir. Bilgi bankanızı [burada](https://aka.ms/qnamaker-docs-createkb) oluşturun.

* **Eşleştirme**: Bilgi bankanız [eğitildikten ve test edildikten](https://aka.ms/qnamaker-docs-trainkb) sonra [yayımlamanız](https://aka.ms/qnamaker-docs-publishkb) gerekir. Bu sayede Soru-Cevap Oluşturma bilgi bankanızda, botunuzda veya uygulamanızda kullanabileceğiniz bir uç nokta etkinleştirilir. Bu uç nokta kullanıcı sorularını kabul eder ve bilgi bankasındaki en iyi soru/yanıt eşleşmesiyle yanıt verip eşleşme güvenilirlik puanını ekler.

## <a name="qna-maker-architecture"></a>Soru-Cevap Oluşturma mimarisi

Soru-Cevap Oluşturma yığını aşağıdaki bölümlerden oluşur:

1. **Soru-Cevap Oluşturma yönetim hizmetleri (kontrol düzlemi)**: İlk oluşturma, güncelleştirme, eğitim ve yayımlama adımlarından oluşan Soru-Cevap Oluşturma bilgi bankası yönetim deneyimi. Bu etkinlikler [portal](https://qnamaker.ai) veya [yönetim API'leri](https://aka.ms/qnamaker-v4-apis) üzerinden gerçekleştirilebilir. Yönetim hizmetleri aşağıdaki çalışma zamanı bileşeniyle iletişim kurar.

2. **Soru-Cevap Oluşturma çalışma zamanı (veri düzlemi)**: Veri ve çalışma zamanı kullanıcının kendi seçtiği bir bölgede ve Azure aboneliğinde dağıtılır. Müşteri soru/yanıt içeriği [Azure Search](https://azure.microsoft.com/services/search/) içinde depolanır ve çalışma zamanı bir [App service](https://azure.microsoft.com/services/app-service/) olarak dağıtılır. Dilerseniz analiz için [Application Insights](https://azure.microsoft.com/services/application-insights/) kaynağı olarak dağıtmayı da seçebilirsiniz.

![Mimari](../media/qnamaker-overview-learnabout/architecture.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma hizmeti oluşturma](../how-to/set-up-qnamaker-service-azure.md)
