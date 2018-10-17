---
title: Azure Application Gateway nedir?
description: Uygulamanıza web trafiğini yönetmek için bir Azure uygulama ağ geçidini nasıl kullanabileceğinizi öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.workload: infrastructure-services
ms.date: 10/11/2018
ms.author: victorh
ms.openlocfilehash: 8352a95fa0701f6d2a0261d8d2fe2431971eccef
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068104"
---
# <a name="what-is-azure-application-gateway"></a>Azure Application Gateway nedir?

Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir. 

Geleneksel yük dengeleyiciler aktarım katmanında (OSI katman 4 - TCP ve UDP) çalışır ve trafiği kaynak IP adresi ve bağlantı noktasına göre hedef bir IP adresi ve bağlantı noktasına yönlendirir. Ancak Application Gateway ile çok daha spesifik olabilirsiniz. Örneğin, gelen URL’yi temel alarak trafiği yönlendirebilirsiniz. Yani `/images` gelen URL’deyse, trafiği görüntüler için yapılandırılmış belirli bir sunucu kümesine (havuz olarak da bilinir) yönlendirebilirsiniz. URL’de `/video` varsa, bu trafik videolar için iyileştirilmiş başka bir havuza yönlendirilir.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Bu yönlendirme türü, uygulama katmanı (OSI katman 7) yük dengelemesi olarak bilinir. Azure Application Gateway, URL tabanlı yönlendirmeyi ve daha fazlasını yapabilir. 

Azure Application Gateway aşağıdaki özellikleri içerir:

## <a name="autoscaling-public-preview"></a>Otomatik ölçeklendirme genel önizleme sürümü

Application Gateway, bu makalede anlatılan özelliklere ek olarak otomatik ölçeklendirme ve diğer kritik performans geliştirmeleri sunan yeni bir SKU [Standard_V2] genel önizleme sürümü de sunmaktadır.

- **Otomatik ölçeklendirme** - Otomatik ölçeklendirme SKU'su kapsamındaki Application Gateway veya WAF dağıtımlarının ölçeği değişen trafik yükü desenlerine göre artırılabilir veya azaltılabilir. Otomatik ölçeklendirme ayrıca sağlama sırasında dağıtım boyutu veya örnek sayısı seçme gereksinimini de ortadan kaldırır. 

- **Alan yedekliliği** - Bir Application Gateway veya WAF dağıtımı birden fazla Kullanılabilirlik Bölgesine dağıtılarak Traffic Manager içindeki her bölgede ayrı bir Application Gateway örneği sağlama ve çalıştırma ihtiyacını ortadan kaldırır.

- **Statik VIP** - Application Gateway VIP özelliği artık VIP türünü de desteklemektedir. Bu da Application Gateway ile ilişkilendirilmiş VIP değerinin yeniden başlatma sonrasında dahi aynı kalmasını sağlar.

- Genel kullanıma açık SKU ile karşılaştırıldığında **daha hızlı dağıtım ve güncelleştirme**. 

- Genel kullanıma açık SKU ile karşılaştırıldığında **5 kat daha iyi SSL yük boşaltma performansı**.

Application Gateway genel önizleme özellikleri hakkında daha fazla bilgi için bkz. [Otomatik Ölçeklendirme Yapan ve Bölgesel Olarak Yedekli Application Gateway (Genel Önizleme)](application-gateway-autoscaling-zone-redundant.md).

## <a name="azure-kubernetes-service-aks-ingress-controller-preview"></a>Azure Kubernetes Service (AKS) giriş denetleyicisi önizlemesi 

Application Gateway giriş denetleyicisi, AKS kümesi içinde bir pod olarak çalışır ve Application Gateway'in AKS kümesi için giriş görevi yapmasını sağlar. 

Daha fazla bilgi için bkz. [Azure Application Gateway Giriş Denetleyicisi](https://azure.github.io/application-gateway-kubernetes-ingress/).

## <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerinin normal bir şekilde kapatılmasına yardımcı olur. Bu ayar bir arka uç http ayarıyla etkinleştirilir ve kural oluşturma sırasında bir arka uç havuzunun tüm üyelerine uygulanabilir. Application Gateway etkinleştirildikten sonra bir arka uç havuzunun tüm kayıt kaldırma örneklerinin yeni istek almamasını ve var olan isteklerin yapılandırılan süre sınırı içinde tamamlanmasını sağlar. Bu durum hem bir API çağrısı ile açıkça arka uç havuzundan kaldırılan arka uç örnekleri hem de durum yoklamaları ile iyi durumda olmadığı bildirilen arka uç örnekleri için geçerlidir.

## <a name="custom-error-pages"></a>Özel hata sayfaları
Application Gateway, varsayılan hata sayfalarını göstermek yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz.

Daha fazla bilgi için bkz. [Application Gateway özel hata sayfası oluşturma](custom-error.md).

## <a name="secure-sockets-layer-ssl-termination"></a>Güvenli Yuva Katmanı (SSL) sonlandırma

Application Gateway, ağ geçidinde SSL sonlandırmasını destekler. Bu sonlandırmanın ardından, trafik genelde arka uç sunucularına şifrelenmemiş olarak akar. Bu özellik, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını sağlar. Ancak sunucularla şifrelenmemiş iletişim, bazen kabul edilebilir bir seçenek olmayabilir. Bunun nedeni, güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Application Gateway, böyle uygulamalar için uçtan uca SSL şifrelemesini desteklemektedir.

## <a name="web-application-firewall"></a>Web uygulaması güvenlik duvarı

Web uygulaması güvenlik duvarı (WAF), web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıklarına karşı merkezi koruma sağlayan bir Application Gateway özelliğidir. WAF bu işlemi [OWASP (Açık Web Uygulaması Güvenlik Projesi) çekirdek kural kümeleri](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 veya 2.2.9’daki kurallara göre yapar. 

Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birçok katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

## <a name="url-based-routing"></a>URL tabanlı yönlendirme

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. Senaryolardan biri, farklı içerik türleri için istekleri farklı havuzlara yönlendirmektir.

Örneğin, `http://contoso.com/video/*` için istekler VideoServerPool’a ve `http://contoso.com/images/*` için istekler ImageServerPool’a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

## <a name="multiple-site-hosting"></a>Birden çok site barındırma

Birden çok site barındırma, aynı uygulama ağ geçidi örneğinde birden fazla web sitesi yapılandırmanızı sağlar. Bu özellik, bir uygulama ağ geçidine en fazla 20 web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak tanır. Her web sitesi kendi havuzuna yönlendirilebilir. Örneğin, uygulama ağ geçidi ContosoServerPool ve FabrikamServerPool adlı iki sunucu havuzundan `contoso.com` ve `fabrikam.com` için trafik sunabilir.

`http://contoso.com` için istekler ContosoServerPool’a ve `http://fabrikam.com` için istekler FabrikamServerPool’a yönlendirilir.

Benzer şekilde, aynı üst etki alanının iki alt etki alanı, aynı uygulama ağ geçidi dağıtımında barındırılabilir. Alt etki alanı kullanım örnekleri, tek bir uygulama ağ geçidi dağıtımında barındırılan `http://blog.contoso.com` ve `http://app.contoso.com` öğelerini içerebilir.

## <a name="redirection"></a>Yönlendirme

Birçok web uygulaması için yaygın bir senaryo, bir uygulama ile kullanıcıları arasındaki tüm iletişimin şifrelenmiş bir yol üzerinden yapıldığından emin olmak için otomatik HTTP’yi HTTPS’ye dönüştürme yeniden yönlendirmesini desteklemektir. 

Geçmişte, tek amacı HTTP üzerinde aldığı istekleri HTTPS’ye yeniden yönlendirmek olan ayrılmış bir havuz oluşturmak gibi teknikler kullanmış olabilirsiniz. Uygulama ağ geçidi, Application Gateway’de trafiği yeniden yönlendirme özelliğini destekler. Bu uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler. Application Gateway yeniden yönlendirme desteği yalnızca HTTP’den HTTPS’ye yönlendirme ile sınırlı değildir. Bu, kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasından ve bağlantı noktasına yeniden yönlendirebilmeniz için genel bir yeniden yönlendirme mekanizmasıdır. Ayrıca, bir dış siteye yönlendirmeyi de destekler.

Application Gateway yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

- Ağ Geçidi üzerindeki bir bağlantı noktasından başka bir bağlantı noktasına genel yeniden yönlendirme. Bu özellik, bir sitede HTTP’den HTTPS’ye yeniden yönlendirmeyi sağlar.
- Yol tabanlı yönlendirme. Bu tür yeniden yönlendirmeler, HTTP’den HTTPS’ye yeniden yönlendirmeyi yalnızca belirli bir site alanında (örneğin `/cart/*` tarafından belirtilen bir alışveriş sepetinde) etkinleştirir.
- Dış siteye yeniden yönlendirme.



## <a name="session-affinity"></a>Oturum benzeşimi

Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı sunucuda tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.




## <a name="websocket-and-http2-traffic"></a>Websocket ve HTTP/2 trafiği

Application Gateway, WebSocket ve HTTP/2 protokolleri için yerel destek sağlar. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur. HTTP/2 desteği, Azure PowerShell kullanılarak etkinleştirilebilir.
 
WebSocket ve HTTP/2 protokolleri, uzun süre çalışan bir TCP bağlantısı üzerinden bir sunucu ile bir istemci arasında tam çift yönlü iletişimi etkinleştirir. Bu, web sunucusu ile istemci arasında HTTP tabanlı uygulamalarda gerektiği gibi yoklama olmadan çift yönlü olabilen daha etkileşimli bir iletişime olanak sağlar. Bu protokoller HTTP’den farklı olarak düşük yüke sahiptir ve kaynakların daha verimli bir şekilde kullanılması için aynı TCP bağlantısını birden fazla istek/yanıt için kullanabilir. Bu protokoller, geleneksel HTTP bağlantı noktaları 80 ve 443 üzerinden çalışmak üzere tasarlanmıştır.



## <a name="next-steps"></a>Sonraki adımlar

Gereksinimleriniz ve ortamınıza bağlı olarak, Azure portalı, Azure PowerShell veya Azure CLI kullanarak bir test Application Gateway oluşturabilirsiniz:

- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure portalı](quick-create-portal.md).
- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure PowerShell](quick-create-powershell.md)
- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI](quick-create-cli.md)
