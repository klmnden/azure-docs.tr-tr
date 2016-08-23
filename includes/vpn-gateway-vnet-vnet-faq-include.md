- Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.

- Bir bulut hizmeti ya da yük dengeleme uç noktası, birbirlerine bağlı olsa da sanal ağlara YAYILAMAZ.

- İşletmeler arası bağlantı gerekmediği sürece birden fazla Azure sanal ağını birleştirmek için şirket içi VPN ağ geçitlerine gerek yoktur. 

- Sanal Ağdan Sanal Ağa, sanal ağları bağlamayı destekler. Bir sanal ağ içinde OLMAYAN sanal makineleri veya bulut hizmetlerini bağlamayı desteklemez.

- Sanal Ağdan Sanal Ağa, Azure VPN ağ geçitlerinin, önceki adı Dinamik Yönlendirme olan RouteBased VPN türleri ile bağlantılarını VPN geçidi ile bağlamalarını gerektirir. 

- Sanal ağ bağlantıları aynı anda çoklu site VPNleri ile kullanılabilir. Diğer sanal ağlara ya da şirket içi sitelere bağlanan sanal ağ VPN ağ geçidinin VPN tünelleri en fazla 10 (Varsayılan/Standart Ağ Geçitleri) veya 30 (Yüksek Performanslı Ağ Geçitleri) adet olabilir.

- Sanal ağlar ve şirket içi yerel ağ sitelerinin adres alanları çakışmamalıdır. Adres alanlarının çakışması, Sanal Ağdan Sanal Ağa bağlantılarının başarısız olmasına sebep olabilir.

- Bir çift sanal ağ arasındaki yedekli tüneller desteklenmez.

- Sanal ağ üzerindeki tüm VPN tünelleri, Azure VPN ağ geçidi ve aynı ağ geçidi çalışma süresi SLA’sı üzerinde aynı kullanılabilir bant genişliğini paylaşır.

- Sanal Ağdan Sanal Ağa trafiği akışı, İnternet üzerinden değil Microsoft Network üzerinden sağlanır.

- Aynı bölge içerisinde Sanal Ağdan Sanal Ağa trafiği her iki yön için de ücretsizdir, çapraz bölge Sanal Ağdan Sanal Ağa çıkış trafiği ise kaynak bölgelerine bağlı giden Sanal Ağlar arası veri aktarım hızına bağlı olarak ücretlendirilir.  Ayrıntılar için lütfen [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/vpn-gateway/)nı inceleyin.


<!--HONumber=Aug16_HO1-->


