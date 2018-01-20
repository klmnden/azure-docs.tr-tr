---
title: "Azure DDoS koruması Azure PowerShell kullanarak standart yönetme | Microsoft Docs"
description: "Azure DDoS koruması Azure PowerShell kullanarak standart yönetmeyi öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: 33ff6cfcacd1632dc49b448e70361e1cb2ce1176
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Azure DDoS koruması Azure PowerShell kullanarak standart yönetme

Etkinleştirme ve dağıtılmış engelleme (DDoS) hizmeti koruma devre dışı bırakın ve Azure DDoS koruması standart ile DDoS saldırı azaltmak için telemetri kullanma öğrenin. DDoS koruması standart korur sanal makineler, yük Dengeleyiciler ve Azure sahip uygulama ağ geçitleri gibi Azure kaynakları [genel IP adresi](virtual-network-public-ip-address.md) atanmış. DDoS koruması standart ve özelliklerini hakkında daha fazla bilgi için bkz: [DDoS koruması standart genel bakış](ddos-protection-overview.md). 

>[!IMPORTANT]
>Azure DDoS koruması standart (DDoS koruması) şu anda önizlemede değil. DDoS koruması Azure kaynaklarını sınırlı sayıda destek ve yalnızca bölgeleri select bir dizi içinde kullanılabilir. Kullanılabilir bölgelerin bir listesi için bkz: [DDoS koruması standart genel bakış](ddos-protection-overview.md). Yapmanız [hizmet için kayıt](http://aka.ms/ddosprotection) DDoS koruması, aboneliğiniz için etkin almak için sınırlı Önizleme sırasında. Kaydolduktan sonra etkinleştirme işleminde size kılavuzluk Azure DDoS ekibi tarafından kurulur.


## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. Gerekirse yükleyin veya Azure PowerShell yükseltme, bakın [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps).

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection-standard---new-virtual-network"></a>DDoS koruması standart - yeni bir sanal ağ etkinleştir

DDoS koruması etkin olan bir sanal ağ oluşturmak için aşağıdaki örnekte çalıştırın:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -EnableDDoSProtection
```

Bu örnekte, iki alt ağ ve iki DNS sunucusu ile bir sanal ağ oluşturur. Sanal ağ DNS sunucuları belirterek bu sanal ağ içinde dağıtılan NIC'ler/VMs varsayılan olarak bu DNS sunucularına devralır etkisidir. DDoS koruması için tüm korumalı sanal ağ kaynaklarında etkinleştirilir.

> [!WARNING]
> Bir bölge seçerken, desteklenen bir bölge listesinden seçin [Azure DDoS koruması standart genel bakış](ddos-protection-overview.md).

## <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Varolan bir sanal ağda DDoS korumayı etkinleştir

Varolan bir sanal ağda DDoS koruması etkinleştirmek için aşağıdaki örnekte çalıştırın:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks -Force
```

> [!WARNING]
> Sanal ağ, desteklenen bir bölgede bulunmalıdır. Desteklenen bölgelerin bir listesi için bkz: [Azure DDoS koruması standart genel bakış](ddos-protection-overview.md).

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Bir sanal ağ üzerinde DDoS koruması devre dışı bırak

Bir sanal ağ üzerinde DDoS koruması devre dışı bırakmak için aşağıdaki örnek çalıştırın:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks" -Force
```

## <a name="review-the-ddos-protection-status-of-a-virtual-network"></a>Bir sanal ağ DDoS koruması durumunu gözden geçirin 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>DDoS koruması telemetri kullanın

Telemetri bir saldırı Azure İzleyicisi gerçek zamanlı olarak sağlanır. Telemetri, bir ortak IP adresi azaltma altında olan süresi için kullanılabilir. Telemetri önce veya sonra bir saldırı azaltıldığından görmezsiniz.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>DDoS koruması ölçümleri uyarılarını yapılandırma

Azure İzleyici uyarı yapılandırması yararlanarak, herhangi bir etkin azaltma saldırının sırasında olduğunda uyarı için kullanılabilir DDoS koruması ölçümleri seçebilirsiniz.

#### <a name="configure-email-alert-rules-via-azure"></a>Azure aracılığıyla e-posta Uyarı kurallarını yapılandırma

1. Bir listesini almak abonelikleri sahip olduğunuz kullanılabilir. Doğru aboneliği çalıştığını doğrulayın. Aksi halde, Get-AzureRmSubscription çıkışı kullanarak doğru olanı ayarlayın. 

    ```powershell
    Get-AzureRmSubscription 
    Get-AzureRmContext 
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

2. Bir kaynak grubu üzerinde mevcut kurallar listelemek için aşağıdaki komutu kullanın: 

    ```powershell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

3. Bir kural oluşturmak için aşağıdaki bilgileri ilk sahip olmanız gerekir: 

    - Bir uyarı için ayarlamak istediğiniz kaynağı için kaynak kimliği.
    - Bu kaynak için ölçüm tanımlarını. Kaynak Kimliği almanın bir yolu, Azure Portalı'nı kullanmaktır. Kaynak zaten oluşturulmuş olduğu varsayılarak, Azure portalında seçin. Sonraki sayfada, ardından *özellikleri* altında *ayarları* bölümü. **Kaynak kimliği** sonraki sayfada bir alandır. Başka bir yolu kullanmaktır [Azure kaynak Gezgini](https://resources.azure.com/). Kaynak kimliği için bir ortak IP örneğidir:`/subscriptions/<Id>/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`

    Aşağıdaki örnek, bir kaynak olarak bir sanal ağ ile ilişkili bir ortak IP adresi için bir uyarı oluşturur. Bir uyarı oluşturmak için ölçümüdür **altında DDoS saldırı veya**. Bir Boole değeri 1 veya 0 budur. A **1** Saldırıya uğramış olduğu anlamına gelir. A **0** Saldırıya uğramış kullanmıyorsanız anlamına gelir. Uyarısı, saldırının son 5 dakika içinde başlatıldığında oluşturulur.

    Bir Web kancası oluşturma veya bir uyarı oluşturulduğunda e-posta göndermek için önce e-posta ve/veya Web kancası oluşturun. E-posta veya Web kancası oluşturduktan sonra hemen kuralla oluşturmak `-Actions` , aşağıdaki örnekte gösterildiği gibi etiketi. Web kancası veya e-postaları PowerShell kullanarak mevcut kurallar ile ilişkilendiremezsiniz.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/<Id>/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail -Description "Under DDoS Attack"
    ```

4. Uyarınız düzgün kuralını bakarak oluşturduğunuz doğrulayın:

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    ```

Ayrıca daha fazla hakkında bilgi edinebilirsiniz [Web kancalarını yapılandırma](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [logic apps](../logic-apps/logic-apps-overview.md) uyarıları oluşturmak için.

## <a name="configure-logging-on-ddos-protection-metrics"></a>DDoS koruması ölçümleri oturum açmayı Yapılandır

Başvurmak [PowerShell hızlı başlangıç örnekleri](../monitoring-and-diagnostics/insights-powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json) erişmek ve Azure PowerShell yoluyla günlüğü tanılama yapılandırmanıza yardımcı olacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tanılama günlükleri hakkında daha fazla bilgi](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Günlük analizi ile Azure depolama biriminden günlüklerini analiz edin](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md?toc=%2fazure%2fvirtual-network%2ftoc.json)