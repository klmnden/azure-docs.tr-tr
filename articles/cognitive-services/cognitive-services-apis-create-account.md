---
title: Bilişsel hizmetler API'leri hesabı Azure portalında oluşturun | Microsoft Docs
description: Azure portalında bir Microsoft Bilişsel hizmetler API'leri hesabı oluşturma
services: cognitive-services
documentationcenter: ''
author: garyericson
manager: cgronlun
editor: ''
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: cognitive-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: garye
ms.reviewer: gibattag
ms.openlocfilehash: 3697dd0628f0028cb486139e92c032f0d757c6ed
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352696"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Azure portalında bir Bilişsel hizmetler API'leri hesabı oluşturun

Microsoft Bilişsel hizmeti API'ları kullanmak için ilk Azure portalında bir hesap oluşturmanız gerekir.

1. [Azure Portal](http://portal.azure.com)’da oturum açın.

2. Tıklatın **+ kaynak oluşturma**.

3. Azure Market altında seçin **AI + Bilişsel Hizmetler** ve kullanılabilen API'lerin listesi keşfedin. Tıklayın **tümünü görmek** Bilişsel hizmetler API'leri tam listesini görmek için. Devam etmek için tercih ettiğiniz API'ı tıklatın.

    ![Bilişsel hizmetler API'ları seçin](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. Üzerinde **oluşturma** sayfasında, aşağıdaki bilgileri sağlayın:

   - **Hesap adı:** hesabının adı. Örneğin açıklayıcı bir ad kullanmanızı öneririz *AFaceAPIAccount*.

   - **Abonelik:** sahip olduğunuz en az kullanılabilir Azure aboneliklerini seçin katkıda bulunan izinleri.

   - **API türü:** kullanmak istediğiniz Bilişsel Services API'ı seçin. Hizmetleri API'leri çeşitli Bilişsel kullanılabilir hakkında daha fazla bilgi için ziyaret [Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/) site.

   - **Fiyatlandırma katmanı:** Bilişsel hizmetler hesabınıza maliyetini gerçek kullanımını ve belirlediğiniz seçeneklere bağlıdır. Her API için fiyatlandırma hakkında daha fazla bilgi için bkz [sayfaları fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/).

   - **Kaynak grubu:** bir kaynak grubu aynı yaşam döngüsü, izinleri ve ilkeleri paylaşan kaynakları koleksiyonudur. Kaynak grupları hakkında daha fazla bilgi için bkz: [yönetmek Azure kaynakları portal üzerinden](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Kaynak grubu konumu:** yalnızca seçili API (bir konuma bağlı değil) genel ise, bu gereklidir. Ancak, API genel bir konuma bağlı ise, Bilişsel Hizmetleri API hesabıyla ilişkilendirilen meta verilerin bulunduğu kaynak grubu için bir konum belirtmeniz gerekir. Bu konum hesabınızı çalışma zamanı kullanılabilirliğini bir etkisi yoktur. Kaynak grubu hakkında daha fazla bilgi için bkz: [yönetmek Azure kaynakları portal üzerinden](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Çevrimiçi hizmet koşulları açık bildirim:** abonelik sahipleri veya katkıda bulunanlar gibi bir hesap oluşturmak için (tarafından tanımlanan [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)) açıkça koşulları kabul etmek gerekir, Bilişsel hizmetler uygulamak [çevrimiçi Hizmet Koşulları'nı](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx). 

     Abonelik sahibi Bilişsel Hizmetler hesabı için belirli bir kaynak grubu veya abonelikle oluşturulmasını devre dışı [Azure ilkeleri](../azure-policy/azure-policy-introduction.md) makaleyi izleyerek [atamak ve yönetmek için kullanarak Azure portalı Kaynak ilkeleri](../azure-policy/assign-policy-definition.md) ve bir "izin verilmeyen kaynak türleri" ilke tanımı atama ve belirterek **Microsoft.CognitiveServices/accounts** hedef kaynak türü.

     Hesap oluşturma devre dışı bırakılmışsa, aşağıdaki hata hesabı oluşturma sırasında görüntülenir:

     ![Hesabı oluşturma hatası](media/cognitive-services-apis-create-account/error-message.png)

5. Azure portalı panosunun hesabına sabitlemek için tıklatın **panoya Sabitle**.

6. Hesabı oluşturmak için **Oluştur**’a tıklayın.

Bilişsel Hizmetler hesabı başarıyla dağıtıldıktan sonra hesap bilgilerini görüntülemek için bir Panoda kutucuğa tıklayın.

Kullanabilirsiniz **uç nokta URL'si** içinde **genel bakış** bölümü ve anahtarlarında **anahtarları** API yapmadan başlatmak için bölüm uygulamalarınızda çağırır.

![Hesap bilgilerini görüntüleme](media/cognitive-services-apis-create-account/display-account.png)

![Görüntü hesabı anahtarları](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Sonraki Adımlar

Tüm Microsoft Bilişsel hizmetler kullanılabilir hakkında daha fazla bilgi için bkz: [Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/).

Hızlı Başlangıç kılavuzları bazı örnek Bilişsel hizmetler API'lerini kullanarak:

 - [Bilgisayar görme C# quickstarts](/computer-vision/quickstarts/csharp.md)
 - [Python ile metin analizi](/text-analytics/quickstarts/python.md)
 - [JavaScript ile API yüzeyi](/face/quickstarts/javascript.md)
