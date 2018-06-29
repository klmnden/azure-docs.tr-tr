---
title: Özel Durum Yönetimi - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
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
ms.openlocfilehash: 3fae9390b41d12361b820e2c37601283b37bc302
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031721"
---
# <a name="security-frame-exception-management--mitigations"></a>Güvenlik çerçevesi: Özel durum yönetimi | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF - dahil etmeyin serviceDebug düğümü yapılandırma dosyasında](#servicedebug)</li><li>[WCF - dahil etmeyin serviceMetadata düğümü yapılandırma dosyasında](#servicemetadata)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API'de uygun özel durum işleme yapıldığından emin olun ](#exception)</li></ul> |
| **Web uygulaması** | <ul><li>[Hata iletileri güvenlik ayrıntıları gösterme ](#messages)</li><li>[Varsayılan hata sayfası işleme uygulama ](#default)</li><li>[IIS'de perakende için dağıtım yöntemini ayarlayın](#deployment)</li><li>[Özel durumlar güvenli bir şekilde başarısız olması](#fail)</li></ul> |

## <a id="servicedebug"></a>WCF - dahil etmeyin serviceDebug düğümü yapılandırma dosyasında

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_debug_information) |
| **Adımları** | Windows Communication Framework (WCF) hizmetlerini hata ayıklama bilgilerini kullanıma sunmak için yapılandırılabilir. Hata ayıklama bilgileri üretim ortamlarında kullanılmamalıdır. `<serviceDebug>` Etiketi hata ayıklama bilgileri özelliği için bir WCF hizmeti etkin olup olmadığını tanımlar. Öznitelik IncludeExceptionDetailInFaults uygulamadan true, özel durum bilgilerini ayarlarsanız istemcilere döndürülür. Saldırganlar framework, veritabanı veya uygulama tarafından kullanılan kaynaklar hedeflenen saldırılar yerleştirmek üzere çıkış hata ayıklama elde ek bilgi yararlanabilirsiniz. |

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma dosyası içerir `<serviceDebug>` etiketi: 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
Hizmetinde hata ayıklama bilgilerini devre dışı bırakın. Bu kaldırarak gerçekleştirilebilir `<serviceDebug>` , uygulamanızın yapılandırma dosyasından etiketi. 

## <a id="servicemetadata"></a>WCF - dahil etmeyin serviceMetadata düğümü yapılandırma dosyasında

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Genel, NET Framework 3 |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_service_enumeration) |
| **Adımları** | Genel olarak bir hizmet hakkında bilgileri gösterme da nasıl hizmet yararlanabilir içine değerli bilgiler sağlayabilir. `<serviceMetadata>` Etiketi meta veri yayımlama özelliğini etkinleştirir. Hizmet meta verileri genel olarak erişilebilir olmamalıdır hassas bilgiler içerebilir. En azından, yalnızca güvenilen kullanıcıların meta verilerine erişmek ve gereksiz bilgileri sunulmaz olun izin verin. Daha iyi bir yöntem tamamen meta verileri yayımlama özelliği devre dışı bırakın. Güvenli bir WCF yapılandırma değil içerecek `<serviceMetadata>` etiketi. |

## <a id="exception"></a>ASP.NET Web API'de uygun özel durum işleme yapıldığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC 5, MVC 6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Özel durum işleme ASP.NET Web API](http://www.asp.net/web-api/overview/error-handling/exception-handling), [Model ASP.NET Web API doğrulama](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Adımları** | Bir HTTP yanıtının durum koduyla içine çevrilen varsayılan olarak, ASP.NET Web API en Yakalanmayan Özel durumları `500, Internal Server Error`|

### <a name="example"></a>Örnek
API tarafından döndürülen durum kodu denetlemek için `HttpResponseException` aşağıda gösterildiği gibi kullanılabilir: 
```csharp
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a>Örnek
Özel durum yanıtı hakkında daha fazla denetim için `HttpResponseMessage` aşağıda gösterildiği gibi sınıfı kullanılabilir: 
```csharp
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
Türü olmayan işlenmeyen özel durumları yakalamak için `HttpResponseException`, özel durum filtreleri kullanılabilir. Özel durum filtreleri uygulamak `System.Web.Http.Filters.IExceptionFilter` arabirimi. Öğesinden türetilen için bir özel durum filtresi yazma en basit yolu olan `System.Web.Http.Filters.ExceptionFilterAttribute` sınıfı ve OnException yöntemini geçersiz kılın. 

### <a name="example"></a>Örnek
Dönüştüren bir filtre işte `NotImplementedException` özel durumlar HTTP durum kodu içine `501, Not Implemented`: 
```csharp
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

Bir Web API özel durum filtresi kaydetmek için birkaç yolu vardır:
- Eylem tarafından
- Denetleyici tarafından
- Genel

### <a name="example"></a>Örnek
Belirli bir eylem filtresi uygulamak için filtre eylemi için bir özniteliği olarak ekleyin: 
```csharp
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a>Örnek
Filtre tüm eylemleri uygulamak için bir `controller`, bir öznitelik olarak filtre eklemek `controller` sınıfı: 

```csharp
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a>Örnek
Tüm Web API denetleyicilerinin genel filtre uygulamak için filtre örneğini ekleme `GlobalConfiguration.Configuration.Filters` koleksiyonu. Özel durum filtreleri bu koleksiyondaki tüm Web API denetleyici eylemi uygular. 
```csharp
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Örnek
Model doğrulama için bir model durumu aşağıda gösterildiği gibi CreateErrorResponse yöntemine geçirilebilir: 
```csharp
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

Onay olağanüstü işleme hakkında ek bilgi için başvurular bölümündeki bağlantıları ve ASP.Net Web API model doğrulama 

## <a id="messages"></a>Hata iletileri güvenlik ayrıntıları gösterme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Genel hata iletileri, önemli uygulama verileri dahil olmak üzere olmadan doğrudan kullanıcıya sağlanır. Gizli verilerin örnekleri şunlardır:</p><ul><li>Sunucu adları</li><li>Bağlantı dizeleri</li><li>Kullanıcı adları</li><li>Parolalar</li><li>SQL yordamları</li><li>Dinamik SQL hatalarının ayrıntıları</li><li>Yığın izleme ve kod satırları</li><li>Bellekte depolanan değişkenleri</li><li>Sürücü ve klasör konumları</li><li>Uygulama yükleme noktaları</li><li>Ana bilgisayar yapılandırma ayarları</li><li>Diğer iç uygulama ayrıntıları</li></ul><p>Bir uygulamadaki tüm hataları yakalama ve genel hata iletileri sağlayarak, yanı sıra IIS içinde özel hatalar etkinleştirme bilgilerin açığa çıkmasına engellemeye yardımcı olur. SQL Server veritabanı ve .NET özel durum işleme, diğer hata mimarileri işleme arasında özellikle ayrıntılı ve uygulamanızı profil kötü niyetli bir kullanıcı için son derece kullanışlıdır. Doğrudan .NET özel durum sınıfından türetilmiş bir sınıf içeriğini görüntülemek ve böylece beklenmeyen bir özel durum yanlışlıkla doğrudan kullanıcıya yükseltilmiş değil uygun özel durum işleme sahip olduğundan emin olun.</p><ul><li>Genel hata iletileri doğrudan doğrudan özel durum/hata iletisinde bulunan koyma belirli Ayrıntılar soyut kullanıcıya sağlayın</li><li>.NET özel durum sınıfı içeriğini doğrudan kullanıcıya gösterme</li><li>Tüm hata iletilerini yakalayabilir ve uygun durumlarda uygulama istemciye gönderilen genel bir hata iletisi aracılığıyla kullanıcı bildirin.</li><li>Özel durum sınıfı içeriğini doğrudan kullanıcıya, özellikle dönüş değerini gösterme `.ToString()`, ya da ileti veya StackTrace özelliklerinin değerlerini. Güvenli bir şekilde bu bilgileri günlüğe kaydetmek ve kullanıcıya daha zararsız bir ileti görüntüler</li></ul>|

## <a id="default"></a>Varsayılan hata sayfası işleme uygulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [ASP.NET hata sayfası ayarlarını Düzenle iletişim kutusu](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Adımları** | <p>Bir ASP.NET uygulaması başarısız olur ve bir HTTP/1.x 500 İç sunucu hatası neden olan veya bir özellik yapılandırması (istek filtreleme gibi) bir sayfa görüntülenmesini engeller, bir hata iletisi oluşturulur. Yöneticiler, uygulamanın bir kolay ileti istemcisi, istemciye ayrıntılı hata iletisi veya yalnızca localhost için ayrıntılı hata iletisi görüntülenmelidir olsun veya olmasın seçebilirsiniz. <customErrors> Web.config etiketinde üç modu vardır:</p><ul><li>**Üzerinde:** özel hatalar etkin olduğunu belirtir. Hiçbir defaultRedirect özniteliği belirtilmezse, kullanıcılar genel bir hata görür. Özel hatalar uzak istemcilere ve yerel ana bilgisayara gösterilir</li><li>**Kapalı:** özel hatalar devre dışı olduğunu belirtir. Ayrıntılı ASP.NET hataları uzak istemcilere ve yerel ana bilgisayara gösterilir</li><li>**RemoteOnly:** özel hatalar yalnızca uzak istemcilere gösterilir ve ASP.NET hatalarının yerel ana bilgisayara görüntüleneceğini belirtir. Bu varsayılan değerdir</li></ul><p>Açık `web.config` uygulama/sitesi için dosya ve etiket ya da sahip olduğundan emin olun `<customErrors mode="RemoteOnly" />` veya `<customErrors mode="On" />` tanımlanmış.</p>|

## <a id="deployment"></a>IIS'de perakende için dağıtım yöntemini ayarlayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Dağıtım öğesi (ASP.NET Ayarlar Şeması)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Adımları** | <p>`<deployment retail>` Anahtar üretim IIS sunucuları tarafından kullanılmak üzere tasarlanmıştır. Bu anahtar en olası performans ve mümkün olan en küçük güvenlik bilgileri leakages İzleme çıkışı için ayrıntılı hata iletilerini görüntüleme yeteneği devre dışı bırakma sayfasında, oluşturulacak uygulamanın özelliği devre dışı bırakarak çalışan uygulamaların yardımcı olmak için kullanılır Son kullanıcılar ve hata ayıklama anahtarı devre dışı bırakma.</p><p>Çoğu zaman, anahtarlar ve geliştirici odaklı başarısız gibi seçenekler izleme istek ve hata ayıklama, etkin geliştirme sırasında etkinleştirilir. Herhangi bir üretim sunucusu üzerinde dağıtım yönteminin, perakende ayarlanması önerilir. machine.config dosyasının açın ve emin `<deployment retail="true" />` true olarak ayarlandığında kalır.</p>|

## <a id="fail"></a>Özel durumlar güvenli bir şekilde başarısız olması

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Güvenli bir şekilde başarısız](https://www.owasp.org/index.php/Fail_securely) |
| **Adımları** | Uygulama güvenli bir şekilde başarısız olması. Hangi belirli karar yapılan, temel bir Boole değeri döndürür herhangi bir yöntemini dikkatle oluşturulan özel durum bloğu olmalıdır. Çok sayıda mantıksal hataları nedeniyle,'de, ne zaman özel durum bloğu carelessly yazılmış Katlama güvenlik konuları vardır.|

### <a name="example"></a>Örnek
```csharp
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check to enable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
Bazı özel durum oluşursa yukarıdaki yöntem her zaman True döndürür. Tarayıcı dikkate alır, hatalı biçimlendirilmiş bir URL, son kullanıcı sağlıyorsa, ancak `Uri()` Oluşturucusu değil, bu bir özel durum oluşturur ve kurbanın geçerli ancak hatalı biçimlendirilmiş URL'sine gerçekleştirilecek. 