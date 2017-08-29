Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU’sunu belirtmeniz gerekir. İş yükü, aktarım hızı, özellik ve SLA türlerine bağlı olarak gereksinimlerinize uyan SKU'ları seçin.

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <a name="workloads"></a>Üretim *ve* Geliştirme-Test İş Yükleri Karşılaştırması

SLA'lardaki ve özellik kümelerindeki farklılıklar nedeniyle üretim ve geliştirme-test iş yükleri için aşağıdaki *farklı*  SKU'ları öneririz:

| **İş yükü**                       | **SKU'lar**               |
| ---                                | ---                    |
| **Üretim, kritik iş yükleri** | VpnGw1, VpnGw2, VpnGw3 |
| **Geliştirme-test veya kavram kanıtı**   | Temel                  |
|                                    |                        |

Eski SKU'ları kullanıyorsanız üretim için Standart ve Yüksek Performanslı SKU'lar önerilir. Eski SKU'lar hakkında bilgi için bkz. [Ağ geçidi SKU'ları (eski SKU’lar)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).

###  <a name="feature"></a>Ağ geçidi SKU'su özellik kümeleri

Yeni ağ geçidi SKU'ları, ağ geçitlerinde sunulan özellik kümeleri açısından kolaylık sağlar:

| **SKU**| **Özellikler**|
| ---    | ---         |
|**Temel**   | **Rota tabanlı VPN**: P2S ile 10 tünel<br><br>**İlke tabanlı VPN** (IKEv1): 1 tünel; P2S yok|
| **VpnGw1, VpnGw2 ve VpnGw3** | **Rol tabanlı VPN**: 30 tünele kadar (*), P2S, BGP, etkin-etkin, özel IPsec/IKE ilkesi, ExpressRoute/VPN birlikte kullanımı |
|        |             |

(*) Rota tabanlı bir VPN ağ geçidini (VpnGw1, VpnGw2, VpnGw3) şirket içi ilke tabanlı birden fazla güvenlik duvarı cihazına bağlamak için "PolicyBasedTrafficSelectors" yapılandırması gerçekleştirebilirsiniz. Ayrıntılı bilgi için bkz. [PowerShell kullanarak VPN ağ geçitlerini şirket içi ilke tabanlı birden fazla VPN cihazına bağlama](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).

###  <a name="resize"></a>Ağ geçidi SKU'larını yeniden boyutlandırma

1. VpnGw1, VpnGw2 ve VpnGw3 SKU'ları arasında yeniden boyutlandırma gerçekleştirebilirsiniz.
2. Eski ağ geçidi SKU'larıyla çalışırken Temel, Standart ve Yüksek Performanslı SKU'lar arasında yeniden boyutlandırma yapabilirsiniz.
2. Temel/Standart/Yüksek Performanslı SKU'ları yeni VpnGw1/VpnGw2/VpnGw3 SKU'larıyla aynı olacak şekilde **yeniden boyutlandıramazsınız**. Bunun yerine yeni SKU'lara [geçiş](#migrate) yapmanız gerekir.

###  <a name="migrate"></a>Eski SKU'lardan yeni SKU'lara geçiş

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
