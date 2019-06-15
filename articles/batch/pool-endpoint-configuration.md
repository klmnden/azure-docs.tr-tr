---
title: Azure Batch havuzunda düğümü uç noktalarını yapılandırma | Microsoft Docs
description: Yapılandırma veya bir Azure Batch havuzundaki işlem düğümlerinde SSH veya RDP bağlantı noktası erişimi devre dışı bırakma.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 02/13/2018
ms.author: lahugh
ms.openlocfilehash: 9b6b28b9f55623fbdff6ab80c889141c8815600f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071524"
---
# <a name="configure-or-disable-remote-access-to-compute-nodes-in-an-azure-batch-pool"></a>Yapılandırma veya bir Azure Batch havuzu düğümlerinde işlem uzak erişimi devre dışı bırak

Varsayılan olarak, Batch sağlar bir [düğümü kullanıcı](/rest/api/batchservice/computenode/adduser) harici olarak Batch havuzunda işlem düğümüne bağlanmak için ağ bağlantısı ile. Örneğin, bir kullanıcı tarafından Uzak Masaüstü (RDP) bir işlem düğümünde bir Windows havuzu için 3389 numaralı bağlantı noktasında bağlanabilirsiniz. Benzer şekilde, varsayılan olarak, bir kullanıcı tarafından Secure Shell (SSH) bağlantı noktası 22 Linux havuzu içindeki bir işlem düğümüne bağlanabilirsiniz. 

Ortamınızda, erişimi kısıtlamak veya bu varsayılan dış erişim ayarlarını devre dışı bırakmak gerekebilir. Bu ayarları ayarlamak üzere Batch API'lerini kullanarak değiştirebileceğiniz [PoolEndpointConfiguration](/rest/api/batchservice/pool/add#poolendpointconfiguration) özelliği. 

## <a name="about-the-pool-endpoint-configuration"></a>Havuz uç nokta yapılandırması hakkında
Bir veya daha fazla uç nokta yapılandırması oluşur [ağ adresi çevirisi (NAT) havuzları](/rest/api/batchservice/pool/add#inboundnatpool) ön uç bağlantı noktası. (İşlem düğümleri Batch havuzu ile bir NAT havuzunu karıştırmayın.) Havuzun işlem düğümleri üzerinde varsayılan bağlantı ayarlarını geçersiz kılmak için her NAT havuzunu ayarlayın. 

Bir veya daha fazla her NAT havuzu yapılandırması içeren [ağ güvenlik grubu (NSG) kuralları](/rest/api/batchservice/pool/add#networksecuritygrouprule). Her bir NSG kuralı izin verir veya belirli ağ trafiği uç noktasına reddeder. Veya tüm trafiği, tarafından tanımlanan trafiği reddetmek seçebileceğiniz bir [hizmet etiketi](../virtual-network/security-overview.md#service-tags) (örneğin, "Internet") veya belirli IP adresleri veya alt ağlara gelen trafiği.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
* Havuz uç nokta yapılandırması havuzun parçası olan [ağ yapılandırması](/rest/api/batchservice/pool/add#networkconfiguration). Ağ Yapılandırması isteğe bağlı olarak havuza eklemenizi ayarları içerebilir bir [Azure sanal ağı](batch-virtual-network.md). Bir sanal ağ içinde havuz ayarlarsanız, bir sanal ağ adresi ayarlarını kullanmaya NSG kuralları oluşturabilirsiniz.
* Bir NAT havuzu yapılandırdığınız sırada birden fazla NSG kuralları yapılandırabilirsiniz. Kurallar öncelik sırasına göre denetlenir. Bir kural uygulandığı zaman eşleştirme için başka hiçbir kural test edilmez.


## <a name="example-deny-all-rdp-traffic"></a>Örnek: Tüm RDP trafiği engelle

Aşağıdaki C# kod parçacığı, tüm ağ trafiği reddetmeye yönelik bir Windows havuzdaki işlem düğümleri üzerinde RDP uç noktası yapılandırmak nasıl gösterir. Uç nokta bağlantı noktası aralığında bir ön uç havuzunu kullanan *60000 60099*. 

```csharp
pool.NetworkConfiguration = new NetworkConfiguration
{
    EndpointConfiguration = new PoolEndpointConfiguration(new InboundNatPool[]
    {
      new InboundNatPool("RDP", InboundEndpointProtocol.Tcp, 3389, 60000, 60099, new NetworkSecurityGroupRule[]
        {
            new NetworkSecurityGroupRule(162, NetworkSecurityGroupRuleAccess.Deny, "*"),
        })
    })    
};
```

## <a name="example-deny-all-ssh-traffic-from-the-internet"></a>Örnek: Tüm SSH trafiği internet'ten Reddet

Aşağıdaki Python kod parçacığı, tüm internet trafiği reddetmeye yönelik bir Linux havuzdaki işlem düğümleri üzerinde SSH uç noktası yapılandırma işlemi gösterilmektedir. Uç nokta bağlantı noktası aralığında bir ön uç havuzunu kullanan *4000 4100*. 

```python
pool.network_configuration=batchmodels.NetworkConfiguration(
    endpoint_configuration=batchmodels.PoolEndpointConfiguration(
        inbound_nat_pools=[batchmodels.InboundNATPool(
            name='SSH',
            protocol='tcp',
            backend_port=22,
            frontend_port_range_start=4000,
            frontend_port_range_end=4100,
            network_security_group_rules=[
                batchmodels.NetworkSecurityGroupRule(
                priority=170,
                access=batchmodels.NetworkSecurityGroupRuleAccess.deny,
                source_address_prefix='Internet'
                )
            ]
        )
        ]
    ) 
)
```

## <a name="example-allow-rdp-traffic-from-a-specific-ip-address"></a>Örnek: Belirli bir IP adresinden gelen RDP trafiğine izin ver

Aşağıdaki C# kod parçacığı, yalnızca IP adresi ağdan RDP erişimine izin vermek için bir Windows havuzdaki işlem düğümleri üzerinde RDP uç noktası yapılandırmak nasıl gösterir *198.51.100.7*. İkinci bir NSG kuralı IP adresi eşleşmeyen trafiği engeller.

```csharp
pool.NetworkConfiguration = new NetworkConfiguration
{
    EndpointConfiguration = new PoolEndpointConfiguration(new InboundNatPool[]
    {
        new InboundNatPool("RDP", InboundEndpointProtocol.Tcp, 3389, 7500, 8000, new NetworkSecurityGroupRule[]
        {   
            new NetworkSecurityGroupRule(179,NetworkSecurityGroupRuleAccess.Allow, "198.51.100.7"),
            new NetworkSecurityGroupRule(180,NetworkSecurityGroupRuleAccess.Deny, "*")
        })
    })    
};
```

## <a name="example-allow-ssh-traffic-from-a-specific-subnet"></a>Örnek: Belirli bir alt ağından gelen SSH trafiğine izin ver

Aşağıdaki Python kod parçacığı, SSH uç noktası yalnızca alt ağından erişime izin vermek için bir Linux havuzdaki işlem düğümleri üzerinde yapılandırma işlemi gösterilmektedir *192.168.1.0/24*. İkinci bir NSG kuralı alt eşleşmeyen trafiği engeller.

```python
pool.network_configuration=batchmodels.NetworkConfiguration(
    endpoint_configuration=batchmodels.PoolEndpointConfiguration(
        inbound_nat_pools=[batchmodels.InboundNATPool(
            name='SSH',
            protocol='tcp',
            backend_port=22,
            frontend_port_range_start=4000,
            frontend_port_range_end=4100,
            network_security_group_rules=[
                batchmodels.NetworkSecurityGroupRule(
                priority=170,
                access='allow',
                source_address_prefix='192.168.1.0/24'
                ),
                batchmodels.NetworkSecurityGroupRule(
                priority=175,
                access='deny',
                source_address_prefix='*'
                )
            ]
        )
        ]
    )
)
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure'da NSG kuralları hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları ile ağ trafiğini filtreleme](../virtual-network/security-overview.md).

- Batch ayrıntılı genel bakış için bkz: [büyük ölçekli paralel işlem çözümleri geliştirme batch'le](batch-api-basics.md).

