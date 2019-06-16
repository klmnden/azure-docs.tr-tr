---
title: İletişim güvenliği - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
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
ms.openlocfilehash: 8534f30c17208e77adfa47ea41506a3a61d3548d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62121332"
---
# <a name="security-frame-communication-security--mitigations"></a>Güvenlik çerçevesi: İletişim güvenliği | Risk azaltma işlemleri 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Azure Event Hub** | <ul><li>[Olay hub'ına SSL/TLS kullanarak güvenli iletişim](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Hizmet hesabının ayrıcalıkları denetleyin ve özel Services veya ASP.NET sayfaları CRM'ın güvenlik saygı denetleyin](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Veri Yönetimi ağ geçidi, şirket içi SQL Server için Azure Data Factory bağlanırken kullanın](#sqlserver-factory)</li></ul> |
| **Kimlik sunucusu** | <ul><li>[Kimlik sunucusu tüm trafik HTTPS bağlantısı üzerinden olduğundan emin olun](#identity-https)</li></ul> |
| **Web uygulaması** | <ul><li>[X.509 doğrulayın SSL, TLS ve DTLS bağlantıların kimliğini doğrulamak için kullanılan sertifikalar](#x509-ssltls)</li><li>[Azure App Service'te özel etki alanı için SSL sertifikası yapılandırma](#ssl-appservice)</li><li>[Tüm trafik HTTPS bağlantısı üzerinden Azure App Service'e zorla](#appservice-https)</li><li>[HTTP katı aktarım güvenliği (HSTS) etkinleştir](#http-hsts)</li></ul> |
| **Veritabanı** | <ul><li>[SQL server bağlantı şifreleme ve sertifika doğrulama emin olun](#sqlserver-validation)</li><li>[SQL sunucusuna şifreli iletişim zorla](#encrypted-sqlserver)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure depolama ile iletişim HTTPS üzerinden olduğundan emin olun](#comm-storage)</li><li>[HTTPS etkinleştirilirse blob indirdikten sonra MD5 karma doğrulama](#md5-https)</li><li>[Azure dosya paylaşımları için aktarım sırasında veri şifrelemesi emin olmak için SMB 3.0 uyumlu bir istemci kullanın](#smb-shares)</li></ul> |
| **Mobil istemci** | <ul><li>[Uygulama sertifikası sabitleme](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[Taşıma kanalının güvenliğini sağlayın - HTTPS'yi etkinleştirme](#https-transport)</li><li>[WCF: İleti güvenliği koruma düzeyi EncryptAndSign olarak ayarlayın.](#message-protection)</li><li>[WCF: WCF hizmetinizi çalıştırmak için en az ayrıcalıklı bir hesap kullanın](#least-account-wcf)</li></ul> |
| **Web API** | <ul><li>[Tüm trafik HTTPS bağlantısı üzerinden Web API'leri için zorla](#webapi-https)</li></ul> |
| **Redis için Azure Önbelleği** | <ul><li>[SSL üzerinden Azure önbelleği için Redis iletişimi olduğundan emin olun](#redis-ssl)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[Alan ağ geçidi ile iletişim cihaz güvenliğini sağlama](#device-field)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Cihaz bulut ağ geçidi iletişim SSL/TLS kullanarak güvenli hale getirme](#device-cloud)</li></ul> |

## <a id="comm-ssltls"></a>Olay hub'ına SSL/TLS kullanarak güvenli iletişim

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Olay Hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Event Hubs kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | Olay hub'ına SSL/TLS kullanarak AMQP veya HTTP bağlantıları güvenli hale getirme |

## <a id="priv-aspnet"></a>Hizmet hesabının ayrıcalıkları denetleyin ve özel Services veya ASP.NET sayfaları CRM'ın güvenlik saygı denetleyin

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Hizmet hesabının ayrıcalıkları denetleyin ve özel Services veya ASP.NET sayfaları CRM'ın güvenlik saygı denetleyin |

## <a id="sqlserver-factory"></a>Veri Yönetimi ağ geçidi, şirket içi SQL Server için Azure Data Factory bağlanırken kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Data Factory | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Bağlantılı hizmet türleri - Azure ve şirket içi |
| **Başvuruları**              |[Şirket içi ve Azure Data Factory arasında veri taşıma](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway), [veri yönetimi ağ geçidi](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) |
| **Adımları** | <p>Veri Yönetimi ağ geçidi (DMG) aracı, corpnet veya bir güvenlik duvarıyla korunan veri kaynaklarına bağlanmak için gereklidir.</p><ol><li>Makineyi kilitleme DMG aracı yalıtır ve zarar ya da veri kaynak makinede Gözetimi yapıyor programların engeller. (Örn.) en son güncelleştirmeler yüklü olmalıdır, etkinleştirme en düşük gerekli bağlantı noktaları, sağlama, denetlenen hesapları denetimi etkinse, disk şifreleme etkin vs.)</li><li>Veri ağ geçidi anahtarı sık aralıklarla veya her DMG hizmet hesabı parolası yeniler döndürülmesi gerekir</li><li>Bağlantı hizmeti aracılığıyla veri transits şifrelenmelidir</li></ol> |

## <a id="identity-https"></a>Kimlik sunucusu tüm trafik HTTPS bağlantısı üzerinden olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [IdentityServer3 - anahtarları, imzalar ve şifreleme](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3 - dağıtım](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Adımları** | Varsayılan olarak, HTTPS üzerinden gelen tüm gelen bağlantıları IdentityServer gerektirir. IdentityServer ile iletişimi yalnızca güvenli aktarım üzerinden yapılır kesinlikle zorunludur. Bu gereksinim burada gevşetilebilir SSL yük boşaltma gibi belirli dağıtım senaryosu vardır. Daha fazla bilgi için başvurularda kimlik sunucusu dağıtım sayfasına bakın. |

## <a id="x509-ssltls"></a>X.509 doğrulayın SSL, TLS ve DTLS bağlantıların kimliğini doğrulamak için kullanılan sertifikalar

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>SSL, TLS ve DTLS kullanan uygulamalar, tam olarak bağlandıkları varlıkları X.509 sertifikaları doğrulamanız gerekir. Bu, doğrulama için sertifikaların içerir:</p><ul><li>Etki alanı adı</li><li>Geçerlilik tarihlerini (hem başlangıç ve sona erme tarihleri)</li><li>İptal durumu</li><li>Kullanım (örneğin, sunucu kimlik doğrulaması için sunucular, istemciler için istemci kimlik doğrulaması)</li><li>Zincir güvenir. Platform tarafından güvenilen veya açıkça yönetici tarafından yapılandırılan bir kök sertifika yetkilisine (CA) sertifikaları zincirine bağlı olmalıdır</li><li>Sertifikanın ortak anahtarı anahtar uzunluğunu olmalıdır > 2048 bit</li><li>Karma algoritması SHA256 olmalıdır ve üzeri |

## <a id="ssl-appservice"></a>Azure App Service'te özel etki alanı için SSL sertifikası yapılandırma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - Azure |
| **Başvuruları**              | [Azure App Service'te bir uygulama için HTTPS'yi etkinleştirme](../app-service/app-service-web-tutorial-custom-ssl.md) |
| **Adımları** | Varsayılan olarak, Azure zaten HTTPS için joker karakter sertifikası ile her uygulama için sağlar *. azurewebsites.net etki alanında. Ancak, tüm joker karakter etki alanları gibi kendi sertifika ile özel bir etki alanı kullanmak kadar güvenli değildir [bakın](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/). Dağıtılan uygulama aracılığıyla erişilen özel etki alanı için SSL etkinleştirmek için önerilir|

## <a id="appservice-https"></a>Tüm trafik HTTPS bağlantısı üzerinden Azure App Service'e zorla

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - Azure |
| **Başvuruları**              | [Azure App Service'te HTTPS'yi zorunlu kılma](../app-service/app-service-web-tutorial-custom-ssl.md#enforce-https) |
| **Adımları** | <p>Azure, HTTPS etki alanının joker nitelikli bir sertifikası ile Azure uygulama hizmetleri için zaten sağlar ancak *. azurewebsites.net, onu zorla HTTPS. Ziyaretçiler uygulamanın güvenliği tehlikeye atabilir, HTTP kullanarak uygulama erişmeye devam ve bu nedenle açıkça yürütülebilmesi HTTPS sahiptir. ASP.NET MVC uygulamaları kullanması gereken [RequireHttps filtre](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) HTTPS üzerinden yeniden gönderilmesini güvenli olmayan bir HTTP isteği zorlar.</p><p>Alternatif olarak, Azure App Service ile birlikte URL yeniden yazma modülü HTTPS zorlama için kullanılabilir. URL yeniden yazma modülü, geliştiricilerin istekleri uygulamanıza edilmeden önce gelen isteklere uygulanan kuralları tanımlamak olanak tanır. URL yeniden yazma kuralları, uygulama kök dizininde depolanmış bir web.config dosyasında tanımlanan</p>|

### <a name="example"></a>Örnek
Aşağıdaki örnek, HTTPS kullanmak üzere tüm gelen trafiği zorlayan bir temel URL yeniden yazma kuralı içerir.
```XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```
Bu kural, bir HTTP durum kodu 301 (kalıcı yeniden yönlendirme) döndürerek çalışır HTTP kullanarak bir sayfa istediğinde kullanıcı. 301 istek istenen ziyaretçi aynı URL'ye yeniden yönlendirir, ancak isteğin HTTP bölümü HTTPS ile değiştirir. Örneğin, HTTP://contoso.com yönlendirileceği HTTPS://contoso.com. 

## <a id="http-hsts"></a>HTTP katı aktarım güvenliği (HSTS) etkinleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [OWASP HTTP katı aktarım güvenliği kural sayfası](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Adımları** | <p>HTTP katı taşıma güvenliği (HSTS) bir özel yanıt üst bilgisi kullanarak bir web uygulaması tarafından belirtilen bir güvenlik katılımı geliştirmedir. Desteklenen bir tarayıcı bu üst bilgi aldıktan sonra bu tarayıcı her türlü iletişimi, HTTP üzerinden belirtilen etki alanına gönderilmesini engeller ve bunun yerine, tüm iletişimler HTTPS üzerinden gönderir. Ayrıca, HTTPS tıklayarak tarayıcılarında istekleri engeller.</p><p>HSTS uygulamak için aşağıdaki yanıt üst bilgisi için bir Web sitesi, genel olarak, kod veya yapılandırma yapılandırılması gerekir. Strict-aktarma-Security:, max-age = 300; Aşağıdaki tehditleri includeSubDomains HSTS ele alır:</p><ul><li>Kullanıcı işaretleri veya el ile türleri https://example.com ve bir adam-de-adam saldırgan tabidir: HSTS HTTP isteklerini otomatik olarak hedef etki alanı için HTTPS için yönlendiren</li><li>Yalnızca HTTPS yanlışlıkla olmaya yönelik bir web uygulaması, HTTP bağlantılarını içerir veya HTTP üzerinden içerik sunar: HSTS HTTP isteklerini otomatik olarak hedef etki alanı için HTTPS için yönlendiren</li><li>ADAM-de-adam saldırganın victim kullanıcıdan geçersiz bir sertifika kullanarak trafiği ıntercept dener ve kullanıcı hatalı sertifikayı kabul planlamaktadır: HSTS sertifika geçersiz ileti geçersiz kılmak bir kullanıcı izin vermiyor</li></ul>|

## <a id="sqlserver-validation"></a>SQL server bağlantı şifreleme ve sertifika doğrulama emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure  |
| **Öznitelikler**              | SQL sürümü - V12 |
| **Başvuruları**              | [En iyi yöntemler üzerinde yazma, SQL veritabanı için bağlantı dizelerini güvenli](https://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Adımları** | <p>Her zaman Güvenli Yuva Katmanı (SSL) kullanarak SQL veritabanı ve bir istemci uygulama arasındaki tüm iletişimler şifrelenir. SQL veritabanı, şifrelenmemiş bağlantılarını desteklemiyor. Uygulama kodu veya araçları ile sertifikalarını doğrulamak için açıkça şifreli bir bağlantı isteği ve sunucu sertifikaları güveniyor musunuz. Uygulama kodu veya araçları şifreli bir bağlantı isteme, bunlar şifrelenmiş bağlantılar hala alırsınız</p><p>Ancak, sunucu sertifikaları doğrulamamasına ve bu nedenle "ortadaki adam" saldırılarına açık olacaktır. ADO.NET uygulama kodu ile sertifikalarını doğrulamak için ayarlandı. `Encrypt=True` ve `TrustServerCertificate=False` veritabanı bağlantı dizesi içinde. SQL Server Management Studio aracılığıyla sertifika doğrulamak için Bağlan sunucusu iletişim kutusunu açın. Bağlantı Özellikleri sekmesinde şifrele bağlantısı</p>|

## <a id="encrypted-sqlserver"></a>SQL sunucusuna şifreli iletişim zorla

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Şirket içi |
| **Öznitelikler**              | SQL sürümü - MsSQL2016, SQL sürüm - MsSQL2012, SQL sürümü - MsSQL2014 |
| **Başvuruları**              | [Şifrelenmiş veritabanı altyapısı bağlantılarını etkinleştirme](https://msdn.microsoft.com/library/ms191192)  |
| **Adımları** | SSL'yi etkinleştirme şifreleme SQL Server örneklerini ve uygulamalar arasında ağ üzerinden aktarılan verilerin güvenliğini artırır. |

## <a id="comm-storage"></a>Azure depolama ile iletişim HTTPS üzerinden olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Azure depolama aktarım düzeyinde şifreleme, HTTPS kullanarak:](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Adımları** | Azure depolama veri aktarım sırasında güvenliğini sağlamak için REST API'lerini çağırma veya erişme depolama nesneleri HTTPS protokolünü her zaman kullanın. Ayrıca, paylaşılan erişim Azure depolama nesnelere erişim devretmek için kullanılabilecek, imzaları, herkes SAS belirteçlerini bağlantıları dışarı gönderme kullanacağını sağlama paylaşılan erişim imzaları kullanırken yalnızca HTTPS protokolünü kullanılabileceğini belirtmek için bir seçenek içerir doğru protokolü.|

## <a id="md5-https"></a>HTTPS etkinleştirilirse blob indirdikten sonra MD5 karma doğrulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | StorageType - Blob |
| **Başvuruları**              | [Windows Azure Blob MD5 genel bakış](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Adımları** | <p>Windows Azure Blob hizmetinin hem Aktarım katmanı ve uygulama veri bütünlüğünü sağlamak için mekanizmaları sağlar. HTTP yerine HTTPS ve, kullanmanız gereken herhangi bir nedenle blok blobları ile çalışıyorsanız, MD5 denetimi bloblarını aktarılan bütünlüğünü doğrulamak amacıyla kullanabilirsiniz</p><p>Bu ağ/aktarım katmanı hataları koruması olan ancak mutlaka Ara saldırıları ile yardımcı olur. Aktarım düzeyi güvenlik sağlayan HTTPS kullanırsanız sonra MD5 denetimi yedekli ve gereksiz kullanmaktır.</p>|

## <a id="smb-shares"></a>Azure dosya paylaşımları için aktarım sırasında veri şifrelemesi emin olmak için SMB 3.0 uyumlu bir istemci kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil istemci | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | StorageType - dosya |
| **Başvuruları**              | [Azure dosya depolama](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [Windows istemcileri için Azure dosya depolama SMB desteği](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Adımları** | Azure dosya depolama REST API kullanırken, HTTPS'yi destekleyen ancak daha sık bir SMB dosya paylaşımı kullanılan bir sanal makineye bağlı değildir. Azure'da aynı bölgede bağlantıları yalnızca izin verilir, böylece SMB 2.1 şifrelemeyi desteklemez. Ancak, SMB 3.0 şifrelemesini destekler ve Windows Server 2012 R2 ile kullanılabilir, Windows 8, Windows 8.1 ve Windows 10, bölgeler arası izin vererek erişmek ve masaüstünde erişim bile. |

## <a id="cert-pinning"></a>Uygulama sertifikası sabitleme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, Windows Phone |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Sertifika ve ortak anahtar sabitleme](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Adımları** | <p>Sertifika sabitleme adam-de-adam (MITM) saldırılarına karşı felaketlerden koruyor. Sabitleme olan bir konak, beklenen X509 ile ilişkilendirme işlemi sertifika veya ortak anahtar. Bir sertifika veya ortak anahtar bilinen veya ana görülen sonra ortak anahtar ve sertifika ilişkili veya 'konağa sabitlenmiş'. </p><p>Bu nedenle, saldırganın çalıştığında SSL MITM saldırı yapmak, SSL el sıkışması sırasında saldırganın sunucu anahtarı sabitlenmiş sertifika anahtarından farklı olacaktır ve istek atılacak, böylece MITM sertifika önleme sabitleme tarafından sağlanabilir. ServicePointManager'ın uygulama `ServerCertificateValidationCallback` temsilci.</p>|

### <a name="example"></a>Örnek
```csharp
using System;
using System.Net;
using System.Net.Security;
using System.Security.Cryptography;

namespace CertificatePinningExample
{
    class CertificatePinningExample
    {
        /* Note: In this example, we're hardcoding a the certificate's public key and algorithm for 
           demonstration purposes. In a real-world application, this should be stored in a secure
           configuration area that can be updated as needed. */

        private static readonly string PINNED_ALGORITHM = "RSA";

        private static readonly string PINNED_PUBLIC_KEY = "3082010A0282010100B0E75B7CBE56D31658EF79B3A1" +
            "294D506A88DFCDD603F6EF15E7F5BCBDF32291EC50B2B82BA158E905FE6A83EE044A48258B07FAC3D6356AF09B2" +
            "3EDAB15D00507B70DB08DB9A20C7D1201417B3071A346D663A241061C151B6EC5B5B4ECCCDCDBEA24F051962809" +
            "FEC499BF2D093C06E3BDA7D0BB83CDC1C2C6660B8ECB2EA30A685ADE2DC83C88314010FFC7F4F0F895EDDBE5C02" +
            "ABF78E50B708E0A0EB984A9AA536BCE61A0C31DB95425C6FEE5A564B158EE7C4F0693C439AE010EF83CA8155750" +
            "09B17537C29F86071E5DD8CA50EBD8A409494F479B07574D83EDCE6F68A8F7D40447471D05BC3F5EAD7862FA748" +
            "EA3C92A60A128344B1CEF7A0B0D94E50203010001";


        public static void Main(string[] args)
        {
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("https://azure.microsoft.com");
            request.ServerCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) =>
            {
                if (certificate == null || sslPolicyErrors != SslPolicyErrors.None)
                {
                    // Error getting certificate or the certificate failed basic validation
                    return false;
                }

                var targetKeyAlgorithm = new Oid(certificate.GetKeyAlgorithm()).FriendlyName;
                var targetPublicKey = certificate.GetPublicKeyString();
                
                if (targetKeyAlgorithm == PINNED_ALGORITHM &&
                    targetPublicKey == PINNED_PUBLIC_KEY)
                {
                    // Success, the certificate matches the pinned value.
                    return true;
                }
                // Reject, either the key or the algorithm does not match the expected value.
                return false;
            };

            try
            {
                var response = (HttpWebResponse)request.GetResponse();
                Console.WriteLine($"Success, HTTP status code: {response.StatusCode}");
            }
            catch(Exception ex)
            {
                Console.WriteLine($"Failure, {ex.Message}");
            }
            Console.WriteLine("Press any key to end.");
            Console.ReadKey();
        }
    }
}
```

## <a id="https-transport"></a>Taşıma kanalının güvenliğini sağlayın - HTTPS'yi etkinleştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.semantic.dotnet.wcf_misconfiguration_transport_security_enabled) |
| **Adımları** | Uygulama yapılandırması, hassas bilgiler tüm erişim HTTPS kullanılır emin olmalısınız.<ul><li>**AÇIKLAMA:** Bir uygulama hassas bilgileri işler ve ileti düzeyinde şifreleme kullanmaz, ardından bunu yalnızca bir şifrelenmiş taşıma kanalı üzerinden iletişim kurmasına izin verilmelidir.</li><li>**ÖNERİLER:** HTTP taşıma devre dışı emin olun ve bunun yerine HTTPS taşımasını etkinleştirme. Örneğin, `<httpTransport/>` ile `<httpsTransport/>` etiketi. Uygulamanın yalnızca güvenli bir kanal üzerinden erişilebilir güvence altına almak için bir ağ yapılandırması (Güvenlik Duvarı) güvenmeyin. Bir felsefi açısından bakıldığında, uygulama ağ güvenliği için bağlı olmamalıdır.</li></ul><p>Değiştikçe bir pratik açısından bakıldığında, kişilerin sorumlu ağ güvenliğini sağlamak için her zaman uygulamanın güvenlik gereksinimlerini izleme (Dnt).</p>|

## <a id="message-protection"></a>WCF: İleti güvenliği koruma düzeyi EncryptAndSign olarak ayarlayın.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Adımları** | <ul><li>**AÇIKLAMA:** Ne zaman koruma düzeyi "none" ayarlanmış iletisi koruma devre dışı bırakır. Gizliliği ve bütünlüğü ayarını uygun düzeyde gerçekleştirilir.</li><li>**ÖNERİLER:**<ul><li>zaman `Mode=None` -ileti koruma devre dışı bırakır</li><li>zaman `Mode=Sign` -işaretleri ancak iletiyi şifrelemek değildir; veri bütünlüğü önemli olduğu durumlarda kullanılmalıdır</li><li>zaman `Mode=EncryptAndSign` -imzalar ve ileti şifreler</li></ul></li></ul><p>Şifreleme devre dışı kapatarak ve endişeleriniz olmadan bilgilerinin gizliliğini, bütünlüğünü doğrulamak, yalnızca ihtiyacınız olduğunda, yalnızca ileti imzalama göz önünde bulundurun. Bu işlemleri için kullanışlı olabilir veya hizmet sözleşmelerinde özgün göndereni doğrulamanız gereken ancak hiçbir hassas veriler iletilir. Koruma düzeyi azaltma, ileti tüm kişisel bilgileri (PII) içermiyor dikkatli olun.</p>|

### <a name="example"></a>Örnek
Yapılandırma hizmeti ve işlem yalnızca iletiyi imzalamak için aşağıdaki örneklerde gösterilir. Hizmet sözleşmesi örneği `ProtectionLevel.Sign`: Hizmet sözleşmesini düzeyinde ProtectionLevel.Sign kullanmaya bir örnek verilmiştir: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Örnek
İşlem anlaşması örneği `ProtectionLevel.Sign` (ayrıntılı denetim için): Kullanımının bir örneği verilmiştir `ProtectionLevel.Sign` OperationContract düzeyinde:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a id="least-account-wcf"></a>WCF: WCF hizmetinizi çalıştırmak için en az ayrıcalıklı bir hesap kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Adımları** | <ul><li>**AÇIKLAMA:** WCF Hizmetleri Yöneticisi veya yüksek ayrıcalıklı hesap altında çalışmaz. Hizmetleri güvenliğinin aşılması durumunda yüksek etkisi neden olur.</li><li>**ÖNERİLER:** Bu, uygulamanızın saldırı yüzeyini azaltmak ve, gerçekleşirse durumdaki potansiyel hasarı azaltmak için WCF Hizmeti barındırma için en az ayrıcalıklı bir hesap kullanın. Hizmet hesabı MSMQ, olay günlüğü, performans sayaçları ve dosya sistemi gibi altyapı kaynaklarını ek erişim haklarını gerektiriyorsa, WCF hizmeti başarıyla çalıştırabilmeniz için bu kaynakları için uygun izinleri verilmelidir.</li></ul><p>Hizmetinizin adına özgün arayan belirli kaynaklara erişmesi gerekiyorsa, kimliğe bürünme ve temsilci çağıranının kimliğini bir aşağı akış yetkilendirme denetimi için akış için kullanın. Bir geliştirme senaryosunda ayrıcalıkları daha az olan özel bir yerleşik hesap yerel ağ hizmeti hesabını kullanın. Bir üretim senaryosunda, en az ayrıcalıklı özel etki alanı hizmet hesabı oluşturun.</p>|

## <a id="webapi-https"></a>Tüm trafik HTTPS bağlantısı üzerinden Web API'leri için zorla

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Bir Web API denetleyicisi, SSL'yi zorunlu tutma](https://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Adımları** | Bir uygulamanın bir HTTPS hem bir HTTP bağlaması varsa, istemciler siteye erişmek için HTTP yine de kullanabilirsiniz. Bunu önlemek için korumalı API'leri için istekleri her zaman HTTPS üzerinden olmasını sağlamak için bir eyleme eylem filtresi kullanın.|

### <a name="example"></a>Örnek 
Aşağıdaki kod, SSL için denetleyen bir Web API kimlik doğrulaması filtre gösterir: 
```csharp
public class RequireHttpsAttribute : AuthorizationFilterAttribute
{
    public override void OnAuthorization(HttpActionContext actionContext)
    {
        if (actionContext.Request.RequestUri.Scheme != Uri.UriSchemeHttps)
        {
            actionContext.Response = new HttpResponseMessage(System.Net.HttpStatusCode.Forbidden)
            {
                ReasonPhrase = "HTTPS Required"
            };
        }
        else
        {
            base.OnAuthorization(actionContext);
        }
    }
}
```
Bu filtre, SSL iste herhangi bir Web API eylemlerini ekleyin: 
```csharp
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a id="redis-ssl"></a>SSL üzerinden Azure önbelleği için Redis iletişimi olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Redis için Azure Önbelleği | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Azure Redis SSL desteği](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Adımları** | Redis sunucu SSL kullanıma hazır desteklemez, ancak Azure önbelleği için Redis yapar. Azure önbelleği için Redis için bağlama ve SSL, StackExchange.Redis gibi istemcinizi destekliyorsa SSL kullanmanız gerekir. Varsayılan olarak SSL olmayan bağlantı noktası yeni Azure önbelleği için Redis örneği için devre dışıdır. Redis istemcileri için SSL desteği üzerinde bir bağımlılık olmadıkça güvenli varsayılan değiştirilmediğinden emin olun. |

Redis güvenilir ortamlar içinde güvenilir istemcileri tarafından erişilecek tasarlanmıştır unutmayın. Bu genellikle, Redis örneği doğrudan Internet'e ya da genel olarak, güvenilmeyen istemciler doğrudan Redis TCP bağlantı noktası veya UNIX yuva erişebileceğiniz bir ortama kullanıma sunmak için iyi bir fikir olmadığını anlamına gelir. 

## <a id="device-field"></a>Alan ağ geçidi ile iletişim cihaz güvenliğini sağlama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | IP tabanlı cihazlar için kullanıcının iletişim protokolü Aktarımdaki verileri korumak için SSL/TLS kanalda genellikle saklanmasını. SSL/TLS desteklemeyen diğer protokoller için taşıma veya ileti katmanında güvenlik sağlamaya güvenli protokol sürümleri varsa araştırın. |

## <a id="device-cloud"></a>Cihaz bulut ağ geçidi iletişim SSL/TLS kullanarak güvenli hale getirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [İletişim protokolü seçme](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Adımları** | HTTP/AMQP veya MQTT protokolleri kullanarak SSL/TLS güvenli hale getirin. |
