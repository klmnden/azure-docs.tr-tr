---
title: "API'nizi Azure API Management kullanarak sürümleri yayımlama | Microsoft Docs"
description: "Birden çok sürümü API Management'te yayımlama hakkında bilgi edinmek için Bu öğreticide adımları izleyin."
services: api-management
documentationcenter: 
author: mattfarm
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2017
ms.author: apimpm
ms.openlocfilehash: 7c355e2feb5ebe5971d8391b326422a1abec1497
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="publish-multiple-versions-of-your-api-in-a-predictable-way"></a>API'nizi birden fazla sürümünü öngörülebilir bir yolla yayımlama
Bu öğreticide, API sürümler ayarlamak ve API geliştiriciler tarafından denir yöntemi seçme açıklar.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için bir API Management hizmeti oluşturabilir ve varolan bir API (konferans API yerine) alter gerekir.

## <a name="about-versions"></a>Sürümleri hakkında
Bazı durumlarda, API tüm arayanlara tam olarak aynı sürümü kullanmaları için zordur. Bazen başkalarının şu anda bunlar için çalışan API ile kullanmayı tercih ederken yeni ya da farklı API özellikleri bazı kullanıcılara yayımlamak istediğiniz. Arayanlara sonraki bir sürüme yükseltmek istediğinizde bu yaklaşımı anlamak kolay bir kullanarak bunu yapabilmek isterler.  Bu kullanarak yapabiliriz **sürümleri** Azure API Management'te.

## <a name="walkthrough"></a>Kılavuz
Bu kılavuzda sürüm oluşturma şeması ve sürüm tanımlayıcısını seçme varolan API'ye, yeni bir sürüm ekleriz.

## <a name="add-a-new-version"></a>Yeni bir sürüm Ekle
![API bağlam menüsü - sürüm Ekle](media/api-management-getstarted-publish-versions/AddVersionMenu.png)
1. Gözat **API'leri** API Management hizmetiniz Azure portalında sayfasında.
2. Seçin **konferans API** API listeden seçip bağlam menüsünü (**...** ) yanında.
3. Seçin **+ sürüm eklemek**.

    > [!TIP]
    > Sürümler, aynı zamanda yeni bir API - ilk oluşturduğunuzda etkinleştirilebilir seçin **sürümü bu API?** üzerinde **API Ekle** ekran.

## <a name="choose-a-versioning-scheme"></a>Sürüm oluşturma düzenini seçin
Azure API Management, API'NİZİN hangi sürümünün istedikleri belirtmek arayanlara izin biçimini seçmenizi sağlar. Seçerek bunu bir **sürüm düzeni**. Bu düzen ya da olabilir **yolu, üst bilgi veya sorgu dizesi**. Bizim örneğimizde, yol kullanın.
![Sürüm ekranına ekleyin](media/api-management-getstarted-publish-versions/AddVersion.PNG)
1. Bırakın **yolu** olarak seçilen, **sürüm düzeni**.
2. Ekleme **v1** olarak, **sürüm tanıtıcısını**.

    > [!TIP]
    > Seçerseniz **üstbilgi** veya **sorgu dizesi** sürüm şema olarak, ek bir değeri - üstbilginin adını belirtin veya sorgu dizesi parametresi gerekir.

3. İsterseniz bir açıklama sağlayın.
4. Seçin **oluşturma** yeni sürümünüzü ayarlamak için.
5. Altındaki **büyük konferans API** API listesinde şimdi iki ayrı API'leri - gördüğünüz **özgün**, ve **v1**.
![Azure portalında bir API altında listelenen sürümler](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Sürüm bilgisi olmayan bir API sürümü eklerseniz, her zaman oluşturuyoruz bir **özgün** , - varsayılan URL'SİNDE yanıt. Bu, bir sürüm ekleme işlemini tarafından tüm mevcut arayanlar bozuk olmayan sağlar. Yeni bir API başlangıcında etkin sürümleriyle oluşturursanız, özgün oluşturulmaz.

6. Şimdi düzenler ve yapılandırmak **v1** için tamamen ayrı bir API olarak **özgün**. Bir sürümünde değişiklik başka etkilemez.

## <a name="add-the-version-to-a-product"></a>Bir ürüne sürüm ekleme
Yeni sürümü görmek arayanlar için onu eklenmeli bir **ürün** (ürünleri üst sürümlerden devralınmaz).

1. Seçin **ürünleri** hizmet yönetim sayfasından.
2. Seçin **sınırsız**.
3. Seçin **API'leri**.
4. **Add (Ekle)** seçeneğini belirleyin.
5. Seçin **konferans API sürümü v1**.
6. Hizmet Yönetimi sayfasına dönmek ve seçmek **API'leri**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Geliştirici Portalı sürümü görmek için Gözat
1. Seçin **Geliştirici Portalı** üstteki menüden.
2. Seçin **API'leri**, dikkat **konferans API** gösterir **özgün** ve **v1** sürümleri.
3. Seçin **v1**.
4. Bildirim **istek URL'si** listedeki ilk işleminin. API URL yolunu içerdiğini gösterir **v1**.
![Geliştirici portalında gösterilen sürümü](media/api-management-getstarted-publish-versions/VersionDevPortal.PNG)
