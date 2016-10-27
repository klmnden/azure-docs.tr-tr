|    | **VPN Gateway işlemesi (1)** | **VPN Gateway maks. IPSec tünelleri (2)** | **ExpressRoute Ağ Geçidi işlemesi** | **VPN Gateway ve ExpressRoute bir arada**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Temel SKU (3)**              |  100 Mbps | 10                         |  500 Mb/sn                           | Hayır   |
| **Standart SKU (4)**           |  100 Mbps | 10                         | 1000 Mb/sn                           | Evet  |
| **Yüksek Performanslı SKU (4)**   | 200 Mbps  | 30                         | 2000 Mb/sn                           | Yes  |

- (1) VPN işlemesi aynı Azure bölgesinde yer alan Vnet'ler arasındaki ölçümleri temel alan kaba bir tahmindir. Bu seçenek, İnternet üzerinden kurulan şirket içi ve şirket dışı karışık bağlantılar için garantili bir verimlilik değildir. Mümkün olan en yüksek verimlilik ölçümüdür.
- (2) RouteBased VPN’lere başvuran tünel sayısı. PolicyBased VPN yalnızca tek bir Siteden Siteye VPN tünelini destekleyebilir.
- (3) BGP, Temel SKU için desteklenmez.
- (4) PolicyBased VPN'ler bu SKU için desteklenmez. Bunlar yalnızca Temel SKU için desteklenir.

<!--HONumber=Oct16_HO3-->


