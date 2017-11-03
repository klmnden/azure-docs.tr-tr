---
title: "Konuk işletim sistemi ailesi 1 devre dışı bırakma dikkat edin. | Microsoft Docs"
description: "Ne zaman Azure konuk işletim sistemi ailesi 1 kullanımdan oldu ve etkilenip etkilenmediğini belirleme hakkında bilgi sağlar"
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 37b422e9-0713-4a81-a942-f553ef478064
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: 3178a09dab1cb972a3460d54dc9908fb95cce68b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="guest-os-family-1-retirement-notice"></a>Konuk işletim sistemi ailesi 1 kullanımdan kaldırma bildirimi
İşletim sistemi ailesi 1 kullanımdan ilk 1 Haziran 2013'te tanıtıldı.

**2 Eylül 2014** Azure konuk işletim sistemi (konuk işletim sistemi) ailesi Windows Server 2008 işletim sistemi temelinde, 1.x resmi olarak Çekildi. Yeni hizmetler dağıtmak veya var olan hizmetleri ailesi 1'i kullanarak yükseltmek için tüm denemeleri konuk işletim sistemi ailesi 1 devre dışı olduğunu bildiren bir hata iletisi ile başarısız olur.

**3 Kasım 2014** konuk işletim sistemi ailesi 1 için genişletilmiş destek sona erdi ve tam olarak Çekildi. Tüm hizmetler hala ailesi 1 üzerinde etkilenir. Biz bu hizmetleri herhangi bir zamanda vermeyebilir. Hizmetlerinizin el ile bunları kendiniz yükseltmeniz sürece çalışmaya devam edecek garantisi yoktur.

Ek sorularınız varsa, ziyaret [Cloud Services forumlarında](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) veya [Azure desteğine başvurun](https://azure.microsoft.com/support/options/).

## <a name="are-you-affected"></a>Etkilenen?
Aşağıdakilerden herhangi biri geçerliyse, bulut hizmetlerinize etkilenir:

1. Değerine sahip "osFamily"1"bulut hizmetiniz için ServiceConfiguration.cscfg dosyasında açıkça belirtilen =.
2. Açıkça bulut hizmetiniz için ServiceConfiguration.cscfg dosyasında belirtilen osFamily için bir değere sahip değil. Şu anda, bu durumda varsayılan değerini "1" sistemi kullanır.
3. Azure portalı ve konuk işletim sistemi ailesi değer "Windows sunucusu 2008" olarak listelenir.

Yapmanız gerekenler rağmen bulut hizmetlerinizi, hangi işletim sistemi ailesi çalıştığı bulmak için aşağıdaki komut dosyası Azure PowerShell'de çalıştırabilirsiniz [Azure PowerShell ayarlamak](/powershell/azureps-cmdlets-docs) ilk. Komut dosyası hakkında daha fazla bilgi için bkz: [Azure konuk işletim sistemi ailesi 1 son, ömrü: Haziran 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Betik çıkışında osFamily sütun boş veya "1" içeriyorsa, bulut hizmetlerinize işletim sistemi ailesi 1 kullanımdan tarafından etkilenir.

## <a name="recommendations-if-you-are-affected"></a>Etkilenen varsa önerileri
Bulut hizmeti rollerinizi desteklenen konuk işletim sistemi ailelerinin birine geçirmek öneririz:

**Konuk işletim sistemi ailesi 4.x** -Windows Server 2012 R2 *(önerilir)*

1. Uygulamanız .NET framework 4.0, 4.5 veya 4.5.1 SDK 2.1 veya üzeri kullandığından emin olun.
2. "4" için osFamily özniteliği ServiceConfiguration.cscfg dosyasında ayarlanan ve bulut hizmeti yeniden dağıtın.

**Konuk işletim sistemi ailesi 3.x** -Windows Server 2012

1. Uygulamanız .NET framework 4.0 veya 4.5 ile SDK 1.8 veya sonraki sürümünü kullandığından emin olun.
2. "3" osFamily özniteliği ServiceConfiguration.cscfg dosyasında ayarlanan ve bulut hizmeti yeniden dağıtın.

**Konuk işletim sistemi ailesi 2.x** -Windows Server 2008 R2

1. Uygulamanızı SDK 1.3 kullandığından emin olun ve yukarıdaki .NET framework 3.5 veya 4.0 ile.
2. "2" osFamily özniteliği ServiceConfiguration.cscfg dosyasında ayarlanan ve bulut hizmeti yeniden dağıtın.

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>3 Kas 2014 konuk işletim sistemi ailesi 1 için genişletilmiş destek sona erdi
Bulut Hizmetleri konuk işletim sistemi ailesinde 1 artık desteklenmemektedir. Mümkün olan en kısa sürede hizmet kesintisi yaşamamak için ailesi 1 geçirin.  

## <a name="next-steps"></a>Sonraki adımlar
En son gözden [konuk işletim sistemi sürümleri](cloud-services-guestos-update-matrix.md).
