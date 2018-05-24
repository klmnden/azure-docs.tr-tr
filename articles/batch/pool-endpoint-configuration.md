---
title: Azure Batch havuzunda düğümü uç noktalarını yapılandırma | Microsoft Docs
description: Yapılandırma veya bir Azure Batch havuzundaki işlem düğümlerinde SSH veya RDP bağlantı noktalarına erişimini devre dışı bırakma.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 02/13/2018
ms.author: danlep
ms.openlocfilehash: 5898206761e5029f94b6d1f1b48223481ae2ca13
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="configure-or-disable-remote-access-to-compute-nodes-in-an-azure-batch-pool"></a>Yapılandırma veya işlem bir Azure Batch havuzunda düğümleri için uzaktan erişim devre dışı bırakma

Varsayılan olarak, toplu sağlayan bir [düğüm kullanıcı](/rest/api/batchservice/computenode/adduser) Batch havuzundaki işlem düğümünde dışarıdan bağlanmak için ağ bağlantısı ile. Örneğin, bir kullanıcı tarafından Uzak Masaüstü (RDP) 3389 numaralı bağlantı noktasına bir Windows havuzdaki işlem düğümü üzerinde bağlanabilir. Benzer şekilde, varsayılan olarak, bir kullanıcı tarafından güvenli Kabuk (SSH) bağlantı noktası 22 bir Linux havuzdaki işlem düğümü üzerinde bağlanabilir. 

Ortamınızda, erişimi kısıtlamak veya bu varsayılan dış erişim ayarlarını devre dışı bırakmak gerekebilir. Ayarlamak için Batch API'lerini kullanarak bu ayarları değiştirebilirsiniz [PoolEndpointConfiguration](/rest/api/batchservice/pool/add#poolendpointconfiguration) özelliği. 

## <a name="about-the-pool-endpoint-configuration"></a>Havuz uç nokta yapılandırması hakkında
Uç nokta yapılandırması bir veya daha fazla oluşur [ağ adresi çevirisi (NAT) havuzları](/rest/api/batchservice/pool/add#inboundnatpool) ön uç bağlantı noktaları. (Toplu işlem düğümü havuzu oluşturacak olan bir NAT havuzu karıştırmayın.) Havuzun işlem düğümleri varsayılan bağlantı ayarlarını geçersiz kılmak için her NAT havuzunu ayarlayın. 

Bir veya daha fazla her NAT havuzu yapılandırması içeren [ağ güvenlik grubu (NSG) kuralları](/rest/api/batchservice/pool/add#networksecuritygrouprule). Her NSG kural sağlar veya belirli ağ trafiğini uç reddeder. İzin verilen veya reddedilen tüm trafiği, tarafından tanımlanan trafiği seçebilirsiniz bir [hizmet etiketi](../virtual-network/security-overview.md#service-tags) (örneğin, "Internet"), ya da belirli IP adresleri veya alt gelen trafiği.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
* Havuz uç nokta yapılandırması havuzunun parçası olan [ağ yapılandırması](/rest/api/batchservice/pool/add#NetworkConfiguration). Ağ Yapılandırması isteğe bağlı olarak havuzuna katılmaya ayarları içerebilir bir [Azure sanal ağı](batch-virtual-network.md). Bir sanal ağ havuzunda ayarladıysanız, sanal ağ adresi ayarları kullanmak NSG kuralları oluşturabilirsiniz.
* NAT havuzunu yapılandırırken, birden fazla NSG kuralları yapılandırabilirsiniz. Kurallar öncelik sırasına göre denetlenir. Bir kural uygulandığı zaman eşleştirme için başka hiçbir kural test edilmez.


## <a name="example-deny-all-rdp-traffic"></a>Örnek: tüm RDP trafiği engelle

Aşağıdaki C# kod parçacığında, tüm ağ trafiğini engellemek için bir Windows havuzdaki işlem düğümleri üzerinde RDP uç noktasını yapılandırma gösterilmektedir. Uç nokta bağlantı noktası aralığındaki bir ön uç havuzu kullanır *60000 60099*. 

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

## <a name="example-deny-all-ssh-traffic-from-the-internet"></a>Örnek: tüm SSH trafiğini internet'ten Reddet

Aşağıdaki Python parçacığını, tüm Internet trafiği engellemek için bir Linux havuzdaki işlem düğümlerinde SSH uç noktasını yapılandırma gösterilmektedir. Uç nokta bağlantı noktası aralığındaki bir ön uç havuzu kullanır *4000 4100*. 

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

Aşağıdaki C# kod parçacığında, yalnızca IP adresinden gelen RDP erişime izin vermek için bir Windows havuzdaki işlem düğümleri üzerinde RDP uç noktası yapılandırmak gösterilmiştir *198.51.100.7*. İkinci NSG kural IP adresi eşleşmiyor trafiği engeller.

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

## <a name="example-allow-ssh-traffic-from-a-specific-subnet"></a>Örnek: Belirli bir alt ağdan SSH trafiğine izin ver

Aşağıdaki Python parçacığını alt ağdan yalnızca erişime izin vermek için bir Linux havuzdaki işlem düğümlerinde SSH uç noktasını yapılandırma gösterilmektedir *192.168.1.0/24*. İkinci NSG kural alt ağ eşleşmiyor trafiği engeller.

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

- Azure'da NSG kuralları hakkında daha fazla bilgi için bkz: [filtre ağ güvenlik grupları ile ağ trafiği](../virtual-network/security-overview.md).

- Toplu ayrıntılı bir bakış için bkz: [geliştirme büyük ölçekli paralel işlem çözümleri yığın](batch-api-basics.md).

