---
title: İlk Bing özel arama örneği - Microsoft Bilişsel hizmetler oluşturma
description: Bing özel aramayı kullanmak için görünümü veya Web slice tanımlayan bir özel arama örneği oluşturmanız gerekir. Örnek genel etki alanlarındaki, siteleri ve arama yapmak için Bing istediğiniz Web sayfalarını belirtin ayarları ve herhangi bir derecelendirme ayarlamalarını içerir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 35f0bca01de1c2087f6ae30949cca9b03192b838
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352883"
---
# <a name="create-your-first-bing-custom-search-instance"></a>İlk Bing özel arama örneği oluşturma
Bing özel aramayı kullanmak için görünümü veya Web slice tanımlayan bir özel arama örneği oluşturmanız gerekir. Ortak etki alanı, Web siteleri ve arama yapmak için Bing istediğiniz Web sayfalarını belirtin ayarları ve herhangi bir derecelendirme ayarlama örneği içerir. Örneği oluşturmak için Bing özel arama kullanmasını [portal](https://customsearch.ai). 

## <a name="create-a-custom-search-instance"></a>Özel arama örneği oluşturma

Bing özel arama örneği oluşturmak için:

1.  Bir anahtar için özel arama API alın. Bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Bir Microsoft hesabı (MSA) kullanarak portalında oturum açın. Tıklatın **oturum** düğmesi. Aksi takdirde portal izleyin aşağıdaki ek adımları kullanarak ilk kez ise, 3. adıma devam edin.
    - Bir MSA yoksa tıklatın **bir Microsoft hesabı oluşturmayı**. Portal, veri erişim izinlerini sorar. **Evet**'e tıklayın.
    - Bilişsel hizmetler koşullarını kabul ediyorsunuz. Denetleme **kabul ediyorum** tıklatıp **kabul et**.  
3.  Oturum açtıktan sonra tıklatın **yeni örnek** ve örnek adı. Anlamlı ve arama döndürür içerik türünü açıklayan bir ad kullanın. Herhangi bir zamanda adını değiştirebilirsiniz. 
4.  Üzerinde **etkin** sekmesinde **arama deneyimi**, bir URL girin veya daha çok sitenin aramanızda dahil etmek istediğiniz.
5.  Örneğinizi sonuçlar döndürür onaylamak için sağ tarafta önizleme bölmesinde bir sorgu girin. Hiç sonuç yoksa yeni bir site belirtin. Bing dizine genel sitelere için sonuçları döndürür.
6.  Tıklatın **Yayımla** üretim için yapılandırma değişiklikleri yayımlamak için. İstendiğinde, tıklatın **Yayımla** onaylamak için.
7.  Tıklatın **üretim** > **uç noktaları** ve kopyalama **özel yapılandırma kimliği**. Özel arama API'sini çağırmak için bu kimliği gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Yalnızca bu nasıl yapılır Kılavuzları'ndaki yönergeleri izleyen tarafından oluşturduğunuz özel arama örneği ile çalışmaya devam eder:

- [Özel arama deneyiminiz yapılandırın](./define-your-custom-view.md)
- [Özel aramanızı çağırın](./search-your-custom-view.md)
- [Özel aramanızı paylaşma](./share-your-custom-search.md)
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulama için decoration işaretlerini kullanın](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)
