|    | **VPN Gateway işlemesi (1)** | **VPN Gateway maks. IPSec tünelleri (2)** | **ExpressRoute Ağ Geçidi işlemesi** | **VPN Gateway ve ExpressRoute bir arada**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Temel SKU**              |  100 Mb/sn | 10                         |  500 Mb/sn                           | Hayır   |
| **Standart SKU**           |  100 Mb/sn | 10                         | 1000 Mb/sn                           | Evet  |
| **Yüksek Performanslı SKU (3)**   | 200 Mbps  | 30                         | 2000 Mb/sn                           | Yes  |

- (1) VPN işlemesi aynı Azure bölgesinde yer alan Vnet'ler arasındaki ölçümleri temel alan kaba bir tahmindir. İnternet’te şirket içi ve dışı karışık bağlantılar için alabileceklerinizin bir garantisi olmasa da en fazla olası bir ölçü olarak kullanılmalıdır.
- (2) Aşağıdaki yol tabanlı VPN’ye başvuran tünel sayısı. İlke tabanlı bir VPN yalnızca tek bir Siteden Siteye VPN tünelini destekleyebilir.
- (3) Yüksek Performanslı SKU, ilke tabanlı VPN türü için desteklenmez.


<!--HONumber=Aug16_HO1-->


