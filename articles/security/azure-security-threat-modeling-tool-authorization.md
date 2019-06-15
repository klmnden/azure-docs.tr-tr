---
title: Yetkilendirme - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
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
ms.openlocfilehash: c922556417ac92cc3667927fc8f846ace960a14a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935876"
---
# <a name="security-frame-authorization--mitigations"></a>Güvenlik çerçevesi: Yetkilendirme | Risk azaltma işlemleri 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Makine güven sınırı** | <ul><li>[Cihazın şirket verilerine yetkisiz erişimi kısıtlamak için uygun ACL'leri yapılandırıldığından emin olun](#acl-restricted-access)</li><li>[Hassas kullanıcıya özgü uygulama içeriğinin kullanıcı profili dizinde depolandığından emin](#sensitive-directory)</li><li>[Dağıtılan uygulamaları en az ayrıcalıkla çalıştırdığınızdan emin olun](#deployed-privileges)</li></ul> |
| **Web uygulaması** | <ul><li>[Sıralı adım sipariş iş mantık akışları işlerken zorla](#sequential-logic)</li><li>[Oranı mekanizması numaralandırma önlemek için sınırlama uygulamak](#rate-enumeration)</li><li>[Uygun yetkilendirme yerinde olduğundan ve en düşük ayrıcalık ilkesini ardından emin olun](#principle-least-privilege)</li><li>[İş mantığı ve kaynak erişim yetkilendirme kararları gelen isteği parametrelere temel alamaz](#logic-request-parameters)</li><li>[Bu içerik sağlamak ve kaynakları numaralandırılabilir veya zorlama gözatma aracılığıyla erişilebilir değildir](#enumerable-browsing)</li></ul> |
| **Veritabanı** | <ul><li>[En az ayrıcalıklı hesapları, veritabanı sunucusuna bağlanmak için kullanıldığından emin olun](#privileged-server)</li><li>[Kiracılar, diğer kişilerin veri erişimini engellemek için satır düzeyi güvenlik RLS uygulayın](#rls-tenants)</li><li>[Sysadmin rolüne yalnızca geçerli gerekli kullanıcıları olmalıdır](#sysadmin-users)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Bulut en az ayrıcalıklı belirteçleri kullanarak ağ geçidine bağlanma](#cloud-least-privileged)</li></ul> |
| **Azure Event Hub** | <ul><li>[Cihaz belirteçleri oluşturmak için bir yalnızca gönderme izinleri SAS anahtarı kullanın.](#sendonly-sas)</li><li>[Olay Hub'ına doğrudan erişim sağlayan erişim belirteçleri kullanmayın](#access-tokens-hub)</li><li>[Gerekli en düşük izinlere sahip bir SAS anahtarları kullanarak Event Hubs'a bağlanma](#sas-minimum-permissions)</li></ul> |
| **Azure belge veritabanı** | <ul><li>[Azure Cosmos DB için mümkün olduğunca bağlanmak için kaynak belirteçleri kullanma](#resource-docdb)</li></ul> |
| **Azure güven sınırı** | <ul><li>[Azure RBAC kullanarak abonelik için ayrıntılı erişim yönetimini etkinleştirme](#grained-rbac)</li></ul> |
| **Service Fabric güven sınırı** | <ul><li>[RBAC kullanarak küme işlemleri için istemci erişimi kısıtlama](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Güvenlik modelleme gerçekleştirmek ve alan düzeyinde güvenliğin kullanmak gerektiğinde](#modeling-field)</li></ul> |
| **Dynamics CRM portalı** | <ul><li>[Portal hesap portalı için güvenlik modeli CRM geri kalanından farklı olduğunu unutmayın tutma güvenlik modelleme gerçekleştirin](#portal-security)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure tablo depolamadaki varlıklar bir dizi ayrıntılı izin ver](#permission-entities)</li><li>[Rol tabanlı erişim denetimi (RBAC) Azure Resource Manager kullanarak Azure depolama hesabına etkinleştir](#rbac-azure-manager)</li></ul> |
| **Mobil istemci** | <ul><li>[Örtük jailbreak veya algılama kök dizini değiştirme](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[Wcf'de zayıf sınıf başvurusu](#weak-class-wcf)</li><li>[WCF uygulama yetkilendirme denetimi](#wcf-authz)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API'de uygun yetkilendirme mekanizmalarını uygular](#authz-aspnet)</li></ul> |
| **IOT cihaz** | <ul><li>[Farklı izin seviyeleri gereken çeşitli eylemler destekliyorsa, cihaz yetkilendirme denetimleri gerçekleştirir.](#device-permission)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[Farklı izin seviyeleri gereken çeşitli eylemler destekliyorsa, alan ağ geçidi yetkilendirme denetimleri gerçekleştirir.](#field-permission)</li></ul> |

## <a id="acl-restricted-access"></a>Cihazın şirket verilerine yetkisiz erişimi kısıtlamak için uygun ACL'leri yapılandırıldığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Cihazın şirket verilerine yetkisiz erişimi kısıtlamak için uygun ACL'leri yapılandırıldığından emin olun|

## <a id="sensitive-directory"></a>Hassas kullanıcıya özgü uygulama içeriğinin kullanıcı profili dizinde depolandığından emin

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Hassas kullanıcıya özgü uygulama içeriği, kullanıcı profili dizininde depolanır emin olun. Bu makinenin birden çok kullanıcı birbirinin verilerine erişmesini önlemek içindir.|

## <a id="deployed-privileges"></a>Dağıtılan uygulamaları en az ayrıcalıkla çalıştırdığınızdan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Dağıtılan uygulamanın en az ayrıcalıkla çalıştırdığınızdan emin olun. |

## <a id="sequential-logic"></a>Sıralı adım sipariş iş mantık akışları işlerken zorla

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bu aşamada, uygulamayı yalnızca işlem iş mantık akışları sıralı adım sırayla gerçekçi insan tarafından işlenen tüm adımları uygulamak istediğiniz orijinal bir kullanıcı tarafından çalıştırıldığı doğrulayın ve sıralamaya, işlemek için adımları atlandı. , işlenen adımları başka bir kullanıcı veya işlem'çok hızlı bir şekilde gönderildi.|

## <a id="rate-enumeration"></a>Oranı mekanizması numaralandırma önlemek için sınırlama uygulamak

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Küçük harf duyarlı tanımlayıcıları rastgele olduğundan emin olun. Anonim sayfalarında CAPTCHA denetimi uygulayın. Hata ve özel durum özel veri sunmamasına emin olun|

## <a id="principle-least-privilege"></a>Uygun yetkilendirme yerinde olduğundan ve en düşük ayrıcalık ilkesini ardından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>İlke, bir kullanıcı hesabı olan kullanıcıların çalışması için gerekli olan ayrıcalıkları verme anlamına gelir. Örneğin, bir yedekleme kullanıcı yazılımı yüklemeniz gerekmez: Bu nedenle, yedekleme kullanıcının yalnızca yedekleme ve yedekleme ile ilgili uygulamaları çalıştırmak için haklara sahip. Yeni yazılım yükleme gibi herhangi diğer ayrıcalıklara engellenir. İlke genellikle normal bir kullanıcı hesabı içinde çalışır ve ayrıcalıklı, parola korumalı hesap (diğer bir deyişle, bir süper kullanıcısı) açılır bir kişisel bilgisayar kullanıcı için de geçerlidir. yalnızca zaman durum kesinlikle gerektirmesi. </p><p>Bu ilke, web uygulamalarınızın da uygulanabilir. Yerine oturumları kullanarak rol tabanlı kimlik doğrulama yöntemleri bağlı olarak yalnızca yerine ayrıcalıkları, bir veritabanı tabanlı kimlik doğrulama sisteminde yoluyla kullanıcılara atamak istiyoruz. Hala oturumları kullanıcı doğru artık yalnızca bu kullanıcı sistemde gerçekleştirmek için ayrıcalıklı yaptığı eylemleri doğrulamak için gerekli ayrıcalıklara sahip biz ona ata, belirli bir role atamak yerine oturum açarsa belirlemek amacıyla kullanırız. Değişikliklerinizi atama önce sona eren gerekiyordu. Aksi takdirde, oturuma bağlı değil olduğundan çalışma sırasında uygulanacak daha az ayrıcalık atanacak bir kullanıcının sahip olduğu her durumda de büyük bir pro bu yöntemin, olur.</p>|

## <a id="logic-request-parameters"></a>İş mantığı ve kaynak erişim yetkilendirme kararları gelen isteği parametrelere temel alamaz

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bir kullanıcının belirli verileri gözden geçirmek için sınırlı olup olmadığını denetleme her erişim kısıtlamalarını işlenen sunucu tarafı olmalıdır. UserId oturum açma üzerinde bir oturum değişkeni içinde saklanmalıdır ve veritabanından kullanıcı verilerini almak için kullanılması gereken |

### <a name="example"></a>Örnek
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Olası bir saldırganın değiştirmesine ve değiştirme değil artık sunucu tarafı veri almak için tanımlayıcı olduğundan uygulama işlemi işlenir.

## <a id="enumerable-browsing"></a>Bu içerik sağlamak ve kaynakları numaralandırılabilir veya zorlama gözatma aracılığıyla erişilebilir değildir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Hassas statik ve yapılandırma dosyalarını web kökü tutulmalıdır değil. Ya da uygun erişim denetimleri ortak olması için gerekli olmayan içerik için uygulanması gereken veya içerik kaldırma.</p><p>Ayrıca, zorla gözatma genellikle dizinleri ve dosyaları bir sunucuda numaralandırma için mümkün olduğunca çok URL'leri erişmeye çalışan tarafından bilgi toplamak için tekniklerini yanılma ile birleştirilir. Saldırganlar genellikle varolan tüm çeşitliliğe göz atabilirsiniz dosyaları. Örneğin, bir parola dosyası arama psswd.txt, password.htm password.dat ve diğer Çeşitlemeler dahil olmak üzere dosyaları kapsayabilir ve.</p><p>Bunu azaltmak için deneme yanılma girişimlerinde algılanması için özellikleri dahil edilecek.</p>|

## <a id="privileged-server"></a>En az ayrıcalıklı hesapları, veritabanı sunucusuna bağlanmak için kullanıldığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [SQL veritabanı izinleri hiyerarşi](https://msdn.microsoft.com/library/ms191465), [SQL veritabanı güvenliği sağlanabilir öğeler](https://msdn.microsoft.com/library/ms190401) |
| **Adımları** | En az ayrıcalıklı hesapları, veritabanına bağlanmak için kullanılması gerekir. Uygulama oturum açma, veritabanında sınırlı olmalıdır ve yalnızca seçilen saklı yordamları yürütme. Uygulamanın oturum açma hiçbir doğrudan tablo erişimi olmalıdır. |

## <a id="rls-tenants"></a>Kiracılar, diğer kişilerin veri erişimini engellemek için satır düzeyi güvenlik RLS uygulayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Sql Azure, OnPrem |
| **Öznitelikler**              | SQL sürümü - V12, SQL sürümü - MsSQL2016 |
| **Başvuruları**              | [SQL Server satır düzeyi güvenlik (RLS)](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Adımları** | <p>Satır Düzeyi Güvenlik, müşterilerin bir veritabanı tablosundaki satırlara erişimi, sorguyu yürüten kullanıcının özelliklerine göre (grup üyeliği veya yürütme bağlamı) denetlemesini sağlar.</p><p>Satır düzeyi güvenlik (RLS), tasarım ve uygulama güvenlik kodlama basitleştirir. RLS, veri satırı erişiminde kısıtlama uygulamanızı sağlar. Örneğin çalışanlar yalnızca kendi bölümleriyle ilgili veri satırlarına erişmesi veya müşterilerin yalnızca kendi şirketleriyle ilgili verilere ulaşması sağlanabilir.</p><p>Veritabanı katmanı bulunur yerine koy verileri başka bir uygulama katmanı kullanarak erişimi kısıtlama mantığı. Veri erişimini herhangi bir katmanı denenir her zaman veritabanı sistem erişim kısıtlamalarını uygular. Bu güvenlik sistemi daha güvenli ve sağlam güvenlik sistemi'nın yüzey alanını azaltarak sağlar.</p><p>|

Lütfen unutmayın, RLS olarak kullanıma hazır veritabanı özelliği yalnızca SQL Server 2016'dan itibaren ve Azure SQL veritabanı için geçerlidir. Kullanıma hazır RLS özelliğini kullanılırsa, veri erişimi kısıtlı kullanarak görünüm ve yordam olduğunu için sağlamış

## <a id="sysadmin-users"></a>Sysadmin rolüne yalnızca geçerli gerekli kullanıcıları olmalıdır

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [SQL veritabanı izinleri hiyerarşi](https://msdn.microsoft.com/library/ms191465), [SQL veritabanı güvenliği sağlanabilir öğeler](https://msdn.microsoft.com/library/ms190401) |
| **Adımları** | SysAdmin sabit sunucu rolünün üyeleri, sınırlı ve hiçbir zaman uygulamaları tarafından kullanılan hesapları içerir.  Lütfen bu roldeki kullanıcılar listesini gözden geçirin ve tüm gereksiz hesaplarını kaldırın|

## <a id="cloud-least-privileged"></a>Bulut en az ayrıcalıklı belirteçleri kullanarak ağ geçidine bağlanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ağ geçidi seçim - Azure IOT hub'ı |
| **Başvuruları**              | [IOT hub'ı erişim denetimi](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Adımları** | Bulut ağ geçidi (IOT Hub) bağlanan çeşitli bileşenler için en az ayrıcalık izinleri sağlar. Tipik bir örnektir – cihaz Yönetimi/sağlama bileşeni registryread/yazma kullanıyorsa, olay işlemcisi (ASA) hizmeti kullanır. Cihaz kimlik bilgilerini kullanarak tek tek cihazları bağlayın|

## <a id="sendonly-sas"></a>Cihaz belirteçleri oluşturmak için bir yalnızca gönderme izinleri SAS anahtarı kullanın.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Olay Hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Event Hubs kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | Bireysel cihaz belirteçleri oluşturmak için bir SAS anahtarı kullanılır. Belirli bir yayımcının cihaz belirteci oluştururken yalnızca gönderme izinleri SAS anahtarının kullanılması|

## <a id="access-tokens-hub"></a>Olay Hub'ına doğrudan erişim sağlayan erişim belirteçleri kullanmayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Olay Hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Event Hubs kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | Olay hub'ına doğrudan erişim veren bir belirteç cihaza verilmemelidir. Yalnızca bir yayımcı için erişim sağlayan cihaz belirlemek ve varsa veya kara listeye yardımcı olan için en az ayrıcalıklı bir belirteç kullanarak bir dolandırıcı olacak şekilde bulunamadı veya cihaz tehlikeye.|

## <a id="sas-minimum-permissions"></a>Gerekli en düşük izinlere sahip bir SAS anahtarları kullanarak Event Hubs'a bağlanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Olay Hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Event Hubs kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | Olay Hub'ına bağlanan çeşitli arka uç uygulamaları için en az ayrıcalık izinleri sağlar. Her arka uç uygulaması için ayrı bir SAS anahtarları oluşturun ve yalnızca gerekli izinleri - gönderme, sağlamak alma veya bunlara Yönet.|

## <a id="resource-docdb"></a>Mümkün olduğunda Cosmos DB'ye bağlanmak için kaynak belirteçleri kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure belge veritabanı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Kaynak belirtecini bir Azure Cosmos DB izni kaynakla ilişkilendirilen ve ilişki yakalayan bir veritabanı kullanıcısı ve izin arasında kullanıcı belirli bir Azure Cosmos DB uygulama kaynak için (örneğin, koleksiyon ve belge) sahiptir. Azure Cosmos DB istemci ana veya salt okunur anahtarları - bir son kullanıcı uygulamayı bir mobil veya masaüstü istemcisi gibi gibi işleme ile güvenilir yoksa erişmek için her zaman kaynak belirtecini kullanın. Ana anahtarı veya bu anahtarları güvenli bir şekilde depolayabilirsiniz arka uç uygulamaları salt okunur anahtarları kullanın.|

## <a id="grained-rbac"></a>Azure RBAC kullanarak abonelik için ayrıntılı erişim yönetimini etkinleştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure güven sınırı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Adımları** | Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz.|

## <a id="cluster-rbac"></a>RBAC kullanarak küme işlemleri için istemci erişimi kısıtlama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ortam - Azure |
| **Başvuruları**              | [Service Fabric istemciler için rol tabanlı erişim denetimi](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Adımları** | <p>Azure Service Fabric, bir Service Fabric kümesine bağlanan istemciler için iki farklı erişim denetim türlerini destekler: Yönetici ve kullanıcı. Küme Yöneticisi, belirli küme işlemleri farklı kümeye daha güvenli hale getirme, kullanıcı grupları için erişimi sınırlandırmak erişim denetimi sağlar.</p><p>Yöneticiler yönetim özellikleri (okuma/yazma özellikleri dahil olmak üzere) tam erişime sahiptir. Kullanıcılar, varsayılan olarak, yalnızca yönetim özelliklerine (örneğin, sorgu işlevleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümlenebilmesi sahiptir.</p><p>Küme oluşturma sırasında her biri için ayrı sertifikalar sağlayarak iki istemci rolleri (Yönetici ve istemci) belirtin.</p>|

## <a id="modeling-field"></a>Güvenlik modelleme gerçekleştirmek ve alan düzeyinde güvenliğin kullanmak gerektiğinde

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Güvenlik modelleme gerçekleştirmek ve alan düzeyinde güvenliğin kullanmak gerektiğinde|

## <a id="portal-security"></a>Portal hesap portalı için güvenlik modeli CRM geri kalanından farklı olduğunu unutmayın tutma güvenlik modelleme gerçekleştirin

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM portalı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Portal hesap portalı için güvenlik modeli CRM geri kalanından farklı olduğunu unutmayın tutma güvenlik modelleme gerçekleştirin|

## <a id="permission-entities"></a>Azure tablo depolamadaki varlıklar bir dizi ayrıntılı izin ver

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | StorageType - tablo |
| **Başvuruları**              | [SAS kullanarak Azure depolama hesabınızdaki nesnelere erişim devretmek nasıl](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Adımları** | Bazı iş senaryolarında, Azure tablo depolama, farklı taraflara oluşturabilmesine olanak sağlar, hassas verileri depolamak için gerekebilir. Farklı ülkelerde/bölgelerde ilişkin örn, hassas veriler. Bir kullanıcı belirli bir ülke/bölge belirli veri erişerek bölüm ve satır anahtarı aralığı belirterek bu gibi durumlarda, SAS imzaları oluşturulabilir.| 

## <a id="rbac-azure-manager"></a>Rol tabanlı erişim denetimi (RBAC) Azure Resource Manager kullanarak Azure depolama hesabına etkinleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlama](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Adımları** | <p>Yeni bir depolama hesabı oluşturduğunuzda, Klasik veya Azure Resource Manager dağıtım modeli seçin. Azure kaynakları oluşturma, Klasik modeli yalnızca abonelik ve ardından depolama hesabını zorlamadan erişim sağlar.</p><p>Azure Resource Manager modeli ile depolama hesabı Azure Active Directory'yi kullanarak bu belirli bir depolama hesabı için yönetim düzlemi bir kaynak grubu ve Denetim erişim yerleştirin. Örneğin, belirli kullanıcılar diğer kullanıcılar için depolama hesabıyla ilgili bilgileri görüntüleyebilirsiniz, ancak bu depolama hesabı anahtarlarını erişilemiyor, depolama hesabı anahtarları, erişim olanağı verebilirsiniz.</p>|

## <a id="rooting-detection"></a>Örtük jailbreak veya algılama kök dizini değiştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil istemci | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Uygulama, telefon kökü belirtilmiş ve jailbreak kendi yapılandırma ve kullanıcı verileri korunması. Kök dizini değiştirme/bozucu jailbreak normal hangi kullanıcıların kendi telefonlarda olmaz, yetkisiz erişim anlamına gelir. Bu nedenle uygulama, telefon verilmediğini belirlemek için uygulama başlatma sırasında örtük algılama mantığı olmalıdır.</p><p>Algılama mantığı yalnızca normal olarak, yalnızca kök kullanıcı, örneğin erişebilir dosyalara erişmek:</p><ul><li>/System/App/Superuser.apk</li><li>/sbin/su</li><li>/System/bin/su</li><li>/system/xbin/su</li><li>/Data/Local/xbin/su</li><li>/Data/local/bin/su</li><li>/system/sd/xbin/su</li><li>/System/bin/failsafe/su</li><li>/Data/Local/su</li></ul><p>Bu dosyalar da uygulamaya erişebilir, uygulama kök kullanıcı olarak çalıştığını gösterir.</p>|

## <a id="weak-class-wcf"></a>Wcf'de zayıf sınıf başvurusu

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference) |
| **Adımları** | <p>Sistem, bir saldırganın yetkisiz kod yürütmesine izin verebilir zayıf sınıf başvurusu kullanır. Program benzersiz olarak tanımlanmamış bir kullanıcı tanımlı bir sınıfa başvuruyor. Zayıf tanımlanan bu sınıf .NET yüklediğinde, CLR türü Yükleyici sınıfı belirtilen sırayla aşağıdaki konumlarda arar:</p><ol><li>Yükleyici türü derleme biliniyorsa, yapılandırma dosyasının yeniden yönlendirme konumları, GAC, yapılandırma bilgilerini ve uygulama temel dizinini kullanarak geçerli derleme arar.</li><li>Derleme bilinmiyorsa, yükleyici geçerli derlemedeki, mscorlib ve TypeResolve olay işleyicisi tarafından döndürülen konum arar.</li><li>Bu CLR arama sırası, tür iletme mekanizması ve AppDomain.TypeResolve olay gibi kancaları kullanılarak değiştirilebilir.</li></ol><p>Aynı ada sahip başka bir sınıfı oluşturarak bir saldırganın CLR arama sırası yararlanan ve CLR ilk olarak, CLR Yükleme farklı bir konumda yerleştirme istemeden saldırgan tarafından sağlanan kodu yürütme</p>|

### <a name="example"></a>Örnek
`<behaviorExtensions/>` WCF yapılandırma dosyasına aşağıdaki öğesinin belirli bir WCF uzantısı için özel davranış sınıfı eklemek için WCF bildirir.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
(Güçlü) tam olarak nitelenmiş adlar benzersiz olarak kullanarak, bir tür tanımlar ve sisteminizin güvenliği daha da artırır. Tam olarak nitelenmiş derleme adları, türleri machine.config ve app.config dosyalarında kaydolurken kullanın.

### <a name="example"></a>Örnek
`<behaviorExtensions/>` WCF yapılandırma dosyasına aşağıdaki öğesinin belirli bir WCF uzantısı için kesin olarak başvurulan özel davranış sınıfı eklemek için WCF bildirir.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""Microsoft.ServiceModel.Samples.MyBehaviorSection, MyBehavior,
            Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```

## <a id="wcf-authz"></a>WCF uygulama yetkilendirme denetimi

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference) |
| **Adımları** | <p>Bu hizmet, bir yetkilendirme denetimini kullanmaz. Bir istemci belirli bir WCF Hizmeti çağırdığında, WCF çağıranın sunucuda hizmet yöntemi yürütmek için izinlere sahip olduğunu doğrulayın, çeşitli Yetkilendirme düzeni sağlar. Kimliği doğrulanmış bir kullanıcı, yetkilendirme denetimleri için WCF hizmetleri etkin değilse, ayrıcalık yükseltme elde edebilirsiniz.</p>|

### <a name="example"></a>Örnek
WCF hizmet yürütülürken istemci yetkilendirme düzeyini denetlemek için aşağıdaki yapılandırma bildirir:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""None"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Hizmet yöntemi çağıran kişi Bunu yapmak için yetkili olup olmadığını doğrulamak için bir hizmet Yetkilendirme düzeni kullanın. WCF iki mod sağlar ve bir özel Yetkilendirme düzeni tanımlanmasına olanak tanır. UseWindowsGroups modu Windows rolleri ve kullanıcıları ve UseAspNetRoles modu kimlik doğrulaması yapmak için SQL Server gibi bir ASP.NET rol sağlayıcısını kullanır.

### <a name="example"></a>Örnek
WCF istemci Ekle hizmet yürütmeden önce Yöneticiler grubunun bir parçası olduğundan emin olmak için aşağıdaki yapılandırma bildirir:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""UseWindowsGroups"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Hizmet, ardından aşağıdaki gibi bildirilir:
```
[PrincipalPermission(SecurityAction.Demand,
Role = ""Builtin\\Administrators"")]
public double Add(double n1, double n2)
{
double result = n1 + n2;
return result;
}
```

## <a id="authz-aspnet"></a>ASP.NET Web API'de uygun yetkilendirme mekanizmalarını uygular

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, MVC5 |
| **Öznitelikler**              | Yok, kimlik sağlayıcısı - AD FS, kimlik sağlayıcısı - Azure AD |
| **Başvuruları**              | [Kimlik doğrulama ve yetkilendirme ASP.NET Web API](https://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Adımları** | <p>Kimlik sağlayıcısı olarak bunlar üzerinde uygulama kullanır veya uygulama olabilir, ADFS talep, sağlanan veya uygulama kullanıcıları için rol bilgileri Azure AD'den elde edilebilir. Tüm durumlarda, kullanıcı rolü bilgilerini özel yetkilendirme uygulama doğrulamalıdır.</p><p>Kimlik sağlayıcısı olarak bunlar üzerinde uygulama kullanır veya uygulama olabilir, ADFS talep, sağlanan veya uygulama kullanıcıları için rol bilgileri Azure AD'den elde edilebilir. Tüm durumlarda, kullanıcı rolü bilgilerini özel yetkilendirme uygulama doğrulamalıdır.</p>

### <a name="example"></a>Örnek
```csharp
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
public class ApiAuthorizeAttribute : System.Web.Http.AuthorizeAttribute
{
        public async override Task OnAuthorizationAsync(HttpActionContext actionContext, CancellationToken cancellationToken)
        {
            if (actionContext == null)
            {
                throw new Exception();
            }

            if (!string.IsNullOrEmpty(base.Roles))
            {
                bool isAuthorized = ValidateRoles(actionContext);
                if (!isAuthorized)
                {
                    HandleUnauthorizedRequest(actionContext);
                }
            }

            base.OnAuthorization(actionContext);
        }

public bool ValidateRoles(actionContext)
{
   //Authorization logic here; returns true or false
}

}
```
Denetleyicileri ve koruma için gereken eylem yöntemlerini ile özniteliği donatılmış.
```csharp
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a id="device-permission"></a>Farklı izin seviyeleri gereken çeşitli eylemler destekliyorsa, cihaz yetkilendirme denetimleri gerçekleştirir.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Cihaz arayan istenen eylemi gerçekleştirmek için gerekli izinlere sahip olup olmadığını denetlemek için çağıranın yetkilendirmeniz. Örneğin sağlar için deyin buluttan izlenebilir akıllı bir kapısını kilitleme cihazdır yanı sıra uzaktan kapısını kilitleme gibi işlevleri sağlar.</p><p>Yalnızca birisi fiziksel olarak kapı kartı söz konusu olduğunda, akıllı kapısını kilitleme kilidini açma işlevselliği sağlar. Bu durumda, uzak komut ve denetim uygulaması bulut ağ geçidi kapının kilidini açmak için bir komut göndermek için yetkili değil olarak kapının kilidini açmak için herhangi bir işlevsellik sağlamaz bir şekilde yapılması gerekir.</p>|

## <a id="field-permission"></a>Farklı izin seviyeleri gereken çeşitli eylemler destekliyorsa, alan ağ geçidi yetkilendirme denetimleri gerçekleştirir.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Alan ağ geçidi arayan istenen eylemi gerçekleştirmek için gerekli izinlere sahip olup olmadığını denetlemek için çağıranın yetkilendirmeniz. İçin bir yönetici kullanıcı arabirimi/bağlanabilir bir alan ağ geçidi v/sn cihazları yapılandırmak için kullanılan API için farklı izinlere örn olması gerekir.|
