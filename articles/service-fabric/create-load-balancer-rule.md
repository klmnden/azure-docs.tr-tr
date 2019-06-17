---
title: Bir küme için bir Azure yük dengeleyici kuralı oluşturma
description: Azure Service Fabric kümeniz için bağlantı noktalarını açmak için bir Azure Load Balancer'ı yapılandırın.
services: service-fabric
documentationcenter: na
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: aljo
ms.openlocfilehash: d95d2802398a61b948ff6c59fb3eab0e1ddddbc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66147470"
---
# <a name="open-ports-for-a-service-fabric-cluster"></a>Service Fabric kümesi için bağlantı noktalarını açma

Azure Service Fabric kümenizle dağıtılan yük dengeleyici trafiği bir düğümde çalışan uygulamanıza yönlendirir. Farklı bir bağlantı noktası kullanacak şekilde değiştirirseniz, bu bağlantı noktasını kullanıma sunma (veya farklı bir bağlantı noktası route gerekir) Azure yük dengeleyicide.

Service Fabric kümenizi Azure'a dağıtıldığında, bir yük dengeleyici sizin için otomatik olarak oluşturuldu. Bir yük dengeleyici yoksa bkz [bir Internet'e yönelik Yük Dengeleyiciyi yapılandırma](../load-balancer/load-balancer-get-started-internet-portal.md).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-service-fabric"></a>Service fabric yapılandırın

Service Fabric uygulamanızı **ServiceManifest.xml** yapılandırma dosyasını kullanmak için uygulamanızı bekliyor uç noktalarını tanımlar. (Veya farklı bir) kullanıma sunmak için bir uç nokta config dosyası güncelleştirildikten sonra Yük Dengeleyici güncelleştirilmelidir bağlantı noktası. Service fabric uç noktası oluşturma hakkında daha fazla bilgi için bkz. [bir uç nokta Kurulum](service-fabric-service-manifest-resources.md).

## <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Bir yük dengeleyici kuralı, internet'e yönelik bir bağlantı noktasını açar ve uygulamanız tarafından kullanılan iç düğümün bağlantı trafiğini. Bir yük dengeleyici yoksa bkz [bir Internet'e yönelik Yük Dengeleyiciyi yapılandırma](../load-balancer/load-balancer-get-started-internet-portal.md).

Bir yük dengeleyici kuralı oluşturmak için aşağıdaki bilgileri toplamak gerekir:

- Yük Dengeleyici adı.
- Yük Dengeleyici ve service fabric küme kaynak grubu.
- Dış bağlantı noktası.
- İç bağlantı noktası.

## <a name="azure-cli"></a>Azure CLI
Yalnızca tek bir komutu ile bir yük dengeleyici kuralı oluşturmak için gereken **Azure CLI**. Her iki yeni bir kural oluşturmak için yük dengeleyici ve kaynak grubunun adını bilmeniz yeterlidir.

>[!NOTE]
>Yük Dengeleyici adını belirlemek gerekiyorsa, hızlı bir şekilde tüm yük dengeleyicileri ve ilişkili kaynak grupları listesini almak için bu komutu kullanın.
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>


```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

Azure CLI komutunu aşağıdaki tabloda açıklanan birkaç parametrelere sahiptir:

| Parametre | Açıklama |
| --------- | ----------- |
| `--backend-port`  | Service Fabric uygulaması bağlantı noktası için dinliyor. |
| `--frontend-port` | Yük Dengeleyici bağlantı noktası için dış bağlantılar sunar. |
| `-lb-name` | Değiştirmek için yük dengeleyicinin adı. |
| `-g`       | Kaynak grubunu yük dengeleyici ve Service Fabric kümesine sahiptir. |
| `-n`       | İstenen kural adı. |


>[!NOTE]
>Azure CLI ile bir yük dengeleyici oluşturma hakkında daha fazla bilgi için bkz. [Azure CLI ile bir yük dengeleyici oluşturma](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="powershell"></a>PowerShell

PowerShell, Azure CLI'yı biraz daha karmaşık bir işlemdir. Bir kural oluşturmak için kavramsal şu adımları izleyin:

1. Yük Dengeleyiciyi Azure'dan alın.
2. Bir kural oluşturun.
3. Kuralı, yük dengeleyiciye ekleyin.
4. Yük Dengeleyiciyi güncelleştirin.

>[!NOTE]
>Yük Dengeleyici adını belirlemek gerekiyorsa, hızlı bir şekilde tüm yük dengeleyicileri ve ilişkili kaynak grupları listesini almak için bu komutu kullanın.
>
>`Get-AzLoadBalancer | Select Name, ResourceGroupName`

```powershell
# Get the load balancer
$lb = Get-AzLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create the rule based on information from the load balancer.
$lbrule = New-AzLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add the rule to the load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update the load balancer on Azure
$lb | Set-AzLoadBalancer
```

İlgili `New-AzLoadBalancerRuleConfig` komutu `-FrontendPort` yük dengeleyici kullanıma sunar, dış bağlantılar için bağlantı noktasını temsil eder ve `-BackendPort` service fabric uygulaması için dinlediği bağlantı noktasını temsil eder.

>[!NOTE]
>PowerShell ile bir yük dengeleyici oluşturma hakkında daha fazla bilgi için bkz. [PowerShell ile bir yük dengeleyici oluşturma](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Service Fabric'te ağ](service-fabric-patterns-networking.md)fabric desenleri networking.md .rvice).