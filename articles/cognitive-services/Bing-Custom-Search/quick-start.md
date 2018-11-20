---
title: 'Hızlı başlangıç: İlk Bing Özel Arama örneğinizi oluşturma'
titlesuffix: Azure Cognitive Services
description: Bing Özel Arama hizmetini kullanmak için görünümünüzü veya web dilimini tanımlayan özel bir arama örneği oluşturmanız gerekir. Örnekte Bing'in aramasını istediğiniz genel etki alanları, alt siteler ve web sayfalarına ek olarak derecelendirme ayarlarını belirten ayarlar bulunur.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2017
ms.author: aahi
ms.openlocfilehash: c9b37486d664920bbc4b85a0715ce7f5ea910365
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161558"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Hızlı başlangıç: İlk Bing Özel Arama örneğinizi oluşturma
Bing Özel Arama hizmetini kullanmak için görünümünüzü veya web dilimini tanımlayan özel bir arama örneği oluşturmanız gerekir. Örnekte Bing'in aramasını istediğiniz genel etki alanları, web siteleri ve web sayfalarına ek olarak derecelendirme ayarlarını belirten ayarlar bulunur. Örneği oluşturmak için Bing Özel Arama [portalını](https://customsearch.ai) kullanın. 

## <a name="create-a-custom-search-instance"></a>Özel arama örneği oluşturma

Bing Özel Arama örneği oluşturmak için:

1.  Özel Arama API'niz için bir anahtar alın. Bkz. [Bilişsel Hizmetler’i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  **Oturum aç** düğmesine tıklayın ve bir Microsoft hesabı (MSA) kullanarak portalda oturum açın. 
    - Hesabınız yoksa **Microsoft hesabı oluşturun**'a tıklayın. Portal verilerinize erişmek için izin ister. **Evet**'e tıklayın.
    - Bilişsel Hizmetler Koşulları'nı kabul edin. **Kabul ediyorum**'a ve **Kabul et**'e tıklayın.  
3.  Oturum açtıktan sonra **Yeni Örnek** bağlantısına tıklayıp örneğe bir ad verin. Anlamlı ve arama tarafından döndürülen içerik türünü anlatan bir ad kullanın. Adı dilediğiniz zaman değiştirebilirsiniz. 
4.  **Arama Deneyimi** bölümündeki **Etkin** sekmesine aramak istediğiniz bir veya daha fazla web sitesinin URL'sini girin.
5.  Örneğinizin sonuç döndürdüğünü doğrulamak için sağ taraftaki önizleme bölmesine bir sorgu girin. Sonuç yoksa yeni bir web sitesi belirtin. Bing yalnızca dizine eklenmiş genel web sitelerinden sonuç döndürür.
6.  Yapılandırma değişikliklerini üretim ortamında yayımlamak için **Yayımla**'ya tıklayın. Sorulduğunda **Yayımla**'ya tıklayarak onaylayın.
7.  **Üretim** > **Uç noktalar**'a tıklayıp **Özel Yapılandırma Kimliği**'ni kopyalayın. Özel Arama API'sini çağırmak için bu kimliği kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzlarındaki yönergeleri izleyerek oluşturduğunuz özel arama örneğiyle çalışmaya devam edebilirsiniz:

- [Özel arama deneyiminizi yapılandırma](./define-your-custom-view.md)
- [Özel arama örneğinizi çağırma](./search-your-custom-view.md)
- [Özel arama örneğinizi paylaşma](./share-your-custom-search.md)
- [Barındırılan kullanıcı arabirimi deneyiminizi yapılandırma](./hosted-ui.md)
- [Metni vurgulamak için süsleme işaretçilerini kullanma](./hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
