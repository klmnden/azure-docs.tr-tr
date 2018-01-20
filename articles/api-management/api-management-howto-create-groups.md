---
title: "Azure API Management'te grupları kullanılarak Geliştirici hesaplarını yönetme | Microsoft Docs"
description: "Geliştirici hesaplarının Azure API Management'te grupları kullanarak yönetmeyi öğrenin"
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: apimpm
ms.openlocfilehash: 08e6e33d65684cbf5a70da28e67c376aaf06d7af
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Oluşturma ve Azure API Management'ta Geliştirici hesaplarını yönetmek için grupları kullanma
API Management’te, ürünlerin geliştiricilere görünürlüğünü yönetmek için gruplar kullanılır. Ürünler ilk gruplar tarafından görünür yapılır ve sonra bu gruplara geliştiriciler görüntülemek ve gruplarıyla ilişkili ürünlere abone. 

API Management şu sabit sistem gruplarına sahiptir:

* **Yöneticiler**: Azure aboneliği yöneticileri bu grubun üyesidir. Yöneticiler, geliştiriciler tarafından kullanılan API’leri, işlemleri ve ürünleri oluşturarak API Management hizmet örneklerini yönetir.
* **Geliştiriciler**: Kimliği doğrulanmış geliştirici portalı kullanıcıları bu gruba girer. Geliştiriciler, API'lerinizi kullanarak uygulama oluşturan müşterilerdir. Geliştiriciler, geliştirici portalına erişim iznine sahiptir ve bir API’nin işlemlerini çağıran uygulamalar oluşturur.
* **Konuklar**: Kimliği doğrulanmamış geliştirici portalı kullanıcıları (bir API Management örneğinin geliştirici portalını ziyaret eden olası müşteriler gibi) bu gruba girer. Bunlara API’leri görüntüleyebilme ancak çağıramama gibi bazı salt okunur erişimler verilebilir.

Bu sistem gruplarına ek olarak, yöneticiler özel gruplar oluşturabilir veya [ilişkili Azure Active Directory kiracılarındaki dış grupları yararlanan][leverage external groups in associated Azure Active Directory tenants]. Özel ve dış gruplar, geliştiricilere görünürlük ve API ürünlerine erişim sağlayan sistem gruplarıyla birlikte kullanılabilir. Örneğin, belirli bir iş ortağı kuruluşla ilişkili geliştiriciler için özel bir grup oluşturabilir ve bunlara yalnızca ilgili API'leri içeren bir üründen API'lere erişim izni verebilirsiniz. Bir kullanıcı birden fazla grubun üyesi olabilir.

Bu kılavuz, nasıl bir API Management örneğinin yöneticileri yeni gruplar eklemek ve ürünleri ve geliştiricilerin ilişkilendirmek gösterir.

Grup oluşturma ve yayımcı portalında yönetme ek olarak, oluşturmak ve gruplarınızı API Management REST API kullanarak yönetmek [grup](https://msdn.microsoft.com/library/azure/dn776329.aspx) varlık.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede görevleri tamamlayın: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-group"></a>Bir grup oluşturun

Bu bölümde, API Management hesabınıza yeni bir grup eklemek gösterilmiştir.

1. Seçin **grupları** ekranın sol sekmesine.
2. Tıklatın **+ Ekle**.
3. Grup ve isteğe bağlı bir açıklama için benzersiz bir ad girin.
4. **Oluştur**’a basın.

    ![Yeni Grup Ekle](./media/api-management-howto-create-groups/groups001.png)

Grup oluşturulduğunda, eklendiğinden **grupları** listesi. <br/>Düzenlenecek **adı** veya **açıklama** grubunu, grubun adını tıklatın ve **ayarları**.<br/>Grubunu silmek için tuşuna basın ve grup adını tıklatın **silmek**.

Grubu oluşturulan, ürünleri ve geliştiriciler ile ilişkili olabilir.

## <a name="associate-group-product"></a>Bir grup ürünü ile ilişkilendirme

1. Seçin **ürünleri** sol sekmesi.
2. İstenen ürün adına tıklayın.
3. Tuşuna **erişim denetimi**.
4. Tıklatın **+ Grup Ekle**.

    ![Yeni Grup Ekle](./media/api-management-howto-create-groups/groups002.png)
5. Eklemek istediğiniz grubu seçin.

    ![Yeni Grup Ekle](./media/api-management-howto-create-groups/groups003.png)

    Ürün grubu kaldırmak için tıklatın **silmek**.

    ![Grup silme](./media/api-management-howto-create-groups/groups004.png)

Bir ürün grubu ile ilişkili olduğunda, o gruptaki geliştiriciler görüntüleyebilir ve ürüne abone.

> [!NOTE]
> Azure Active Directory grupları eklemek için bkz: [Azure API Management'te Azure Active Directory'yi kullanarak Geliştirici hesaplarını yetkilendirmede nasıl](api-management-howto-aad.md).

## <a name="associate-group-developer"></a>Grupları geliştiricilerle ilişkilendirme

Bu bölüm grupları üyeleriyle ilişkilendirme gösterir.

1. Seçin **grupları** ekranın sol sekmesine.
2. Seçin **üyeleri**.

    ![Üye ekleme](./media/api-management-howto-create-groups/groups005.png)
3. Tuşuna **+ Ekle** ve bir üye seçin.

    ![Üye ekleme](./media/api-management-howto-create-groups/groups006.png)
4. Tuşuna **seçin**.


Geliştirici ve grubu ilişki eklendikten sonra içinde görüntüleyebilirsiniz **kullanıcılar** sekmesi.

## <a name="next-steps"> </a>Sonraki adımlar
* Bir geliştirici bir gruba eklendikten sonra görüntüleyin ve bu grupla ilişkili ürünlere abone. Daha fazla bilgi için bkz: [oluşturma ve Azure API Management'te bir ürün yayımlama][How create and publish a product in Azure API Management],
* Grup oluşturma ve yayımcı portalında yönetme ek olarak, oluşturmak ve gruplarınızı API Management REST API kullanarak yönetmek [grup](https://msdn.microsoft.com/library/azure/dn776329.aspx) varlık.

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
