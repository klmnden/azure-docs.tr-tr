---
title: Azure Application Gateway'de uçtan uca SSL'yi etkinleştirme
description: Bu makalede, Application Gateway uçtan uca SSL desteği bir genel bakıştır.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 3/19/2019
ms.author: victorh
ms.openlocfilehash: ee901fdcae9717cc6d03d7653bcaacc0c32518e0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66254315"
---
# <a name="overview-of-ssl-termination-and-end-to-end-ssl-with-application-gateway"></a>Uygulama ağ geçidi ile uçtan uca SSL ve SSL sonlandırma genel bakış

Güvenli Yuva Katmanı (SSL), bir web sunucusu ve tarayıcı arasındaki şifreli bir bağlantı kurmak için standart bir güvenlik teknolojisidir. Bu bağlantı, tüm veriler web sunucusu ve tarayıcılar arasında geçirilen sağlar özel ve şifreli olarak kalır. Application gateway ağ geçidi yanı sıra uçtan uca SSL şifrelemesini her iki SSL sonlandırmasını destekler.

## <a name="ssl-termination"></a>SSL sonlandırma

Application Gateway, ağ geçidinde SSL sonlandırmasını destekler. Bu sonlandırmanın ardından, trafik genelde arka uç sunucularına şifrelenmemiş olarak akar. Application Gateway SSL sonlandırma yapmanın avantajları vardır:

- **Geliştirilmiş performans** – SSL şifre çözme yaparken isabet büyük performans ilk el sıkışma olduğu. Performansı artırmak için şifre çözme yapılması sunucu SSL oturum kimliklerini önbelleğe alır ve TLS oturum biletlerini yönetir. Bu uygulama ağ geçidinde yapıldıysa, aynı istemciden gelen tüm isteklerin önbelleğe alınan değerleri kullanabilirsiniz. Arka uç sunucularda gerçekleştirilir, istemci istekleri, istemci farklı bir sunucuya gidin her zaman için re‑authenticate bulunur. TLS biletlerinin kullanılması bu sorunu gidermek yardımcı olabilir, ancak tüm istemciler tarafından desteklenmez ve yapılandırmak ve yönetmek zor olabilir.
- **Daha iyi kullanımı arka uç sunucularının** – SSL/TLS işleme çok CPU bakımından yoğun olan ve anahtar boyutu arttıkça daha yoğun gelmektedir. Bu işi arka uç sunuculardan kaldırma sağlayan bunları en verimli nedir üzerinde odaklanmak, içerik teslim etme.
- **Akıllı yönlendirme** – uygulama ağ geçidi tarafından trafiği şifresini çözme, istek içeriği, üst bilgiler, URI ve benzeri gibi erişebilir ve istekleri bu verileri kullanabilirsiniz.
- **Sertifika yönetimi** – sertifikaları yalnızca gereksinim satın alınan ve uygulama ağ geçidi ve tüm arka uç sunucularda yüklü olması. Bu, hem zaman ve para kazandırır.

SSL sonlandırma yapılandırmak için bir simetrik anahtar SSL protokolü belirtimi uyarınca türetmek uygulama ağ geçidi'ni etkinleştirmek için dinleyici eklenecek bir SSL sertifikası gereklidir. Simetrik anahtar, ardından şifrelemek ve şifresini çözmek için ağ geçidi gönderilen trafiği için kullanılır. SSL sertifikası kişisel bilgi değişimi (PFX) biçiminde olması gerekir. Bu dosya biçimi, uygulama ağ geçidi tarafından şifreleme ve şifre çözme trafik gerçekleştirmek için gerekli olan özel anahtarı dışarı olanak tanır.

> [!NOTE] 
>
> Uygulama ağ geçidi, yeni bir sertifika oluşturun veya bir sertifika yetkilisi sertifika isteğini göndermek için herhangi bir özelliği sağlamaz.

SSL bağlantısı çalışmak SSL sertifikasının aşağıdaki koşulları karşıladığından emin olmak gerekir:

- Geçerli tarih ve saat içinde "Geçerlilik" ve "Geçerlilik sonu" tarih aralığı sertifikadaki olmasıdır.
- Sertifikanın "Ortak Ad" (CN) değeri, istekteki ana bilgisayar üst bilgisiyle eşleşiyor. Örneğin istemci isteğinin hedefi `https://www.contoso.com/` ise CN `www.contoso.com` olmalıdır.

### <a name="certificates-supported-for-ssl-termination"></a>SSL sonlandırma için desteklenen sertifika

Uygulama ağ geçidi, aşağıdaki sertifika türlerini destekler:

- (Sertifika yetkilisi) CA sertifikası: Bir CA sertifikası olan bir sertifika yetkilisi (CA) tarafından verilen bir dijital sertifika
- EV (Genişletilmiş Doğrulama) sertifikası: Bir sektör standart sertifika yönergeleri bir EV sertifikasıdır. Bu tarayıcı Bulucu çubuk yeşile ve şirket adı yayımlayın.
- Joker karakter sertifikası: Bu sertifika destekleyen herhangi bir sayıda alt etki alanlarını temel *. Burada, alt etki alanı yerine site.com, *. Bunu ancak site.com, desteklemiyor böylece başında "www" yazmanıza gerek kalmadan sitenize erişen kullanıcıların durumda joker sertifikası, kapsamaz.
- Otomatik olarak imzalanan sertifikalar: İstemci tarayıcıları bu sertifikalara güvenmesi değil ve sanal hizmetin sertifika güven zinciri parçası değil kullanıcıyı uyarır. Otomatik olarak imzalanan sertifikalar, test veya burada Yöneticiler istemcilerin denetim ve güvenli bir şekilde tarayıcının güvenlik uyarılarını devre dışı bırakabilir ortamlar için uygundur. Üretim iş yükleri, hiçbir zaman otomatik olarak imzalanan sertifikalar kullanmanız gerekir.

Daha fazla bilgi için [SSL sonlandırma application gateway ile yapılandırma](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="size-of-the-certificate"></a>Sertifikanın boyutu
Denetleme [Application Gateway sınırlar](https://docs.microsoft.com/azure/azure-subscription-service-limits#application-gateway-limits) bölüm en fazla SSL bilmek sertifika desteklenen boyutu.

## <a name="end-to-end-ssl-encryption"></a>SSL şifrelemesini uçtan uca

Bazı müşteriler arka uç sunucularıyla şifrelenmemiş iletişim bağlamasına değil. Bunun nedeni, güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Application Gateway, böyle uygulamalar için uçtan uca SSL şifrelemesini desteklemektedir.

Uçtan uca SSL, gizli verileri şifrelenmiş olarak arka uca aktarmanızı sağlar ve Application Gateway’in sunduğu Katman 7 yük dengeleme özelliklerinin avantajlarından yararlanmaya devam eder. Bu özelliklerin bazıları tanımlama bilgisi temelli oturum benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleyebilmedir.

Application Gateway uçtan uca SSL iletişimi modu ile yapılandırıldığında, ağ geçidindeki SSL oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Ardından Application Gateway, arka uç sunucusuyla yeni bir SSL bağlantısı başlatır ve isteği arka uca aktarmadan önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler. Uçtan uca SSL etkin protokolünü ayarlayarak [arka uç HTTP ayarı](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) HTTPS için ardından uygulandığı bir arka uç havuzuna.

SSL ilkesi hem ön uç ve arka uç trafiği için geçerlidir. Ön uç, uygulama ağ geçidi sunucusu gibi davranan ve ilke zorunlu kılar. Arka uç, uygulama ağ geçidi istemci olarak davranır ve protokolü/şifreleme bilgileri SSL el sıkışması sırasında tercih gönderir.

Uygulama ağ geçidi, her iki beyaz listeye sahip bu arka uç örnekleriyle yalnızca sertifikalarını uygulama ağ geçidi veya sertifikalarını sertifika genel adı HTTP ana bilgisayar adı eşleştiği bilinen CA yetkilisi tarafından imzalanmış ile iletişim kurar. arka uç ayarları. Bunlar, Azure App service web apps ve Azure API Management gibi güvenilir Azure hizmetlerini içerir.

Arka uç havuzundaki üyelerin sertifikaları tanınmış bir CA yetkilileri tarafından imzalanmamışsa, uçtan uca SSL etkin ile arka uç havuzundaki her örneği bir sertifikayla güvenli iletişime izin verecek şekilde yapılandırılması gerekir. Sertifika ekleme, uygulama ağ geçidi yalnızca bilinen arka uç örnekleriyle iletişim sağlar. Bu daha fazla uçtan uca iletişimin güvenliğini sağlar.

> [!NOTE] 
>
> Kimlik doğrulama sertifikası Kurulumu güvenilir, Azure App service web apps ve Azure API Management gibi Azure Hizmetleri için gerekli değildir.

> [!NOTE] 
>
> Eklenen sertifika **arka uç HTTP ayarı** arka uç kimlik doğrulaması için sunucuları aynı eklenen sertifika olabilir **dinleyici** için uygulama ağ geçidinde ya da farklı için SSL sonlandırma Gelişmiş Güvenlik.

![uçtan uca SSL senaryosu][1]

Bu örnekte, TLS1.2 kullanan istekler, uçtan uca SSL kullanılarak Pool1'deki sunuculara yönlendirilir.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Uçtan uca SSL ve sertifikaların güvenilir listeye alınması

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için, arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine (kök sertifika değil) yüklemeniz gerekir. Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geriye kalan arka uçlar, ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bunun gibi sertifikaların kullanılabilmesi için önce önceki adımlarda açıklandığı şekilde uygulama ağ geçidiyle Güvenilenler listesinde olmalıdır.

> [!NOTE]
> Kimlik doğrulama sertifikası Kurulumu güvenilir, Azure App Service gibi Azure Hizmetleri için gerekli değildir.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>SSL v2 SKU ile uçtan uca

Kimlik doğrulama sertifikaları kullanım dışı ve uygulama ağ geçidi v2 SKU güvenilen kök sertifikalarda değiştirilmiştir. Bunlar kimlik doğrulama sertifikalarını bazı önemli farklar ile benzer şekilde çalışır:

- Ana bilgisayar adıyla arka uç HTTP ayarları, CN bilinen CA yetkilisi tarafından imzalanmış sertifikalar çalışmak uçtan uca SSL için herhangi bir ek adımı gerektirmez. 

   Örneğin, arka uç sertifikaları tanınmış bir CA tarafından verilen ve bir CN contoso.com olan ve arka uç http ayarı'nın ana bilgisayar adı alanı, contoso.com etki alanına de ayarlanır, sonra ek adımlar gereklidir. Protokol ayarı HTTPS için arka uç http ayarlayabilirsiniz ve sistem durumu araştırması ve veri yolu, SSL etkin olacaktır. Azure App Service veya diğer Azure web Hizmetleri, arka uç olarak kullanıyorsanız, bunlar da örtük olarak güvenilir ve uçtan uca SSL için başka bir adım gereklidir.
- Sertifika otomatik olarak imzalanan veya bilinmeyen Aracılar tarafından imzalanmış, ardından v2 SKU güvenilen kök sertifika uçtan uca SSL etkinleştirmek için tanımlanmalıdır. Uygulama ağ geçidi, yalnızca arka uçları, sunucu sertifikasının kök sertifikasını havuzuyla ilişkili arka uç http ayarı güvenilen kök sertifikaların listesi biriyle eşleşen ile iletişim kurar.
- Arka uç sunucusunun SSL sertifikası tarafından sunulan ortak ad (CN) ana bilgisayar arka uç http ayarlarında belirtilen ayarını eşleşiyorsa, kök sertifika eşleşme ek olarak, Application Gateway de doğrular. Application Gateway sunucu adı belirtme (SNI) uzantısına arka uç bir SSL bağlantısı kurmaya çalışırken arka uç http ayarlarında belirtilen konağa ayarlar.
- Varsa **arka uç adresi ana bilgisayar adını çekme** arka uç http ayarı bilgisindeki Host alanı yerine FQDN ve CN SNI başlığı her zaman arka uç havuzuna ayarlandıysa seçilen arka uç sunucusunda SSL sertifikası, FQDN ile eşleşmesi gerekir. Bu senaryoda, arka uç havuzu üyelerine IP'leri ile desteklenmez.
- Arka uç sunucusu sertifikalarının bir Base64 ile kodlanmış bir kök sertifika kök sertifikadır.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca SSL hakkında daha fazla edindikten sonra Git [ve PowerShell kullanarak Application Gateway uçtan uca SSL'yi yapılandırma](application-gateway-end-to-end-ssl-powershell.md) uçtan uca SSL kullanan bir uygulama ağ geçidi oluşturmak için.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
