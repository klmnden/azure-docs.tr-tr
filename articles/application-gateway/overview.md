---
title: Azure Application Gateway nedir?
description: Uygulamanıza web trafiğini yönetmek için bir Azure uygulama ağ geçidini nasıl kullanabileceğinizi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.date: 4/18/2019
ms.author: victorh
ms.openlocfilehash: 91e94c70444430725ffec836d1663aef99eb5496
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60003369"
---
# <a name="what-is-azure-application-gateway"></a>Azure Application Gateway nedir?

Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir. Geleneksel yük dengeleyiciler aktarım katmanında (OSI katman 4 - TCP ve UDP) çalışır ve trafiği kaynak IP adresi ve bağlantı noktasına göre hedef bir IP adresi ve bağlantı noktasına yönlendirir.

![Uygulama ağ geçidi kavramsal](media/overview/figure1-720.png)

Ancak Application Gateway ile çok daha spesifik olabilirsiniz. Örneğin, gelen URL’yi temel alarak trafiği yönlendirebilirsiniz. Yani `/images` gelen URL’deyse, trafiği görüntüler için yapılandırılmış belirli bir sunucu kümesine (havuz olarak da bilinir) yönlendirebilirsiniz. URL’de `/video` varsa, bu trafik videolar için iyileştirilmiş başka bir havuza yönlendirilir.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Bu yönlendirme türü, uygulama katmanı (OSI katman 7) yük dengelemesi olarak bilinir. Azure Application Gateway, URL tabanlı yönlendirmeyi ve daha fazlasını yapabilir. 

Azure Application Gateway aşağıdaki özellikleri içerir:

## <a name="autoscaling-public-preview"></a>Otomatik ölçeklendirme genel önizleme sürümü

Bu makalede açıklanan özelliklerine ek olarak, Application Gateway, ayrıca otomatik ölçeklendirme ve diğer önemli performans geliştirmeleri sunan yeni bir SKU'ya [Standard_V2] genel önizlemesini sunar.

- **Otomatik ölçeklendirme** - Otomatik ölçeklendirme SKU'su kapsamındaki Application Gateway veya WAF dağıtımlarının ölçeği değişen trafik yükü desenlerine göre artırılabilir veya azaltılabilir. Otomatik ölçeklendirme ayrıca sağlama sırasında dağıtım boyutu veya örnek sayısı seçme gereksinimini de ortadan kaldırır. 

- **Alan yedekliliği** - Bir Application Gateway veya WAF dağıtımı birden fazla Kullanılabilirlik Bölgesine dağıtılarak Traffic Manager içindeki her bölgede ayrı bir Application Gateway örneği sağlama ve çalıştırma ihtiyacını ortadan kaldırır.

- **Statik VIP** - Application Gateway VIP özelliği artık VIP türünü de desteklemektedir. Bu da Application Gateway ile ilişkilendirilmiş VIP değerinin yeniden başlatma sonrasında dahi aynı kalmasını sağlar.

- Genel kullanıma açık SKU ile karşılaştırıldığında **daha hızlı dağıtım ve güncelleştirme**. 

- Genel kullanıma açık SKU ile karşılaştırıldığında **5 kat daha iyi SSL yük boşaltma performansı**.

Application Gateway genel önizleme özellikleri hakkında daha fazla bilgi için bkz. [Otomatik Ölçeklendirme Yapan ve Alanlar Arası Yedekli Application Gateway (Genel Önizleme)](application-gateway-autoscaling-zone-redundant.md).

## <a name="secure-sockets-layer-ssl-termination"></a>Güvenli Yuva Katmanı (SSL) sonlandırma

Application Gateway, ağ geçidinde SSL sonlandırmasını destekler. Bu sonlandırmanın ardından, trafik genelde arka uç sunucularına şifrelenmemiş olarak akar. Bu özellik, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını sağlar. Ancak bazen şifrelenmemiş iletişimin sunucularına kabul edilebilir bir seçenek değildir. Bu güvenlik gereksinimleri, uyumluluk gereksinimleri nedeniyle olabilir veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Bu uygulamalar için application gateway uçtan uca SSL şifrelemesini destekler.

## <a name="azure-kubernetes-service-aks-ingress-controller-preview"></a>Azure Kubernetes Service (AKS) giriş denetleyicisi önizlemesi 

Application Gateway giriş denetleyicisi, AKS kümesi içinde bir pod olarak çalışır ve Application Gateway'in AKS kümesi için giriş görevi yapmasını sağlar. Bu, yalnızca uygulama ağ geçidi v2 ile desteklenir.

Daha fazla bilgi için bkz. [Azure Application Gateway Giriş Denetleyicisi](https://azure.github.io/application-gateway-kubernetes-ingress/).

## <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerinin normal bir şekilde kapatılmasına yardımcı olur. Bu ayar bir arka uç http ayarıyla etkinleştirilir ve kural oluşturma sırasında bir arka uç havuzunun tüm üyelerine uygulanabilir. Application Gateway etkinleştirildikten sonra bir arka uç havuzunun tüm kayıt kaldırma örneklerinin yeni istek almamasını ve var olan isteklerin yapılandırılan süre sınırı içinde tamamlanmasını sağlar. Bu durum hem bir API çağrısı ile açıkça arka uç havuzundan kaldırılan arka uç örnekleri hem de durum yoklamaları ile iyi durumda olmadığı bildirilen arka uç örnekleri için geçerlidir.

## <a name="custom-error-pages"></a>Özel hata sayfaları
Application Gateway, varsayılan hata sayfalarını göstermek yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz.

Daha fazla bilgi için bkz. [Application Gateway özel hata sayfası oluşturma](custom-error.md).

## <a name="web-application-firewall"></a>Web uygulaması güvenlik duvarı

Web uygulaması güvenlik duvarı (WAF), web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir Application Gateway özelliğidir. WAF bu işlemi [OWASP (Açık Web Uygulaması Güvenlik Projesi) çekirdek kural kümeleri](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9’daki kurallara göre yapar. 

Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birçok katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

Daha fazla bilgi için [Application Gateway içindeki Web uygulaması Güvenlik Duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/waf-overview)).

## <a name="url-based-routing"></a>URL tabanlı yönlendirme

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. Senaryolardan biri, farklı içerik türleri için istekleri farklı havuzlara yönlendirmektir.

Örneğin, `http://contoso.com/video/*` için istekler VideoServerPool’a ve `http://contoso.com/images/*` için istekler ImageServerPool’a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

Daha fazla bilgi için [Application Gateway ile URL tabanlı yönlendirme](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

## <a name="multiple-site-hosting"></a>Birden çok site barındırma

Birden çok site barındırma, aynı uygulama ağ geçidi örneğinde birden fazla web sitesi yapılandırmanızı sağlar. Bu özellik, bir uygulama ağ geçidine en fazla 100 web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak tanır. Her web sitesi kendi havuzuna yönlendirilebilir. Örneğin, uygulama ağ geçidi ContosoServerPool ve FabrikamServerPool adlı iki sunucu havuzundan `contoso.com` ve `fabrikam.com` için trafik sunabilir.

`http://contoso.com` için istekler ContosoServerPool’a ve `http://fabrikam.com` için istekler FabrikamServerPool’a yönlendirilir.

Benzer şekilde, aynı üst etki alanının iki alt etki alanı, aynı uygulama ağ geçidi dağıtımında barındırılabilir. Alt etki alanı kullanım örnekleri, tek bir uygulama ağ geçidi dağıtımında barındırılan `http://blog.contoso.com` ve `http://app.contoso.com` öğelerini içerebilir.

Daha fazla bilgi için [uygulama ağ geçidi ile birden çok site barındırma](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview).

## <a name="redirection"></a>Yönlendirme

Birçok web uygulaması için yaygın bir senaryo, bir uygulama ile kullanıcıları arasındaki tüm iletişimin şifrelenmiş bir yol üzerinden yapıldığından emin olmak için otomatik HTTP’yi HTTPS’ye dönüştürme yeniden yönlendirmesini desteklemektir. 

Geçmişte, tek amacı HTTP üzerinde aldığı istekleri HTTPS’ye yeniden yönlendirmek olan ayrılmış bir havuz oluşturmak gibi teknikler kullanmış olabilirsiniz. Uygulama ağ geçidi, Application Gateway’de trafiği yeniden yönlendirme özelliğini destekler. Bu uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler. Application Gateway yeniden yönlendirme desteği yalnızca HTTP’den HTTPS’ye yönlendirme ile sınırlı değildir. Bu, kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasından ve bağlantı noktasına yeniden yönlendirebilmeniz için genel bir yeniden yönlendirme mekanizmasıdır. Ayrıca, bir dış siteye yönlendirmeyi de destekler.

Application Gateway yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

- Ağ Geçidi üzerindeki bir bağlantı noktasından başka bir bağlantı noktasına genel yeniden yönlendirme. Bu özellik, bir sitede HTTP’den HTTPS’ye yeniden yönlendirmeyi sağlar.
- Yol tabanlı yönlendirme. Bu tür yeniden yönlendirmeler, HTTP’den HTTPS’ye yeniden yönlendirmeyi yalnızca belirli bir site alanında (örneğin `/cart/*` tarafından belirtilen bir alışveriş sepetinde) etkinleştirir.
- Dış siteye yeniden yönlendirme.

Daha fazla bilgi için [trafik yönlendirme](https://docs.microsoft.com/azure/application-gateway/redirect-overview) uygulama ağ geçidi ile.

## <a name="session-affinity"></a>Oturum benzeşimi

Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı sunucuda tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.

## <a name="websocket-and-http2-traffic"></a>Websocket ve HTTP/2 trafiği

Application Gateway, WebSocket ve HTTP/2 protokolleri için yerel destek sağlar. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur.

WebSocket ve HTTP/2 protokolleri, uzun süre çalışan bir TCP bağlantısı üzerinden bir sunucu ile bir istemci arasında tam çift yönlü iletişimi etkinleştirir. Bu, web sunucusu ile istemci arasında HTTP tabanlı uygulamalarda gerektiği gibi yoklama olmadan çift yönlü olabilen daha etkileşimli bir iletişime olanak sağlar. Bu protokoller HTTP’den farklı olarak düşük yüke sahiptir ve kaynakların daha verimli bir şekilde kullanılması için aynı TCP bağlantısını birden fazla istek/yanıt için kullanabilir. Bu protokoller, geleneksel HTTP bağlantı noktaları 80 ve 443 üzerinden çalışmak üzere tasarlanmıştır.

Daha fazla bilgi için [WebSocket desteği](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) ve [HTTP/2 desteği](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support). 

## <a name="rewrite-http-headers-public-preview"></a>HTTP üstbilgileri (genel Önizleme) yeniden yazma

HTTP üstbilgileri istemci ve sunucu istek veya yanıt ek bilgilerle geçmesine izin verin. Bu HTTP üstbilgileri yeniden yazma gibi birçok önemli senaryoyu gerçekleştirmenize yardımcı olur:
- Güvenlikle ilgili üstbilgi alanlarını HSTS gibi / X XSS koruma ekleniyor.
- Hassas bilgilerin ortaya koyabilir yanıt üstbilgi alanlarını kaldırılıyor.
- X-iletilen-için üst bağlantı noktası bilgilerini arındırılıyor.

Uygulama ağ geçidi eklemek, kaldırmak veya HTTP istek ve yanıt üstbilgileri, istek ve yanıt paketleri istemci ve arka uç havuzları arasında taşırken güncelleştirme yeteneğini destekler. Ayrıca, belirtilen üst bilgiler, yalnızca belirli koşullar karşılandığında yazılır emin olmak için koşulları ekleme olanağı sağlar.

Bu genel önizleme özelliği hakkında daha fazla bilgi için bkz. [yeniden HTTP üstbilgileri](rewrite-http-headers.md).

## <a name="sizing"></a>Boyutlandırma

Uygulama ağ geçidi, şu anda üç büyüklükte sunulmaktadır: **Küçük**, **orta**, ve **büyük**. Küçük örnek boyutları, geliştirme ve test senaryolarına yöneliktir.

Application Gateway limitlerinin tam listesi için bkz. [Application Gateway hizmet limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Aşağıdaki tabloda, SSL boşaltmasının etkin olduğu her bir Application Gateway örneği için ortalama performans aktarım hızı gösterilmiştir:

| Ortalama arka uç sayfa yanıtı boyutu | Küçük | Orta | Büyük |
| --- | --- | --- | --- |
| 6 KB |7,5 Mbps |13 Mbps |50 Mbps |
| 100 KB |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Bu değerler bir uygulama ağ geçidi verimliliği için yaklaşık değerlerdir. Gerçek verimlilik; ortalama sayfa boyutu, arka uç örneklerinin konumu ve bir sayfaya hizmet etmek için işleme süresi gibi çeşitli ortam ayrıntılarına bağlıdır. Tam performans rakamlarına ulaşmak için kendi testlerinizi çalıştırmanız gerekir. Bu değerler yalnızca kapasite planlama konusunda yardımcı olmak için verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Gereksinimleriniz ve ortamınıza bağlı olarak, Azure portalı, Azure PowerShell veya Azure CLI kullanarak bir test Application Gateway oluşturabilirsiniz:

- [Hızlı Başlangıç: Azure Application Gateway - Azure portalı ile web trafiğini doğrudan](quick-create-portal.md).
- [Hızlı Başlangıç: Azure Application Gateway - Azure PowerShell ile doğrudan web trafiği](quick-create-powershell.md)
- [Hızlı Başlangıç: Azure Application Gateway - Azure CLI ile doğrudan web trafiği](quick-create-cli.md)
