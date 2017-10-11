## <a name="route-tables"></a>Yönlendirme tabloları
Rota tablosu kaynakları Azure altyapısı içinde trafiğinin nasıl akacağını belirlemek için kullanılan yolları içerir. Bir güvenlik duvarı veya izinsiz giriş algılama sistem gibi (Kimlikler) tüm trafik için sanal bir gereç, belirli bir alt ağ üzerinden göndermek için kullanıcı tanımlı yolları (UDR) kullanabilirsiniz. Bir yol tablosu alt ağlara ilişkilendirebilirsiniz. 

Yönlendirme tabloları aşağıdaki özellikleri içerir.

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **yollar** |Kullanıcı koleksiyonunu rota tablosunda tanımlı yollar |bkz: [kullanıcı tanımlı yollar](#User-defined-routes) |
| **alt ağlar** |Alt ağlar koleksiyonunu rota tablosu uygulanır. |bkz: [alt ağlar](#Subnets) |

### <a name="user-defined-routes"></a>Kullanıcı tanımlı yollar
Hedef adresine göre burada trafiği için gönderilmesi gerektiğini belirtmek için Udr'ler oluşturabilirsiniz. Bir ağ paketi hedef adresini temel alarak varsayılan ağ geçidi tanımı bir yolu düşünebilirsiniz.

Udr'ler aşağıdaki özellikleri içerir. 

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **addressPrefix** |Adres ön eki veya hedef için tam IP adresi |192.168.1.0/24, 192.168.1.101 |
| **nextHopType** |Trafiğin gönderileceği aygıt türü |Değerinin VirtualAppliance, VPN ağ geçidi, Internet |
| **Nexthopıpaddress** |Sonraki atlama IP adresi |192.168.1.4 |

JSON biçiminde örnek yol tablosu:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>Ek kaynaklar
* Hakkında daha fazla bilgi almak [Udr'ler](../articles/virtual-network/virtual-networks-udr-overview.md).
* Okuma [REST API başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt502549.aspx) yönlendirme tabloları için.
* Okuma [REST API başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt502539.aspx) için kullanıcı tanımlı yollar (Udr'ler).

