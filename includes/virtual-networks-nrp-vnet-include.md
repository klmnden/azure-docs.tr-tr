## <a name="virtual-network"></a>Sanal Ağ
Sanal ağ (VNET) ve alt kaynakları Azure'da çalışan iş yükleri için güvenlik sınırı tanımlamaya yardımcı olacak. Bir sanal ağ adres alanları, CIDR bloğu tanımlı bir koleksiyon tarafından belirlenir. 

> [!NOTE]
> Ağ yöneticileri ile CIDR gösteriminde biliyorsunuzdur. CIDR ile bilmiyorsanız [hakkında daha fazla bilgi](http://whatismyipaddress.com/cidr).
> 
> 

![Birden çok alt ağa sahip VNet](./media/resource-groups-networking/Figure4.png)

Sanal ağlar aşağıdaki özellikleri içerir.

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **addressSpace** |CIDR gösteriminde VNet oluşturan adres öneklerini koleksiyonu |192.168.0.0/16 |
| **alt ağlar** |VNet yapmak alt koleksiyonu |bkz: [alt ağlar](#Subnets) aşağıda. |
| **IP adresi** |Nesne için atanan IP adresi. Bu salt okunur bir özelliktir. |104.42.233.77 |

### <a name="subnets"></a>Alt ağlar
Bir alt ağ alt bir vnet'in kaynaktır ve adres alanları IP adresi öneklerini kullanarak bir CIDR bloğu içinde kesimleri yardımcı tanımlayın. NIC alt ağlara eklendi ve çeşitli iş yükleri için bağlantı sağlama VM'ler bağlı.

Alt ağları aşağıdaki özellikleri içerir. 

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **addressPrefix** |Alt ağ CIDR gösteriminde oluşturan tek adresi öneki |192.168.1.0/24 |
| **networkSecurityGroup** |NSG alt ağına uygulanır |bkz: [Nsg'ler](#Network-Security-Group) |
| **routeTable** |Alt ağa uygulanan yol tablosu |bkz: [UDR](#Route-table) |
| **Ipconfigurations** |Alt ağına bağlı NIC tarafından kullanılan IP yapılandırma nesnelerinin koleksiyonunu |bkz: [UDR](#Route-table) |

JSON biçiminde örnek VNet:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Ek kaynaklar
* Hakkında daha fazla bilgi almak [VNet](../articles/virtual-network/virtual-networks-overview.md).
* Okuma [REST API başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt163650.aspx) sanal ağlar için.
* Okuma [REST API başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt163618.aspx) alt ağlar için.

