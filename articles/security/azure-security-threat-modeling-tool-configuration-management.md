---
title: Yapılandırma Yönetimi - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Azaltıcı Etkenler tehdit modelleme Aracı kullanıma sunulan tehditleri
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 1f3de9ba6615a9b2232cca237a822b308d89426d
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28019832"
---
# <a name="security-frame-configuration-management--mitigations"></a>Güvenlik çerçevesi: Yapılandırma yönetimi | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Web uygulaması** | <ul><li>[İçerik güvenlik ilkesi (CSP) uygulamak ve satır içi javascript devre dışı bırak](#csp-js)</li><li>[Tarayıcının XSS filtresi etkinleştir](#xss-filter)</li><li>[ASP.NET uygulamaları izleme ve dağıtımdan önce hata ayıklama devre dışı bırakmanız gerekir](#trace-deploy)</li><li>[Yalnızca güvenilir kaynaklardan gelen erişim üçüncü taraf JavaScript'ler](#js-trusted)</li><li>[Kimliği doğrulanmış ASP.NET sayfaları UI Redressing veya savunma tıklatın jacking dahil emin olun](#ui-defenses)</li><li>[ASP.NET Web uygulamalarını CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun](#cors-aspnet)</li><li>[ASP.NET sayfaları ValidateRequest özniteliği etkinleştir](#validate-aspnet)</li><li>[JavaScript kitaplıklarını en son sürümlerini yerel olarak barındırılan kullanın](#local-js)</li><li>[Otomatik MIME algılaması devre dışı bırak](#mime-sniff)</li><li>[Standart sunucu başlıkları Windows Azure Web sağlayan önlemek için sitelerindeki Kaldır](#standard-finger)</li></ul> |
| **Veritabanı** | <ul><li>[Veritabanı altyapısı erişimi için bir Windows Güvenlik Duvarı'nı yapılandırma](#firewall-db)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun](#cors-api)</li><li>[Hassas verileri içeren Web API'nin yapılandırma dosyalarını bölümlerini şifrele](#config-sensitive)</li></ul> |
| **IOT cihaz** | <ul><li>[Tüm yönetim arabirimleri güçlü kimlik bilgileriyle güvenli olduğundan emin olmak](#admin-strong)</li><li>[Bilinmeyen kod cihazlarda yürütülemiyor emin olun](#unknown-exe)</li><li>[İşletim sistemi ve ek IOT cihaz bölümlerle bit kilidi şifrele](#partition-iot)</li><li>[Yalnızca en az Hizmetleri/özellikleri aygıtlarda etkin olduğundan emin olun](#min-enable)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[İşletim sistemi ve ek IOT alan ağ geçidi bölümlerle bit kilidi şifrele](#field-bit-locker)</li><li>[Alan ağ geçidi varsayılan oturum açma kimlik bilgilerini yükleme sırasında değiştirilir emin olun](#default-change)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Bulut ağ geçidi bağlı aygıtlar üretici yazılımı güncel tutmak için bir işlem gerçekleştirdiğinden emin olun](#cloud-firmware)</li></ul> |
| **Makine güven sınırı** | <ul><li>[Aygıtları son nokta güvenlik denetimleri kuruluş ilkelerini uygun şekilde yapılandırılmış olduğundan emin olun](#controls-policies)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure depolama erişim anahtarlarını güvenli yönetim emin olun](#secure-keys)</li><li>[CORS'yi Azure depolama alanında etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun](#cors-storage)</li></ul> |
| **WCF** | <ul><li>[WCF'ın hizmeti özellik azaltma etkinleştir](#throttling)</li><li>[Meta veri aracılığıyla WCF bilginin açığa çıkması](#info-metadata)</li></ul> | 

## <a id="csp-js"></a>İçerik güvenlik ilkesi (CSP) uygulamak ve satır içi javascript devre dışı bırak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [İçerik güvenlik ilkesi giriş](http://www.html5rocks.com/en/tutorials/security/content-security-policy/), [içerik güvenlik ilkesi başvurusu](http://content-security-policy.com/), [güvenlik özellikleri](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/security/), [içerik Güvenlik İlkesiGiriş](https://docs.webplatform.org/wiki/tutorials/content-security-policy), [CSP kullanabilir miyim?](http://caniuse.com/#feat=contentsecuritypolicy) |
| **Adımları** | <p>İçerik güvenlik ilkesi (CSP), web uygulama sahipleri sitelerinde katıştırılmış içerik üzerinde denetime sahip olmasını sağlayan bir savunma güvenlik, standart, bir W3C mekanizmadır. CSP web sunucusundaki bir HTTP yanıt üst bilgisi olarak eklenir ve istemci tarafında tarayıcılar tarafından zorlanır. Beyaz liste tabanlı bir ilke olduğunu - JavaScript yüklenebilir gibi bir Web sitesi hangi etkin içerik güvenilir etki alanlarından kümesi bildirebilirsiniz.</p><p>CSP aşağıdaki güvenlik avantajları sağlar:</p><ul><li>**XSS karşı koruma:** bir saldırgan, bir sayfa için XSS saldırılara açık olduğunda 2 yollarla yararlanabilir:<ul><li>Eklenmeye `<script>malicious code</script>`. Bu yararlanma CSP'ın temel kısıtlama-1 nedeniyle çalışmaz</li><li>Eklenmeye `<script src=”http://attacker.com/maliciousCode.js”/>`. Denetlenen saldırgan etki alanlarının CSP'ın beyaz olmayacaktır bu yana bu yararlanma çalışmaz</li></ul></li><li>**Veri exfiltration üzerinde denetim:** bir Web sayfasındaki kötü amaçlı içerik bir dış Web sitesine bağlanmak ve verileri çalmaya çalışırsa, CSP tarafından bağlantı iptal edilecek. Hedef etki alanına CSP'ın beyaz olmayacaktır Bunun nedeni</li><li>**Tıklatın jacking karşı savunma:** tıklatın jacking olduğundan, bir saldırganın, kullanıcı Arabirimi öğeleri tıklattığınızdan orijinal bir Web sitesi ve zorla kullanıcılar çerçeve kullanarak bir saldırı teknik. Şu anda tıklatın jacking korunmanın bir yanıt üstbilgisi-X-Frame-Options yapılandırılarak elde edilir. Tüm tarayıcılar bu başlığı dikkate alır ve iletme CSP giderek tıklatın jacking karşı korumak için standart bir biçimde olacaktır</li><li>**Gerçek zamanlı saldırı raporlama:** tarayıcılar ekleme saldırının CSP özellikli bir Web sitesi varsa, otomatik olarak Web sunucusu üzerinde yapılandırılmış bir uç nokta için bir bildirim tetikler. Bu şekilde, CSP gerçek zamanlı bir uyarı sistem olarak görev yapar.</li></ul> |

### <a name="example"></a>Örnek
Örnek İlkesi: 
```csharp
Content-Security-Policy: default-src 'self'; script-src 'self' www.google-analytics.com 
```
Bu ilke yalnızca web uygulamasının server ve google analytics Server'dan yüklemek komut dosyaları sağlar. Başka bir siteden yüklenen komut dosyalarını reddedilir. CSP bir Web sitesinde etkin olduğunda, aşağıdaki özellikleri otomatik olarak XSS saldırıları azaltmak için devre dışı bırakılır. 

### <a name="example"></a>Örnek
Satır içi komut dosyaları çalıştırmaz. Satır içi komut dosyaları örnekleri aşağıda verilmiştir 
```javascript
<script> some Javascript code </script>
Event handling attributes of HTML tags (e.g., <button onclick=”function(){}”>
javascript:alert(1);
```

### <a name="example"></a>Örnek
Dizeleri kodu olarak değerlendirilmez. 
```javascript
Example: var str="alert(1)"; eval(str);
```

## <a id="xss-filter"></a>Tarayıcının XSS filtresi etkinleştir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [XSS koruma filtresi](https://www.owasp.org/index.php/List_of_useful_HTTP_headers#X-XSS-Protection) |
| **Adımları** | <p>X XSS koruma yanıt üstbilgisi yapılandırma tarayıcının siteler arası komut dosyası filtresi denetler. Bu yanıt üstbilgisi değerleri aşağıdaki sahip olabilir:</p><ul><li>`0:`Bu filtreyi devre dışı bırakır</li><li>`1: Filter enabled`Siteler arası komut dosyası saldırı algılanırsa, saldırı durdurmak için tarayıcı sayfa temizlenmeye</li><li>`1: mode=block : Filter enabled`. Bunun yerine XSS saldırısı algılandığında sayfa temizlenmeye küçük, tarayıcı sayfanın işlenmesi engeller.</li><li>`1: report=http://[YOURDOMAIN]/your_report_URI : Filter enabled`. Tarayıcı sayfa temizlenmeye ve ihlalin rapor.</li></ul><p>Bu tercih ettiğiniz bir URI ayrıntıları göndermek için CSP ihlali raporları kullanılarak Chromium işlevdir. Son 2 seçenekleri güvenli değerleri olarak kabul edilir.</p>|

## <a id="trace-deploy"></a>ASP.NET uygulamaları izleme ve dağıtımdan önce hata ayıklama devre dışı bırakmanız gerekir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Genel Bakış hata ayıklama ASP.NET](http://msdn2.microsoft.com/library/ms227556.aspx), [ASP.NET izlemeye genel bakış](http://msdn2.microsoft.com/library/bb386420.aspx), [nasıl yapılır: bir ASP.NET uygulaması için izlemeyi etkinleştirmek](http://msdn2.microsoft.com/library/0x5wc973.aspx), [nasıl yapılır: ASP.NET uygulamaları için hata ayıklamayı etkinleştir](http://msdn2.microsoft.com/library/e8z01xdh(VS.80).aspx) |
| **Adımları** | İç sunucu durumu ve iş akışı hakkında veri içerir izleme bilgilerini de alır isteyen her tarayıcı sayfa için izleme etkin olmadığında. Bu bilgileri güvenlik duyarlı olabilir. Hata ayıklama sayfa için etkinleştirildiğinde, sunucuda gerçekleştiği hataları tarayıcıya sunulan tam yığın izleme verileri sonuçlanır. Bu verileri sunucunun iş akışıyla ilgili güvenlik bakımından hassas bilgiler getirebilir. |

## <a id="js-trusted"></a>Yalnızca güvenilir kaynaklardan gelen erişim üçüncü taraf JavaScript'ler

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | üçüncü taraf JavaScript'ler yalnızca güvenilir kaynaklardan gelen başvurulan. Başvuru uç noktalar, her zaman SSL olmalıdır. |

## <a id="ui-defenses"></a>Kimliği doğrulanmış ASP.NET sayfaları UI Redressing veya savunma tıklatın jacking dahil emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Savunma kopya sayfası tıklatın jacking OWASP](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet), [tıklatın jacking ile X-Frame-Options mücadele IE iç-Ayrıntılar](https://blogs.msdn.microsoft.com/ieinternals/2010/03/30/combating-click-jacking-with-x-frame-options/) |
| **Adımları** | <p>tıklatın jacking olarak da bilinen bir "UI redress", saldırısıdır bir saldırganın bir kullanıcı bir düğmesini tıklatarak içine kandırarak veya üst düzey sayfasında tıklatın amaçlama zaman başka bir sayfada bağlamak için birden çok saydam veya donuk Katmanlar kullandığında.</p><p>Katmanlama, kötü amaçlı bir sayfa kurbanın sayfasını yükler IFRAME ile hazırlayın tarafından sağlanır. Bu nedenle, saldırgan "için kendi sayfası anlamına gelir ve onları başka bir sayfaya, büyük olasılıkla başka bir uygulama, etki alanı veya her ikisi tarafından sahip olunan yönlendirme tıklama ele". Tıklatın jacking saldırılarını önlemek için diğer etki alanlarından çerçeveleme izin vermeyecek şekilde tarayıcısına uygun X-Frame-Options HTTP yanıt üstbilgilerini Ayarla</p>|

### <a name="example"></a>Örnek
X-FRAME-OPTIONS üstbilgisi IIS web.config ayarlanabilir. Web.config kod parçacığını hiçbir zaman Çerçeveli siteler için: 
```csharp
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="DENY"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

### <a name="example"></a>Örnek
Web.config kod sayfaları aynı etki alanında yalnızca Çerçeveli siteler için: 
```csharp
    <system.webServer>
        <httpProtocol>
            <customHeader>
                <add name="X-FRAME-OPTIONS" value="SAMEORIGIN"/>
            </customHeaders>
        </httpProtocol>
    </system.webServer>
```

## <a id="cors-aspnet"></a>ASP.NET Web uygulamalarını CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web Forms, MVC5 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Tarayıcı güvenlik bir web sayfası AJAX istekleri başka bir etki alanına yapmasını engeller. Bu kısıtlama aynı kaynak ilkesi adı verilir ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazen, diğer siteleri tüketebileceği API'leri güvenli bir şekilde kullanıma sunmak için gerekli olabilir. Çapraz kaynak kaynak paylaşımı (CORS), aynı kaynak İlkesi hafifletin sunucusunun sağlar W3C standardıdır. CORS kullanarak, bir sunucu açıkça bazı cross-origin istekleri başkalarının reddetme çalışırken izin verebilirsiniz.</p><p>CORS daha güvenli ve JSONP gibi önceki teknikler daha esnektir. Özünde, birkaç HTTP yanıt üstbilgilerini eklemek için CORS etkinleştirme çevirir (Access - Control-*) için web uygulaması ve bu yolla birkaç içinde yapılabilir.</p>|

### <a name="example"></a>Örnek
Web.config erişimi varsa, CORS aşağıdaki kodu eklenebilir: 
```XML
<system.webServer>
    <httpProtocol>
      <customHeaders>
        <clear />
        <add name="Access-Control-Allow-Origin" value="http://example.com" />
      </customHeaders>
    </httpProtocol>
```

### <a name="example"></a>Örnek
Web.config erişimi yoksa, CORS aşağıdaki CSharp kodu ekleyerek yapılandırılabilir: 
```csharp
HttpContext.Response.AppendHeader("Access-Control-Allow-Origin", "http://example.com")
```

"Access-Control-Allow-Origin" özniteliğinde kaynakları listesini çıkış sonlu ve güvenilir kümesini ayarlandığından emin olmak için önemli olduğunu unutmayın. Bu açamayacağı yapılandırmak başarısız (örn., ayar değeri olarak ' *') web uygulaması için çapraz origin istekleri tetiklemek kötü amaçlı siteleri sağlayacak > herhangi bir kısıtlama olmadan, böylece uygulamayı CSRF saldırılara karşı savunmasız hale getirme. 

## <a id="validate-aspnet"></a>ASP.NET sayfaları ValidateRequest özniteliği etkinleştir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web Forms, MVC5 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [İstek doğrulama - komut dosyası saldırılarını önleme](http://www.asp.net/whitepapers/request-validation) |
| **Adımları** | <p>İstek doğrulama, bir ASP.NET özelliği sürüm 1.1 beri sunucunun içerik içeren beklemediğiniz kodlanmış HTML kabul etmesini engeller. Bu özellik, yapabildiği istemci komut dosyası kodu veya HTML bilmeden bir sunucuya gönderilen, depolanabilir ve diğer kullanıcılara sunulan bazı komut dosyası ekleme saldırıları önlemeye yardımcı olmak için tasarlanmıştır. Hala tüm giriş verilerini doğrulamak ve HTML kodlama, uygun olduğunda öneririz.</p><p>İstek doğrulama, potansiyel olarak tehlikeli olabilecek bir değer listesi tüm giriş verilerini karşılaştırarak gerçekleştirilir. Bir eşleşme olursa, ASP.NET başlatır bir `HttpRequestValidationException`. Varsayılan olarak, istek doğrulama özelliği etkinleştirilir.</p>|

### <a name="example"></a>Örnek
Ancak, bu özellik sayfa düzeyinde devre dışı bırakılabilir: 
```XML
<%@ Page validateRequest="false" %> 
```
veya uygulama düzeyinde 
```XML
<configuration>
   <system.web>
      <pages validateRequest="false" />
   </system.web>
</configuration>
```
İstek doğrulama özellik desteklenmiyor ve MVC6 ardışık düzen parçası olmayan unutmayın. 

## <a id="local-js"></a>JavaScript kitaplıklarını en son sürümlerini yerel olarak barındırılan kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>JQuery kullanmalısınız gibi standart JavaScript kitaplıklarını kullanarak geliştiriciler bilinen güvenlik açıkları içermeyen ortak JavaScript kitaplıklarını sürümleri onaylanmış. Eski sürümlerine bilinen güvenlik açıkları için güvenlik düzeltmelerini içeren bu yana kitaplıkları en son sürümünü kullanmak iyi bir uygulamadır.</p><p>En son sürüm uyumluluğu nedenlerden ötürü kullandıysanız en düşük sürümlerle kullanılmalıdır.</p><p>Kabul edilebilir en düşük sürümler:</p><ul><li>**JQuery**<ul><li>JQuery 1.7.1</li><li>JQueryUI 1.10.0</li><li>JQuery 1.9 doğrula</li><li>JQuery Mobile 1.0.1</li><li>JQuery döngüsü 2.99</li><li>JQuery DataTables 1.9.0</li></ul></li><li>**Ajax Control Toolkit**<ul><li>Ajax Control Toolkit 40412</li></ul></li><li>**ASP.NET Web formları ve Ajax**<ul><li>ASP.NET Web formları ve Ajax 4</li><li>ASP.NET Ajax 3.5</li></ul></li><li>**ASP.NET MVC**<ul><li>ASP.NET MVC 3.0</li></ul></li></ul><p>Hiçbir zaman genel CDN'ler gibi dış sitelerden herhangi bir JavaScript kitaplığı yüklenemiyor</p>|

## <a id="mime-sniff"></a>Otomatik MIME algılaması devre dışı bırak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [IE8 güvenlik bölümü V: kapsamlı koruma](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx), [MIME türü](http://en.wikipedia.org/wiki/Mime_type) |
| **Adımları** | Geliştiriciler kendi içerik MIME sniffed olmamalıdır belirtmek izin veren bir HTTP üstbilgisi içerik türü seçenekleri X başlığıdır. Bu üst MIME algılaması saldırıları azaltmak için tasarlanmıştır. Kullanıcı denetlenebilir içeriği içerebilir her bir sayfa için HTTP üst bilgisi X kullanmalısınız-içerik-tür-seçenekleri: nosniff. Uygulamadaki tüm sayfalar için genel gerekli üstbilgisi etkinleştirmek için aşağıdakilerden birini yapabilirsiniz|

### <a name="example"></a>Örnek
Uygulama Internet Information Services (IIS tarafından) 7 veya sonraki sürümleri barındırılıyorsa üstbilgi web.config dosyasına ekleyin. 
```XML
<system.webServer>
<httpProtocol>
<customHeaders>
<add name="X-Content-Type-Options" value="nosniff"/>
</customHeaders>
</httpProtocol>
</system.webServer>
```

### <a name="example"></a>Örnek
Genel Uygulama aracılığıyla üstbilgisi eklemek\_BeginRequest 
```csharp
void Application_BeginRequest(object sender, EventArgs e)
{
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
}
```

### <a name="example"></a>Örnek
Uygulama özel HTTP modülü 
```csharp
public class XContentTypeOptionsModule : IHttpModule
{
#region IHttpModule Members
public void Dispose()
{
}
public void Init(HttpApplication context)
{
context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders);
}
#endregion
void context_PreSendRequestHeaders(object sender, EventArgs e)
{
HttpApplication application = sender as HttpApplication;
if (application == null)
  return;
if (application.Response.Headers["X-Content-Type-Options "] != null)
  return;
application.Response.Headers.Add("X-Content-Type-Options ", "nosniff");
}
}
```

### <a name="example"></a>Örnek
Yalnızca belirli sayfaları için gerekli üstbilgisi tek tek yanıtlarını ekleyerek etkinleştirebilirsiniz: 

```csharp
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
```

## <a id="standard-finger"></a>Standart sunucu başlıkları Windows Azure Web sağlayan önlemek için sitelerindeki Kaldır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | EnvironmentType - Azure |
| **Başvuruları**              | [Standart sunucu üstbilgiler Windows Azure Web sitelerindeki kaldırılıyor](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/) |
| **Adımları** | Sunucu, X-gücü-tarafından X AspNet sürüm üstbilgileri sunucusu ve temel teknolojileri hakkında bilgi ortaya. Böylece bu üstbilgileri gizlemek için önerilen uygulama sağlayan önleme |

## <a id="firewall-db"></a>Veritabanı altyapısı erişimi için bir Windows Güvenlik Duvarı'nı yapılandırma

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure, OnPrem |
| **Öznitelikleri**              | Yok, SQL sürümü - V12 |
| **Başvuruları**              | [Bir Azure SQL veritabanı Güvenlik Duvarı'nı yapılandırmak nasıl](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/), [veritabanı altyapısı erişimi için bir Windows Güvenlik Duvarı'nı yapılandırma](https://msdn.microsoft.com/library/ms175043) |
| **Adımları** | Güvenlik Duvarı sistemlerini bilgisayar kaynakları için yetkisiz erişimi önlemeye yardımcı olur. SQL Server veritabanı altyapısı örneği bir güvenlik duvarı üzerinden erişmek için erişime izin vermek için SQL Server çalıştıran bilgisayarda güvenlik duvarını yapılandırmalısınız. |

## <a id="cors-api"></a>ASP.NET Web API CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC 5 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [ASP.NET Web API 2 çıkış noktaları arası istekleri etkinleştirme](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api), [ASP.NET Web API - ASP.NET Web API 2 CORS desteği](https://msdn.microsoft.com/magazine/dn532203.aspx) |
| **Adımları** | <p>Tarayıcı güvenlik bir web sayfası AJAX istekleri başka bir etki alanına yapmasını engeller. Bu kısıtlama aynı kaynak ilkesi adı verilir ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazen, diğer siteleri tüketebileceği API'leri güvenli bir şekilde kullanıma sunmak için gerekli olabilir. Çapraz kaynak kaynak paylaşımı (CORS), aynı kaynak İlkesi hafifletin sunucusunun sağlar W3C standardıdır.</p><p>CORS kullanarak, bir sunucu açıkça bazı cross-origin istekleri başkalarının reddetme çalışırken izin verebilirsiniz. CORS daha güvenli ve JSONP gibi önceki teknikler daha esnektir.</p>|

### <a name="example"></a>Örnek
App_Start/WebApiConfig.cs içinde WebApiConfig.Register yöntemine aşağıdaki kodu ekleyin 
```csharp
using System.Web.Http;
namespace WebService
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // New code
            config.EnableCors();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
        }
    }
}
```

### <a name="example"></a>Örnek
EnableCors özniteliği bir denetleyici eylem yöntemlerinde aşağıdaki gibi uygulanabilir: 

```csharp
public class ResourcesController : ApiController
{
  [EnableCors("http://localhost:55912", // Origin
              null,                     // Request headers
              "GET",                    // HTTP methods
              "bar",                    // Response headers
              SupportsCredentials=true  // Allow credentials
  )]
  public HttpResponseMessage Get(int id)
  {
    var resp = Request.CreateResponse(HttpStatusCode.NoContent);
    resp.Headers.Add("bar", "a bar value");
    return resp;
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "PUT",                          // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  [EnableCors("http://localhost:55912",       // Origin
              "Accept, Origin, Content-Type", // Request headers
              "POST",                         // HTTP methods
              PreflightMaxAge=600             // Preflight cache duration
  )]
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
}
```

Çıkış EnableCors öznitelik listesi çıkış sonlu ve güvenilir kümesini ayarlandığından emin olmak için önemli olduğunu unutmayın. Bu açamayacağı yapılandırmak başarısız (örn., ayar değeri olarak ' *') herhangi bir kısıtlamanın olmadığı API çapraz origin istekleri tetiklemek kötü amaçlı sitelere izin verir > API böylece CSRF saldırılara karşı savunmasız hale getirme. Denetleyici düzeyinde EnableCors tasarlanabilir. 

### <a name="example"></a>Örnek
CORS bir sınıftaki belirli bir yöntemi devre dışı bırakmanız için DisableCors özniteliği aşağıda gösterildiği gibi kullanılabilir: 
```csharp
[EnableCors("http://example.com", "Accept, Origin, Content-Type", "POST")]
public class ResourcesController : ApiController
{
  public HttpResponseMessage Put(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  public HttpResponseMessage Post(Resource data)
  {
    return Request.CreateResponse(HttpStatusCode.OK, data);
  }
  // CORS not allowed because of the [DisableCors] attribute
  [DisableCors]
  public HttpResponseMessage Delete(int id)
  {
    return Request.CreateResponse(HttpStatusCode.NoContent);
  }
}
```

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC 6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [ASP.NET Core 1.0 (CORS) çıkış noktaları arası istekleri etkinleştirme](https://docs.asp.net/en/latest/security/cors.html) |
| **Adımları** | <p>ASP.NET Core 1.0 CORS ara yazılımı kullanarak veya MVC kullanarak etkinleştirilebilir. MVC CORS'yi etkinleştirmeniz kullanırken aynı CORS Hizmetleri kullanılır, ancak CORS Ara değil.</p>|

**Yaklaşım 1** etkinleştirme CORS ara yazılımı ile: tüm uygulama için CORS etkinleştirmek için UseCors genişletme yöntemi kullanarak istek ardışık düzenine CORS Ara ekleyin. Bir çıkış noktaları arası ilkesi CorsPolicyBuilder sınıfını kullanarak CORS Ara eklerken belirtilebilir. Bunu yapmanın iki yolu vardır:

### <a name="example"></a>Örnek
İlk UseCors ile bir lambda çağırmaktır. Lambda CorsPolicyBuilder nesnesini alır: 
```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseCors(builder =>
        builder.WithOrigins("http://example.com")
        .WithMethods("GET", "POST", "HEAD")
        .WithHeaders("accept", "content-type", "origin", "x-custom-header"));
}
```

### <a name="example"></a>Örnek
Bir veya daha fazla adlandırılmış CORS ilkelerini tanımlamak ve ardından ilkeyi çalışma zamanında adına göre seçmek için saniyedir. 
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowSpecificOrigin",
            builder => builder.WithOrigins("http://example.com"));
    });
}
public void Configure(IApplicationBuilder app)
{
    app.UseCors("AllowSpecificOrigin");
    app.Run(async (context) =>
    {
        await context.Response.WriteAsync("Hello World!");
    });
}
```

**Yaklaşım 2** etkinleştirme CORS mvc'de: geliştiriciler alternatif olarak MVC eylem, denetleyici başına veya genel olarak tüm denetleyicileri için başına belirli CORS uygulamak için kullanabilirsiniz.

### <a name="example"></a>Örnek
Eylem başına: belirli bir eylemi için ilke bir CORS belirtmek için eyleme [EnableCors] özniteliğini ekleyin. İlke adı belirtin. 
```csharp
public class HomeController : Controller
{
    [EnableCors("AllowSpecificOrigin")] 
    public IActionResult Index()
    {
        return View();
    }
```

### <a name="example"></a>Örnek
Denetleyici: 
```csharp
[EnableCors("AllowSpecificOrigin")]
public class HomeController : Controller
{
```

### <a name="example"></a>Örnek
Genel olarak: 
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<MvcOptions>(options =>
    {
        options.Filters.Add(new CorsAuthorizationFilterFactory("AllowSpecificOrigin"));
    });
}
```
Çıkış EnableCors öznitelik listesi çıkış sonlu ve güvenilir kümesini ayarlandığından emin olmak için önemli olduğunu unutmayın. Bu açamayacağı yapılandırmak başarısız (örn., ayar değeri olarak ' *') herhangi bir kısıtlamanın olmadığı API çapraz origin istekleri tetiklemek kötü amaçlı sitelere izin verir > API böylece CSRF saldırılara karşı savunmasız hale getirme. 

### <a name="example"></a>Örnek
Denetleyici veya eylem için CORS devre dışı bırakmak için [DisableCors] özniteliğini kullanın. 
```csharp
[DisableCors]
    public IActionResult About()
    {
        return View();
    }
```

## <a id="config-sensitive"></a>Hassas verileri içeren Web API'nin yapılandırma dosyalarını bölümlerini şifrele

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Nasıl yapılır: ASP.NET 2.0 kullanarak DPAPI yapılandırma bölümlerinin şifrelemek](https://msdn.microsoft.com/library/ff647398.aspx), [korumalı bir yapılandırma sağlayıcısı belirtme](https://msdn.microsoft.com/library/68ze1hb2.aspx), [uygulama parolaları korumak için Azure anahtar kasası kullanma](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Adımları** | Yapılandırma dosyaları gibi Web.config appsettings.json genellikle kullanıcı adları, parolalar, veritabanı bağlantı dizelerini ve şifreleme anahtarları gibi hassas bilgiler tutmak için kullanılır. Bu bilgileri korumak, uygulamanızın saldırganların veya kötü amaçlı kullanıcılara hesap kullanıcı adları ve parolalar, veritabanı adları ve sunucu adları gibi hassas bilgileri alma savunmasızdır. (Azure/şirket içi) dağıtım türüne göre yapılandırma dosyaları DPAPI veya Azure anahtar kasası gibi hizmetleri kullanarak önemli bölümlerini şifreler. |

## <a id="admin-strong"></a>Tüm yönetim arabirimleri güçlü kimlik bilgileriyle güvenli olduğundan emin olmak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Cihaz veya alan ağ geçidi sunan herhangi yönetim arabirimleri güçlü kimlik bilgileri kullanılarak güvenli hale getirilmelidir. Ayrıca, WiFi, SSH herhangi diğer bir arabirimleri ister, dosya paylaşımları, FTP güçlü kimlik bilgileri ile güvenli hale getirilmelidir. Varsayılan Zayıf parolalar kullanılmamalıdır. |

## <a id="unknown-exe"></a>Bilinmeyen kod cihazlarda yürütülemiyor emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Güvenli Önyükleme ve Windows 10 IOT Core üzerinde bit kilidi cihaz şifrelemeyi etkinleştirme](https://developer.microsoft.com/windows/iot/win10/sb_bl) |
| **Adımları** | UEFI Güvenli Önyükleme, yalnızca belirtilen yetkilisi tarafından imzalanmış ikili dosyaları yürütülmesi izin vermek üzere sistemi kısıtlar. Bu özellik bilinmeyen kod platformuna yürütülen ve olası güvenlik duruşunu zayıflatmanın engeller. UEFI Güvenli Önyükleme etkinleştirin ve kod imzalama için güvenilen sertifika yetkililerinin listesini kısıtlamak. Cihazda güvenilen yetkililer birini kullanarak dağıtılan tüm kod oturum açın. |

## <a id="partition-iot"></a>İşletim sistemi ve ek IOT cihaz bölümlerle bit kilidi şifrele

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Windows 10 IOT Core hafif bir sürümüdür ve gerekli ölçümleri yürütür UEFI'da gerekli preOS Protokolü dahil olmak üzere bu platform TPM'de varlığına güçlü bir bağımlılığa sahip bit kilidi cihaz şifreleme uygular. Bu preOS ölçümlerinin işletim sistemi, işletim Sisteminin nasıl başlatıldı kesin kaydını daha sonra olduğundan emin olun. Herhangi bir duyarlı veri depolamak durumda bit kasası ve herhangi bir ek bölümü de kullanarak işletim sistemi bölümleri şifreleyin. |

## <a id="min-enable"></a>Yalnızca en az Hizmetleri/özellikleri aygıtlarda etkin olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Etkinleştirmeyin veya herhangi bir özellik veya hizmetleri çözümünü çalışması için gerekli olmayan işletim kapatın. İçin örneğin cihazın dağıtılması için bir kullanıcı Arabirimi gerektirmiyorsa Windows IOT Core gözetimsiz modunda yüklemeniz gerekir. |

## <a id="field-bit-locker"></a>İşletim sistemi ve ek IOT alan ağ geçidi bölümlerle bit kilidi şifrele

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Windows 10 IOT Core hafif bir sürümüdür ve gerekli ölçümleri yürütür UEFI'da gerekli preOS Protokolü dahil olmak üzere bu platform TPM'de varlığına güçlü bir bağımlılığa sahip bit kilidi cihaz şifreleme uygular. Bu preOS ölçümlerinin işletim sistemi, işletim Sisteminin nasıl başlatıldı kesin kaydını daha sonra olduğundan emin olun. Herhangi bir duyarlı veri depolamak durumda bit kasası ve herhangi bir ek bölümü de kullanarak işletim sistemi bölümleri şifreleyin. |

## <a id="default-change"></a>Alan ağ geçidi varsayılan oturum açma kimlik bilgilerini yükleme sırasında değiştirilir emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Alan ağ geçidi varsayılan oturum açma kimlik bilgilerini yükleme sırasında değiştirilir emin olun |

## <a id="cloud-firmware"></a>Bulut ağ geçidi bağlı aygıtlar üretici yazılımı güncel tutmak için bir işlem gerçekleştirdiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ağ geçidi seçim - Azure IOT Hub |
| **Başvuruları**              | [IOT Hub cihaz yönetimine genel bakış](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-overview/), [aygıt bellenimi güncelleştirme](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-device-jobs/) |
| **Adımları** | LWM2M açık mobil Alliance IOT cihaz yönetimi için gelen bir protokoldür. Cihaz işleri kullanarak fiziksel cihazları ile etkileşim kurmak için Azure IOT cihaz yönetimini sağlar. Bulut ağ geçidi düzenli olarak cihaz ve diğer yapılandırma verilerini Azure IOT Hub cihaz yönetimini kullanarak güncel tutmak için bir işlem gerçekleştirdiğinden emin olun. |

## <a id="controls-policies"></a>Aygıtları son nokta güvenlik denetimleri kuruluş ilkelerini uygun şekilde yapılandırılmış olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Aygıtları bit kilidi disk düzeyinde şifreleme, güncelleştirilmiş imzalarla virüsten koruma gibi son nokta güvenlik denetimleri, ana bilgisayar tabanlı güvenlik duvarı, işletim sistemi yükseltme, vb. Kurumsal güvenlik ilkelerini uygun şekilde yapılandırılmış ilkeler Grup dikkat emin olun. |

## <a id="secure-keys"></a>Azure depolama erişim anahtarlarını güvenli yönetim emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Azure depolama Güvenlik Kılavuzu - bilgisayarınızı depolama hesabı anahtarlarını yönetme](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_managing-your-storage-account-keys) |
| **Adımları** | <p>Anahtar depolama: Azure depolama erişim tuşlarını Azure anahtar kasası gizli olarak depolamak ve anahtarı anahtar Kasası'nı alıp uygulamaları sağlamak için önerilir. Bu, aşağıdaki nedenlerden ötürü önerilir:</p><ul><li>Uygulama hiçbir zaman birisi belirli izniniz olmadan anahtarlarına erişim sağlama, o avenue kaldırır bir yapılandırma dosyasında depolama anahtar sabit kodlanmış gerekir</li><li>Azure Active Directory'yi kullanarak anahtarlarına erişimi denetlenebilir. Başka bir deyişle, bir hesap sahibi anahtarları Azure anahtar Kasası'almak için gereken uygulamalar sayıda erişim izni verebilir. Diğer uygulamalar izni özellikle vermeden için bunları erişim anahtarları mümkün olmayacaktır.</li><li>Anahtarını yeniden üretme: Güvenlik nedenleriyle Azure depolama erişim anahtarlarını yeniden yerinde bir işlem olması önerilir. Neden ilgili ayrıntılar ve anahtarını yeniden üretme işlemi için nasıl Azure depolama Güvenlik Kılavuzu başvurusu makalesinde belgelenen</li></ul>|

## <a id="cors-storage"></a>CORS'yi Azure depolama alanında etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Azure Storage Hizmetleri için CORS desteği](https://msdn.microsoft.com/library/azure/dn535601.aspx) |
| **Adımları** | Azure depolama arası kaynak paylaşımı kaynak CORS – etkinleştirmenize olanak sağlar. Her Depolama hesabı için bu depolama hesabındaki kaynaklara erişebilir etki alanları belirtebilirsiniz. Varsayılan olarak, CORS tüm hizmetleri devre dışıdır. CORS hizmeti ilkeleri ayarlamak için yöntemi çağırmak için REST API veya depolama istemci kitaplığı kullanarak etkinleştirebilirsiniz. |

## <a id="throttling"></a>WCF'ın hizmeti özellik azaltma etkinleştir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Adımları** | <p>Bir sınır sistem kaynaklarının kullanımını yerleştirme değil, Kaynak Tükenmesi ve sonuçta hizmet reddine neden olabilir.</p><ul><li>**Açıklama:** Windows Communication Foundation (WCF) hizmet istekleri azaltma olanağı sunar. Çok fazla sayıda istemci isteklerini izin vererek, bir sistem bölgesini doldurmak ve kaynaklarını tüketebilir. Diğer taraftan, yalnızca az sayıda hizmet isteklerine izin verme yetkili kullanıcıları hizmetini kullanarak engelleyebilirsiniz. Her hizmet ayrı ayrı şekilde ayarlanır ve uygun miktarda kaynak izin verecek şekilde yapılandırılmış gerekir.</li><li>**Öneriler** etkinleştirmek WCF'ın Hizmeti azaltma özelliğini ve kümesi sınırları, uygulamanız için uygun.</li></ul>|

### <a name="example"></a>Örnek
Azaltma etkinleştirildi ile örnek bir yapılandırma verilmiştir:
```
<system.serviceModel> 
  <behaviors>
    <serviceBehaviors>
    <behavior name="Throttled">
    <serviceThrottling maxConcurrentCalls="[YOUR SERVICE VALUE]" maxConcurrentSessions="[YOUR SERVICE VALUE]" maxConcurrentInstances="[YOUR SERVICE VALUE]" /> 
  ...
</system.serviceModel> 
```

## <a id="info-metadata"></a>Meta veri aracılığıyla WCF bilginin açığa çıkması

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Adımları** | Meta veri sistemi hakkında bilgi edinin ve saldırı form planı saldırganlar yardımcı olabilir. WCF hizmetleri meta verilerini kullanıma sunmak için yapılandırılabilir. Meta veri ayrıntılı hizmet açıklaması bilgilerini sağlar ve üretim ortamlarında yayını değil. `HttpGetEnabled`  /  `HttpsGetEnabled` ServiceMetaData sınıf özelliklerini tanımlayan bir hizmet meta verilerin açığa çıkarır | 

### <a name="example"></a>Örnek
Aşağıdaki kod bir hizmetin meta verilerini yayınlamak için WCF talimatı verir.
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb); 
```
Hizmet meta verilerini bir üretim ortamında yayın değil. De ayarlamak / sınıf ServiceMetaData de özellikleri false. 

### <a name="example"></a>Örnek
Aşağıdaki kod, bir hizmetin meta veri değil yayınlanacak WCF bildirir. 
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior(); 
smb.HttpGetEnabled = false; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb);
```
