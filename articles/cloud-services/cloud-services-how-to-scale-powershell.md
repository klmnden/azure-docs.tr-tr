---
title: Windows PowerShell'de bir Azure bulut hizmetini ölçekleme | Microsoft Docs
description: (Klasik) Azure'da bir web rolü veya çalışan rolü veya dışa ölçeklendirme PowerShell kullanmayı öğrenin.
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
ms.author: memccror
ms.openlocfilehash: 6c013687faaca33938d0b27303e9b1252482fcb9
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65963324"
---
# <a name="how-to-scale-a-cloud-service-in-powershell"></a>PowerShell'de bir bulut hizmetini ölçekleme

Bir web rolü veya çalışan rolü veya örnekleri ekleyerek veya kaldırarak göre ölçeklendirmek için Windows PowerShell'i kullanabilirsiniz.  

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Aboneliğinizde PowerShell üzerinden herhangi bir işlem gerçekleştirmeden önce oturum gerekir:

```powershell
Add-AzureAccount
```

Hesabınızla ilişkili birden fazla aboneliğiniz varsa, bulut hizmetinizin bulunduğu bağlı olarak geçerli abonelikte değişiklik yapmanız gerekebilir. Geçerli abonelik denetlemek için çalıştırın:

```powershell
Get-AzureSubscription -Current
```

Geçerli abonelik değiştirmeniz gerekiyorsa, çalıştırın:

```powershell
Set-AzureSubscription -SubscriptionId <subscription_id>
```

## <a name="check-the-current-instance-count-for-your-role"></a>Rolünüz için geçerli örnek sayısını denetleyin

Geçerli rolünüz durumunu denetlemek için çalıştırın:

```powershell
Get-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>'
```

Geçerli işletim sistemi sürümü ve örneğindeki sayımına dahil rolünün hakkında bilgi geri almalısınız. Bu durumda, rol tek bir örneği vardır.

![Rolü hakkında bilgi](./media/cloud-services-how-to-scale-powershell/get-azure-role.png)

## <a name="scale-out-the-role-by-adding-more-instances"></a>Daha fazla örnek ekleyerek rolünün ölçeğini genişletme

Rolünün ölçeğini genişletmek için istediğiniz sayıda örnekleri olarak geçirmek **sayısı** parametresi **kümesi AzureRole** cmdlet:

```powershell
Set-AzureRole -ServiceName '<your_service_name>' -RoleName '<your_role_name>' -Slot <target_slot> -Count <desired_instances>
```

Cmdlet blokları yeni örnekleri sırasında kısa bir süre içinde sağlanabilir ve başlatıldı. Yeni bir PowerShell penceresi ve çağrı açarsanız, bu süre boyunca **Get-AzureRole** daha önce gösterildiği gibi yeni hedef örnek sayısını görürsünüz. Ve Portalı'nda rol durumu inceleyin, yeni örneği başlatılıyor görmeniz gerekir:

![Portalı ile bir sanal makine örneği](./media/cloud-services-how-to-scale-powershell/role-instance-starting.png)

Yeni örnekleri başladıktan sonra cmdlet başarıyla döndürür:

![Rol örneği artış başarılı](./media/cloud-services-how-to-scale-powershell/set-azure-role-success.png)

## <a name="scale-in-the-role-by-removing-instances"></a>Rol örnekleri kaldırarak ölçeklendirin

Bir rol örneklerinin aynı şekilde kaldırarak ölçeklendirebilirsiniz. Ayarlama **sayısı** parametresi **kümesi AzureRole** ölçek işlemi tamamlandıktan sonra istediğiniz örnek sayısı için.

## <a name="next-steps"></a>Sonraki adımlar

Powershell'den bulut Hizmetleri için otomatik ölçeklendirmeyi yapılandırma mümkün değildir. Bunu yapmak için bkz: [bir bulut hizmeti otomatik olarak nasıl](cloud-services-how-to-scale-portal.md).
