---
title: Hassas verileri - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
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
ms.openlocfilehash: 27028903daeaf62a25584300944538341a861c80
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60610575"
---
# <a name="security-frame-sensitive-data--mitigations"></a>Güvenlik çerçevesi: Hassas verileri | Risk azaltma işlemleri 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Makine güven sınırı** | <ul><li>[Bunlar hassas bilgileri içeriyorsa, ikili dosyaları gizlenmiş emin olun.](#binaries-info)</li><li>[Şifrelenmiş dosya sistemi (EFS) kullanarak gizli kullanıcıya özgü verileri korumak için kullanılan göz önünde bulundurun](#efs-user)</li><li>[Dosya sisteminde uygulama tarafından depolanan hassas verilerin şifrelendiğinden emin olun](#filesystem)</li></ul> | 
| **Web uygulaması** | <ul><li>[Tarayıcıda hassas içerik önbelleğe alınmamış emin olun.](#cache-browser)</li><li>[Hassas veriler içeren Web uygulamanın yapılandırma dosyalarının bölümleri şifrelemek](#encrypt-data)</li><li>[Gizli formlar ve girişleri otomatik tamamlama HTML öznitelik açıkça devre dışı](#autocomplete-input)</li><li>[Kullanıcının ekranda görüntülenen hassas veriler maskelenir emin olun.](#data-mask)</li></ul> | 
| **Veritabanı** | <ul><li>[Uygulama dinamik veri maskeleme, hassas verilerin açığa ayrıcalıklı olmayan kullanıcıları sınırlamak için](#dynamic-users)</li><li>[Parolaları karma salted biçiminde depolandığından emin olun](#salted-hash)</li><li>[Veritabanı sütunları hassas verilerin şifrelendiğinden emin olun](#db-encrypted)</li><li>[Bu veritabanı düzeyinde şifrelemeyi (TDE) etkin olduğundan emin olun](#tde-enabled)</li><li>[Veritabanı Yedeklemeleri şifrelendiğinden emin olun](#backup)</li></ul> | 
| **Web API** | <ul><li>[Hassas verileri Web API'sine ilgili tarayıcının depolamada depolanmaz emin olun.](#api-browser)</li></ul> | 
| Azure belge veritabanı | <ul><li>[Azure Cosmos DB'de depolanan hassas verileri şifrele](#encrypt-docdb)</li></ul> | 
| **Azure Iaas VM güven sınırı** | <ul><li>[Sanal makineler tarafından kullanılan diskleri şifreleme için Azure Disk Şifrelemesi'ni kullanın](#disk-vm)</li></ul> | 
| **Service Fabric güven sınırı** | <ul><li>[Service Fabric uygulamaları gizli dizileri şifrelemek](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Güvenlik modelleme gerçekleştirebilir ve iş birimleri/ekiplerin gerekli olduğu durumlarda](#modeling-teams)</li><li>[Kritik varlıkları özelliğini paylaşmak için erişim simge durumuna küçült](#entities)</li><li>[İyi güvenlik uygulamaları ve Dynamics CRM Paylaşımı özelliği ile ilişkili riskleri kullanıcıları eğitme](#good-practices)</li><li>[Özel durum yönetimi, yapılandırma ayrıntıları gösteren proscribing geliştirme standartları Kuralı Ekle](#exception-mgmt)</li></ul> | 
| **Azure Depolama** | <ul><li>[(Önizleme) bekleyen veri için Azure depolama hizmeti şifrelemesi (SSE) kullanma](#sse-preview)</li><li>[Hassas verileri Azure depolamada depolamak için istemci tarafı şifreleme kullanma](#client-storage)</li></ul> | 
| **Mobil istemci** | <ul><li>[Duyarlı ya da telefonlar yerel depolama alanına yazılan PII veri şifreleme](#pii-phones)</li><li>[Son kullanıcılara dağıtmadan önce oluşturulan ikili dosyaları karartmak](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[Sertifika veya Windows clientCredentialType ayarlayın](#cert)</li><li>[WCF güvenlik modu etkin değil](#security)</li></ul> | 

## <a id="binaries-info"></a>Bunlar hassas bilgileri içeriyorsa, ikili dosyaları gizlenmiş emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bunlar gibi ticari sırlar, hassas bilgileri ters gereken hassas iş mantığı içeriyorsa, ikili dosyaları gizlenmiş emin olun. Bu derlemelerin tersine mühendislik önlemektir. Gibi araçları `CryptoObfuscator` bu amaç için kullanılabilir. |

## <a id="efs-user"></a>Şifrelenmiş dosya sistemi (EFS) kullanarak gizli kullanıcıya özgü verileri korumak için kullanılan göz önünde bulundurun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Şifrelenmiş dosya sistemi (EFS) kullanarak bilgisayara fiziksel erişimi olan saldırganlar gizli kullanıcıya özgü verileri korumak için kullanılan göz önünde bulundurun. |

## <a id="filesystem"></a>Dosya sisteminde uygulama tarafından depolanan hassas verilerin şifrelendiğinden emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Dosya sisteminde uygulama tarafından depolanan hassas verilerin şifrelendiğinden emin olun (örneğin, DPAPI kullanarak), EFS zorlanamaz varsa |

## <a id="cache-browser"></a>Tarayıcıda hassas içerik önbelleğe alınmamış emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Generic, Web Forms, MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Tarayıcılar, önbelleğe alma ve geçmiş amacıyla bilgi depolayabilirsiniz. Bu önbelleğe alınan dosyalar geçici Internet dosyaları klasörünün söz konusu olduğunda Internet Explorer gibi bir klasörde depolanır. Bu sayfaları yeniden adlandırılır, tarayıcı önbelleğinden bunları görüntüler. Hassas bilgileri kullanıcıya (örneğin, kendi adres, kredi kartı bilgileri, sosyal güvenlik numarası veya kullanıcı adı) gösterilirse, ardından bu bilgileri tarayıcının önbelleğini depolanır ve bu nedenle alınabilir, tarayıcının önbelleğini İnceleme yoluyla veya olabilir yalnızca tarayıcı "Geri" düğmesine basarak. Cache-control yanıt üst bilgisi değeri "no-tüm sayfalar için depolama için" olarak ayarlayın. |

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

## <a id="encrypt-data"></a>Hassas veriler içeren Web uygulamanın yapılandırma dosyalarının bölümleri şifrelemek

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Nasıl Yapılır: ASP.NET 2.0 kullanarak DPAPI yapılandırma bölümlerde şifrelemek](https://msdn.microsoft.com/library/ff647398.aspx), [korumalı bir yapılandırma sağlayıcısı belirtme](https://msdn.microsoft.com/library/68ze1hb2.aspx), [uygulama parolalarını korumak için Azure Key Vault kullanma](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Adımları** | Yapılandırma dosyaları gibi Web.config appsettings.json genellikle kullanıcı adları, parolalar, veritabanı bağlantı dizelerini ve şifreleme anahtarları gibi hassas bilgileri tutmak için kullanılır. Bu bilgi koruma değil, saldırganların veya kötü amaçlı kullanıcıların hesap kullanıcı adları ve parolalar, veritabanı adları ve sunucu adları gibi hassas bilgileri almak için uygulamanızı savunmasızdır. Dağıtım türüne (azure/şirket içi) göre yapılandırma dosyaları DPAPI veya Azure anahtar kasası gibi hizmetleri kullanarak önemli bölümlerini şifreler. |

## <a id="autocomplete-input"></a>Gizli formlar ve girişleri otomatik tamamlama HTML öznitelik açıkça devre dışı

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN: otomatik tamamlama özniteliği](https://msdn.microsoft.com/library/ms533486(VS.85).aspx), [kullanarak otomatik tamamlama HTML](https://msdn.microsoft.com/library/ms533032.aspx), [HTML temizleme Güvenlik Açığı](https://technet.microsoft.com/security/bulletin/MS10-071), [otomatik tamamlama. yeniden?!](https://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Adımları** | Otomatik Tamamlama öznitelik, bir form otomatik tamamlama açıp olup olmayacağını belirtir. Otomatik Tamamlama açık olduğunda, tarayıcının otomatik tamamlanacak önce kullanıcının girdiği değerlerini temel alan değerleri. Örneğin, bir yeni adı ve parola bir formda girilir ve form gönderildiğinde, tarayıcı parola kaydedilen ister. Bundan sonra form görüntülendiğinde, adını ve parolasını adı girildiğinde tamamlanmış veya otomatik olarak doldurulur. Yerel erişimine sahip bir saldırgan, düz metin parolası tarayıcı önbelleğinden edinebilir. Otomatik Tamamlama varsayılan olarak etkindir ve onu açıkça devre dışı bırakılmalıdır. |

### <a name="example"></a>Örnek
```csharp
<form action="Login.aspx" method="post " autocomplete="off" >
      Social Security Number: <input type="text" name="ssn" />
      <input type="submit" value="Submit" />    
</form>
```

## <a id="data-mask"></a>Kullanıcının ekranda görüntülenen hassas veriler maskelenir emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Parolalar, kredi kartı numaraları SSN vb. gibi hassas verilerin ekranda görüntülendiğinde maskelenmiş. Yetkisiz personelin verileri (örneğin, omuz gezinme parolalar, görüntüleme kullanıcıları SSN sayıda destek personeli) erişmesini engellemek için budur. Bu veri öğeleri düz metin olarak görünür değildir ve uygun şekilde maskelenmiş emin olun. Bu geçen (ör. girdi olarak kabul ederken bir sorun dikkatli gerekir Giriş türü = "password") yanı sıra geri ekranında (örneğin, kredi kartı numarasının görüntü yalnızca son 4 rakam) görüntüleme. |

## <a id="dynamic-users"></a>Uygulama dinamik veri maskeleme, hassas verilerin açığa ayrıcalıklı olmayan kullanıcıları sınırlamak için

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Sql Azure, OnPrem |
| **Öznitelikler**              | SQL sürümü - V12, SQL sürümü - MsSQL2016 |
| **Başvuruları**              | [Dinamik veri maskeleme](https://msdn.microsoft.com/library/mt130841) |
| **Adımları** | Amacı, dinamik veri maskeleme, hassas verilerin, verileri görüntülemesini erişmemesi gereken kullanıcıların çalıştırmanız gerekir. Dinamik veri maskeleme veritabanı kullanıcıların doğrudan veritabanına bağlanarak ve önemli veri parçalarını kullanıma sunan kapsamlı sorgular önlemek için hedeflenir değil. Dinamik veri maskeleme (Denetim, şifreleme, satır düzeyi güvenlik...) diğer SQL Server güvenlik özellikleri için tamamlayıcı ve bu özelliği bunları birlikte ek olarak hassas verileri daha iyi korumak için kullanmak için önerilir Veritabanı. Bu özellik yalnızca SQL Server 2016'dan itibaren ve Azure SQL veritabanı tarafından desteklenip desteklenmediğini unutmayın. |

## <a id="salted-hash"></a>Parolaları karma salted biçiminde depolandığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Parola Hashing .NET Crypto API'ları kullanma](https://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Adımları** | Parolaları özel kullanıcı deposu veritabanlarında depolanması. Parola karmalarının salt değerlerle yerine depolanması gerekir. Kullanıcı için güvenlik değeri her zaman benzersizdir ve parola, deneme yanılma yapma olasılığını ortadan kaldırmak için 150.000 döngüler en düşük iş faktörü yineleme sayısı ile depolamadan önce b-crypt, s-crypt veya PBKDF2 uygulamak emin olun.| 

## <a id="db-encrypted"></a>Veritabanı sütunları hassas verilerin şifrelendiğinden emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | SQL sürümü - tüm |
| **Başvuruları**              | [SQL Server'da hassas verileri şifrelemeye](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [nasıl yapılır: Bir SQL Server'da veri sütununu şifreleme](https://msdn.microsoft.com/library/ms179331), [sertifika ile şifreleme](https://msdn.microsoft.com/library/ms188061) |
| **Adımları** | Kredi kartı numaraları gibi hassas veriler veritabanında şifrelenmiş gerekir. Sütun düzeyinde şifreleme kullanılarak veri şifrelenebilir veya şifreleme işlevleri kullanarak bir uygulama işleve göre. |

## <a id="tde-enabled"></a>Bu veritabanı düzeyinde şifrelemeyi (TDE) etkin olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [SQL Server saydam veri şifrelemesi (TDE) Anlama](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Adımları** | SQL server saydam veri şifrelemesi (TDE) özelliği, bir veritabanındaki hassas verileri şifrelemeye de yardımcı olur ve bir sertifika ile verileri şifrelemek için kullanılan anahtarları koruyun. Bu anahtarları olmadan herkes veri kullanmasını önler. TDE, verileri koruyan "bekleyen verilerin ve günlük dosyalarının anlamına gelen". Bu, birçok yasaları, düzenlemeleri ve çeşitli sektörlerde oluşturduğu yönergeleri ile uyumlu olanağı sağlar. |

## <a id="backup"></a>Veritabanı Yedeklemeleri şifrelendiğinden emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure, şirket içi |
| **Öznitelikler**              | SQL sürümü - V12, SQL sürümü - MsSQL2014 |
| **Başvuruları**              | [SQL veritabanı yedekleme şifrelemesi](https://msdn.microsoft.com/library/dn449489) |
| **Adımları** | SQL Server Yedekleme oluşturulurken verileri şifrelemek için özelliğine sahiptir. Şifreleme algoritması ve Şifreleyici (bir sertifika veya asimetrik anahtar) belirterek bir yedek oluştururken bir şifrelenmiş bir yedekleme dosyası oluşturabilirsiniz. |

## <a id="api-browser"></a>Hassas verileri Web API'sine ilgili tarayıcının depolamada depolanmaz emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC 5, MVC 6 |
| **Öznitelikler**              | Kimlik sağlayıcısı - AD FS, kimlik sağlayıcısı - Azure AD |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Belirli uygulamalarında, Web API'SİNİN kimlik doğrulamasıyla ilgili hassas yapıtları tarayıcının yerel depolama alanında depolanır. Örneğin, Azure AD kimlik doğrulaması yapıtları adal.idtoken, adal.nonce.idtoken, adal.access.token.key, adal.token.keys, adal.state.login, adal.session.state, vb. adal.expiration.key ister.</p><p>Tüm bu yapıtların sonra bile kullanılabilir durumda oturumu kapatın veya tarayıcı kapatıldı. Bir saldırganın bu yapıt için erişim alırsa, derse (API) korumalı kaynaklara erişmek için bunları yeniden kullanabilirsiniz. Web API'si tüm hassas yapıtları depolanmaz, tarayıcının depolamada emin olun. İstemci tarafı depolama olduğu kaçınılmaz durumlarda (örneğin, tek sayfa uygulamaları (örtük Openıdconnect/OAuth akışlar yararlanan SPA) gerekli erişim belirteçleri yerel olarak depolamak) kullanım depolama seçenekleriyle Kalıcılık sahip değil. Örneğin, SessionStorage LocalStorage için tercih eder.</p>| 

### <a name="example"></a>Örnek
JavaScript kimlik doğrulama yapıtları yerel depolama alanında depolar bir özel kimlik doğrulama kitaplığından kod parçacığıdır. Bu tür uygulamaları kaçınılmalıdır. 
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

## <a id="encrypt-docdb"></a>Cosmos DB'de depolanan hassas verileri şifrele

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure belge veritabanı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Belge DB içinde depolamadan önce uygulama düzeyinde hassas verileri şifrelemek veya Azure depolama veya Azure SQL gibi diğer depolama çözümleri hassas verileri depolayın| 

## <a id="disk-vm"></a>Sanal makineler tarafından kullanılan diskleri şifreleme için Azure Disk Şifrelemesi'ni kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Iaas VM güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Diskleri, sanal makineler tarafından kullanılan şifreleme için Azure Disk Şifrelemesi'ni kullanma](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Adımları** | <p>Azure Disk şifrelemesi şu anda önizlemede olan yeni bir özelliktir. Bu özellik işletim sistemi diskleri ve veri diskleri bir Iaas sanal makine tarafından kullanılan şifrelemenizi sağlar. Windows için sürücüleri, endüstri standardı BitLocker şifreleme teknolojisi kullanılarak şifrelenir. Linux için DM-Crypt teknolojisini kullanarak diskleri şifrelenir. Bu, disk şifreleme anahtarlarını yönetmek ve denetlemek izin vermek için Azure anahtar kasası ile tümleştirilmiştir. Azure Disk şifrelemesi çözümü aşağıdaki üç müşteri şifreleme senaryolarını destekler:</p><ul><li>Müşteri şifreli VHD dosyaları ve Azure anahtar Kasası'nda depolanan müşteri tarafından sağlanan şifreleme anahtarı, oluşturulan yeni Iaas Vm'leri şifrelemeyi etkinleştirin.</li><li>Iaas VM'ler ile Azure marketinden oluşturulan yeni şifrelemeyi etkinleştirin.</li><li>Zaten Azure'da çalışan mevcut Iaas Vm'leri şifrelemeyi etkinleştirin.</li></ul>| 

## <a id="fabric-apps"></a>Service Fabric uygulamaları gizli dizileri şifrelemek

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ortam - Azure |
| **Başvuruları**              | [Service Fabric uygulamalarında gizli anahtarları yönetme](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Adımları** | Gizli dizileri depolama bağlantı dizeleri, parolalar veya düz metin olarak işleneceğini değil diğer değerleri gibi herhangi bir önemli bilgi olabilir. Azure Key Vault, anahtarlarınızı ve gizli service fabric uygulamaları yönetmek için kullanın. |

## <a id="modeling-teams"></a>Güvenlik modelleme gerçekleştirebilir ve iş birimleri/ekiplerin gerekli olduğu durumlarda

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Güvenlik modelleme gerçekleştirebilir ve iş birimleri/ekiplerin gerekli olduğu durumlarda |

## <a id="entities"></a>Kritik varlıkları özelliğini paylaşmak için erişim simge durumuna küçült

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Kritik varlıkları özelliğini paylaşmak için erişim simge durumuna küçült |

## <a id="good-practices"></a>İyi güvenlik uygulamaları ve Dynamics CRM Paylaşımı özelliği ile ilişkili riskleri kullanıcıları eğitme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | İyi güvenlik uygulamaları ve Dynamics CRM Paylaşımı özelliği ile ilişkili riskleri kullanıcıları eğitme |

## <a id="exception-mgmt"></a>Özel durum yönetimi, yapılandırma ayrıntıları gösteren proscribing geliştirme standartları Kuralı Ekle

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Geliştirme dışında özel durum yönetimi, yapılandırma ayrıntıları gösteren proscribing geliştirme standartları kural içerir. Bu kod incelemelerini veya düzenli İnceleme bir parçası olarak test edin.|

## <a id="sse-preview"></a>(Önizleme) bekleyen veri için Azure depolama hizmeti şifrelemesi (SSE) kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | StorageType - Blob |
| **Başvuruları**              | [Azure depolama hizmeti şifrelemesi (Önizleme) bekleyen veriler için](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Adımları** | <p>Azure depolama hizmeti şifrelemesi (SSE) bekleyen veriler için Kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine verilerinizi koruyarak yardımcı olur. Bu özellik ile Azure Depolama, verilerinizi depolama alanında kalıcı hale gelmeden önce otomatik olarak şifreler ve alınmadan önce bunların şifresini çözer. Şifreleme, şifre çözme ve anahtar yönetimi kullanıcılara tamamen şeffaf. SSE, yalnızca blok blobları, sayfa blobları ve ilave blobları için geçerlidir. Veri tabloları, kuyrukları ve dosyaları dahil olmak üzere, diğer türleri şifrelenmez.</p><p>Şifreleme ve şifre çözme iş akışı:</p><ul><li>Müşteri depolama hesabında şifreleme sağlar.</li><li>Ne zaman müşteri (PUT Blob, blok YERLEŞTİRME, PUT sayfası, vb.), yeni verileri Blob depolamaya Yazar; Her yazma 256 bit AES şifrelemesi, en güçlü blok şifreleme özelliklerinden biri kullanılarak şifrelenir.</li><li>Müşteri verilerini (Blob alma, vb.) erişmesi gerektiğinde, veri kullanıcıya döndürmeden önce otomatik olarak çözülür</li><li>Şifreleme devre dışı bırakılırsa, yeni yazma işlemleri artık şifrelenir ve kullanıcı tarafından yazılan kadar mevcut şifrelenmiş veriler şifrelenmiş kalır. Şifreleme etkin durumdayken, Blob depolamaya yazma şifrelenir. Kullanıcının etkinleştirme/depolama hesabı için şifrelemeyi devre dışı bırakma arasında geçiş ile veri durumunu değiştirmez</li><li>Tüm şifreleme anahtarları depolanan, şifrelenmiş ve Microsoft tarafından yönetilen</li></ul><p>Microsoft tarafından kullanılan şifreleme anahtarlarını şu anda yönetilen unutmayın. Microsoft anahtarlarını başlangıçta oluşturur ve güvenli depolama anahtarları ve bunun yanı sıra normal döndürme iç Microsoft İlkesi tarafından tanımlandığı şekilde yönetin. Gelecekte, müşterilerin yönetme olanağı, kendi alacağı > şifreleme anahtarlarını ve Microsoft tarafından yönetilen anahtarlar bir geçiş yolu müşteri tarafından yönetilen anahtarlar sağlar.</p>| 

## <a id="client-storage"></a>Hassas verileri Azure depolamada depolamak için istemci tarafı şifreleme kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [İstemci tarafı şifreleme ve Azure anahtar kasası için Microsoft Azure depolama](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [Öğreticisi: Şifreleme ve şifre çözme Azure anahtar Kasası'nı kullanarak Microsoft Azure depolama BLOB'ları](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/), [Azure şifreleme uzantıları ile Azure Blob depolamadaki verileri güvenle depolamak](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) |
| **Adımları** | <p>.NET Nuget paketi için Azure depolama istemci kitaplığı Azure Storage'a yüklemeden ve istemciye indirirken verilerin şifresini çözme önce istemci uygulamalar içinde verilerin şifrelenmesini destekler. Kitaplık ayrıca depolama hesabı anahtarı yönetimi için Azure Anahtar Kasası ile tümleştirmeyi destekler. Aşağıda, istemci tarafı şifreleme nasıl çalışır hakkında kısa bir açıklaması verilmiştir:</p><ul><li>Azure depolama istemci SDK'sı bir kerelik kullanım simetrik anahtarı olan bir içerik şifreleme anahtarı (CEK) oluşturur.</li><li>Müşteri verilerini bu CEK kullanılarak şifrelenir.</li><li>CEK (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) paketlenir. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti veya bir simetrik anahtar ve yerel olarak yönetilebilir veya Azure anahtar Kasası'nda depolanır. Depolama istemcisi KEK hiçbir zaman erişebilir. Yeni Key Vault tarafından sağlanan anahtar sarmalama algoritması da çağırır. Müşteriler, özel sağlayıcılar için kaydırma/istediğinde çözülme anahtarı kullanmayı da tercih edebilirsiniz</li><li>Şifrelenmiş veriler, ardından Azure depolama hizmetine yüklenir. Alt düzey uygulama ayrıntıları için başvurular bölümündeki bağlantıları kontrol edin.</li></ul>|

## <a id="pii-phones"></a>Duyarlı ya da telefonlar yerel depolama alanına yazılan PII veri şifreleme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil istemci | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, Xamarin  |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Ayarları ve özellikleri cihazlarınızda Intune ilkeleriyle yönetmek](https://docs.microsoft.com/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies), [Anahtarlık Vale](https://components.xamarin.com/view/square.valet) |
| **Adımları** | <p>Kullanıcının PII (e-posta, telefon numarası, ad, Soyadı, tercihleri vb.) gibi hassas bilgileri uygulama yazma -mobile'nın dosya sisteminde, sonra da yerel dosya sistemine yazmadan önce şifrelenmelidir. Ardından uygulama, kuruluş uygulaması ise, Windows Intune kullanarak yayımlama uygulaması olasılığını keşfedin.</p>|

### <a name="example"></a>Örnek
Intune, hassas verileri korumak için aşağıdaki güvenlik ilkeleri ile yapılandırılabilir: 
```csharp
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Örnek
Uygulama, kuruluş uygulaması değil, ardından sağlanan platform keystore'u kullan, dosya sisteminde şifreleme işlemi kullanarak, şifreleme anahtarlarını depolamak için anahtarlıklar gerçekleştirilebilir. Aşağıdaki kod parçacığı, erişim anahtarı kullanarak xamarin anahtarlığınızdan gösterilmektedir: 
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

## <a id="binaries-end"></a>Son kullanıcılara dağıtmadan önce oluşturulan ikili dosyaları karartmak

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil istemci | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [.Net için şifreleme gizleme](https://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Adımları** | Derlemelerin tersine mühendislik durdurmak için oluşturulan ikili dosyaları (apk içinde bütünleştirilmiş kodları) gizlenmiş. Gibi araçları `CryptoObfuscator` bu amaç için kullanılabilir. |

## <a id="cert"></a>Sertifika veya Windows clientCredentialType ayarlayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_token) |
| **Adımları** | Bir UsernameToken şifrelenmemiş bir kanal üzerinden düz metin parola kullanarak parola SOAP iletilerini bulmak saldırganların kullanıma sunar. UsernameToken kullanan bir hizmet sağlayıcıları, parolaları düz metin olarak gönderilen kabul edebilirsiniz. SOAP iletisi bulmak saldırganlar için kimlik bilgilerini düz metin parolalar şifrelenmemiş bir kanal üzerinden gönderme üzerinden kullanıma sunabilirsiniz. | 

### <a name="example"></a>Örnek
Aşağıdaki WCF hizmet sağlayıcısı yapılandırmasını UsernameToken kullanır: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
Sertifika veya Windows clientCredentialType ayarlayın. 

## <a id="security"></a>WCF güvenlik modu etkin değil

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Generic, .NET Framework 3 |
| **Öznitelikler**              | Güvenlik modu - taşıma, güvenlik modu - ileti |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference), [kod Magazine WCF güvenlik temel ilkeleri](https://www.codemag.com/article/0611051) |
| **Adımları** | Hiçbir taşıma veya ileti güvenlik tanımlandı. İletileri taşıma olmadan veya güvenlik bütünlüğünü veya iletilerin gizliliğini garanti edemez ileti iletme uygulamalar. WCF güvenlik bağlama None olarak ayarlandığında, hem aktarım hem de ileti güvenlik devre dışı bırakıldı. |

### <a name="example"></a>Örnek
Aşağıdaki yapılandırmayı güvenlik modu None olarak ayarlar. 
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
Tüm hizmet bağlamaları arasında güvenlik modu beş olası güvenlik modlarını şunlardır: 
* Yok. Güvenlik devre dışı bırakır. 
* Taşıma. Kullanır, karşılıklı kimlik doğrulaması ve ileti koruma için güvenlik taşıma. 
* İleti. İleti güvenliği için karşılıklı kimlik doğrulaması ve ileti koruması kullanır. 
* Her ikisi de. Taşıma ve ileti düzeyi güvenlik (yalnızca MSMQ bu destekler) için ayarları sağlamanıza olanak tanır. 
* TransportWithMessageCredential. Kimlik bilgileri iletisi ve ileti koruma ile geçirilir ve sunucu kimlik doğrulaması, Aktarım katmanı tarafından sağlanır. 
* TransportCredentialOnly. İstemci kimlik bilgileri ile Aktarım katmanı geçirilir ve ileti koruma uygulanır. Aktarım iletisi güvenlik, bütünlük ve iletilerin gizliliğini korumak için kullanın. Aşağıdaki yapılandırmayı aktarım güvenliği ile ileti kimlik bilgilerini kullanmak için hizmet söyler.
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
