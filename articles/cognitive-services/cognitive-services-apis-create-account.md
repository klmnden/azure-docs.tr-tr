---
title: Azure portalında bir Bilişsel hizmetler API hesabı oluşturma | Microsoft Docs
description: Azure portalında Microsoft Bilişsel hizmetler API hesabı oluşturma
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
ms.openlocfilehash: ed5f19b23375ecb83e19274c7405e9a1208a7985
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036169"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Azure portalında bir Bilişsel hizmetler API hesabı oluşturma

Microsoft Bilişsel hizmetler API'leri kullanmak için öncelikle Azure portalında bir hesap oluşturmanız gerekir.

1. [Azure Portal](http://portal.azure.com)’da oturum açın.

2. Tıklayın **+ kaynak Oluştur**.

3. Azure Marketi bölümünde seçin **yapay ZEKA ve Bilişsel Hizmetler** ve kullanılabilen API'lerin listesi keşfedin. Tıklayarak **tümünü gör** Bilişsel hizmetler API'leri tam listesini görmek için. Devam etmek için tercih ettiğiniz API'ye tıklayın.

    ![Bilişsel hizmetler API'leri seçin](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. Üzerinde **Oluştur** sayfasında, aşağıdaki bilgileri sağlayın:

   - **Hesap adı:** hesabının adı. Örneğin, açıklayıcı bir ad kullanmanızı öneririz *AFaceAPIAccount*.

   - **Abonelik:** kullanılabilir Azure abonelikleri sahip olduğunuz en az birini katkıda bulunan izinleri.

   - **API türü:** Bilişsel hizmetler API'si kullanmak istediğinizi seçin. Çeşitli Bilişsel hizmetler API'leri kullanılabilir hakkında daha fazla bilgi için ziyaret [Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/) site.

   - **Fiyatlandırma katmanı:** Bilişsel hizmetler hesabınızın maliyeti, gerçek kullanım ve belirttiğiniz seçeneklere bağlıdır. Her API'si için fiyatlandırma hakkında daha fazla bilgi için [fiyatlandırma sayfalarına](https://azure.microsoft.com/pricing/details/cognitive-services/).

   - **Kaynak grubu:** bir kaynak grubu, aynı yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynaklar koleksiyonudur. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı portal üzerinden yönetme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Kaynak grubu konumu:** yalnızca seçili API (bir konuma bağlı değil) genel ise, bu gereklidir. Ancak, API, genel ve konuma bağlı ise, Bilişsel hizmetler API hesabı ile ilişkili meta verilerin bulunduğu kaynak grubu için bir konum belirtmeniz gerekir. Bu konum, hesabınızın çalışma zamanı kullanılabilirliğini etkilemez. Kaynak grubu hakkında daha fazla bilgi için bkz: [Azure kaynaklarınızı portal üzerinden yönetme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Çevrimiçi Hizmet Koşulları'nın açık onayını:** abonelik sahipleri veya katkıda bulunan bir hesabı oluşturmak için (tarafından tanımlandığı gibi [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview)) açıkça koşulları kabul etmek ihtiyacınız olan Cognitive Services uygulamak [çevrimiçi hizmet koşulları](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx). 

     Abonelik sahibi Bilişsel Hizmetler hesabı için belirli bir kaynak grubu veya abonelikle oluşturulmasını devre dışı bırakabilirsiniz [Azure ilkeleri](../azure-policy/azure-policy-introduction.md) makaleyi izleyerek [atamak ve yönetmek için portalı kullanarak Azure Kaynak ilkeleri](../azure-policy/assign-policy-definition.md) "izin verilmeyen kaynak türleri" için bir ilke tanımı atadıktan ve belirterek **Microsoft.CognitiveServices/accounts** hedef kaynak türü.

     Hesap oluşturma devre dışı bırakılmışsa aşağıdaki hata hesap oluşturma sırasında görüntülenir:

     ![Hesabı oluşturma hatası](media/cognitive-services-apis-create-account/error-message.png)

5. Hesap Azure portal panosuna sabitlemek için tıklayın **panoya Sabitle**.

6. Hesabı oluşturmak için **Oluştur**’a tıklayın.

Bilişsel Hizmetler hesabı başarıyla dağıtıldıktan sonra hesap bilgilerini görüntülemek için panodaki bir kutucuğa tıklayın.

Kullanabileceğiniz **uç nokta URL'si** içinde **genel bakış** bölümü ve anahtarları **anahtarları** API oluşturmaya başlamak için bölümün çağırdığı uygulamalarınızda.

![Hesap bilgilerini görüntüleme](media/cognitive-services-apis-create-account/display-account.png)

![Hesap anahtarlarını görüntüle](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Sonraki Adımlar

Tüm Microsoft Bilişsel hizmetler hakkında daha fazla bilgi için bkz. [Bilişsel Hizmetler](https://azure.microsoft.com/services/cognitive-services/).

Hızlı Başlangıç kılavuzları için bazı örnek Bilişsel hizmetler API'lerini kullanarak:

 - [Bilgisayar işleme C# hızlı başlangıçları](computer-vision/quickstarts/csharp.md)
 - [Python ile metin analizi](text-analytics/quickstarts/python.md)
 - [Yüz tanıma API'si ile JavaScript](face/quickstarts/javascript.md)
