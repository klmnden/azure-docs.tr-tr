---
title: StorSimple için rol tabanlı erişim denetimini kullanma | Microsoft Docs
description: Azure rol tabanlı erişim denetimi (RBAC) StorSimple bağlamında kullanmayı açıklar.
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
ms.openlocfilehash: be0c1611856a1fa68d20696c32b5fadcd8572004
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793620"
---
# <a name="role-based-access-control-for-storsimple"></a>StorSimple için rol tabanlı erişim denetimi

Bu makalede, Azure rol tabanlı Access Control (RBAC), StorSimple cihazınız için nasıl kullanılabileceğini ilgili kısa bir açıklama sağlar. RBAC, Azure için ayrıntılı erişim yönetimi sunar. RBAC, herkesin vermek yerine işlerini yapmak için StorSimple kullanıcılara erişimi yalnızca doğru miktarda sınırsız erişimi vermek için kullanın. Azure'da erişim yönetimini temel bilgileri hakkında daha fazla bilgi için bkz. [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../role-based-access-control/overview.md).

Bu makale, daha sonra Azure portalında ya da güncelleştirme 3.0 çalıştıran StorSimple 8000 serisi cihazlar için geçerlidir.

## <a name="rbac-roles-for-storsimple"></a>RBAC rolleri için StorSimple

RBAC, rollerine göre atanabilir. Rol, ortamda kullanılabilir kaynaklara dayalı belirli izin düzeyleri emin olun. StorSimple kullanıcıların seçebileceği rolleri iki tür vardır: yerleşik veya özel.

* **Yerleşik roller** -yerleşik roller sahip, katkıda bulunan, okuyucu veya kullanıcı erişimi Yöneticisi olabilir. Daha fazla bilgi için [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md).

* **Özel roller** -yerleşik roller ihtiyaçlarınıza uygun değil, StorSimple için özel bir RBAC rolleri oluşturabilirsiniz. Özel bir RBAC rolü oluşturmak için yerleşik bir rol ile başlatmak, düzenlemek ve ortamında içeri aktarın. Rolünün karşıya yükleme ve indirme, Azure PowerShell veya Azure CLI kullanılarak yönetilir. Daha fazla bilgi için [rol tabanlı erişim denetimi için özel roller oluşturma](../role-based-access-control/custom-roles.md).

Azure portalında StorSimple cihaz kullanıcı için kullanılabilen farklı rolleri görüntülemek için StorSimple cihaz Yöneticisi hizmetinize gidin ve ardından Git **erişim denetimi (IAM) > rolleri**.


## <a name="create-a-custom-role-for-storsimple-infrastructure-administrator"></a>StorSimple altyapı yöneticisi için özel bir rol oluşturun

Aşağıdaki örnekte, biz yerleşik rolü ile Başlat **okuyucu** tüm kaynak kapsamlar görüntülemek için ancak bunları düzenlemek veya yenilerini oluşturma olanağı sağlar. Biz sonra yeni bir özel rol StorSimple altyapı yöneticisi oluşturmak için bu rol genişletme Bu rol, StorSimple cihazlar için altyapı yönetebilen kullanıcılara atanır.

1. Windows PowerShell'i yönetici olarak çalıştırın.

2. Azure'da oturum açın.

    `Connect-AzureRmAccount`

3. Okuyucu rolü, bilgisayarınızda bir JSON şablon olarak dışarı aktarın.

    ```powershell
    Get-AzureRMRoleDefinition -Name "Reader"

    Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
    ```

4. JSON dosyasını Visual Studio'da açın. Tipik bir RBAC rolü üç ana bölümden oluşur gördüğünüz **eylemleri**, **NotActions**, ve **AssignableScopes**.

    İçinde **eylem** bölümü, tüm bu rol için izin verilen işlemler listelenmiştir. Her eylem bir kaynak Sağlayıcısı'ndan atanır. Bir StorSimple altyapı yöneticisi kullanan `Microsoft.StorSimple` kaynak sağlayıcısı.

    Tüm kaynak sağlayıcıları kullanılabilir ve kaydedilen aboneliğinizdeki görmek için PowerShell kullanın.

    `Get-AzureRMResourceProvider`

    Ayrıca, kaynak sağlayıcıları yönetmek tüm kullanılabilir için PowerShell cmdlet'lerini de denetleyebilirsiniz.

    İçinde **NotActions** bölümler, tüm özel bir RBAC rolü için kısıtlı eylemleri listelenir. Bu örnekte, hiçbir Eylemler kısıtlıdır.
    
    Altında **AssignableScopes**, abonelik kimlikleri listelenir. RBAC rolü kullanıldığı açık bir abonelik kimliği içerdiğinden emin olun. Doğru abonelik kimliği belirtilmezse, aboneliğinizdeki bir role içeri aktarmak için izin verilmez.

    Önceki konuları göz önünde bulundurarak dosyasını düzenleyin.

    ```json
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

6. Özel bir RBAC rolü ortamına geri alın.

    `New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"`


Bu rol artık rolleri listesinde görünmelidir **erişim denetimi** dikey penceresi.

![Görünüm RBAC rolleri](./media/storsimple-8000-role-based-access-control/rbac-role-types.png)

Daha fazla bilgi için Git [özel roller](../role-based-access-control/custom-roles.md).

### <a name="sample-output-for-custom-role-creation-via-the-powershell"></a>Örnek çıktı PowerShell aracılığıyla özel rol oluşturma

```powershell
Connect-AzureRmAccount
```

```Output
Environment           : AzureCloud
Account               : john.doe@contoso.com
TenantId              : <tenant_ID>
SubscriptionId        : <subscription_ID>
SubscriptionName      : Internal Consumption
CurrentStorageAccount :
```

```powershell
Get-AzureRMRoleDefinition -Name "Reader"
```

```Output
Name             : Reader
Id               : <guid>
IsCustom         : False
Description      : Lets you view everything, but not make any changes.
Actions          : {*/read}
NotActions       : {}
AssignableScopes : {/}
```

```powershell
Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\ssrbaccustom.json
New-AzureRMRoleDefinition -InputFile "C:\ssrbaccustom.json"
```

```Output
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
```

## <a name="add-users-to-the-custom-role"></a>Kullanıcılar için özel Rol Ekle

Rol atamasının kapsamı olan kaynak, kaynak grubu veya abonelik içinden erişim verebilirsiniz. Erişim sağlarken, üst düğümüne erişim izni unutmayın alt tarafından devralınır. Daha fazla bilgi için Git [rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

1. Git **erişim denetimi (IAM)**. Tıklayın **+ Ekle** erişim denetimi dikey penceresinde.

    ![Erişim için RBAC rolü Ekle](./media/storsimple-8000-role-based-access-control/rbac-add-role.png)

2. Atamak istediğiniz rolü seçin, bu durumda **StorSimple altyapı yöneticisi**.

3. Dizininizde, erişim vermek istediğiniz kullanıcı, grup veya uygulamayı seçin. Görünen adlar, e-posta adresleri ve nesne tanımlayıcıları ile dizinde arama yapabilirsiniz.

4. Seçin **Kaydet** atamayı oluşturmak için.

    ![RBAC rolü için izinleri ekleme](./media/storsimple-8000-role-based-access-control/rbac-create-role-infra-admin.png)

Bir **kullanıcı ekleme** bildirim ilerleme durumunu izler. Kullanıcı başarıyla eklendikten sonra kullanıcılar erişim denetimi listesi güncelleştirilir.

## <a name="view-permissions-for-the-custom-role"></a>Özel rolü için izinleri görüntüle

Bu rolü oluşturduktan sonra Azure portalında bu role sahip ilişkilendirilmiş izinleri görüntüleyebilirsiniz.

1. Bu rolle ilişkilendirilen izinleri görüntülemek için Git **erişim denetimi (IAM) > rolleri > StorSimple altyapı yöneticisi**. Bu roldeki kullanıcılar listesi görüntülenir.

2. StorSimple altyapı yönetici bir kullanıcı seçin ve tıklayın **izinleri**.

    ![StorSimple altyapısı Yönetici rolü için izinleri görüntüle](./media/storsimple-8000-role-based-access-control/rbac-roles-view-permissions.png)

3. Bu rolle ilişkilendirilen izinleri görüntülenir.

    ![StorSimple altyapısı yönetici rolünde kullanıcıları görüntüle](./media/storsimple-8000-role-based-access-control/rbac-infra-admin-permissions1.png)


## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [iç ve dış kullanıcılar için özel roller atama](../role-based-access-control/role-assignments-external-users.md).