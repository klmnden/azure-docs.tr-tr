---
title: Azure rol tabanlı erişim denetimi (Destek isteklerini oluşturmak ve yönetmek için RBAC) için erişim haklarını denetleme | Microsoft Docs
description: Azure rol tabanlı erişim denetimi (Destek isteklerini oluşturmak ve yönetmek için RBAC) için erişim haklarını denetleme
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: azure
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: d98d0637c6d520193b11f4267c59016772ef063a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60809925"
---
# <a name="azure-role-based-access-control-rbac-to-control-access-rights-to-create-and-manage-support-requests"></a>Azure rol tabanlı erişim denetimi (Destek isteklerini oluşturmak ve yönetmek için RBAC) için erişim haklarını denetleme

[Rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) Azure için ayrıntılı erişim yönetimi sağlar.
Azure portalında isteği oluşturmayı destekleyen [portal.azure.com](https://portal.azure.com), Azure'nın RBAC modelinde kimin oluşturabilir ve Destek isteklerini yönetin tanımlamak için kullanır.
Kullanıcıları, grupları ve uygulamaları bir abonelik, kaynak grubu veya bir kaynak olarak belirli bir kapsama, uygun RBAC rolü atanarak erişim verilir.

Bir örneği ele alalım: Abonelik kapsamında Okuma izinleri olan bir kaynak grubu sahibi Web siteleri, sanal makineler ve alt ağları gibi bir kaynak grubu altındaki tüm kaynakları yönetebilir.
Ancak, sanal makine kaynağı karşı bir destek isteği oluşturmayı denediğinizde, şu hatayla karşılaşırsanız

![Abonelik hata](./media/create-manage-support-requests-using-access-control/subscription-error.png)

Destek isteği Yönetimi'nde iznine veya destek isteklerini oluşturmak ve yönetmek için abonelik kapsamında destek eylem Microsoft.Support/* sahip bir rol yazmanız gerekir.

Aşağıdaki makalede, Azure portalında destek isteklerini oluşturmak ve yönetmek için Azure'nın özel rol tabanlı erişim denetimi (RBAC) nasıl kullanabileceğinizi açıklar.

## <a name="getting-started"></a>Başlarken

Yukarıdaki örneği kullanarak, abonelik sahibi tarafından abonelik üzerinde özel bir RBAC rolü atandıysa, kaynağınız için bir destek isteği oluşturmak mümkün olacaktır.
[Özel bir RBAC rollerini](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) Azure PowerShell, Azure komut satırı arabirimi (CLI) ve REST API'si kullanılarak oluşturulabilir.

Actions özelliği özel bir rol, rol erişim veren Azure işlemleri belirtir.
Destek isteği yönetimi için özel bir rol oluşturmak için rolü Microsoft.Support/* eylemi olması gerekir

Destek isteklerini oluşturmak ve yönetmek için kullanabileceğiniz özel bir rol örneği aşağıda verilmiştir.
Biz bu rol "Destek isteği Katılımcısı" adlı ve nasıl özel rolü bu makaledeki diyoruz.

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

Özetlenen adımları izleyin [bu videoyu](https://www.youtube.com/watch?v=-PaBaDmfwKI) aboneliğiniz için özel bir rol oluşturma hakkında bilgi edinmek için.

## <a name="create-and-manage-support-requests-in-the-azure-portal"></a>Azure portalında destek isteklerini oluşturmak ve yönetmek

Bir örneği ele alalım: "Visual Studio MSDN aboneliği." aboneliğin sahibi
ALi, bir kaynak sahibi bazı bu Abonelikteki kaynak grupları ve okuma izni aboneliğe eşdüzey olur.
Eşdüzey, ALi, oluşturma ve bu abonelik kapsamındaki kaynaklar için destek biletlerini yönetme olanağı erişmesini istiyor.

1. Aboneliklere Git için ilk adımıdır ve "Ayarlar altında" kullanıcıların listesini görürsünüz. Kullanıcı abonelik üzerinde okuyucu erişimi olan Joe tıklayın ve şimdi kendisine yeni bir özel rol atayın.

    ![Rol Ekle](./media/create-manage-support-requests-using-access-control/add-role.png)

2. "Ekle" altındaki "Kullanıcılar" dikey penceresinde'yi tıklatın. Özel rol "Destek isteği Katılımcısı" rol listesinden seçin

    ![Destek katkıda bulunan rolü Ekle](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. Rol adı'nı seçtikten sonra "Kullanıcı Ekle" ye tıklayın ve Can'ın e-posta kimlik bilgilerini girin. "Seç"e tıklayın

    ![Kullanıcı ekle](./media/create-manage-support-requests-using-access-control/add-users.png)

4. Devam etmek için "Tamam" düğmesini tıklatın

    ![Erişim Ekle](./media/create-manage-support-requests-using-access-control/add-access.png)

5. "Destek isteği Katılımcısı" sahip olduğunuz abonelik altında yeni eklenen özel rol ile kullanıcı göreceksiniz

    ![Kullanıcı eklendi](./media/create-manage-support-requests-using-access-control/user-added.png)

    ALi Portalı'nda oturum açtığında, kendisinin eklendiği abonelik görür.

7. ALi, "Yeni destek isteği" "Yardım ve Destek" dikey penceresinden tıklar ve "Visual Studio Ultimate ile MSDN için" destek istekleri oluşturmak

    ![Yeni destek isteği](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. "Tüm destek istekleri" tıklayarak ALi Bu abonelik için oluşturulan destek istekleri listesi görürsünüz ![durum görünümü ayrıntıları](./media/create-manage-support-requests-using-access-control/case-details-view.png)

## <a name="remove-support-request-access-in-the-azure-portal"></a>Azure portalında destek isteği erişimi Kaldır

Destek isteklerini oluşturmak ve yönetmek için bir kullanıcı için erişim vermek mümkün olduğu gibi kullanıcı da erişimi kaldırmak mümkündür.
Oluşturma ve Destek isteklerini yönetin, aboneliklere Git özelliği kaldırmak için "Ayarlar"'a tıklayın ve kullanıcı (Bu durumda, ALi) tıklayın.
"Destek isteği Katılımcısı" rol adına sağ tıklayın ve "Kaldır" a tıklayın

![Destek isteği erişimi Kaldır](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

ALi portalında bir destek isteği oluşturmak çalışır, kendisi aşağıdaki hatayla karşılaşan

![Abonelik hata-2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

ALi herhangi göremezsiniz he "Tüm destek istekleri" tıkladığında destek istekleri

![Servis Talebi Ayrıntıları görünümü-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
