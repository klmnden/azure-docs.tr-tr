---
title: StorSimple için rol tabanlı erişim denetimini kullanma | Microsoft Docs
description: StorSimple bağlamında Azure rol tabanlı erişim denetimi (RBAC) kullanmayı açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2017
ms.author: alkohli
ms.openlocfilehash: 674f4ec53300643450d8a576db6fcb50e86dd9d2
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="role-based-access-control-for-storsimple"></a>StorSimple için rol tabanlı erişim denetimi

Bu makale Azure rol tabanlı erişim denetimi (RBAC) StorSimple cihazınız için nasıl kullanılabileceğini ilgili kısa bir açıklama sağlar. RBAC, Azure için ayrıntılı erişim yönetimi sağlar. RBAC herkes vermiş yerine işlerini yapmak için StorSimple kullanıcılara erişimi yalnızca doğru miktarda sınırsız erişimi vermek için kullanın. Azure erişim yönetimi temel kavramları hakkında daha fazla bilgi için bkz: [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../role-based-access-control/overview.md).

Bu makalede, StorSimple 8000 serisi cihazlar Update 3.0 çalıştıran veya daha sonra Azure Portalı'ndaki geçerlidir.

## <a name="rbac-roles-for-storsimple"></a>StorSimple için RBAC rolleri

RBAC rollere göre atanabilir. Rolleri ortamında kullanılabilir kaynaklara dayalı belirli izin düzeyleri emin olun. StorSimple kullanıcıların seçebileceği rolleri iki tür vardır: yerleşik veya özel.

* **Yerleşik roller** -yerleşik roller sahip, katkıda bulunan, okuyucu veya kullanıcı erişimi Yöneticisi olabilir. Daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/overview.md#built-in-roles).

* **Özel roller** -yerleşik roller gereksinimlerinize göre değil, StorSimple için özel RBAC rolleri oluşturabilirsiniz. Bir özel RBAC rolü oluşturmak için sahip yerleşik bir rol Başlat, düzenlemek ve ortamı geri alma. İndirme ve yükleme rolünün Azure PowerShell veya Azure CLI kullanarak yönetilir. Daha fazla bilgi için bkz: [rol tabanlı erişim denetimi için özel roller oluşturmanızı](../role-based-access-control/custom-roles.md).

Azure portalında bir StorSimple cihaz kullanıcı için kullanılabilir farklı roller görüntülemek için StorSimple cihaz Yöneticisi hizmetinize gidin ve gidin **erişim denetimi (IAM) > rolleri**.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>Özel bir rol için StorSimple altyapı yöneticisi oluşturun

Aşağıdaki örnekte, biz yerleşik rolüyle Başlat **okuyucu** tüm kaynak kapsamları görüntülemek üzere ancak bunları düzenleyin veya yeni kampanya oluşturmak kullanıcıların sağlar. Biz sonra yeni bir özel rolü StorSimple altyapı yöneticisine oluşturmak için bu rol genişletme Bu rol, StorSimple cihazlar için altyapı yönetebilen kullanıcılara atanır.

1. Windows PowerShell'i yönetici olarak çalıştırın.

2. Azure'da oturum açın.

    `Connect-AzureRmAccount`

3. Okuyucu rolü, bilgisayarınızdaki JSON şablon olarak dışarı aktarın.

    ```
    Get-AzureRMRoleDefinition -Name "Reader"

    Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. JSON dosyasını Visual Studio'da açın. Tipik bir RBAC rolü, üç ana bölümden oluşur gördüğünüz **Eylemler**, **NotActions**, ve **AssignableScopes**.

    İçinde **eylem** bölümünde, tüm bu rol için izin verilen işlemler listelenmiştir. Her eylem, bir kaynak sağlayıcısından atanır. Bir StorSimple altyapı yöneticisi için kullanmak `Microsoft.StorSimple` kaynak sağlayıcısı.

    Tüm kaynak sağlayıcıları kullanılabilir ve aboneliğinizde kayıtlı görmek için PowerShell kullanın.

    `Get-AzureRMResourceProvider`

    Kaynak sağlayıcıları yönetmek tüm kullanılabilir için PowerShell cmdlet'leri de denetleyebilirsiniz.

    İçinde **NotActions** bölümlerinde, tüm belirli bir RBAC rolü için kısıtlı eylemleri listelenir. Bu örnekte, hiçbir eylem kısıtlanır.
    
    Altında **AssignableScopes**, abonelik kimlikleri listelenir. RBAC rolü kullanıldığı açık abonelik kimliği içerdiğinden emin olun. Doğru abonelik kimliği belirtilmezse, aboneliğinizde rolünü almasına izin verilmiyor.

    Önceki konuları göz önünde bulundurarak dosyasını düzenleyin.

    ```
    {
        "Name":  "StorSimple Infrastructure Admin",
        "Id":  "<guid>",
        "IsCustom":  true,
        "Description":  "Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install, download etc.",
        "Actions":  [
                        "Microsoft.StorSimple/managers/alerts/read",
                        "Microsoft.StorSimple/managers/devices/volumeContainers/read",
                        "Microsoft.StorSimple/managers/devices/jobs/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/read",
                        "Microsoft.StorSimple/managers/devices/alertSettings/write",
                        "Microsoft.StorSimple/managers/clearAlerts/action",
                        "Microsoft.StorSimple/managers/devices/networkSettings/read",
                        "Microsoft.StorSimple/managers/devices/publishSupportPackage/action",
                        "Microsoft.StorSimple/managers/devices/scanForUpdates/action",
                        "Microsoft.StorSimple/managers/devices/metrics/read"

                    ],
        "NotActions":  [

                    ],
        "AssignableScopes":  [
                                "/subscriptions/<subscription_ID>/"
                            ]
    }
    ```

6. Özel RBAC rolü ortamına geri alın.

    `New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Bu rol artık rollerinde listesinde görünmesi gereken **erişim denetimi** dikey.

![Görünüm RBAC rolleri](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Daha fazla bilgi için Git [özel roller](../role-based-access-control/custom-roles.md).

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>Örnek çıktı PowerShell aracılığıyla özel rol oluşturma

```
PS C:\WINDOWS\system32> Connect-AzureRmAccount

Environment           : AzureCloud
Account               : john.doe@contoso.com
TenantId              : <tenant_ID>
SubscriptionId        : <subscription_ID>
SubscriptionName      : Internal Consumption
CurrentStorageAccount :

PS C:\WINDOWS\system32> Get-AzureRMRoleDefinition -Name "Reader"

Name             : Reader
Id               : <guid>
IsCustom         : False
Description      : Lets you view everything, but not make any changes.
Actions          : {*/read}
NotActions       : {}
AssignableScopes : {/}

PS C:\WINDOWS\system32> Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json

PS C:\WINDOWS\system32> New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"

Name             : StorSimple Infrastructure Admin
Id               : <tenant_ID>
IsCustom         : True
Description      : Lets you view everything, but not make any changes except for Clear alerts, Clear settings, install,
                   download etc.
Actions          : {Microsoft.StorSimple/managers/alerts/read,
                   Microsoft.StorSimple/managers/devices/volumeContainers/read,
                   Microsoft.StorSimple/managers/devices/jobs/read,
                   Microsoft.StorSimple/managers/devices/alertSettings/read...}
NotActions       : {}
AssignableScopes : {/subscriptions/<subscription_ID>/}

PS C:\WINDOWS\system32>
```

## <a name="add-users-to-the-custom-role"></a>Özel rolüne kullanıcılar ekleyin

Rol atamasının kapsamı olan kaynak, kaynak grubu veya abonelik içinden erişim verebilirsiniz. Erişim sağlarken üst düğümü erişim izni aklınızda ayı alt tarafından devralınır. Daha fazla bilgi için Git [kaynak hiyerarşisi ve erişim devralma](../role-based-access-control/overview.md#resource-hierarchy-and-access-inheritance).

1. Git **erişim denetimi (IAM)**. Tıklatın **+ Ekle** erişim denetimi dikey.

    ![Erişim için RBAC rolü Ekle](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Atamak istediğiniz rolü seçin, bu durumda **StorSimple altyapı yönetici**.

3. Dizininizde, erişim vermek istediğiniz kullanıcı, grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.

4. Seçin **kaydetmek** atamayı oluşturmak için.

    ![RBAC rolü izinleri ekleyin](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

Bir **kullanıcı ekleme** bildirim ilerleme durumunu izler. Kullanıcı başarıyla eklendikten sonra kullanıcı erişim denetimi listesi güncelleştirilir.

## <a name="view-permissions-for-the-custom-role"></a>Özel rol izinlerini görüntüleme

Bu rolü oluşturulduktan sonra Azure portalında bu rolüyle ilişkili izinleri görüntüleyebilirsiniz.

1. Bu rolle ilişkilendirilen izinleri görüntülemek için Git **erişim denetimi (IAM) > rolleri > StorSimple altyapı yönetici**. Bu roldeki kullanıcılar listesi görüntülenir.

2. StorSimple altyapı yönetici bir kullanıcı seçin ve tıklatın **izinleri**.

    ![StorSimple altyapısı Yönetici rolü için izinleri görüntüleme](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. Bu rol ile ilişkili izinleri görüntülenir.

    ![StorSimple altyapısı yönetici roldeki kullanıcıları görüntüle](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [iç ve dış kullanıcılar için özel roller atama](../role-based-access-control/role-assignments-external-users.md).

