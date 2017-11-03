## <a name="nic"></a>NIC
Bir ağ arabirimi kartı (NIC) kaynak VNet kaynak mevcut bir alt ağ bağlantısını sağlar. Tek başına nesne olarak bir NIC oluşturabilirsiniz, ancak gerçekte bağlantı sağlamak için başka bir nesneye ilişkilendirmeniz gerekir. Bir NIC VM bir alt ağ, bir ortak IP adresi veya bir yük dengeleyici bağlanmak için kullanılabilir.  

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **sanal makinesi** |VM NIC ile ilişkili. |/Subscriptions/{guid}/../microsoft.COMPUTE/virtualMachines/VM1 |
| **macAddress** |NIC MAC adresi |4 ile 30 arasında herhangi bir değer |
| **networkSecurityGroup** |NIC'ye ilişkili NSG |/Subscriptions/{guid}/../microsoft.Network/networkSecurityGroups/myNSG1 |
| **dnsSettings** |NIC için DNS ayarları |bkz: [PIP](#Public-IP-address) |

Bir ağ arabirim kartı veya NIC, sanal makine (VM) ilişkilendirilebilir bir ağ arabirimi temsil eder. Bir VM, bir veya birden çok NIC olabilir.

![NIC kişinin tek bir VM'de](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a>IP yapılandırması
Nıc'lerinin adlı bir alt nesne **Ipconfigurations** aşağıdaki özellikleri içeren:

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **alt ağ** |Alt NIC onnected için değil. |/Subscriptions/{guid}/../microsoft.Network/virtualNetworks/myvnet1/Subnets/mysub1 |
| **privateIPAddress** |NIC alt ağdaki IP adresi |10.0.0.8 |
| **privateıpallocationmethod değeri** |IP ayırma yöntemi |Dinamik veya statik |
| **enableIPForwarding** |NIC yönlendirme için kullanılıp kullanılamayacağını |TRUE veya false |
| **birincil** |NIC VM birincil NIC olup olmadığı |TRUE veya false |
| **Publicıpaddress** |PIP NIC ile ilişkili |bkz: [DNS ayarları](#DNS-settings) |
| **loadBalancerBackendAddressPools** |Bitiş adresi havuzları NIC ile ilişkili geri | |
| **Loadbalancerınboundnatrules** |Gelen NIC ile ilişkili yük dengeleyicisi NAT kuralları | |

Örnek genel IP adresi JSON biçiminde:

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a>Ek kaynaklar
* Okuma [REST API başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt163579.aspx) NIC'ler için.

