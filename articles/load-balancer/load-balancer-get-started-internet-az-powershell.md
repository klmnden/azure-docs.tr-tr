---
title: "PowerShell kullanarak bölge olarak yedekli genel IP adresi ön uç ile bir genel yük dengeleyiciye standart oluşturun | Microsoft Docs"
description: "PowerShell kullanarak bir bölge olarak yedekli genel IP adresi ön ile ortak yük dengeleyici standart oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2017
ms.author: kumud
ms.openlocfilehash: abe970eb72be5ea6944acc253c2c3d9008b20c56
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-powershell"></a>Bir genel yük dengeleyiciye standart PowerShell kullanarak bölge olarak yedekli genel IP adresi ön uç ile oluşturma

Bu makalede adımları genel oluşturmada size [yük dengeleyici standart](https://aka.ms/azureloadbalancerstandard) ortak IP standart bir adresi kullanarak bir bölge olarak yedekli ön ile.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Kullanılabilirlik bölgeleri, yük dengeleyici standart ve genel IP Standard Önizleme için kaydolun

Bu makalede sürüm 4.4.0 veya daha yüksek AzureRM modülün yüklü olmasını gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. AzureRM modülünden en son sürümünü yüklemek veya yükseltmek gerekiyorsa, yükleme [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM).

>[!NOTE]
[Yük Dengeleyici standart SKU](https://aka.ms/azureloadbalancerstandard) şu anda önizlemede değil. Önizleme sırasında bu özellik genel kullanılabilirlik sunumundaki özelliklerle aynı seviyede kullanılabilirliğe ve güvenilirliğe sahip olmayabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Genel olarak kullanılabilir kullanmak [yük dengeleyici temel SKU](load-balancer-overview.md) üretim hizmetleriniz için. 

> [!NOTE]
> Kullanılabilirlik bölgeleri önizlemede ve geliştirme için hazır olduğunu ve test senaryoları. Destek select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Bir bölge veya yük dengeleyici için ön uç genel IP adresi için bölge olarak yedekli seçeneği belirlemeden önce ilk adımları tamamlamanız gereken [kullanılabilirlik bölgeleri Önizleme için kaydetmek](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
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
- Bilgi nasıl [bir kullanılabilirlik bölgesinde bir genel IP oluşturun](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)



