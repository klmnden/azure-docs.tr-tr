---
title: PowerShell kullanarak bölge olarak yedekli genel IP adresi ön uç ile bir genel yük dengeleyiciye standart oluşturun | Microsoft Docs
description: PowerShell kullanarak bir bölge olarak yedekli genel IP adresi ön ile ortak yük dengeleyici standart oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: kumud
ms.openlocfilehash: ba76037f36d3f4f8a06103105d65b3f2ddc88c96
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-powershell"></a>Bir genel yük dengeleyiciye standart PowerShell kullanarak bölge olarak yedekli genel IP adresi ön uç ile oluşturma

Bu makalede adımları genel oluşturmada size [yük dengeleyici standart](https://aka.ms/azureloadbalancerstandard) ortak IP standart bir adresi kullanarak bir bölge olarak yedekli ön ile.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
 Kullanılabilirlik bölgeler için destek, select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun:

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Ortak IP standart oluşturma 
Bir ortak IP aşağıdaki komutu kullanarak standart oluşturun:

```powershell
$publicIp = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Name 'myPublicIP' `
  -Location westeurope -AllocationMethod Static -Sku Standard 
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Web sitesi için bir ön uç IP yapılandırması oluştur

Aşağıdaki komutu kullanarak bir ön uç IP yapılandırması oluşturun:

```powershell
$feip = New-AzureRmLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>Arka uç adres havuzu oluşturma

Aşağıdaki komutu kullanarak bir arka uç adres havuzu oluşturun:

```powershell
$bepool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>Bağlantı noktası 80 üzerinde bir yük dengeleyici araştırması oluştur

Aşağıdaki komutu kullanarak yük dengeleyici için bağlantı noktası 80 üzerinde bir sistem durumu araştırması oluşturun:

```powershell
$probe = New-AzureRmLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma
 Aşağıdaki komutu kullanarak bir yük dengeleyici kuralı oluşturun:

```powershell
   $rule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma
Bir yük dengeleyici aşağıdaki komutu kullanarak standart oluşturun:

```powershell
$lb = New-AzureRmLoadBalancer -ResourceGroupName myResourceGroup -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [standart yük dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).



