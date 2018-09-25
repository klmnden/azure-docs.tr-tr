---
title: İlk Bing özel arama örneği - Microsoft Bilişsel hizmetler oluşturma
description: Bing özel arama kullanmak için görünümü veya Web dilim tanımlayan bir özel arama örneği oluşturmanız gerekir. Genel etki alanları, siteler ve Bing arama yapmak istediğiniz Web sayfalarını belirten ayarları ve herhangi bir derecelendirme ayarlama örneği içerir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 25d622772fe47ffad001834d476e612f8c606904
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981684"
---
# <a name="create-your-first-bing-custom-search-instance"></a>İlk Bing özel arama örneğinizin oluşturma
Bing özel arama kullanmak için görünümü veya Web dilim tanımlayan bir özel arama örneği oluşturmanız gerekir. Ortak etki alanı, Web siteleri ve Bing arama yapmak istediğiniz Web sayfalarını belirten ayarları ve herhangi bir derecelendirme ayarlama örneği içerir. Bing özel arama örneği oluşturmak için kullanmak [portalı](https://customsearch.ai). 

## <a name="create-a-custom-search-instance"></a>Özel arama örneği oluşturma

Bing özel arama örneği oluşturmak için:

1.  Özel arama API'si için bir anahtar alın. Bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Tıklayın **oturum** düğme ve bir Microsoft hesabı (MSA) kullanarak portalda oturum açın. 
    - Bir MSA yoksa tıklayın **Microsoft hesabı oluşturmanızda**. Portal, verilerinize erişmek için gerekli izinleri ister. **Evet**'e tıklayın.
    - Bilişsel hizmetler koşulları kabul etmiş olursunuz. Denetleme **kabul ediyorum** tıklatıp **kabul et**.  
3.  Oturum açtıktan sonra tıklayın **yeni örneği** ve örnek adı. Arama döndürür içerik türünü açıklar ve anlamlı bir ad kullanın. Herhangi bir zamanda adını değiştirebilirsiniz. 
4.  Üzerinde **etkin** sekmesinde altında **arama deneyimi**, bir URL girin veya daha fazla Web siteleri aramaya dahil etmek istediğiniz.
5.  Örneğiniz sonuçlar döndürüyor onaylamak için sağ tarafta önizleme bölmesinde bir sorgu girin. Hiç sonuç yoksa yeni bir Web sitesi belirtin. Bing dizine yalnızca ortak Web siteleri için sonuçları döndürür.
6.  Tıklayın **Yayımla** üretim için yapılandırma değişiklikleri yayımlamak için. Sorulduğunda **Yayımla** onaylamak için.
7.  Tıklayın **üretim** > **uç noktaları** ve kopyalama **özel yapılandırma kimliği**. Özel arama API'sini çağırmak için bu kodu ihtiyacınız vardır.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzlarından konusundaki yönergeleri izleyerek oluşturduğunuz özel arama örneği ile çalışmaya devam eder:

- [Özel arama deneyiminizi yapılandırın](./define-your-custom-view.md)
- [Özel arama çağırın](./search-your-custom-view.md)
- [Özel arama paylaşın](./share-your-custom-search.md)
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulayacak şekilde decoration işaretçileri kullanma](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)
