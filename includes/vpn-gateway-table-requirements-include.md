Aşağıdaki tabloda, ilke temelli ve rota temelli VPN Gateway’lerinin gereksinimleri listelenmektedir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır. Klasik model için, İlke temelli VPN ağ geçitleri Statik ağ geçitleriyle aynıdır; Rota temelli ağ geçitleri de Dinamik ağ geçitleriyle aynıdır.

|  | **İlke Temelli Temel VPN Gateway** | **Rota Temelli Temel VPN Gateway** | **Rota Temelli Standart VPN Gateway** | **Rota Temelli Yüksek Performanslı VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Siteden Siteye bağlantı   (S2S)** |İlke temelli VPN yapılandırması |Rota temelli VPN yapılandırması |Rota temelli VPN yapılandırması |Rota temelli VPN yapılandırması |
| **Noktadan Siteye bağlantı (P2S**) |Desteklenmiyor |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |
| **Kimlik doğrulama Yöntemi** |Önceden paylaşılan anahtar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |
| **S2S bağlantılarının en yüksek sayısı** |1 |10 |10 |30 |
| **P2S bağlantılarının en yüksek sayısı** |Desteklenmiyor |128 |128 |128 |
| **Etkin yönlendirme desteği (BGP)** |Desteklenmiyor |Desteklenmiyor |Destekleniyor |Destekleniyor |

<!--HONumber=Aug16_HO1-->


