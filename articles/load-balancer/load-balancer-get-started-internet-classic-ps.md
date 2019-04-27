---
title: Internet'e yönelik Yük Dengeleyici - Azure PowerShell Klasik oluşturma
titlesuffix: Azure Load Balancer
description: Klasik modda PowerShell kullanarak İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: genlin
manager: cshepard
tags: azure-service-management
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: genli
ms.openlocfilehash: 54ef8782620a387d60454023d0e446279e467a99
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516978"
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>PowerShell’de İnternet’e yönelik yük dengeleyici (klasik) oluşturmaya başlama

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure kaynaklarıyla çalışmadan önce Azure'da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Azure Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarlarını](../azure-classic-rm.md) iyice anladığınızdan emin olun. Bu makalenin en üstündeki sekmelere tıklayarak farklı araçlarla ilgili belgeleri görüntüleyebilirsiniz. Bu makale, klasik dağıtım modelini kapsamaktadır. [Azure Resource Manager kullanarak İnternet’e yönelik yük dengeleyici oluşturma](load-balancer-get-started-internet-arm-ps.md) sayfasını da inceleyebilirsiniz.

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>PowerShell kullanarak iç yük dengeleyici kurma

PowerShell kullanarak bir yük dengeleyici kurmak için aşağıdaki adımları tamamlayın:

1. Azure PowerShell’i hiç kullanmadıysanız bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview) ve Azure'a giriş yapıp aboneliğinizi seçene kadar da tüm bu süreç boyunca tüm talimatları uygulayın.
2. Sanal makine oluşturduktan sonra PowerShell cmdlet’lerini kullanarak aynı bulut hizmeti içindeki bir sanal makineye yük dengeleyici ekleyebilirsiniz.

Aşağıdaki örnekte "web1" ve "web2" adlı sanal makinelere yük dengeleyici uç noktaları ekleyerek "mytestcloud" (veya myctestcloud.cloudapp.net) bulut hizmetine "webfarm" adlı bir yük dengeleyici kümesi eklersiniz. Yük dengeleyici 80 numaralı bağlantı noktasından ağ trafiğini alır ve TCP kullanarak yerel uç noktası (bu durumda 80 numaralı bağlantı noktası) tarafından tanımlanan sanal makineler arasında yük dengeleme gerçekleştirir.

### <a name="step-1"></a>1. Adım

İlk VM olan "web1" için yük dengeli uç nokta oluşturma

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a>2. Adım

İkinci VM olan "web2" için aynı yük dengeleyici kümesi adını kullanarak başka bir uç nokta oluşturma

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Yük dengeleyiciden sanal makineyi kaldırma

Bir sanal makine uç noktasını yük dengeleyiciden kaldırmak için Remove-AzureEndpoint komutunu kullanabilirsiniz

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a>Sonraki adımlar

[İç yük dengeleyici oluşturmaya başlayabilir](load-balancer-get-started-ilb-classic-ps.md) ve belirli bir yük dengeleyici ağ trafiği davranışı için [dağıtım modu](load-balancer-distribution-mode.md) türünü yapılandırabilirsiniz.

Uygulamanızın yük dengeleyici arkasındaki sunucular için bağlantıları canlı tutması gerekiyorsa [yük dengeleyici için boşta kalma TCP zaman aşımı ayarları](load-balancer-tcp-idle-timeout.md) hakkında daha fazla bilgi edinebilirsiniz. Bu, Azure Load Balancer kullanırken boşta kalan bağlantı davranışları hakkında bilgi edinmenizi sağlar.
