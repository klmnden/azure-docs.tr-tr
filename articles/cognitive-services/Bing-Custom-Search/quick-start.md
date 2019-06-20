---
title: 'Hızlı Başlangıç: Bir ilk Bing özel arama örneği oluşturma | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Özel bir Bing arama etki alanı örneği ve tanımladığınız Web sayfaları oluşturmak için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 6949824f598194456837544526223b823dcfc3e5
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273340"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Hızlı Başlangıç: İlk Bing özel arama örneğinizin oluşturma

Bing Özel Arama hizmetini kullanmak için görünümünüzü veya web dilimini tanımlayan özel bir arama örneği oluşturmanız gerekir. Bu örnek, bir ortak etki alanı, Web siteleri ve, isteyebileceğiniz derecelendirme ayarlamaları birlikte arama yapmak istediğiniz Web sayfalarını içerir. 

Örneği oluşturmak için kullanın [Bing özel arama portalı](https://customsearch.ai). 

![Bing özel arama portalı resmi](media/blockedCustomSrch.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-a-custom-search-instance"></a>Özel arama örneği oluşturma

Bing Özel Arama örneği oluşturmak için:

1. Tıklayın **Başlarken** üzerinde [Bing özel arama portalı](https://customsearch.ai) Web sayfasını ve Microsoft hesabınızla oturum açın.

2. Tıklayın **yeni örneği**, açıklayıcı bir ad girin. Herhangi bir zamanda örneğinizin adını değiştirebilirsiniz.
 
3. **Arama Deneyimi** bölümündeki **Etkin** sekmesine aramak istediğiniz bir veya daha fazla web sitesinin URL'sini girin. 

    > [!NOTE]
    > Bing özel arama örnekleri yalnızca etki alanları ve genel ve Bing tarafından dizini oluşturulmuş Web sayfaları için sonuçları döndürür.

4. Bing özel arama portalın sağ tarafındaki bir sorgu girin ve arama örneğiniz tarafından döndürülen arama sonuçlarını incelemek için kullanabilirsiniz. Hiçbir sonuç döndürmedi, farklı bir URL girmeyi deneyin.  

5. Tıklayın **Yayımla** üretim ortamına değişikliklerinizi yayımlayın ve örneğinin uç noktalarını güncelleştirmek için.

6.  Tıklayarak **üretim** sekmesinde altında **uç noktaları**, kopyalayın, **özel yapılandırma kimliği**. Bu kimliği için ekleyerek özel arama API'sini çağırmak için gereksinim duyduğunuz `customconfig=` çağrılarınızı parametresinde sorgu.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bing özel arama uç noktanızı arayın](./call-endpoint-csharp.md)
