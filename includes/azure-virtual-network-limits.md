<a name="virtual-networking-limits-classic"></a>Aşağıdaki sınırlar yalnızca abonelik başına klasik dağıtım modeliyle yönetilen ağ kaynakları için geçerlidir.

| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Sanal ağlar |50 |100 |
| Yerel ağ siteleri |20 |desteğe başvurun |
| Sanal ağ başına DNS sunucusu sayısı |20 |100 |
| Sanal ağ başına özel IP Adresi sayısı |4096 |4096 |
| Bir sanal makine veya rol örneği NIC eşzamanlı TCP veya UDP akışlar |500K |500K |
| Ağ Güvenlik Grupları (NSG) |100 |200 |
| NSG başına NSG kuralları |200 |400 |
| Kullanıcı tanımlı yol tabloları |100 |200 |
| Yol tablosu başına kullanıcı tanımlı yol sayısı |100 |400 |
| Genel IP adresleri (dinamik) |5 |desteğe başvurun |
| Ayrılmış genel IP adresleri |20 |desteğe başvurun |
| Dağıtım başına genel VIP |5 |desteğe başvurun |
| Dağıtım başına özel VIP (ILB) |1 |1 |
| Uç Nokta Erişim Denetim Listeleri (ACL’ler) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Ağ Limitleri - Azure Resource Manager
Aşağıdaki sınırlar yalnızca abonelik başına bölgeye göre Azure Resource Manager ile yönetilen ağ kaynakları için geçerlidir.

| Kaynak | Varsayılan limit | Üst Sınır |
| --- | --- | --- |
| Sanal ağlar |50 |1000 |
| Sanal ağ başına alt ağ sayısı |1000 |10000 |
| Sanal ağ başına sanal ağ eşlemesi bulunabilir |10 |50 |
| Sanal ağ başına DNS sunucusu sayısı |9 |25 |
| Sanal ağ başına özel IP Adresi sayısı |16384 ** |16384 |
| Ağ arabirimi başına özel IP Adresleri |256 |1024 |
| Bir sanal makine veya rol örneği NIC eşzamanlı TCP veya UDP akışlar |500K |500K |
| Ağ Arabirimleri (NIC) |24000 ** |24000 |
| Ağ Güvenlik Grupları (NSG) |100 |5000 |
| NSG başına NSG kuralları |1000 ** |1000 |
| IP adresleri ve aralıkları kaynak veya hedef bir güvenlik grubu için belirtilen |2000 |4000 |
| Uygulama güvenliği grupları |200 |500 |
| NIC başına IP yapılandırması başına uygulama güvenlik grupları |10 |20 |
| Uygulama güvenlik grubu başına IP yapılandırmaları |1000 |4000 |
| Ağ güvenlik grubunun tüm güvenlik kuralları içinde belirtilen uygulama güvenlik grupları |50 |100 |
| Kullanıcı tanımlı yol tabloları |100 |200 |
| Yol tablosu başına kullanıcı tanımlı yol sayısı |100 |400 |
| Genel IP adresleri - dinamik |(Temel) 60 |desteğe başvurun |
| Genel IP adresleri - statik |(Temel) 20 |desteğe başvurun |
| Genel IP adresleri - statik |(Standart) 20 |desteğe başvurun |
| VPN Ağ Geçidi başına Noktadan Siteye Kök Sertifika Sayısı |20 |20 |

** Daha önce desteğini artan bu sınırları uygulanmamış aboneliklere bu varsayılan sınırları Uygula

#### <a name="load-balancer"></a>Yük Dengeleyici sınırları

| Kaynak | Varsayılan limit | Üst Sınır |
| --- | --- | --- |
| Yük Dengeleyiciler | 100 | 1000 |
| Her bir kaynak, Basic kuralları | 150 | 250 |
| Her bir kaynak, standart kuralları | 1250 | 1500 |
| IP yapılandırması başına kuralı | 299 |299 |
| Ön uç IP yapılandırmaları, Basic | 10 | 200 |
| Ön uç IP yapılandırmaları, standart | 10 | 600 |
| Arka uç havuzu, Basic | 100, tek bir kullanılabilirlik kümesi | 100, tek bir kullanılabilirlik kümesi |
| Arka uç havuzu, standart | VNet 1000 tek | VNet 1000 tek |
| HA bağlantı noktaları, standart | İç ön uç başına 1 | İç ön uç başına 1 |

Varsayılan sınırları artırmanız gerekirse [desteğe başvurun](../articles/azure-supportability/resource-manager-core-quotas-request.md ).

