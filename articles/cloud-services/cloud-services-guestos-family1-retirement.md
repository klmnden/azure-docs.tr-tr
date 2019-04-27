---
title: Konuk işletim sistemi ailesi 1 kullanımdan kaldırma bildirimi | Microsoft Docs
description: Azure konuk işletim sistemi ailesi 1 kullanımdan kaldırma ne zaman oluştuğunu ve etkilenip etkilenmediğini belirleme hakkında bilgi sağlar
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: ''
ms.assetid: 37b422e9-0713-4a81-a942-f553ef478064
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: d6429766b6aac547fd99279659acb1067298e77c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60518961"
---
# <a name="guest-os-family-1-retirement-notice"></a>Konuk işletim sistemi ailesi 1 kullanımdan kaldırma bildirimi
İşletim sistemi ailesi 1 kullanımdan kaldırma, ilk 1 Haziran 2013'te tanıtıldı.

**2 Eylül 2014'ten** Azure konuk işletim sistemi (konuk OS) ailesi Windows Server 2008 işletim sistemi temelinde, 1.x, resmi olarak Çekildi. Yeni hizmetler dağıtmak veya mevcut hizmetlerden ailesi 1'i kullanarak yükseltmek için tüm girişimleri, konuk işletim sistemi ailesi 1 kullanımdan bildiren bir hata iletisiyle başarısız olur.

**3 Kasım 2014'ten** tamamen kullanımdan kaldırıldığında ve konuk işletim sistemi ailesi 1 için'genişletilmiş destek sona erdi. Aile 1 tüm hizmetlerinde yine de etkilenir. Biz bu hizmetlerin herhangi bir zamanda durdurabilir. Hizmetlerinizi, el ile bunları kendiniz yükseltmediğiniz sürece çalışmaya devam edecek bir garanti yoktur.

Başka sorularınız varsa [Cloud Services forumları](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) veya [Azure desteğine başvurun](https://azure.microsoft.com/support/options/).

## <a name="are-you-affected"></a>Etkilenir?
Cloud Services, aşağıdakilerden herhangi biri geçerliyse etkilenir:

1. Değerine sahip "osFamily ="1"açıkça bulut hizmetiniz için ServiceConfiguration.cscfg dosyasında belirtilen.
2. Açıkça bulut hizmetiniz için ServiceConfiguration.cscfg dosyasında belirtilen osFamily için bir değere sahip değil. Şu anda, bu durumda varsayılan değer olan "1" sistemi kullanır.
3. Azure portalı, konuk işletim sistemi ailesi değeri "Windows sunucusu olarak 2008" listelenir.

Gerekir ancak cloud services'ın hangi işletim sistemi ailesi çalıştığı bulmak için aşağıdaki betiği Azure PowerShell'de çalıştırabileceğiniz [Azure PowerShell ayarlama ayarlamak](/powershell/azureps-cmdlets-docs) ilk. Komut dosyası hakkında daha fazla bilgi için bkz. [Azure konuk işletim sistemi ailesi 1 uç, ömrü: Haziran 2014'te](https://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx).

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Betik çıktısı osFamily sütununda boşsa veya "1" içeren cloud services, işletim sistemi ailesi 1 kullanımdan kaldırma tarafından etkilenir.

## <a name="recommendations-if-you-are-affected"></a>Etkilenirse, önerileri
Bulut hizmeti rollerinizi desteklenen konuk işletim sistemi ailelerinin birine geçiş öneririz:

**Konuk işletim sistemi ailesi 4.x** -Windows Server 2012 R2 *(önerilir)*

1. Uygulamanızı .NET framework 4.0, 4.5 veya 4.5.1 SDK 2.1 veya üzeri kullandığından emin olun.
2. "4" osFamily özniteliği ServiceConfiguration.cscfg dosyasında ayarlamanız ve bulut hizmetinizi yeniden dağıtın.

**Konuk işletim sistemi ailesi 3.x** -Windows Server 2012

1. Uygulamanızı .NET framework 4.0 veya 4.5 ile SDK 1.8 veya sonraki kullandığından emin olun.
2. "3" osFamily özniteliği ServiceConfiguration.cscfg dosyasında ayarlamanız ve bulut hizmetinizi yeniden dağıtın.

**Konuk işletim sistemi ailesi 2.x** -Windows Server 2008 R2

1. Uygulama SDK'sı 1.3 kullandığından emin olun ve yukarıdaki .NET framework 3.5 veya 4.0 ile.
2. "2" osFamily özniteliği ServiceConfiguration.cscfg dosyasında ayarlamanız ve bulut hizmetinizi yeniden dağıtın.

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Konuk işletim sistemi ailesi 1 için'genişletilmiş destek sona erdi 3 Kasım 2014
Konuk işletim sistemi ailesi 1'te bulut Hizmetleri artık desteklenmemektedir. Olabildiğince çabuk hizmet kesintisi yaşamamak için devre dışı ailesi 1'in geçirin.  

## <a name="next-steps"></a>Sonraki adımlar
En son gözden [konuk işletim sistemi sürümlerinden](cloud-services-guestos-update-matrix.md).
