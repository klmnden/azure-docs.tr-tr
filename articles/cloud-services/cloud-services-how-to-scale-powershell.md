---
title: Windows PowerShell'de Azure bulut hizmeti ölçeklendirin | Microsoft Docs
description: (Klasik) Bir web rolü veya çalışan rolü veya Azure'da ölçeklendirmek için PowerShell kullanmayı öğrenin.
services: cloud-services
documentationcenter: ''
author: mmccrory
manager: timlt
editor: ''
ms.assetid: ee37dd8c-6714-4c61-adb8-03d6bbf76c9a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: mmccrory
ms.openlocfilehash: a7ae8ff202d403dff19b8c9a6a09492235db27ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23842954"
---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a>Bir bulut hizmeti PowerShell'de ölçeklendirme

Bir web rolü veya çalışan rolü giriş veya çıkış ekleyerek veya kaldırarak örnekleri ölçeklendirmek için Windows PowerShell'i kullanabilirsiniz.  

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Aboneliğinizi PowerShell aracılığıyla herhangi bir işlem gerçekleştirmeden önce oturum gerekir:

```powershell
Add-AzureAccount
```

Hesabınızla ilişkili birden çok aboneliğiniz varsa, bulut hizmetinizin bulunduğu bağlı olarak geçerli abonelik değiştirmeniz gerekebilir. Geçerli aboneliğe denetlemek için çalıştırın:

```powershell
Get-AzureSubscription -Current
```

Geçerli aboneliğe değiştirmeniz gerekiyorsa, çalıştırın:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a>Geçerli örnek sayısını rolünüz için denetleyin

Rolünüze geçerli durumunu denetlemek için çalıştırın:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

Geçerli işletim sistemi sürümü ve örneğindeki sayımına dahil rolünün hakkında bilgi geri almanız gerekir. Bu durumda, rolü tek bir örneği vardır.

![Rolü hakkında bilgi](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a>Daha fazla örnekleri ekleyerek rolünün ölçeğini genişletme

Rolünün ölçeğini genişletmek için örnek istenen sayısı geçirmek **sayısı** parametresi **kümesi AzureRole** cmdlet:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

Cmdlet blokları yeni örnekleri sırasında kısa bir süre içinde sağlanan ve başlatıldı. Yeni bir PowerShell penceresi ve çağrı açarsanız, bu süre boyunca **Get-AzureRole** daha önce gösterildiği gibi yeni hedef örneği sayısını görürsünüz. Ve Portalı'nda rol durumu inceleyin, başlamasını yeni örnek görmeniz gerekir:

![VM örneği portalında başlatılıyor](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

Yeni örnekleri başladıktan sonra cmdlet başarıyla döndürür:

![Rol örneği artış başarılı](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a>Rol örnekleri kaldırarak ölçeklendirme

Aynı şekilde örnekleri kaldırarak bir rolde ölçeklendirebilirsiniz. Ayarlama **sayısı** parametresini **kümesi AzureRole** istediğinizi ölçek işlemi tamamlandıktan sonra örnek sayısı için.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell bulut hizmetlerinden için Otomatik ölçek yapılandırmak mümkün değil. Bunu yapmak için bkz: [ölçek bir bulut hizmeti otomatik olarak nasıl](cloud-services-how-to-scale-portal.md).
