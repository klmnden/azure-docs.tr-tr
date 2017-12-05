---
title: "API'nizi Azure API Management kullanarak sürümleri yayımlama | Microsoft Docs"
description: "Birden çok sürümü API Management'te yayımlama hakkında bilgi edinmek için Bu öğreticide adımları izleyin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: d63bdd3110f5c5db3e7bfec424644fdbc8d8d90c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="publish-multiple-versions-of-your-api"></a>API'nizi birden fazla sürümünü yayımlama 

Tam olarak aynı sürüm tüm arayanlara API kullanımınız için pratik olduğu zaman zaman vardır. Bazen başkalarının şu anda bunlar için çalışan API ile kullanmayı tercih ederken yeni ya da farklı API özellikleri bazı kullanıcılara yayımlamak istediğiniz. Arayanlara sonraki bir sürüme yükseltmek istediğinizde bu yaklaşımı anlamak kolay bir kullanarak bunu yapabilmek isterler.  Bu kullanarak yapabiliriz **sürümleri** Azure API Management'te. Daha fazla bilgi için bkz: [sürümleri & düzeltmelerini](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir sürümü için mevcut bir API ekleme
> * Sürüm düzenini seçin
> * Bir ürüne sürüm ekleme
> * Geliştirici Portalı sürümü görmek için Gözat

![Geliştirici portalında gösterilen sürümü](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Ön koşullar

+ Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, aşağıdaki öğreticiye: [alma ve ilk API'nizi yayımlama](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-version"></a>Yeni bir sürüm Ekle

![API bağlam menüsü - sürüm Ekle](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Seçin **konferans API** API listeden.
2. Bağlam menüsünü (**...** ) yanında.
3. Seçin **+ sürüm eklemek**.

    > [!TIP]
    > Sürümler, aynı zamanda yeni bir API - ilk oluşturduğunuzda etkinleştirilebilir seçin **sürümü bu API?** üzerinde **API Ekle** ekran.

## <a name="choose-a-versioning-scheme"></a>Sürüm oluşturma düzenini seçin

Azure API Management, API'NİZİN hangi sürümünün istedikleri belirtmek arayanlara izin biçimini seçmenizi sağlar. Seçerek bunu bir **sürüm düzeni**. Bu düzen ya da olabilir **yolu, üst bilgi veya sorgu dizesi**. Bizim örneğimizde, yol kullanın.

![Sürüm ekranına ekleyin](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Bırakın **yolu** olarak seçilen, **sürüm düzeni**.
2. Ekleme **v1** olarak, **sürüm tanıtıcısını**.

    > [!TIP]
    > Seçerseniz **üstbilgi** veya **sorgu dizesi** sürüm şema olarak ek bir değeri - üstbilginin adını belirtin veya sorgu dizesi parametresi gerekir.

3. İsterseniz bir açıklama sağlayın.
4. Seçin **oluşturma** yeni sürümünüzü ayarlamak için.
5. Altındaki **büyük konferans API** API listesinde şimdi iki ayrı API'leri - gördüğünüz **özgün**, ve **v1**.

    ![Azure portalında bir API altında listelenen sürümler](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Sürüm bilgisi olmayan bir API sürümü eklerseniz, her zaman oluşturuyoruz bir **özgün** , - varsayılan URL'SİNDE yanıt. Bu, bir sürüm ekleme işlemini tarafından tüm mevcut arayanlar bozuk olmayan sağlar. Yeni bir API başlangıcında etkin sürümleriyle oluşturursanız, özgün oluşturulmaz.

6. Şimdi düzenler ve yapılandırmak **v1** için ayrı bir API olarak **özgün**. Bir sürümünde değişiklik başka etkilemez.

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

    ![API bağlam menüsü - sürüm Ekle](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir sürümü için mevcut bir API ekleme
> * Sürüm düzenini seçin 
> * Bir ürüne sürüm ekleme
> * Geliştirici Portalı sürümü görmek için Gözat

Sonraki öğretici ilerleyin:

> [!div class="nextstepaction"]
> [Yükseltme ve ölçeklendirme](upgrade-and-scale.md)