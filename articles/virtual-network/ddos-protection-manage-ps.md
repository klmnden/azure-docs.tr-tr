---
title: "Azure DDoS koruması Azure PowerShell kullanarak standart yönetme | Microsoft Docs"
description: "Azure DDoS koruması Azure PowerShell kullanarak standart yönetmeyi öğrenin."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: kumud
ms.openlocfilehash: a1a3688d4ff215d05d2f78cdfa7d402e3fc20be2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Azure DDoS koruması Azure PowerShell kullanarak standart yönetme

>[!IMPORTANT]
>Azure DDoS koruması standart (DDoS koruması) şu anda önizlemede değil. Sınırlı sayıda Azure kaynaklarını destek DDoS koruması ve bölgeler select sayısı. Yapmanız [hizmet için kayıt](http://aka.ms/ddosprotection) DDoS koruması, aboneliğiniz için etkin almak için sınırlı Önizleme sırasında. Etkinleştirme işlemi boyunca size yol göstermesi için kayıt sırasında Azure DDoS ekibi tarafından kurulur. DDoS koruması Doğu ABD, Batı ABD ve Batı Orta ABD bölgelerde kullanılabilir. Önizleme sırasında hizmeti kullandığınız için sizden ücret istenmese.

Bu makalede Azure PowerShell'in DDoS koruması etkinleştirmek, DDoS koruması devre dışı bırakın ve bir saldırının etkisini azaltmak için telemetri kullanmak için nasıl kullanılacağı gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. Gerekirse yükleyin veya Azure PowerShell yükseltme, bakın [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection"></a>DDoS koruması etkinleştir

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Yeni bir sanal ağ oluşturun ve DDoS korumayı etkinleştir

DDoS koruması etkin olan bir sanal ağ oluşturmak için aşağıdaki örnekte çalıştırın:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -DnsServer 10.0.1.5,10.0.1.6 -EnableDDoSProtection
```

Bu örnekte, iki alt ağ ve iki DNS sunucusu ile bir sanal ağ oluşturur. Sanal ağ DNS sunucuları belirterek bu sanal ağ içinde dağıtılan NIC'ler/VMs varsayılan olarak bu DNS sunucularına devralır etkisidir. DDoS koruması için tüm korumalı sanal ağ kaynaklarında etkinleştirilir.

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Varolan bir sanal ağda DDoS korumayı etkinleştir

Varolan bir sanal ağda DDoS koruması etkinleştirmek için aşağıdaki örnekte çalıştırın:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks
```

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Bir sanal ağ üzerinde DDoS koruması devre dışı bırak

Bir sanal ağ üzerinde DDoS koruması devre dışı bırakmak için aşağıdaki örnek çalıştırın:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks"
```

## <a name="review-the-ddos-protection-status-of-virtual-networks"></a>Sanal ağlar DDoS koruması durumunu gözden geçirin 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>DDoS koruması telemetri kullanın

Telemetri bir saldırı Azure İzleyicisi gerçek zamanlı olarak sağlanır. Telemetri, bir ortak IP adresi azaltma altında olan süresi için kullanılabilir. Telemetri önce veya sonra bir saldırı azaltıldığından görmezsiniz.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>DDoS koruması ölçümleri uyarılarını yapılandırma

Azure İzleyici uyarı yapılandırması yararlanarak, herhangi bir etkin azaltma saldırının sırasında olduğunda uyarı için kullanılabilir DDoS koruması ölçümleri seçebilirsiniz.

#### <a name="configure-email-alert-rules-via-azure-powershell"></a>Azure PowerShell aracılığıyla e-posta Uyarı kurallarını yapılandırma

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

3. Bir kural oluşturmak için birkaç önemli bilgi parçasından önce olması gerekir. 

    - Bir uyarı için ayarlamak istediğiniz kaynağı için kaynak kimliği.
    - Bu kaynak için ölçüm tanımlarını. Kaynak Kimliği almanın bir yolu, Azure Portalı'nı kullanmaktır. Kaynak zaten oluşturulmuş olduğu varsayılarak, Azure portalında seçin. Sonraki sayfada, ardından *özellikleri* altında *ayarları* bölümü. **Kaynak kimliği** sonraki sayfada bir alandır. Başka bir yolu kullanmaktır [Azure kaynak Gezgini](https://resources.azure.com/). Kaynak kimliği için bir ortak IP örneğidir:`/subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`

    Aşağıdaki örnek bir uyarı genel bir sanal ağa ilişkili IP üzerinde ayarlar. Bir uyarı oluşturmak için ölçümüdür **altında DDoS saldırı veya**. Bir Boole değeri 1 veya 0 budur. A **1** Saldırıya uğramış olduğu anlamına gelir. A **0** Saldırıya uğramış kullanmıyorsanız anlamına gelir. Son 5 dakika içinde saldırıya olduğunda uyarı oluşturulur.

    Bir Web kancası oluşturma veya bir uyarı oluşturulduğunda e-posta göndermek için önce e-posta ve/veya Web kancası oluşturun. Ardından hemen kural daha sonra Eylemler etikete sahip ve aşağıdaki örnekte gösterildiği gibi oluşturun. PowerShell kullanarak kurallar önceden oluşturulmuş Web kancası veya e-posta ilişkilendiremezsiniz.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail-Description "Under DDoS Attack" 
    ```

4. Uyarılarınızı düzgün tek tek kurallarında bakarak oluşturulduğunu doğrulamak için.

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

Ayrıca daha fazla hakkında bilgi edinebilirsiniz [Web kancalarını yapılandırma](../monitoring-and-diagnostics/insights-webhooks-alerts.md) ve [logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) uyarıları oluşturmak için.

## <a name="configure-logging-on-ddos-protection-metrics"></a>DDoS koruması ölçümleri oturum açmayı Yapılandır

Başvurmak [PowerShell hızlı başlangıç örnekleri](../monitoring-and-diagnostics/insights-powershell-samples.md) erişmek ve Azure PowerShell yoluyla günlüğü tanılama yapılandırmanıza yardımcı olacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure tanılama günlükleri hakkında daha fazla bilgi](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Günlük analizi ile Azure depolama biriminden günlüklerini analiz edin](../log-analytics/log-analytics-azure-storage.md)
- [Event Hubs kullanmaya başlayın](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)