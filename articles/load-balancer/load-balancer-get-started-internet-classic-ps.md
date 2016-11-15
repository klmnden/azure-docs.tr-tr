---
title: "Klasik modda PowerShell kullanarak İnternet’e yönelik yük dengeleyici oluşturmaya başlama | Microsoft Belgeleri"
description: "Klasik modda PowerShell kullanarak İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 73e8bfa4-8086-4ef0-9e35-9e00b24be319
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7344f2c3eeb7d52f8bc60e564d66b2cc51f10f75

---

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>PowerShell’de İnternet’e yönelik yük dengeleyici (klasik) oluşturmaya başlama

[!INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır. [Azure Resource Manager kullanarak İnternet’e yönelik yük dengeleyici oluşturma](load-balancer-get-started-internet-arm-ps.md) sayfasını da inceleyebilirsiniz.

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>PowerShell kullanarak iç yük dengeleyici kurma

PowerShell kullanarak yük dengeleyici oluşturmak için aşağıdaki adımları uygulayın:

1. Daha önce Azure PowerShell kullanmadıysanız, [Azure PowerShell’i Yükleme ve Yapılandırma](../powershell-install-configure.md) sayfasına gidin ve Azure’da oturum açıp aboneliğinizi seçmek için talimatları sonuna kadar uygulayın.
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
    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM
```

## <a name="next-steps"></a>Sonraki adımlar

[İç yük dengeleyici oluşturmaya başlayabilir](load-balancer-get-started-ilb-classic-ps.md) ve belirli bir yük dengeleyici ağ trafiği davranışı için [dağıtım modu](load-balancer-distribution-mode.md) türünü yapılandırabilirsiniz.

Uygulamanızın yük dengeleyici arkasındaki sunucular için bağlantıları canlı tutması gerekiyorsa [yük dengeleyici için boşta kalma TCP zaman aşımı ayarları](load-balancer-tcp-idle-timeout.md) hakkında daha fazla bilgi edinebilirsiniz. Bu, Azure Load Balancer kullanırken boşta kalan bağlantı davranışları hakkında bilgi edinmenizi sağlayacaktır.



<!--HONumber=Nov16_HO2-->


