---
title: "İnternet’e yönelik yük dengeleyicisi oluşturma - Azure PowerShell klasik | Microsoft Docs"
description: "Klasik modda PowerShell kullanarak İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: c06be7a2d17b655c958c4ba4618739f5b218b8d7
ms.contentlocale: tr-tr
ms.lasthandoff: 04/27/2017

---

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>PowerShell’de İnternet’e yönelik yük dengeleyici (klasik) oluşturmaya başlama

> [!div class="op_single_selector"]
> * [Klasik Azure portalı](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure kaynaklarıyla çalışmadan önce Azure’da şu anda iki dağıtım modeli olduğunu anlamak önemlidir: Azure Resource Manager ve klasik. Azure kaynaklarıyla çalışmadan önce [dağıtım modellerini ve araçlarlarını](../azure-classic-rm.md) iyice anladığınızdan emin olun. Bu makalenin en üstündeki sekmelere tıklayarak farklı araçlarla ilgili belgeleri görüntüleyebilirsiniz. Bu makale, klasik dağıtım modelini kapsamaktadır. [Azure Resource Manager kullanarak İnternet’e yönelik yük dengeleyici oluşturma](load-balancer-get-started-internet-arm-ps.md) sayfasını da inceleyebilirsiniz.

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>PowerShell kullanarak iç yük dengeleyici kurma

PowerShell kullanarak yük dengeleyici oluşturmak için aşağıdaki adımları uygulayın:

1. Daha önce Azure PowerShell kullanmadıysanız, [Azure PowerShell’i Yükleme ve Yapılandırma](/powershell/azure/overview) sayfasına gidin ve Azure’da oturum açıp aboneliğinizi seçmek için talimatları sonuna kadar uygulayın.
2. Sanal makine oluşturduktan sonra PowerShell cmdlet’lerini kullanarak aynı bulut hizmeti içindeki bir sanal makineye yük dengeleyici ekleyebilirsiniz.

Aşağıdaki örnekte "web1" ve "web2" adlı sanal makinelere yük dengeleyici uç noktaları ekleyerek "mytestcloud" (veya myctestcloud.cloudapp.net) bulut hizmetine "webfarm" adlı bir yük dengeleyici kümesi ekleyeceksiniz. Yük dengeleyici 80 numaralı bağlantı noktasından ağ trafiğini alır ve TCP kullanarak yerel uç noktası (bu durumda 80 numaralı bağlantı noktası) tarafından tanımlanan sanal makineler arasında yük dengeleme gerçekleştirir.

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

Uygulamanızın yük dengeleyici arkasındaki sunucular için bağlantıları canlı tutması gerekiyorsa [yük dengeleyici için boşta kalma TCP zaman aşımı ayarları](load-balancer-tcp-idle-timeout.md) hakkında daha fazla bilgi edinebilirsiniz. Bu, Azure Load Balancer kullanırken boşta kalan bağlantı davranışları hakkında bilgi edinmenizi sağlayacaktır.

