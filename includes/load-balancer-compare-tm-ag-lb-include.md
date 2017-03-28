## <a name="load-balancer-differences"></a>Load Balancer farklılıkları

Microsoft Azure’u kullanarak ağ trafiğini dağıtmak için farklı seçenekler bulunur. Bu seçenekler birbirlerinden farklı şekilde çalışır, farklı özelliklere sahiptir ve farklı senaryoları destekler. Birbirlerinden ayrı olarak veya birleştirilerek kullanılabilirler.

* **Azure Load Balancer**, aktarım katmanında (OSI ağ başvurusu yığınında Katman 4) çalışır. Aynı Azure veri merkezinde çalışan uygulama örnekleri arasında trafiğin ağ düzeyinde dağıtılmasını sağlar.
* **Application Gateway**, uygulama katmanında (OSI ağ başvurusu yığınında Katman 7) çalışır. İstemci bağlantısını sonlandıran ve istekleri arka uç noktalarına ileten ters proxy hizmeti olarak çalışır.
* **Traffic Manager**, DNS düzeyinde çalışır.  Son kullanıcı trafiğini küresel ölçekte dağıtılmış uç noktalara yönlendirmek için DNS yanıtları kullanır. İstemciler daha sonra bu uç noktalara doğrudan bağlanır.

Aşağıdaki tabloda, hizmetler tarafından sunulan özellikler özetlenmiştir:

| Hizmet | Azure Load Balancer | Application Gateway | Traffic Manager |
| --- | --- | --- | --- |
| Teknoloji |Aktarım düzeyi (Katman 4) |Uygulama düzeyi (Katman 7) |DNS düzeyi |
| Desteklenen uygulama protokolleri |Herhangi biri |HTTP, HTTPS ve WebSockets |Herhangi biri (Uç nokta izleme için HTTP uç noktası gereklidir) |
| Uç Noktalar |Azure VM’leri ve Cloud Services rol örnekleri |Herhangi bir iç Azure IP adresi, genel internet IP adresi, Azure VM veya Azure Bulut Hizmeti |Azure VM’leri, Cloud Services, Azure Web Apps ve dış uç noktalar |
| Vnet desteği |Hem İnternet’e yönelik hem de iç (Vnet) uygulamalar için kullanılabilir |Hem İnternet’e yönelik hem de iç (Vnet) uygulamalar için kullanılabilir |Yalnızca İnternet'e yönelik uygulamaları destekler |
| Uç Nokta İzleme |Araştırmalar aracılığıyla desteklenir |Araştırmalar aracılığıyla desteklenir |HTTP/HTTPS GET aracılığıyla desteklenir |

Azure Load Balancer ve Application Gateway, ağ trafiğini uç noktalara yönlendirir, ancak hangi trafiğin işleneceğine ilişkin farklı kullanım senaryolara sahiptir. Aşağıdaki tablo, iki yük dengeleyici arasındaki farkın anlaşılmasına yardımcı olabilir:

| Tür | Azure Load Balancer | Application Gateway |
| --- | --- | --- |
| Protokoller |UDP/TCP |HTTP, HTTPS ve WebSockets |
| IP ayırma |Destekleniyor |Desteklenmiyor |
| Yük dengeleme modu |5’li demet (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası, protokol türü) |Hepsini Bir Kez Deneme<br>URL'ye dayalı yönlendirme |
| Yük dengeleme modu (kaynak IP/yapışkan oturumlar) |2’li demet (kaynak IP ve hedef IP), 3’lü demet (kaynak IP, hedef IP ve bağlantı noktası). Sanal makine sayısına bağlı olarak ölçeği artırabilir veya azaltılabilir |Tanımlama bilgisi tabanlı benzeşim<br>URL'ye dayalı yönlendirme |
| Sistem durumu araştırmaları |Varsayılan: araştırma aralığı - 15 saniye Yönlendirme dışına çıkarma: Arka arkaya 2 hata. Kullanıcı tanımlı araştırmaları destekler |Boşta araştırma aralığı 30 saniye. Arka arkaya 5 canlı trafik hatasından veya boşta modunda tek bir araştırma hatasından sonra çıkarılır. Kullanıcı tanımlı araştırmaları destekler |
| SSL aktarma |Desteklenmiyor |Destekleniyor |
| URL tabanlı yönlendirme | Desteklenmiyor | Destekleniyor|
| SSL İlkesi | Desteklenmiyor | Destekleniyor|
