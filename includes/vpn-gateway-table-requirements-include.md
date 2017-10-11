Aşağıdaki tabloda PolicyBased ve RouteBased VPN ağ geçitleri için gereksinimleri listelenmiştir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır. Klasik modeli PolicyBased VPN ağ geçitleri statik ağ geçitleri ile aynıdır ve rota tabanlı ağ geçitleri dinamik ağ geçitleri ile aynıdır.

|  | **PolicyBased temel VPN ağ geçidi** | **RouteBased temel VPN ağ geçidi** | **RouteBased standart VPN ağ geçidi** | **RouteBased yüksek performanslı VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Siteden siteye bağlantı (S2S)** |PolicyBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |
| **Noktadan Siteye bağlantı (P2S**) |Desteklenmiyor |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |
| **Kimlik doğrulama yöntemi** |Önceden paylaşılan anahtar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |
| **S2S bağlantısı sayısı** |1 |10 |10 |30 |
| **P2S bağlantısı sayısı** |Desteklenmiyor |128 |128 |128 |
| **Etkin yönlendirme desteği (BGP)** |Desteklenmiyor |Desteklenmiyor |Destekleniyor |Destekleniyor |

