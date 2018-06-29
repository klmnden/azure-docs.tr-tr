---
title: İletişim güvenliği - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
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
ms.openlocfilehash: c361f74147862585074f3c4475209ba6eb0c1e0c
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029807"
---
# <a name="security-frame-communication-security--mitigations"></a>Güvenlik çerçevesi: İletişim güvenliği | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Azure Event hub'ı** | <ul><li>[Olay hub'ına SSL/TLS kullanan güvenli iletişim](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Hizmet hesabının ayrıcalıkları denetleyin ve özel Services veya ASP.NET sayfaları CRM'ın güvenlik saygı denetleyin](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Veri Yönetimi ağ geçidi şirket içi SQL Server için Azure Data Factory bağlanırken kullanın](#sqlserver-factory)</li></ul> |
| **Kimlik sunucusu** | <ul><li>[Kimlik sunucusu tüm trafik HTTPS bağlantısı üzerinden olduğundan emin olun](#identity-https)</li></ul> |
| **Web uygulaması** | <ul><li>[X.509 doğrulayın SSL, TLS ve DTLS bağlantılarının kimliğini doğrulamak için kullanılan sertifikaları](#x509-ssltls)</li><li>[Azure App Service'te özel etki alanı için SSL sertifikası yapılandırma](#ssl-appservice)</li><li>[Tüm trafiği HTTPS bağlantısı üzerinden Azure App Service'e zorla](#appservice-https)</li><li>[HTTP katı taşıma güvenliği (HSTS) etkinleştir](#http-hsts)</li></ul> |
| **Veritabanı** | <ul><li>[SQL server bağlantı şifreleme ve sertifika doğrulaması emin olun](#sqlserver-validation)</li><li>[SQL Server şifreli iletişim zorla](#encrypted-sqlserver)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure Storage iletişim HTTPS üzerinden olduğundan emin olun](#comm-storage)</li><li>[HTTPS etkinleştirilirse blob indirdikten sonra MD5 karma değeri doğrulanamıyor](#md5-https)</li><li>[Azure dosya paylaşımları için aktarım sırasında veri şifreleme sağlamak için SMB 3.0 uyumlu İstemcisi'ni kullanın](#smb-shares)</li></ul> |
| **Mobil istemci** | <ul><li>[Sertifika sabitleme uygulama](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[HTTPS enable - aktarım kanalının güvenliğini sağlayın.](#https-transport)</li><li>[WCF: Kümesi ileti güvenliği da EncryptAndSign için koruma düzeyi](#message-protection)</li><li>[WCF: WCF hizmeti çalıştırmak için en az ayrıcalıklı bir hesap kullanın.](#least-account-wcf)</li></ul> |
| **Web API** | <ul><li>[Tüm trafiği HTTPS bağlantısı üzerinden Web API'lerine zorla](#webapi-https)</li></ul> |
| **Azure Redis Önbelleği** | <ul><li>[Azure Redis önbelleği iletişimin SSL üzerinden olduğundan emin olun](#redis-ssl)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[Aygıt için alan ağ geçidi iletişimin güvenliğini sağlama](#device-field)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[SSL/TLS kullanılarak bulut ağ geçidi iletişim için güvenli aygıt](#device-cloud)</li></ul> |

## <a id="comm-ssltls"></a>Olay hub'ına SSL/TLS kullanan güvenli iletişim

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Olay Hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Olay hub'ları kimlik doğrulaması ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | Güvenli olay hub'ına SSL/TLS kullanarak AMQP veya HTTP bağlantıları |

## <a id="priv-aspnet"></a>Hizmet hesabının ayrıcalıkları denetleyin ve özel Services veya ASP.NET sayfaları CRM'ın güvenlik saygı denetleyin

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Hizmet hesabının ayrıcalıkları denetleyin ve özel Services veya ASP.NET sayfaları CRM'ın güvenlik saygı denetleyin |

## <a id="sqlserver-factory"></a>Veri Yönetimi ağ geçidi şirket içi SQL Server için Azure Data Factory bağlanırken kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Data Factory | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Bağlantılı hizmet - Azure ve şirket içi türleri |
| **Başvuruları**              |[ Şirket içi ve Azure Data Factory arasında veri taşıma](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway), [veri yönetimi ağ geçidi](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) |
| **Adımları** | <p>Veri Yönetimi ağ geçidi (DMG) aracı, corpnet veya bir güvenlik duvarının arkasındaysa korunan veri kaynaklarına bağlanmak için gereklidir.</p><ol><li>Makineyi kilitleme DMG aracı yalıtır ve zarar veya veri kaynak makinede Gözetimi hatalı çalışan programların önler. (Örn. en son güncelleştirmelerin yüklü olmalıdır, etkinleştirme minimum gerekli bağlantı noktaları, denetlenen hesapları sağlama, Denetim etkinse, disk şifreleme etkin vs.)</li><li>Veri ağ geçidi anahtarı sık aralıklarla veya DMG hizmet hesabı parolası yeniler her Döndürülmüş gerekir</li><li>Bağlantı hizmeti aracılığıyla veri transits şifrelenmelidir</li></ol> |

## <a id="identity-https"></a>Kimlik sunucusu tüm trafik HTTPS bağlantısı üzerinden olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [IdentityServer3 - anahtarları, imzalar ve şifreleme](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3 - dağıtım](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Adımları** | Varsayılan olarak, HTTPS üzerinden gelen tüm gelen bağlantıları IdentityServer gerektirir. IdentityServer ile iletişim yalnızca güvenli aktarım üzerinden yapılır kesinlikle zorunludur. Bu gereksinim nerede gevşetilebilir SSL boşaltma gibi belirli dağıtım senaryoları vardır. Daha fazla bilgi için başvurular içinde kimlik sunucusunu dağıtım sayfasına bakın. |

## <a id="x509-ssltls"></a>X.509 doğrulayın SSL, TLS ve DTLS bağlantılarının kimliğini doğrulamak için kullanılan sertifikaları

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>SSL, TLS ve DTLS kullanan uygulamalar, tam olarak bağlandıkları varlıkların X.509 sertifikalarını doğrulamanız gerekir. Bu doğrulama için sertifika içerir:</p><ul><li>Etki alanı adı</li><li>Geçerlilik Tarihleri (başlangıç ve sona erme tarihleri)</li><li>İptal durumu</li><li>Kullanım (örneğin, sunucu kimlik doğrulaması sunucuları, istemcileri için istemci kimlik doğrulaması için)</li><li>Güven zinciri. Platform tarafından güvenilen veya açıkça yönetici tarafından yapılandırılan bir kök sertifika yetkilisine (CA) sertifikaları zincirine bağlı olmalıdır</li><li>Sertifikanın ortak anahtarı anahtar uzunluğu olmalıdır > 2048 bit</li><li>Karma algoritma SHA256 olmalıdır ve üstü |

## <a id="ssl-appservice"></a>Azure App Service'te özel etki alanı için SSL sertifikası yapılandırma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | EnvironmentType - Azure |
| **Başvuruları**              | [Azure uygulama hizmetinde bir uygulama için HTTPS'yi etkinleştir](../app-service/app-service-web-tutorial-custom-ssl.md) |
| **Adımları** | Varsayılan olarak, Azure zaten HTTPS için her uygulama için bir joker karakter sertifikası ile etkinleştirir *. azurewebsites.net etki alanı. Ancak, tüm joker etki alanları gibi kendi sertifika ile özel bir etki alanı kullanmak kadar güvenli değildir [bakın](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/). Dağıtılan uygulama aracılığıyla erişilen özel etki alanı için SSL etkinleştirmek için önerilir.|

## <a id="appservice-https"></a>Tüm trafiği HTTPS bağlantısı üzerinden Azure App Service'e zorla

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | EnvironmentType - Azure |
| **Başvuruları**              | [Azure uygulama Hizmeti'nde HTTPS zorla](../app-service/app-service-web-tutorial-custom-ssl.md#enforce-https) |
| **Adımları** | <p>Azure etki alanının joker nitelikli bir sertifikası ile Azure uygulama hizmetleri için HTTPS zaten sağlar ancak *. azurewebsites.net, onu zorla HTTPS. Ziyaretçilerin uygulamanın güvenliği tehlikeye atabilir HTTP kullanarak uygulamayı erişmeye devam ve bu nedenle açıkça zorlanacak HTTPS sahiptir. ASP.NET MVC uygulamaları kullanması gereken [RequireHttps filtre](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) HTTPS üzerinden yeniden gönderilmesini güvenli olmayan bir HTTP isteği zorlar.</p><p>Alternatif olarak, Azure App Service ile birlikte URL yeniden yazma modülü HTTPS zorlamak için kullanılabilir. URL yeniden yazma modülü, istek uygulamanıza edilmeden önce gelen isteklere uygulanan kurallar tanımlamak geliştiricilerin sağlar. URL yeniden yazma kuralları uygulama kök dizininde depolanmış bir web.config dosyasında tanımlanır</p>|

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
Bu kural bir HTTP durum kodu 301 (kalıcı yeniden yönlendirme) döndürerek çalışan bir sayfaya HTTP kullanarak kullanıcı isteğinde bulunduğunda. 301 isteğin istenen ziyaretçi aynı URL'ye yönlendirilmesini sağlar, ancak isteğin HTTP bölümü HTTPS ile değiştirir. Örneğin, HTTP://contoso.com yönlendirileceği HTTPS://contoso.com. 

## <a id="http-hsts"></a>HTTP katı taşıma güvenliği (HSTS) etkinleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [OWASP HTTP katı taşıma güvenliği kopya sayfası](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Adımları** | <p>HTTP katı Aktarım güvenlik (HSTS) bir özel yanıt üstbilgisi kullanımı ile bir web uygulaması tarafından belirtilen bir güvenlik katılımı yeniliktir. Bu üst desteklenen bir tarayıcı aldıktan sonra bu tarayıcı iletişimlerle belirtilen etki alanı için HTTP üzerinden gönderilmesini engeller ve bunun yerine tüm iletişimler HTTPS üzerinden gönderir. Ayrıca, HTTPS istekleri tarayıcılarda geçişli tıklatma önler.</p><p>HSTS uygulamak için aşağıdaki yanıt üst bilgisi için bir Web sitesi genel olarak, kod veya yapılandırma yapılandırılması gerekir. Strict-aktarım-güvenliği:, max-age 300; = includeSubDomains HSTS aşağıdaki tehditleri ele alır:</p><ul><li>Kullanıcı yer işaretleri veya el ile türleri http://example.com ve man-in--middle saldırgan tabidir: HSTS otomatik olarak yeniden yönlendiren HTTP isteklerini HTTPS için hedef etki alanı için</li><li>Yalnızca HTTPS yanlışlıkla olması amaçlanmıştır web uygulaması HTTP bağlantılarını içerir veya HTTP üzerinden içerik sunar: HSTS otomatik olarak yeniden yönlendiren HTTP isteklerini HTTPS için hedef etki alanı için</li><li>ADAM-in--middle saldırgan geçersiz bir sertifika kullanarak bir kurban kullanıcı trafiğinden müdahale çalışır ve kullanıcı hatalı sertifika kabul planlamaktadır: HSTS geçersiz sertifika iletisi geçersiz kılmak bir kullanıcı izin verme</li></ul>|

## <a id="sqlserver-validation"></a>SQL server bağlantı şifreleme ve sertifika doğrulaması emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure  |
| **Öznitelikleri**              | SQL sürümü - V12 |
| **Başvuruları**              | [SQL veritabanı için bağlantı dizelerini yazma üzerinde en iyi yöntemler güvenli](http://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Adımları** | <p>SQL veritabanı ile bir istemci uygulama arasındaki tüm iletişimler, her zaman Güvenli Yuva Katmanı (SSL) kullanılarak şifrelenir. SQL veritabanı şifrelenmemiş bağlantıları desteklemiyor. Uygulama kodu veya araçları ile sertifikaları doğrulamak için açıkça şifreli bir bağlantı isteği ve sunucu sertifikaları güvenmeyin. Uygulama kodu veya Araçlar şifreli bir bağlantı isteme, şifreli bağlantıları hala alacağı</p><p>Ancak, sunucu sertifikalarını doğrulamamasına ve bu nedenle "ortadaki adam" saldırılarına olacaktır. ADO.NET uygulama kodu sertifikalarla doğrulamak için `Encrypt=True` ve `TrustServerCertificate=False` veritabanı bağlantı dizesi içinde. SQL Server Management Studio aracılığıyla sertifikaları doğrulamak için Bağlan Server iletişim kutusunu açın. Bağlantı Özellikleri sekmesinde şifrele bağlantısına tıklayın</p>|

## <a id="encrypted-sqlserver"></a>SQL Server şifreli iletişim zorla

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | OnPrem |
| **Öznitelikleri**              | SQL sürümü - MsSQL2016, SQL sürüm - MsSQL2012, SQL sürümü - MsSQL2014 |
| **Başvuruları**              | [Şifrelenmiş veritabanı altyapısı bağlantılarını etkinleştirme](https://msdn.microsoft.com/library/ms191192)  |
| **Adımları** | SSL etkinleştirme şifreleme SQL Server örneklerini ve uygulamalar arasında ağlar üzerinden aktarılan verilerin güvenliğini artırır. |

## <a id="comm-storage"></a>Azure Storage iletişim HTTPS üzerinden olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Azure depolama aktarım düzeyinde şifreleme – HTTPS kullanarak](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Adımları** | REST API'larını çağırma veya erişme depolama nesneleri Azure Storage veri aktarım sırasında güvenliğini sağlamak için her zaman HTTPS protokolünü kullanır. Ayrıca, paylaşılan erişim Azure Storage nesnelere erişimi devretmek için kullanılabilen, imzalar, yalnızca HTTPS protokolü herkes SAS belirteci bağlantılarıyla dışarı gönderme kullanacağı sağlama paylaşılan erişim imzaları kullanırken kullanılabilir belirtmek için bir seçenek içerir uygun protokolü.|

## <a id="md5-https"></a>HTTPS etkinleştirilirse blob indirdikten sonra MD5 karma değeri doğrulanamıyor

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | StorageType - Blob |
| **Başvuruları**              | [Windows Azure Blob MD5 genel bakış](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Adımları** | <p>Windows Azure Blob hizmeti hem uygulama ve taşıma katmanları veri bütünlüğünü sağlamak için mekanizma sağlar. HTTPS ve yerine HTTP kullanmanız gereken herhangi bir nedenle blok blobları ile çalışıyorsanız, MD5 denetimi aktarılan BLOB'ları bütünlüğünü doğrulamak amacıyla kullanabilirsiniz</p><p>Bu ağ/aktarım katmanı hataları korumadan, ancak mutlaka Ara saldırılarla yardımcı olur. Aktarım düzeyi güvenlik sağlayan HTTPS kullanırsanız, MD5 denetimi kullanarak yedekli ve gereksiz.</p>|

## <a id="smb-shares"></a>Azure dosya paylaşımları için aktarım sırasında veri şifreleme sağlamak için SMB 3.0 uyumlu İstemcisi'ni kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil istemci | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | StorageType - dosya |
| **Başvuruları**              | [Azure File Storage](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [Windows istemcileri için Azure dosya depolama SMB desteği](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Adımları** | Azure File Storage REST API'sini kullanarak, HTTPS destekler, ancak daha sık SMB dosya paylaşımı kullanılan bir VM öğesine bağlı. Bağlantıları yalnızca Azure aynı bölgede içinde izin için SMB 2.1 şifrelemeyi desteklemiyor. Ancak, SMB 3.0 Şifreleme destekler ve Windows Server 2012 R2 ile kullanılabilir, Windows 8, Windows 8.1 ve Windows 10, çapraz bölge izin vererek erişmek ve masaüstünde erişim hatta. |

## <a id="cert-pinning"></a>Sertifika sabitleme uygulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, Windows Phone |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Sertifika ve ortak anahtar sabitleme](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Adımları** | <p>Sertifika sabitleme Man-In--Middle (MITM) saldırılarına karşı defends. Sabitleme olan bir konak kendi beklenen X509 ile ilişkilendirme işlemi sertifikası veya ortak anahtar. Bir sertifika veya ortak anahtar bilinen veya bir konak için görülen sonra sertifika ya da ortak anahtar ilişkili veya 'konağa sabitlenmiş'. </p><p>Bu nedenle, bir saldırganın girişiminde bulunduğunda SSL MITM saldırı yapın, SSL el sıkışması sırasında saldırganın sunucudan anahtar sabitlenmiş sertifika anahtarından farklı olacaktır ve istek atılacak, böylece MITM sertifika önleme sabitleme tarafından sağlanabilir ServicePointManager'ın uygulama `ServerCertificateValidationCallback` temsilci.</p>|

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

## <a id="https-transport"></a>HTTPS enable - aktarım kanalının güvenliğini sağlayın.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | NET Framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.semantic.dotnet.wcf_misconfiguration_transport_security_enabled) |
| **Adımları** | Uygulama yapılandırması HTTPS tüm erişim hassas bilgiler için kullanıldığından emin olmak.<ul><li>**Açıklama:** bir uygulama hassas bilgileri işler ve ileti düzeyi şifreleme kullanmaz durumunda, yalnızca bir şifrelenmiş taşıma kanalı üzerinden iletişim kurmasına izin verilmelidir.</li><li>**ÖNERİLER:** HTTP aktarımı devre dışı bırakıldığından emin olun ve HTTPS aktarımı yerine etkinleştirin. Örneğin, `<httpTransport/>` ile `<httpsTransport/>` etiketi. Uygulama yalnızca güvenli bir kanal üzerinden erişilebilir olmasını garanti etmek için bir ağ yapılandırması (Güvenlik Duvarı) kullanmayın. Bir felsefi açısından bakıldığında, uygulama güvenliğini ağ üzerinde bağlı olmaması gerekir.</li></ul><p>Bunlar geliştikçe bir pratik açısından bakıldığında, ağ güvenliğini sağlamak için sorumlu kişi her zaman uygulamanın güvenlik gereksinimlerini izlemek değil.</p>|

## <a id="message-protection"></a>WCF: Kümesi ileti güvenliği da EncryptAndSign için koruma düzeyi

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Adımları** | <ul><li>**Açıklama:** koruma düzeyi ayarlanır "yok" iletisi korumayı devre dışı bıraktığınızda. Gizliliği ve bütünlük uygun düzeyde bir ayar ile elde edilir.</li><li>**ÖNERİLER:**<ul><li>zaman `Mode=None` -ileti koruma devre dışı bırakır</li><li>zaman `Mode=Sign` -işaretlerini ancak iletiyi şifrelemez; veri bütünlüğü önemli olduğunda kullanılmalıdır</li><li>zaman `Mode=EncryptAndSign` -açar ve iletiyi şifreler</li></ul></li></ul><p>Şifreleme devre dışı kapatma ve iletinizi gizliliği endişeler olmadan bilgilerinin bütünlüğünü doğrulamak yeterlidir yalnızca imzalama göz önünde bulundurun. Bu işlemler için yararlı olabilir veya hizmet sözleşmelerinde özgün göndereni doğrulamanız gerekiyor ancak hiçbir hassas veriler iletilir. Koruma düzeyini azaltır, ileti tüm kişisel bilgileri (PII) içermiyor dikkatli olun.</p>|

### <a name="example"></a>Örnek
Yapılandırma hizmeti ve yalnızca iletiyi imzalamak için işlemi aşağıdaki örneklerde gösterilmiştir. Hizmet sözleşmesi örneği `ProtectionLevel.Sign`: Hizmet sözleşmesi düzeyinde ProtectionLevel.Sign kullanmaya ilişkin bir örnek verilmiştir: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Örnek
İşlemi sözleşme örneği `ProtectionLevel.Sign` (ayrıntılı denetim için): kullanmaya ilişkin bir örnek verilmiştir `ProtectionLevel.Sign` OperationContract düzeyinde:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a id="least-account-wcf"></a>WCF: WCF hizmeti çalıştırmak için en az ayrıcalıklı bir hesap kullanın.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Adımları** | <ul><li>**Açıklama:** WCF hizmetleri yönetici veya yüksek ayrıcalıklı hesap altında çalışmaz. Hizmetleri güvenliğinin aşılması durumunda yüksek etkileri neden olur.</li><li>**ÖNERİLER:** , uygulamanızın saldırı yüzeyini küçültmek ve saldırı, potansiyel hasarı azaltmak için WCF Hizmeti barındırma için en az ayrıcalıklı bir hesap kullanın. Hizmet hesabı ek erişim hakları MSMQ, olay günlüğü, performans sayaçları ve dosya sistemi gibi altyapı kaynaklardaki gerektiriyorsa, WCF hizmeti başarıyla çalıştırabilmeniz için bu kaynaklara uygun izinleri verilmelidir.</li></ul><p>Hizmetinizi adına özgün çağıran belirli kaynaklara erişmesi gerekirse arayanın kimliği için bir aşağı akış yetkilendirme denetimi akış için kimliğe bürünme ve temsilci kullanın. Bir geliştirme senaryosunda ayrıcalıkları daha az olan özel bir yerleşik hesap yerel ağ hizmeti hesabı kullanın. Bir üretim senaryosunda, en az ayrıcalıklı özel etki alanı hizmeti hesabı oluşturun.</p>|

## <a id="webapi-https"></a>Tüm trafiği HTTPS bağlantısı üzerinden Web API'lerine zorla

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | MVC5, MVC6 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Bir Web API denetleyicisi SSL zorlama](http://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Adımları** | Bir uygulama, bir HTTPS ve HTTP bağlaması içeriyorsa, istemciler siteye erişmek için HTTP hala kullanabilirsiniz. Bunu önlemek için korumalı API'leri isteklerine daima HTTPS üzerinden olduğundan emin olmak için bir eylem filtresi kullanın.|

### <a name="example"></a>Örnek 
Aşağıdaki kod, SSL için denetler bir Web API kimlik doğrulama filtre gösterir: 
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
Bu filtre SSL gereken tüm Web API eylemler ekleyin: 
```csharp
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a id="redis-ssl"></a>Azure Redis önbelleği iletişimin SSL üzerinden olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Redis Cache | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Azure Redis SSL desteği](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Adımları** | Redis sunucu SSL kutu dışı desteklemez ancak Azure Redis önbelleği desteklemez. Azure Redis önbelleği bağlanan ve istemci StackExchange.Redis gibi SSL destekliyorsa SSL kullanmanız gerekir. Varsayılan olarak SSL olmayan bağlantı noktası yeni Azure Redis önbelleği örnekleri için devre dışıdır. SSL desteği, redis istemcileri üzerinde bir bağımlılık olmadıkça güvenli varsayılanlar değiştirilmediğinden emin olun. |

Redis güvenilen ortamları içindeki güvenilen istemciler tarafından erişilmek üzere tasarlanmıştır unutmayın. Bu genellikle, Redis örneği doğrudan Internet'e ya da genel olarak, güvenilmeyen istemciler doğrudan Redis TCP bağlantı noktası veya UNIX yuva erişebileceğiniz ortamında kullanıma sunmak için iyi bir fikir olmadığı anlamına gelir. 

## <a id="device-field"></a>Aygıt için alan ağ geçidi iletişimin güvenliğini sağlama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | IP tabanlı cihazlar için iletişim protokolü Aktarımdaki verileri korumak için bir SSL/TLS kanal genellikle saklanmasını. SSL/TLS desteklemeyen diğer protokoller için taşıma veya ileti katmanında güvenliği sağlayan güvenli sürümlerini protokolü varsa araştırın. |

## <a id="device-cloud"></a>SSL/TLS kullanılarak bulut ağ geçidi iletişim için güvenli aygıt

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [İletişim protokolü seçin](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Adımları** | HTTP/AMQP veya MQTT protokolleri kullanarak SSL/TLS güvenli hale getirin. |
