- **İlke temelli VPN türü:** İlke temelli VPN'lere daha önce, klasik dağıtım modelinde statik yönlendirme ağ geçitleri adı veriliyordu. İlke temelli VPN'ler, şirket içi ağınızla Azure Vnet'iniz arasında adres öneklerinin birleşimleriyle yapılandırılmış IPSec ilkeleri temelindeki IPSec tüneller üzerinden paketleri şifreler ve yönlendirirler. İlke (veya trafik seçici) çoğunlukla VPN cihazı yapılandırmasında bir erişim listesi olarak tanımlanır. İlke temelli VPN türüyle ilgili değer *PolicyBased*’dir.

- **Rota temelli VPN türü:** Rota temelli VPN'lere daha önce, klasik dağıtım modelinde dinamik yönlendirme ağ geçitleri adı veriliyordu. Rota temelli VPN'ler, paketleri kendi ilgili arabirimlerine yönlendirmek için IP iletme veya yönlendirme tablosunda "yolları" seçeneğini kullanır. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. Rota temelli VPN’lerle ilgili ilke (veya trafik seçici) herhangi birinden herhangi birine (veya joker karakterler) olarak yapılandırılır. Rota temelli VPN türüyle ilgili değer *RouteBased*’dır.


<!--HONumber=Jun16_HO2-->


