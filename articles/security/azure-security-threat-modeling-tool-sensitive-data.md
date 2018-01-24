---
title: "Hassas verileri - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs"
description: "Azaltıcı Etkenler tehdit modelleme Aracı kullanıma sunulan tehditleri"
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
ms.openlocfilehash: 8d7189ea4b01d43cea709e3300d8ed71d266f5c9
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="security-frame-sensitive-data--mitigations"></a>Güvenlik çerçeve: Hassas verileri | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Makine güven sınırı** | <ul><li>[Hassas bilgiler içeriyorsa, ikili dosyaları gizlenmiş emin olun](#binaries-info)</li><li>[Şifrelenmiş dosya sistemi (EFS) kullanarak gizli kullanıcıya özgü verileri korumak için kullanılan göz önünde bulundurun](#efs-user)</li><li>[Dosya sistemindeki uygulama tarafından depolanan hassas verilerin şifrelendiğinden emin olun](#filesystem)</li></ul> | 
| **Web uygulaması** | <ul><li>[Hassas içerikleri tarayıcıda önbelleğe alınmamış emin olun](#cache-browser)</li><li>[Hassas verileri içeren Web uygulamanızın yapılandırma dosyalarını bölümlerini şifrele](#encrypt-data)</li><li>[Otomatik Tamamlama HTML öznitelik hassas formlar ve girişleri açıkça devre dışı](#autocomplete-input)</li><li>[Kullanıcının ekranda görüntülenen hassas verileri maskelenir emin olun](#data-mask)</li></ul> | 
| **Veritabanı** | <ul><li>[Gizli verilerin açığa olmayan ayrıcalıklı kullanıcıları sınırlamak için dinamik veri maskeleme gerçekleştir](#dynamic-users)</li><li>[Parolaları karma güvenlik biçiminde depolandığından emin olun](#salted-hash)</li><li>[Veritabanı sütunlarını postalardaki hassas verilerin şifrelendiğinden emin olun](#db-encrypted)</li><li>[Bu veritabanı düzeyi şifreleme (TDE) etkin olduğundan emin olun](#tde-enabled)</li><li>[Veritabanı Yedeklemeleri şifrelendiğinden emin olun](#backup)</li></ul> | 
| **Web API** | <ul><li>[Hassas verileri Web API'sine ilgili tarayıcının depoda depolanmaz emin olun](#api-browser)</li></ul> | 
| Azure belge DB | <ul><li>[Azure Cosmos DB içinde depolanan hassas verileri şifrele](#encrypt-docdb)</li></ul> | 
| **Azure Iaas VM güven sınırı** | <ul><li>[Sanal makineler tarafından kullanılan diskler şifrelemek için Azure Disk şifrelemesi kullanın](#disk-vm)</li></ul> | 
| **Service Fabric güven sınırı** | <ul><li>[Service Fabric uygulamaları parolaları şifrelemek](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Güvenlik modelleme gerçekleştirebilir ve iş birimleri/takımlar kullanmak gerektiğinde](#modeling-teams)</li><li>[Kritik varlıkları özelliğini paylaşmak için erişim simge durumuna küçült](#entities)</li><li>[Dynamics CRM paylaşım özelliğini ve iyi güvenlik uygulamaları ile ilişkili riskleri kullanıcıları eğitme](#good-practices)</li><li>[Özel Durum Yönetimi'nde yapılandırma ayrıntıları gösteren proscribing geliştirme standartları Kuralı Ekle](#exception-mgmt)</li></ul> | 
| **Azure Depolama** | <ul><li>[Rest (Önizleme) verileri için Azure Storage hizmeti şifreleme (SSE) kullanın](#sse-preview)</li><li>[Azure depolama alanında hassas verileri depolamak için istemci tarafı şifreleme kullanın](#client-storage)</li></ul> | 
| **Mobil istemci** | <ul><li>[Duyarlı veya telefonları yerel depolama alanına yazılan PII veri şifreleme](#pii-phones)</li><li>[Son kullanıcılara dağıtmadan önce oluşturulan ikili dosyaları belirsizleştirirseniz](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[Sertifika veya Windows clientCredentialType ayarlayın](#cert)</li><li>[WCF güvenlik modu etkin değil](#security)</li></ul> | 

## <a id="binaries-info"></a>Hassas bilgiler içeriyorsa, ikili dosyaları gizlenmiş emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Hassas bilgiler ters gereken hassas iş mantığı ticari sırlar içeriyorsa, ikili dosyaları gizlenmiş emin olun. Derlemelerin ters mühendislik durdurmak için budur. Araçlar `CryptoObfuscator` bu amaç için kullanılabilir. |

## <a id="efs-user"></a>Şifrelenmiş dosya sistemi (EFS) kullanarak gizli kullanıcıya özgü verileri korumak için kullanılan göz önünde bulundurun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Şifrelenmiş dosya sistemi (EFS) kullanarak bilgisayara fiziksel erişimi olan rakiplerin gizli kullanıcıya özgü verileri korumak için kullanılan göz önünde bulundurun. |

## <a id="filesystem"></a>Dosya sistemindeki uygulama tarafından depolanan hassas verilerin şifrelendiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Dosya sistemindeki uygulama tarafından depolanan hassas verilerin şifrelendiğinden emin olun (örneğin, DPAPI kullanarak), EFS zorlanamaz varsa |

## <a id="cache-browser"></a>Hassas içerikleri tarayıcıda önbelleğe alınmamış emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, Web Forms, MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Tarayıcılar önbelleğe alma ve geçmiş amacıyla bilgi depolayabilir. Bu önbelleğe alınan dosyalar, Internet Explorer durumunda geçici Internet dosyaları klasörünün gibi bir klasörde depolanır. Bu sayfaları yeniden adlandırılır, tarayıcı bunları kendi önbellekten görüntüler. Hassas bilgileri (örneğin, kendi adres, kredi kartı bilgileri, sosyal güvenlik numarası veya kullanıcı adı) kullanıcıya gösterilirse, sonra bu bilgileri tarayıcının önbelleğinde depolanan ve bu nedenle alınabilir, tarayıcının önbellek inceleniyor yoluyla veya olabilir yalnızca tarayıcının "Geri" düğmesine basarak. Hayır-tüm sayfalar için depo"" ön bellek denetimi yanıt üstbilgisi değerini ayarlayın. |

### <a name="example"></a>Örnek
```XML
<configuration>
  <system.webServer>
   <httpProtocol>
    <customHeaders>
        <add name="Cache-Control" value="no-cache" />
        <add name="Pragma" value="no-cache" />
        <add name="Expires" value="-1" />
    </customHeaders>
  </httpProtocol>
 </system.webServer>
</configuration>
```

### <a name="example"></a>Örnek
Bu filtre uygulanabilir. Aşağıdaki örnek kullanılabilir: 
```csharp
public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            if (filterContext == null || (filterContext.HttpContext != null && filterContext.HttpContext.Response != null && filterContext.HttpContext.Response.IsRequestBeingRedirected))
            {
                //// Since this is MVC pipeline, this should never be null.
                return;
            }

            var attributes = filterContext.ActionDescriptor.GetCustomAttributes(typeof(System.Web.Mvc.OutputCacheAttribute), false);
            if (attributes == null || **Attributes**.Count() == 0)
            {
                filterContext.HttpContext.Response.Cache.SetNoStore();
                filterContext.HttpContext.Response.Cache.SetCacheability(HttpCacheability.NoCache);
                filterContext.HttpContext.Response.Cache.SetExpires(DateTime.UtcNow.AddHours(-1));
                if (!filterContext.IsChildAction)
                {
                    filterContext.HttpContext.Response.AppendHeader("Pragma", "no-cache");
                }
            }

            base.OnActionExecuting(filterContext);
        }
``` 

## <a id="encrypt-data"></a>Hassas verileri içeren Web uygulamanızın yapılandırma dosyalarını bölümlerini şifrele

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Nasıl yapılır: ASP.NET 2.0 kullanarak DPAPI yapılandırma bölümlerinin şifrelemek](https://msdn.microsoft.com/library/ff647398.aspx), [korumalı bir yapılandırma sağlayıcısı belirtme](https://msdn.microsoft.com/library/68ze1hb2.aspx), [uygulama parolaları korumak için Azure anahtar kasası kullanma](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Adımları** | Yapılandırma dosyaları gibi Web.config appsettings.json genellikle kullanıcı adları, parolalar, veritabanı bağlantı dizelerini ve şifreleme anahtarları gibi hassas bilgiler tutmak için kullanılır. Bu bilgileri korumak, uygulamanızın saldırganların veya kötü amaçlı kullanıcılara hesap kullanıcı adları ve parolalar, veritabanı adları ve sunucu adları gibi hassas bilgileri alma savunmasızdır. (Azure/şirket içi) dağıtım türüne göre yapılandırma dosyaları DPAPI veya Azure anahtar kasası gibi hizmetleri kullanarak önemli bölümlerini şifreler. |

## <a id="autocomplete-input"></a>Otomatik Tamamlama HTML öznitelik hassas formlar ve girişleri açıkça devre dışı

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN: otomatik tamamlama özniteliği](http://msdn.microsoft.com/library/ms533486(VS.85).aspx), [HTML AutoComplete seçeneğini kullanarak](http://msdn.microsoft.com/library/ms533032.aspx), [HTML temizleme Güvenlik Açığı](http://technet.microsoft.com/security/bulletin/MS10-071), [otomatik tamamlama., yeniden?](http://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Adımları** | Otomatik Tamamlama özniteliği bir form otomatik tamamlama açmak veya kapatmak olup olmayacağını belirtir. Otomatik Tamamlama açık olduğunda, tarayıcı otomatik olarak tamamlamak önce kullanıcının girdiği değerlerine göre değerler. Örneğin, yeni bir ad ve parola bir formda girilir ve form gönderildiğinde, tarayıcı parolayı kaydetmiş sorar. Bundan sonra form görüntülendiğinde, adı ve parola otomatik olarak doldurulur veya adı girildiğinde tamamlandı. Yerel erişimi olan bir saldırgan, tarayıcı önbelleğinden düz metin parolası elde edilemedi. Otomatik Tamamlama varsayılan olarak etkindir ve onu açıkça devre dışı bırakılması gerekir. |

### <a name="example"></a>Örnek
```csharp
<form action="Login.aspx" method="post " autocomplete="off" >
      Social Security Number: <input type="text" name="ssn" />
      <input type="submit" value="Submit" />    
</form>
```

## <a id="data-mask"></a>Kullanıcının ekranda görüntülenen hassas verileri maskelenir emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Parolalar, kredi kartı numaraları SSN vb. gibi hassas verileri ekranda görüntülendiğinde maskelenmiş. Bu yetkisiz personelin (örn., Kama gezinme parolalar, kullanıcıların SSN numaralarını görüntüleme destek personeli) veri erişimini önlemek için yapılır. Bu veri öğeleri düz metin olarak görünür değildir ve uygun şekilde maskelenmiş emin olun. Bu (örneğin,. giriş olarak kabul ederken yapılan verdiğiniz gerekir Giriş türü = "parola") geri ekranda (örn., kredi kartı numarası görüntü yalnızca son 4 basamağıyla) görüntüleme yanı sıra. |

## <a id="dynamic-users"></a>Gizli verilerin açığa olmayan ayrıcalıklı kullanıcıları sınırlamak için dinamik veri maskeleme gerçekleştir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Sql Azure, OnPrem |
| **Öznitelikleri**              | SQL sürüm - V12, SQL sürümü - MsSQL2016 |
| **Başvuruları**              | [Dinamik veri maskeleme](https://msdn.microsoft.com/library/mt130841) |
| **Adımları** | Verileri görüntüleme gelen erişimi olmaması kullanıcılar önleme gizli verilerin açığa sınırlamak için dinamik veri maskeleme amacı budur. Dinamik veri maskeleme veritabanı kullanıcıların veritabanına doğrudan bağlanarak ve parça gizli verilerin açığa kapsamlı sorgular önlemek için hedeflenir değil. Dinamik veri maskeleme (Denetim, şifreleme, satır düzeyi güvenlik...) diğer SQL Server güvenlik özellikleri için tamamlayıcı ve bu özelliği bunları birlikte ayrıca daha iyi hassas verileri korumak için kullanmak için önerilir Veritabanı. Bu özellik yalnızca SQL Server 2016 ile başlayan ve Azure SQL veritabanı tarafından desteklenip desteklenmediğini unutmayın. |

## <a id="salted-hash"></a>Parolaları karma güvenlik biçiminde depolandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Parola Hashing .NET Crypto API'ları kullanma](http://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Adımları** | Parolalar özel kullanıcı deposu veritabanlarında depolanması. Parola karmaları salt değerlerle yerine depolanması gerekir. Salt kullanıcı için her zaman benzersizdir ve parola deneme yanılma yapma olasılığını ortadan kaldırmak için 150.000 döngüler en düşük iş faktörü yineleme sayısı ile depolamadan önce b-crypt, s-crypt veya PBKDF2 geçerli emin olun.| 

## <a id="db-encrypted"></a>Veritabanı sütunlarını postalardaki hassas verilerin şifrelendiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | SQL sürümü - tüm |
| **Başvuruları**              | [SQL Server'da hassas verileri şifrelemek](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [nasıl yapılır: SQL Server bir sütun, verileri şifrelemek](https://msdn.microsoft.com/library/ms179331), [sertifika tarafından şifrele](https://msdn.microsoft.com/library/ms188061) |
| **Adımları** | Kredi kartı numaraları gibi hassas verileri veritabanında şifrelenmiş gerekir. Veri sütun düzeyinde şifreleme kullanılarak şifrelenir veya şifreleme işlevleri kullanarak bir uygulama işlevi. |

## <a id="tde-enabled"></a>Bu veritabanı düzeyi şifreleme (TDE) etkin olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [SQL Server saydam veri şifrelemesi (TDE) Anlama](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Adımları** | SQL Server'da saydam veri şifreleme (TDE) özelliği bir veritabanındaki hassas verileri şifrelemek yardımcı olur ve bir sertifika ile verileri şifrelemek için kullanılan anahtarları koruyun. Bu anahtarları olmayan birisi verileri kullanmasını önler. TDE verileri koruduğu "sırada veri ve günlük dosyalarını anlamına gelen". Birçok yasalar, düzenlemeler ve çeşitli sektörün belirlenen talimatları uyması olanağı sağlar. |

## <a id="backup"></a>Veritabanı Yedeklemeleri şifrelendiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure, OnPrem |
| **Öznitelikleri**              | SQL sürüm - V12, SQL sürümü - MsSQL2014 |
| **Başvuruları**              | [SQL veritabanı yedekleme şifreleme](https://msdn.microsoft.com/library/dn449489) |
| **Adımları** | SQL Server Yedekleme oluşturulurken verileri şifrelemek için özelliğine sahiptir. Şifreleme algoritması ve Şifreleyici (sertifika veya asimetrik anahtar) belirterek bir yedek oluşturulduğunda, şifrelenmiş bir yedek dosya oluşturmayı seçebilirsiniz. |

## <a id="api-browser"></a>Hassas verileri Web API'sine ilgili tarayıcının depoda depolanmaz emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC 5, MVC 6 |
| **Öznitelikleri**              | Kimlik sağlayıcısı - ADFS, kimlik sağlayıcısı - Azure AD |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Belirli uygulamalarında hassas yapıları Web API'nin kimlik doğrulamasıyla ilgili tarayıcının yerel depolama alanına depolanır. Örneğin, Azure AD kimlik doğrulama yapıları adal.idtoken, adal.nonce.idtoken, adal.access.token.key, adal.token.keys, adal.state.login, adal.session.state, adal.expiration.key vb. ister.</p><p>Tüm bu yapıtların sonra bile kullanılabilir oturumu veya tarayıcı kapalı. Bir saldırganın bu yapılara erişimi alırsa, klasöründe (API) korunan kaynaklara erişim için bunları yeniden kullanabilirsiniz. Web API ile ilgili tüm hassas yapıları depolanmaz, tarayıcının depoda emin olun. İstemci-tarafı depolama olduğu kaçınılmaz durumlarda (örneğin, tek sayfa uygulamaları (örtük Openıdconnect/OAuth akış yararlanan SPA) gerek erişim belirteçleri yerel olarak depolamak), kullanım depolama seçenekleriyle Kalıcılık sahip değil. Örneğin, SessionStorage LocalStorage için tercih edilir.</p>| 

### <a name="example"></a>Örnek
JavaScript yerel depolama alanına kimlik doğrulaması yapıları depolayan bir özel kimlik doğrulama kitaplığı parçacığı arasındadır. Bu tür uygulamalar kaçınılmalıdır. 
```javascript
ns.AuthHelper.Authenticate = function () {
window.config = {
instance: 'https://login.microsoftonline.com/',
tenant: ns.Configurations.Tenant,
clientId: ns.Configurations.AADApplicationClientID,
postLogoutRedirectUri: window.location.origin,
cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
};
```

## <a id="encrypt-docdb"></a>Cosmos DB içinde depolanan hassas verileri şifrele

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure belge DB | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Belge DB depolamadan önce uygulama düzeyinde hassas verileri şifrelemek veya herhangi bir duyarlı veri Azure Storage veya Azure SQL gibi diğer depolama çözümleri depolayın| 

## <a id="disk-vm"></a>Sanal makineler tarafından kullanılan diskler şifrelemek için Azure Disk şifrelemesi kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Iaas VM güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Sanal makineler tarafından kullanılan diskler şifrelemek için Azure Disk şifrelemesi kullanma](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Adımları** | <p>Azure Disk şifrelemesi şu anda önizlemede olan yeni bir özelliktir. Bu özellik, işletim sistemi ve bir Iaas sanal makine tarafından kullanılan veri disklerle şifrelemek sağlar. Windows için sürücüleri, endüstri standardı BitLocker şifreleme teknolojisi kullanılarak şifrelenir. Linux için DM-Crypt teknolojisini kullanan diskleri şifrelenir. Bu, denetlemek ve disk şifreleme anahtarlarını yönetmek izin vermek için Azure anahtar kasası ile tümleşiktir. Azure Disk şifrelemesi çözüm aşağıdaki üç müşteri şifreleme senaryolarını destekler:</p><ul><li>Müşteri şifreli VHD dosyaları ve Azure anahtar kasasında depolanan müşteri tarafından sağlanan şifreleme anahtarlarını oluşturulan yeni Iaas VM'ler şifrelemeyi etkinleştirin.</li><li>Azure Marketi'nde oluşturulan yeni Iaas VM'ler şifrelemeyi etkinleştirin.</li><li>Zaten Azure'da çalışan Iaas VM'ler şifrelemeyi etkinleştirin.</li></ul>| 

## <a id="fabric-apps"></a>Service Fabric uygulamaları parolaları şifrelemek

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ortam - Azure |
| **Başvuruları**              | [Service Fabric uygulamaları parolaları yönetme](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Adımları** | Gizli depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini olmayan diğer değerleri gibi herhangi bir önemli bilgi olabilir. Azure anahtar kasası, anahtarları ve gizli anahtarları service fabric uygulamaları yönetmek için kullanın. |

## <a id="modeling-teams"></a>Güvenlik modelleme gerçekleştirebilir ve iş birimleri/takımlar kullanmak gerektiğinde

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Güvenlik modelleme gerçekleştirebilir ve iş birimleri/takımlar kullanmak gerektiğinde |

## <a id="entities"></a>Kritik varlıkları özelliğini paylaşmak için erişim simge durumuna küçült

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Kritik varlıkları özelliğini paylaşmak için erişim simge durumuna küçült |

## <a id="good-practices"></a>Dynamics CRM paylaşım özelliğini ve iyi güvenlik uygulamaları ile ilişkili riskleri kullanıcıları eğitme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Dynamics CRM paylaşım özelliğini ve iyi güvenlik uygulamaları ile ilişkili riskleri kullanıcıları eğitme |

## <a id="exception-mgmt"></a>Özel Durum Yönetimi'nde yapılandırma ayrıntıları gösteren proscribing geliştirme standartları Kuralı Ekle

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Özel durum yönetimi geliştirme dışında yapılandırma ayrıntıları gösteren proscribing geliştirme standartları kuralı içerir. Bu kod gözden geçirme ya da düzenli denetleme bir parçası olarak test edin.|

## <a id="sse-preview"></a>Rest (Önizleme) verileri için Azure Storage hizmeti şifreleme (SSE) kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | StorageType - Blob |
| **Başvuruları**              | [Azure Storage hizmeti şifreleme (Önizleme) bekleyen veri için](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Adımları** | <p>Azure Storage hizmeti şifreleme (SSE) bekleyen veri için korumak ve Kuruluş güvenliği ve uyumluluk taahhüt karşılamak için verilerinizi korumaya yardımcı olur. Bu özellik ile Azure depolama otomatik olarak depolama birimine devam ettirmeden önce verilerinizi şifreler ve alma önce şifresini çözer. Şifreleme, şifre çözme ve anahtar yönetimi kullanıcılara tamamen saydam. SSE yalnızca, sayfa blobları blok ve ilave blobları için geçerlidir. Veri tabloları, kuyrukları ve dosyaları da dahil olmak üzere, başka türlerde şifrelenmez.</p><p>Şifreleme ve şifre çözme iş akışı:</p><ul><li>Müşterinin depolama hesabı üzerinde şifrelemeyi etkinleştirir</li><li>Müşteri (PUT Blob, PUT bloğu, PUT sayfası, vb.) yeni verileri Blob depolama alanına yazdığında; Her yazma 256 bit AES şifreleme, güçlü blok şifrelemeler biri kullanılarak şifrelenir</li><li>Müşteri verilerini (Blob alma, vb.) erişim gerektiğinde veriler kullanıcıya geri dönmeden önce otomatik olarak çözülür</li><li>Şifreleme devre dışı bırakılırsa, yeni yazma artık şifrelenir ve kullanıcı tarafından yeniden yazılmıştır kadar mevcut şifrelenmiş veriler şifrelenmiş kalır. Şifreleme etkinken Blob depolama alanına yazma şifrelenir. Etkinleştirme/depolama hesabı için şifrelemeyi devre dışı bırakma arasında geçiş kullanıcıyla veri durumunu değiştirmez</li><li>Tüm şifreleme anahtarları depolanan, şifrelenmiş ve Microsoft tarafından yönetilen</li></ul><p>Lütfen şu anda unutmayın, şifreleme için kullanılan anahtarları Microsoft tarafından yönetilir. Microsoft anahtarları başlangıçta oluşturur ve iç Microsoft İlkesi tarafından tanımlanan normal döndürme yanı sıra anahtarları Güvenli Depolama yönetin. Gelecekte, müşterilerin yönetme olanağı kendi alacağı > şifreleme anahtarlarını ve müşteri tarafından yönetilen anahtarları anahtarlardan Microsoft tarafından yönetilen bir geçiş yolunu girin.</p>| 

## <a id="client-storage"></a>Azure depolama alanında hassas verileri depolamak için istemci tarafı şifreleme kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [İstemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [Öğreticisi: şifrelemek ve şifresini çözmek Azure anahtar kasası kullanılarak Microsoft Azure Storage blobları](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/), [verileri güvenli depolama Azure Blob içinde Azure şifreleme uzantılı depolama](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) |
| **Adımları** | <p>.NET Nuget paketi için Azure Storage istemci kitaplığı Azure Storage'a yüklemeden ve istemciye indirirken verilerin şifresini çözmek önce istemci uygulamalar içinde verilerin şifrelenmesi destekler. Kitaplık ayrıca depolama hesabı anahtarı yönetimi için Azure Anahtar Kasası ile tümleştirmeyi destekler. Aşağıda, istemci tarafı şifreleme çalışma biçimine kısa bir açıklaması verilmiştir:</p><ul><li>Simetrik anahtar bir kerelik kullan bir içerik şifreleme anahtarı (CEK), Azure Storage istemci SDK oluşturur</li><li>Müşteri verileri bu CEK kullanılarak şifrelenir</li><li>CEK (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) paketlenir. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti ya da bir simetrik anahtar olması ve yerel olarak yönetilebilir veya Azure anahtar Kasası ' depolanır. Depolama istemcisi KEK hiçbir zaman erişebilir. Yalnızca, anahtar kasası tarafından sağlanan anahtar kaydırma algoritması çağırır. Müşteriler için anahtar kaydırma/bunlar istiyorsanız açmak özel sağlayıcılar kullanmayı seçebilirsiniz</li><li>Şifrelenmiş veriler daha sonra Azure Storage hizmetine yüklenir. Alt düzey uygulama ayrıntıları için başvurular bölümündeki bağlantıları denetleyin.</li></ul>|

## <a id="pii-phones"></a>Duyarlı veya telefonları yerel depolama alanına yazılan PII veri şifreleme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil istemci | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, Xamarin  |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Ayarları ve özellikleri cihazlarınızda Microsoft Intune ilkeleriyle yönetme](https://docs.microsoft.com/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies#create-a-configuration-policy), [Anahtarlık Valet](https://components.xamarin.com/view/square.valet) |
| **Adımları** | <p>Uygulama kullanıcının PII (e-posta, telefon numarası, ad, Soyadı, Tercihler vb.) gibi hassas bilgileri yazıyorsa -mobile'nın dosya sisteminde, ardından bunu yerel dosya sistemine yazmadan önce şifrelenmelidir. Uygulama Kurumsal uygulama ise, Windows Intune kullanarak yayımlama uygulama olasılığını keşfedin.</p>|

### <a name="example"></a>Örnek
Intune, hassas verilerinizi korumak için aşağıdaki güvenlik ilkeleri ile yapılandırılabilir: 
```csharp
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Örnek
Uygulama Kurumsal uygulama değilse, sağlanan platformu anahtar deposu kullanın, hangi şifreleme işlemi kullanarak şifreleme anahtarlarını saklamak için anahtarlıklar dosya sistemi üzerinde gerçekleştirilebilir. Aşağıdaki kod parçacığını xamarin kullanarak Anahtarlık erişim tuşu gösterilmektedir: 
```csharp
        protected static string EncryptionKey
        {
            get
            {
                if (String.IsNullOrEmpty(_Key))
                {
                    var query = new SecRecord(SecKind.GenericPassword);
                    query.Service = NSBundle.MainBundle.BundleIdentifier;
                    query.Account = "UniqueID";

                    NSData uniqueId = SecKeyChain.QueryAsData(query);
                    if (uniqueId == null)
                    {
                        query.ValueData = NSData.FromString(System.Guid.NewGuid().ToString());
                        var err = SecKeyChain.Add(query);
                        _Key = query.ValueData.ToString();
                    }
                    else
                    {
                        _Key = uniqueId.ToString();
                    }
                }

                return _Key;
            }
        }
```

## <a id="binaries-end"></a>Son kullanıcılara dağıtmadan önce oluşturulan ikili dosyaları belirsizleştirirseniz

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil istemci | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [.Net için şifre gizleme](http://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Adımları** | Derlemelerin ters mühendislik durdurmak için oluşturulan ikili dosyaları (apk içindeki derlemelerde) gizlenmiş. Araçlar `CryptoObfuscator` bu amaç için kullanılabilir. |

## <a id="cert"></a>Sertifika veya Windows clientCredentialType ayarlayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Fortify](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Adımları** | Şifrelenmemiş bir kanal üzerinden düz metin parola ile bir UsernameToken kullanarak parola SOAP iletilerini bulmak saldırganlara kullanıma sunar. UsernameToken kullanan hizmet sağlayıcılar, parolaları düz metin olarak gönderilen kabul edebilir. Düz metin parolalarını şifrelenmemiş bir kanal üzerinden gönderme SOAP iletisi bulmak saldırganlar için kimlik bilgileri getirebilir. | 

### <a name="example"></a>Örnek
Aşağıdaki WCF hizmet sağlayıcısı yapılandırmasını UsernameToken kullanır: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
Sertifika veya Windows clientCredentialType ayarlayın. 

## <a id="security"></a>WCF güvenlik modu etkin değil

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, .NET Framework 3 |
| **Öznitelikleri**              | Güvenlik modu - taşıma, güvenlik modu - iletisi |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/vulncat/index.html), [WCF güvenlik temel ilkeleri dergisi kod](http://www.codemag.com/article/0611051) |
| **Adımları** | Taşıma veya ileti güvenlik tanımlandı. İletileri aktarım olmadan veya güvenlik bütünlüğü veya gizliliği iletilerin garanti edemez ileti iletme uygulamalar. WCF güvenlik bağlama None olarak ayarlandığında, taşıma ve ileti güvenliği devre dışı bırakılır. |

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma güvenlik modu None olarak ayarlar. 
```
<system.serviceModel> 
  <bindings> 
    <wsHttpBinding> 
      <binding name=""MyBinding""> 
        <security mode=""None""/> 
      </binding> 
  </bindings> 
</system.serviceModel> 
```

### <a name="example"></a>Örnek
Tüm hizmet bağlamaları arasında güvenlik modu beş olası güvenlik modu bulunmaktadır: 
* Yok. Güvenlik kapatır. 
* Taşıma. Aktarım güvenliği karşılıklı kimlik doğrulaması ve ileti koruması için kullanır. 
* İleti. İleti güvenliği için karşılıklı kimlik doğrulama ve ileti koruması kullanır. 
* Her ikisi de. Taşıma ve ileti düzeyi güvenlik (yalnızca MSMQ bu destekler) ayarlarını sağlamanıza olanak tanır. 
* TransportWithMessageCredential. Kimlik bilgileri iletisi ve ileti koruma ile aktarılır ve sunucu kimlik doğrulaması, Aktarım katmanı tarafından sağlanır. 
* TransportCredentialOnly. İstemci kimlik bilgileri ile Aktarım katmanı geçirilir ve ileti koruma uygulanır. Güvenlik taşıma ve ileti bütünlüğü ve gizliliği iletilerinin korumak için kullanın. Yapılandırma hizmetini ileti kimlik bilgileri ile taşıma güvenliği kullanacak biçimde söyler.
```
<system.serviceModel>
  <bindings>
    <wsHttpBinding>
    <binding name=""MyBinding""> 
    <security mode=""TransportWithMessageCredential""/> 
    <message clientCredentialType=""Windows""/> 
    </binding> 
  </bindings> 
</system.serviceModel> 
```
