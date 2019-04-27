---
title: Özel Durum Yönetimi - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
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
ms.openlocfilehash: 7d881454eb857080f1178f228a1f7bec36cae178
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610706"
---
# <a name="security-frame-exception-management--mitigations"></a>Güvenlik çerçevesi: Özel durum yönetimi | Risk azaltma işlemleri 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF iş yapılandırma dosyasında serviceDebug düğüm içermiyor](#servicedebug)</li><li>[WCF iş yapılandırma dosyasında serviceMetadata düğüm içermiyor](#servicemetadata)</li></ul> |
| **Web API** | <ul><li>[Uygun özel durum işleme ASP.NET Web API'de yapıldığından emin olun](#exception)</li></ul> |
| **Web uygulaması** | <ul><li>[Hata iletilerinde güvenlik ayrıntılarını gösterme](#messages)</li><li>[Varsayılan hata sayfa işleme uygulama](#default)</li><li>[Dağıtım yöntemi sürümünden perakende sürümüne IIS'de ayarlayın](#deployment)</li><li>[Özel durumlar güvenli bir şekilde başarısız olması](#fail)</li></ul> |

## <a id="servicedebug"></a>WCF iş yapılandırma dosyasında serviceDebug düğüm içermiyor

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_debug_information) |
| **Adımları** | Windows Communication Framework (WCF) Hizmetleri, hata ayıklama bilgilerini kullanıma sunmak için yapılandırılabilir. Hata ayıklama bilgileri, üretim ortamlarında kullanılmamalıdır. `<serviceDebug>` Etiketi, hata ayıklama bilgilerini özelliği bir WCF hizmeti için etkin olup olmadığını tanımlar. İstemcilere özniteliği IncludeExceptionDetailInFaults uygulamadan true, özel durum bilgilerini ayarlanmışsa döndürülmez. Saldırganlar, çerçeve, veritabanı veya uygulama tarafından kullanılan diğer kaynaklar hedeflenen saldırılar yerleştirmek üzere çıkış hata ayıklamasından elde ek bilgi yararlanabilir. |

### <a name="example"></a>Örnek
Şu yapılandırma dosyasını içeren `<serviceDebug>` etiketi: 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
Hizmetinde hata ayıklama bilgilerini devre dışı bırakın. Bu kaldırarak gerçekleştirilebilir `<serviceDebug>` uygulamanızın yapılandırma dosyasına etiketi. 

## <a id="servicemetadata"></a>WCF iş yapılandırma dosyasında serviceMetadata düğüm içermiyor

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Genel, NET Framework 3 |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_service_enumeration) |
| **Adımları** | Genel olarak bir hizmet ile ilgili bilgileri gösterme nasıl hizmet yararlanabilir değerli Öngörüler sayesinde saldırganlar sağlayabilir. `<serviceMetadata>` Etiketi meta veri yayımlama özelliğini etkinleştirir. Hizmet meta verileri genel olarak erişilebilir olmaması gereken hassas bilgiler içerebilir. En azından, yalnızca güvenilen kullanıcıların meta verilerine erişmek ve gereksiz bilgi sunulmaz emin olmak izin verir. Üstelik, tamamen meta verileri yayımlama özelliğini devre dışı. Güvenli bir WCF yapılandırma olmayan öğeler `<serviceMetadata>` etiketi. |

## <a id="exception"></a>Uygun özel durum işleme ASP.NET Web API'de yapıldığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC 5, MVC 6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Özel durum işleme ASP.NET Web API](https://www.asp.net/web-api/overview/error-handling/exception-handling), [Model doğrulama ASP.NET Web API](https://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Adımları** | Varsayılan olarak, ASP.NET Web API en Yakalanmayan Özel durum kodu ile bir HTTP yanıtı çevrilir `500, Internal Server Error`|

### <a name="example"></a>Örnek
API tarafından döndürülen durum kodunu denetlemek için `HttpResponseException` aşağıda gösterildiği gibi kullanılabilir: 
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
Daha fazla denetimin özel durum yanıt `HttpResponseMessage` sınıfı, aşağıda gösterildiği gibi kullanılabilir: 
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
Türü olmayan işlenmeyen özel durumları yakalamak için `HttpResponseException`, özel durum filtreleri kullanılabilir. Özel durum filtreleri uygulamak `System.Web.Http.Filters.IExceptionFilter` arabirimi. Türetilmesi için bir özel durum filtresi yazma en kolay yolu olan `System.Web.Http.Filters.ExceptionFilterAttribute` sınıfı ve OnException yöntemini geçersiz kılın. 

### <a name="example"></a>Örnek
İşte dönüştüren bir filtre `NotImplementedException` HTTP durum kodu özel durumları `501, Not Implemented`: 
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

Bir Web API özel durum filtre kaydetmek için birkaç yol vardır:
- Eylem tarafından
- Denetleyici tarafından
- Genel olarak

### <a name="example"></a>Örnek
Belirli bir eylem için filtre uygulamak için filtre eylemi için bir özniteliği olarak ekleyin: 
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
Filtre tüm eylemleri uygulamak için bir `controller`, öznitelik olarak Filtre Ekle `controller` sınıfı: 

```csharp
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a>Örnek
Filtre için tüm Web APİ'si denetleyicilerinin genel olarak uygulamak için filtre bir örneğini ekleme `GlobalConfiguration.Configuration.Filters` koleksiyonu. Özel durum filtreleri bu koleksiyondaki herhangi bir Web API denetleyici eylemi için geçerlidir. 
```csharp
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Örnek
Model doğrulama için bir model durumu aşağıda gösterildiği gibi CreateErrorResponse yöntemi geçirilebilir: 
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

Onay olağanüstü işleme hakkında ek ayrıntılar için başvurular bölümündeki bağlantıları ve ASP.NET Web API'de model doğrulama 

## <a id="messages"></a>Hata iletilerinde güvenlik ayrıntılarını gösterme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Genel hata iletileri doğrudan kullanıcıya hassas uygulama verileri dahil olmak üzere sağlanır. Hassas verilerin örnekleri şunlardır:</p><ul><li>Sunucu adları</li><li>Bağlantı dizeleri</li><li>Kullanıcı adları</li><li>Parolalar</li><li>SQL yordamları</li><li>Dinamik SQL hatalarının ayrıntıları</li><li>Yığın izlemesi ve kod satırları</li><li>Değişkenler bellekte</li><li>Sürücü ve klasör konumları</li><li>Uygulama yükleme noktaları</li><li>Ana bilgisayar yapılandırma ayarları</li><li>Diğer iç uygulama ayrıntıları</li></ul><p>Bir uygulamadaki tüm hataları yakalama ve genel hata iletileri sağlama, hem de IIS içinde özel hatalar'ı etkinleştirme, bilgilerin açıklanması engellemeye yardımcı olur. SQL Server veritabanı ve .NET, diğer hata mimarileri, işleme arasında işleme özel durum, özellikle ayrıntılı ve uygulamanızın profilini oluşturmanız, kötü niyetli bir kullanıcı için son derece yararlıdır. Doğrudan .NET özel durum sınıfından türetilen bir sınıf içeriğini görüntüleyebilir ve böylece beklenmeyen bir özel durum doğrudan kullanıcıya yanlışlıkla yükseltilmiş olmayan uygun özel durum işleme sahip olduğundan emin olun.</p><ul><li>Genel hata iletileri doğrudan doğrudan özel durum/hata iletisinde bulunan koyma belirli Ayrıntılar soyut kullanıcıya sağlayın</li><li>.NET özel durum sınıfı içeriğini doğrudan kullanıcıya gösterme</li><li>Tüm hata iletilerini yakalayabilir ve uygunsa, kullanıcıyı uygulama istemciye gönderilen genel bir hata iletisi yoluyla bildirin</li><li>Özel durum sınıfı içeriğini kullanıcıya, özellikle dönüş değeri açığa çıkarmayın `.ToString()`, ya da ileti veya StackTrace özelliklerin değerlerini. Güvenli bir şekilde bu bilgileri günlüğe kaydetmek ve kullanıcıya daha zararsız bir ileti görüntüler</li></ul>|

## <a id="default"></a>Varsayılan hata sayfa işleme uygulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ASP.NET hata sayfaları ayarlarını Düzenle iletişim kutusu](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Adımları** | <p>Bir ASP.NET uygulama başarısız olduğunda ve bir HTTP/1.x 500 İç sunucu hatası neden olan veya bir özellik yapılandırması (örneğin, istek filtreleme) bir sayfa görüntülenmesini engeller, bir hata iletisi oluşturulur. Yöneticiler uygulama kolay bir ileti istemcisi, istemciye ayrıntılı hata iletisi ya da yalnızca localhost için ayrıntılı hata iletisi görüntülenmelidir kullanılıp kullanılmayacağını seçebilirsiniz. `<customErrors>` Etiketi Web.config dosyasında üç modu vardır:</p><ul><li>**Üzerinde:** Özel hataların etkinleştirildiğini belirtir. Kullanıcılar, hiçbir defaultRedirect özniteliği belirtilmezse, genel bir hata görür. Özel hatalar, uzak istemciler ve yerel ana bilgisayarda gösterilir.</li><li>**Kapalı:** Özel hatalar devre dışı olduğunu belirtir. Uzak istemciler ve yerel ana bilgisayar için ayrıntılı ASP.NET hataları gösterilir</li><li>**RemoteOnly:** Özel hatalar yalnızca uzak istemcilere gösterildiğini ve ASP.NET hataları yerel ana bilgisayara görüntülendiğini belirtir. Varsayılan değer budur.</li></ul><p>Açık `web.config` etiket ya da olduğundan emin olun ve uygulama/site için dosya `<customErrors mode="RemoteOnly" />` veya `<customErrors mode="On" />` tanımlı.</p>|

## <a id="deployment"></a>Dağıtım yöntemi sürümünden perakende sürümüne IIS'de ayarlayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Dağıtım öğesi (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Adımları** | <p>`<deployment retail>` Anahtar üretim IIS sunucuları tarafından kullanılmak üzere tasarlanmıştır. Bu anahtar en iyi performansı ve olabildiğince az güvenlik bilgileri leakages ile izleme çıktısına ayrıntılı hata iletileri görüntülenecek özelliği devre dışı bırakma sayfasında, oluşturulacak uygulamanın özelliği devre dışı bırakarak çalışan uygulamaların yardımcı olmak için kullanılır Son kullanıcılar ve hata ayıklama anahtarı devre dışı bırakılıyor.</p><p>Çoğu zaman, anahtarlar ve geliştirici odaklı başarısız oldu gibi seçenekleri izleme istek ve hata ayıklama, etkin geliştirme sırasında etkinleştirilir. Herhangi bir üretim sunucusu üzerinde dağıtım yönteminin, perakende ayarlanması önerilir. machine.config dosyasının açın ve emin `<deployment retail="true" />` true olarak ayarlanırsa kalır.</p>|

## <a id="fail"></a>Özel durumlar güvenli bir şekilde başarısız olması

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Güvenli bir şekilde başarısız](https://www.owasp.org/index.php/Fail_securely) |
| **Adımları** | Uygulama güvenli bir şekilde başarısız olmalı. Belirli bir karar verme yapılır üzerinde temel bir Boole değeri döndüren herhangi bir yöntemi, dikkatli bir şekilde oluşturulan özel durum bloğu olmalıdır. Çok sayıda mantıksal hataları nedeniyle,'de, özel durum bloğu carelessly yazılan yayılmasını güvenlik konuları vardır.|

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
Bazı özel durum oluşursa yukarıdaki yöntemi her zaman True döndürür. Tarayıcı uyar, hatalı biçimlendirilmiş bir URL, son kullanıcının sağlıyorsa, ancak `Uri()` Oluşturucu değil, bir özel durum oluşturur ve victim geçerli, ancak hatalı biçimlendirilmiş URL'ye yönlendirilirsiniz. 
