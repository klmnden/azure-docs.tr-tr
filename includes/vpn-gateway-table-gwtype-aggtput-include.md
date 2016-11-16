Aşağıdaki tabloda ağ geçidi türleri ve ağ geçidi SKU’suna göre tahmini toplam verimlilik gösterilmiştir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır. Ağ geçidi SKU'ları arasında fiyatlandırma farklılık gösterir. Daha fazla bilgi için bkz. [VPN Gateway Fiyatlandırması](https://azure.microsoft.com/pricing/details/vpn-gateway).

UltraPerformance ağ geçidi SKU’sunun bu tabloda temsil edilmediğini unutmayın. UltraPerformance SKU’su hakkında bilgi edinmek için [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) belgelerine bakın.

|  | **VPN Gateway performansı (1)** | **VPN Gateway maks. IPsec tüneli (2)** | **ExpressRoute Gateway performansı** | **VPN Gateway ve ExpressRoute bir arada** |
| --- | --- | --- | --- | --- |
| **Temel SKU (3)(5)** |100 Mbps |10 |500 Mb/sn |Hayır |
| **Standart SKU (4)(5)** |100 Mbps |10 |1000 Mb/sn |Evet |
| **Yüksek Performanslı SKU (4)** |200 Mbps |30 |2000 Mb/sn |Yes |

* (1) VPN işlemesi aynı Azure bölgesinde yer alan Vnet'ler arasındaki ölçümleri temel alan kaba bir tahmindir. Bu seçenek, İnternet üzerinden kurulan şirket içi ve şirket dışı karışık bağlantılar için garantili bir verimlilik değildir. Mümkün olan en yüksek verimlilik ölçümüdür.
* (2) RouteBased VPN’lere başvuran tünel sayısı. PolicyBased VPN yalnızca tek bir Siteden Siteye VPN tünelini destekleyebilir.
* (3) BGP, Temel SKU için desteklenmez.
* (4) PolicyBased VPN'ler bu SKU için desteklenmez. Bunlar yalnızca Temel SKU için desteklenir.
* (5) Etkin-etkin S2S VPN Gateway bağlantıları bu SKU için desteklenmiyor. Etkin-etkin, yalnızca Yüksek Performanslı SKU üzerinde desteklenir.



<!--HONumber=Nov16_HO2-->


