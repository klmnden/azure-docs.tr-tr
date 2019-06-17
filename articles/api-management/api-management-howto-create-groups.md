---
title: Grupları kullanarak Azure API Management'ta Geliştirici hesaplarını yönetme | Microsoft Docs
description: Grupları kullanarak Azure API Management'ta Geliştirici hesaplarını yönetme hakkında bilgi edinin
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 5392cf5463dd0b11d1ce53856c8e4e2e788892b0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60658469"
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Oluşturma ve Azure API Management'ta Geliştirici hesaplarını yönetmek için grupları kullanma

API Management’te, ürünlerin geliştiricilere görünürlüğünü yönetmek için gruplar kullanılır. Ürünleri ilk gruplar tarafından görünür yapılır ve ardından bu gruplara geliştiriciler görüntüleyebilir ve gruplarıyla ilişkilendirilmiş ürünlere abone. 

API Management şu sabit sistem gruplarına sahiptir:

* **Yöneticiler**: Azure aboneliği yöneticileri bu grubun üyesidir. Yöneticiler, geliştiriciler tarafından kullanılan API’leri, işlemleri ve ürünleri oluşturarak API Management hizmet örneklerini yönetir.
* **Geliştiriciler**: Kimliği doğrulanmış geliştirici portalı kullanıcıları bu gruba girer. Geliştiriciler, API'lerinizi kullanarak uygulama oluşturan müşterilerdir. Geliştiriciler, geliştirici portalına erişim iznine sahiptir ve bir API’nin işlemlerini çağıran uygulamalar oluşturur.
* **Konuklar**: Kimliği doğrulanmamış geliştirici portalı kullanıcıları (bir API Management örneğinin geliştirici portalını ziyaret eden olası müşteriler gibi) bu gruba girer. Bunlara API’leri görüntüleyebilme ancak çağıramama gibi bazı salt okunur erişimler verilebilir.

Bu sistem gruplarına ek olarak, yöneticiler özel gruplar oluşturabilir veya [ilişkili Azure Active Directory kiracılarındaki dış grupları yararlanarak][leverage external groups in associated Azure Active Directory tenants]. Özel ve dış gruplar, geliştiricilere görünürlük ve API ürünlerine erişim sağlayan sistem gruplarıyla birlikte kullanılabilir. Örneğin, belirli bir iş ortağı kuruluşla ilişkili geliştiriciler için özel bir grup oluşturabilir ve bunlara yalnızca ilgili API'leri içeren bir üründen API'lere erişim izni verebilirsiniz. Bir kullanıcı birden fazla grubun üyesi olabilir.

Bu kılavuz, nasıl bir API Management örneğinin yöneticileri yeni gruplar eklemek ve ürün ve geliştiriciler ile ilişkilendirmek gösterir.

Oluşturma ve yayımcı portalı içinde grupları yönetme ek olarak, oluşturma ve API Management REST API kullanarak gruplarınızı yönetmek [grubu](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity) varlık.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki görevleri tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-group"> </a>Bir grup oluşturun

Bu bölümde, API Management hesabınıza yeni bir grup ekleme işlemi gösterilmektedir.

1. Seçin **grupları** ekranın sol tarafındaki için sekmesinde.
2. Tıklayın **+ Ekle**.
3. Grubu ve isteğe bağlı bir açıklama için benzersiz bir ad girin.
4. **Oluştur**’a basın.

    ![Yeni grubu eklemek](./media/api-management-howto-create-groups/groups001.png)

Grup oluşturulduktan sonra kümeye eklenirse **grupları** listesi. <br/>Düzenlenecek **adı** veya **açıklama** grubunun grubun adına tıklayın ve **ayarları**.<br/>Grubu silmek için Grup ve ENTER tuşuna adına **Sil**.

Grup oluşturulduktan sonra ürün ve geliştiriciler ile ilişkili olabilir.

## <a name="associate-group-product"> </a>Bir gruba bir ürünle ilişkilendirmek

1. Seçin **ürünleri** sol için sekmesinde.
2. İstenen ürün adına tıklayın.
3. Tuşuna **erişim denetimi**.
4. Tıklayın **+ Grup Ekle**.

    ![Bir gruba bir ürünle ilişkilendirmek](./media/api-management-howto-create-groups/groups002.png)
5. Eklemek istediğiniz grubu seçin.

    ![Bir gruba bir ürünle ilişkilendirmek](./media/api-management-howto-create-groups/groups003.png)

    Bir grup, üründen kaldırmak için tıklayın **Sil**.

    ![Grup silme](./media/api-management-howto-create-groups/groups004.png)

Bir ürün grubu ile ilişkili olduğunda, o gruptaki geliştiriciler görüntüleyebilir ve ürüne abone olma.

> [!NOTE]
> Azure Active Directory grupları eklemek için bkz [Azure Active Directory'yi kullanarak Azure API Management'ta Geliştirici hesaplarını yetkilendirme nasıl](api-management-howto-aad.md).

## <a name="associate-group-developer"> </a>Grupları geliştiricilerle ilişkilendirme

Bu bölümde, grup üyeleriyle ilişkilendirme işlemi gösterilmektedir.

1. Seçin **grupları** ekranın sol tarafındaki için sekmesinde.
2. Seçin **üyeleri**.

    ![Üye ekleme](./media/api-management-howto-create-groups/groups005.png)
3. Tuşuna **+ Ekle** ve bir üyesini seçin.

    ![Üye ekleme](./media/api-management-howto-create-groups/groups006.png)
4. Tuşuna **seçin**.

Geliştirici ve grubu ilişki eklendikten sonra onu görüntüleyebilir **kullanıcılar** sekmesi.

## <a name="next-steps"> </a>Sonraki adımlar

* Bir geliştirici grubuna eklendikten sonra görüntüleyebilir ve bu grupla ilişkili ürünlere abone. Daha fazla bilgi için [oluşturma ve Azure API Management'ta ürün yayımlama][How create and publish a product in Azure API Management],
* Oluşturma ve yayımcı portalı içinde grupları yönetme ek olarak, oluşturma ve API Management REST API kullanarak gruplarınızı yönetmek [grubu](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity) varlık.

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md
