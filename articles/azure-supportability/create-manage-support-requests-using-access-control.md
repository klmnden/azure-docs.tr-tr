---
title: "Azure rol tabanlı erişim denetimi (oluşturmak ve Destek isteklerini yönetmek için RBAC) için erişim haklarını denetleme | Microsoft Docs"
description: "Azure rol tabanlı erişim denetimi (oluşturmak ve Destek isteklerini yönetmek için RBAC) için erişim haklarını denetleme"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: 20ebd324cbf379980b43d255d468673de2b6d950
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-role-based-access-control-rbac-to-control-access-rights-to-create-and-manage-support-requests"></a>Azure rol tabanlı erişim denetimi (oluşturmak ve Destek isteklerini yönetmek için RBAC) için erişim haklarını denetleme

[Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) Azure için ayrıntılı erişim yönetimi sağlar.
Azure portalında isteği oluşturmayı destekleyen [portal.azure.com](https://portal.azure.com), kimin oluşturmak ve Destek isteklerini yönetmek tanımlamak için Azure RBAC modeli kullanır.
Kullanıcılar, gruplar ve bir abonelik, kaynak grubu veya bir kaynak olabilen bir kapsamda belirli, uygulamalara uygun RBAC rolü atayarak erişimi verilir.

Bir örnek atalım: Abonelik kapsamında Okuma izinlerine sahip bir kaynak grubu sahibi olarak Web siteleri, sanal makineler ve alt ağlar gibi bir kaynak grubundaki tüm kaynakları yönetebilir.
Ancak, sanal makine kaynağı karşı bir destek isteği oluşturmak çalıştığınızda aşağıdaki hatayla karşılaşırsanız

![Abonelik hatası](./media/create-manage-support-requests-using-access-control/subscription-error.png)

Destek isteği Yönetimi'nde, izin veya destek eylem Microsoft.Support/* oluşturmak ve Destek isteklerini yönetmek için abonelik kapsamda sahip olduğu bir rol yazmanız gerekir.

Aşağıdaki makalede oluşturmak ve destek istekleri Azure portalında yönetmek için Azure'nın özel rol tabanlı erişim denetimi (RBAC) nasıl kullanabileceğinizi açıklar.

## <a name="getting-started"></a>Başlarken

Yukarıdaki örneği kullanarak, bir özel RBAC rolü aboneliğe abonelik sahibi tarafından atanırsa, kaynak için bir destek isteği oluşturmak mümkün olacaktır.
[Özel RBAC rolleri](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) Azure PowerShell, Azure komut satırı arabirimi (CLI) ve REST API'si kullanılarak oluşturulabilir.

Özel bir rol Eylemler özelliği rolü erişim verdiği Azure işlemleri belirtir.
Destek isteği yönetimi için özel bir rol oluşturmak için rol Microsoft.Support/* eylemi olması gerekir

Özel bir rol oluşturmak ve Destek isteklerini yönetmek için kullanabileceğiniz bir örneği burada verilmiştir.
Bu rolün "Destek isteği katkıda bulunan" adlı ve nasıl Biz bu makalede özel role başvurmalıdır.

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Özetlenen adımları izleyin [bu videoyu](https://www.youtube.com/watch?v=-PaBaDmfwKI) aboneliğiniz için özel bir rol oluşturmak nasıl öğrenin.

## <a name="create-and-manage-support-requests-in-the-azure-portal"></a>Oluşturma ve Azure portalında destek isteklerini yönetme

Bir örnek atalım – abonelik "Visual Studio MSDN aboneliğiniz." sahibi olan
Joe kimin kaynak sahibi bazı bu abonelikte bulunan kaynak gruplarının ve okuma izni aboneliğe, eş ' dir.
Erişim, eş, Can, oluşturun ve bu abonelik altında kaynaklar için destek biletlerini yönetme olanağı vermek istiyor.

1. Abonelik gitmek için ilk adımdır ve "Ayarlar" altında kullanıcıların bir listesini görürsünüz. Kullanıcı abonelik üzerinde okuyucu erişimi olan Can'i tıklatın ve şimdi yeni bir özel rolü kendisine atayın.

    ![Rol Ekle](./media/create-manage-support-requests-using-access-control/add-role.png)

2. "Ekle" altında "Kullanıcılar" dikey tıklayın. Özel rol "Destek isteği katkıda bulunan" rollerinin listesinden seçin

    ![Destek katkıda bulunan rolü Ekle](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. Rol adı seçtikten sonra "Kullanıcı Ekle" yi tıklatın ve Can'ın e-posta kimlik bilgilerini girin. "Seç"'i tıklatın

    ![Kullanıcı ekle](./media/create-manage-support-requests-using-access-control/add-users.png)

4. Devam etmek için "Tamam" düğmesini tıklatın

    ![Erişim Ekle](./media/create-manage-support-requests-using-access-control/add-access.png)

5. Şimdi yeni eklenen özel rol "Destek isteği katkıda bulunan" sahibi olduğunuz abonelik altında kullanıcıyla bakın

    ![Eklenen kullanıcı](./media/create-manage-support-requests-using-access-control/user-added.png)

    Joe Portalı'nda oturum açtığında, kendisinin eklendiği abonelik görür.

7. Joe "Yeni destek isteği" "Yardım ve desteği" dikey penceresinden tıklar ve "Visual Studio Ultimate ile MSDN için" destek istekleri oluşturabilirsiniz

    ![Yeni destek isteği](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. "Tüm istekleri desteği" tıklatarak CAN Bu abonelik için oluşturduğunuz destek isteklerini listesini görebilir ![durum görünümü ayrıntıları](./media/create-manage-support-requests-using-access-control/case-details-view.png)

## <a name="remove-support-request-access-in-the-azure-portal"></a>Azure portalında destek isteği erişimi Kaldır

Oluşturmak ve Destek isteklerini yönetmek için bir kullanıcı için erişim vermek üzere mümkün olduğu gibi kullanıcı da için erişimi kaldırmak mümkündür.
Oluşturma ve Destek isteklerini yönetmek için aboneliği Git özelliğini kaldırmak için "Ayarlar"'a tıklayın ve kullanıcıya (Bu örnekte, Can) tıklayın.
Rol adı, "Destek isteği katkıda bulunan" sağ tıklatın ve "Kaldır"

![Destek isteği erişimi Kaldır](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

Joe portalında oturum ve bir destek isteği oluşturmak çalışırsa, kendisine şu hatayla karşılaştığında

![Abonelik hata-2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

Joe herhangi göremezsiniz "Tüm istekleri desteği" tıklatır dağıtırken istekleri desteği

![Servis Talebi Ayrıntıları görünümü-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
