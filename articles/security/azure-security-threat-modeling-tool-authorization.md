---
title: "Yetkilendirme - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs"
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
ms.openlocfilehash: 312a66544a5e64daa86b4902b57d4050f1f66af5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-authorization--mitigations"></a>Güvenlik çerçevesi: Yetkilendirme | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Makine güven sınırı** | <ul><li>[Uygun ACL'ler aygıttaki verilere yetkisiz erişimi kısıtlamak için yapılandırıldığından emin olun](#acl-restricted-access)</li><li>[Hassas kullanıcıya özgü uygulama içeriği kullanıcı profili dizininde depolanır emin olun](#sensitive-directory)</li><li>[Dağıtılan uygulamaları en az ayrıcalıkla çalıştırdığınızdan emin olun](#deployed-privileges)</li></ul> |
| **Web uygulaması** | <ul><li>[İş mantığı akışları işlerken sıralı adım sipariş zorla](#sequential-logic)</li><li>[Numaralandırma önlemek için bir mekanizma hız sınırı uygulama](#rate-enumeration)</li><li>[Uygun yetkilendirme yerinde olduğundan ve en düşük ayrıcalık ilkesini ardından emin olun](#principle-least-privilege)</li><li>[İş mantığı ve kaynak erişim yetkilendirme kararları gelen isteği parametrelere dayanmalıdır değil](#logic-request-parameters)</li><li>[Bu içerik sağlamak ve kaynakları numaralandırılabilir veya zorlama gözatma yoluyla erişilebilir değil](#enumerable-browsing)</li></ul> |
| **Veritabanı** | <ul><li>[En az ayrıcalıklı hesapları veritabanı sunucusuna bağlanmak için kullanıldığından emin olun](#privileged-server)</li><li>[Kiracıların diğer kişilerin veri erişimini engellemek için satır düzeyi güvenlik RLS uygulama](#rls-tenants)</li><li>[Sysadmin rolünün yalnızca geçerli gerekli kullanıcıların olmalıdır](#sysadmin-users)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Bulut en az ayrıcalıklı belirteçleri kullanarak ağ geçidine bağlanmak](#cloud-least-privileged)</li></ul> |
| **Azure Event hub'ı** | <ul><li>[Cihaz belirteçleri oluşturmak için bir yalnızca gönderme izinleri SAS anahtarı kullan](#sendonly-sas)</li><li>[Olay Hub'ına doğrudan erişim sağlayan erişim belirteçleri kullanmayın](#access-tokens-hub)</li><li>[Olay gerekli en az izinlere sahip SAS anahtarları kullanarak Hub'ına bağlanın](#sas-minimum-permissions)</li></ul> |
| **Azure belge DB** | <ul><li>[DocumentDB için mümkün olduğunca bağlanmak için kaynak belirteçleri kullanın](#resource-docdb)</li></ul> |
| **Azure güven sınırı** | <ul><li>[Azure RBAC kullanarak abonelik için ayrıntılı erişim yönetimini etkinleştirme](#grained-rbac)</li></ul> |
| **Service Fabric güven sınırı** | <ul><li>[RBAC kullanarak küme işlemleri için istemci erişimi kısıtlama](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Güvenlik modelleme gerçekleştirmek ve alan düzeyinde güvenliğin kullanmanız gerektiğinde](#modeling-field)</li></ul> |
| **Dynamics CRM portalı** | <ul><li>[Portal için güvenlik modeli CRM geri kalanından farklı olduğunu unutmayın tutma portal hesaplarının güvenlik modelleme gerçekleştirin](#portal-security)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure Table Storage varlıklarda bir dizi hassas izin ver](#permission-entities)</li><li>[Rol tabanlı erişim denetimi (RBAC) Azure Resource Manager kullanarak Azure depolama hesabı etkinleştir](#rbac-azure-manager)</li></ul> |
| **Mobil istemci** | <ul><li>[Örtük kaçış veya algılama kök dizini değiştirme uygulanması](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[WCF'de zayıf sınıf başvurusu](#weak-class-wcf)</li><li>[WCF uygulayan yetkilendirme denetimi](#wcf-authz)</li></ul> |
| **Web API** | <ul><li>[ASP.NET Web API'de uygun yetkilendirme mekanizması uygulayın](#authz-aspnet)</li></ul> |
| **IOT cihaz** | <ul><li>[Farklı izin düzeyleri gereken çeşitli eylemler destekliyorsa, cihaz yetkilendirme denetimleri gerçekleştirin](#device-permission)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[Farklı izin düzeyleri gereken çeşitli eylemler destekliyorsa, alan ağ geçidi yetkilendirme denetimleri gerçekleştirin](#field-permission)</li></ul> |

## <a id="acl-restricted-access"></a>Uygun ACL'ler aygıttaki verilere yetkisiz erişimi kısıtlamak için yapılandırıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Uygun ACL'ler aygıttaki verilere yetkisiz erişimi kısıtlamak için yapılandırıldığından emin olun|

## <a id="sensitive-directory"></a>Hassas kullanıcıya özgü uygulama içeriği kullanıcı profili dizininde depolanır emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Hassas kullanıcıya özgü uygulama içeriği kullanıcı profili dizininde depolanır emin olun. Bu makinenin birden çok kullanıcı birbirinin verilerine erişimini önlemek için yapılır.|

## <a id="deployed-privileges"></a>Dağıtılan uygulamaları en az ayrıcalıkla çalıştırdığınızdan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Dağıtılmış uygulamanın en az ayrıcalıkla çalıştırdığınızdan emin olun. |

## <a id="sequential-logic"></a>İş mantığı akışları işlerken sıralı adım sipariş zorla

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bu aşamada yalnızca gerçekçi İnsan zamanında işlenmekte olan tüm adımda iş mantığı akışları sıralı adım sırayla işlemek ve bozuk, işlemek için uygulama uygulamak istediğinize gerçek bir kullanıcı tarafından çalıştırıldığı olduğunu doğrulamak için adımlar, başka bir kullanıcı ya da çok hızlı bir şekilde gönderildi işlemleri işlenen adımları atlandı.|

## <a id="rate-enumeration"></a>Numaralandırma önlemek için bir mekanizma hız sınırı uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Hassas tanımlayıcıları rastgele olduğundan emin olun. Anonim sayfalarında CAPTCHA denetim uygulayın. Hata ve özel durum belirli veri ortaya çıkarır değil emin olun|

## <a id="principle-least-privilege"></a>Uygun yetkilendirme yerinde olduğundan ve en düşük ayrıcalık ilkesini ardından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>İlkeye bir kullanıcı hesabı temel kullanıcıların çalıştığını olan ayrıcalıkları vermek anlamına gelir. Örneğin, bir yedekleme kullanıcı yazılımı yüklemeniz gerekmez: Bu nedenle, yedekleme kullanıcı yalnızca yedekleme ve yedekleme ile ilgili uygulamaları çalıştırmak için haklarına sahip. Yeni yazılım yükleme gibi başka ayrıcalıklar, engellenir. İlke genellikle normal bir kullanıcı hesabı içinde çalışır ve ayrıcalıklı, parola korumalı hesap (diğer bir deyişle, bir süper kullanıcı) açılır bir kişisel bilgisayarda kullanıcı için de geçerlidir. yalnızca zaman durum kesinlikle, talep. </p><p>Bu ilke, ayrıca, web uygulamalarınızı uygulanabilir. Yerine yalnızca rol tabanlı kimlik doğrulama yöntemlerini oturumları kullanarak bağlı olarak bunun yerine ayrıcalıkları veritabanı tabanlı kimlik doğrulama sistemi yoluyla kullanıcılara atamak istiyoruz. Hala oturumları kullanıcı doğru artık yalnızca o kullanıcıyla sistemde gerçekleştirmek için ayrıcalıklı kendisinin hangi eylemleri doğrulayın ayrıcalıklarıyla biz ona atamak belirli bir rol atamak yerine oturum açarsa belirlemek amacıyla kullanırız. Daha az ayrıcalıklar atamadan önce sona eren gerekiyordu Aksi takdirde, oturumda bağlı değildir beri değişikliklerinizi anında uygulanacak atanmış bir kullanıcı sahip ne zaman da bu yöntemin büyük pro, açıktır.</p>|

## <a id="logic-request-parameters"></a>İş mantığı ve kaynak erişim yetkilendirme kararları gelen isteği parametrelere dayanmalıdır değil

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Bir kullanıcının belirli verileri gözden geçirmek için kısıtlı olup olmadığını denetleme zaman, erişim kısıtlamaları işlenen sunucu tarafı olmalıdır. UserId oturum açma üzerinde oturum değişkeni içinde saklanmalıdır ve kullanıcı verilerini veritabanından almak için kullanılması gereken |

### <a name="example"></a>Örnek
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Olası bir saldırganın değiştirmesine ve değiştirebileceğini değil artık veri almak için tanımlayıcı olduğundan uygulama işlemi sunucu tarafı işleme.

## <a id="enumerable-browsing"></a>Bu içerik sağlamak ve kaynakları numaralandırılabilir veya zorlama gözatma yoluyla erişilebilir değil

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Hassas statik ve yapılandırma dosyaları web kökte tutulmalıdır değil. Ortak olması için gerekli olmayan içerik için her iki doğru erişim denetimleri uygulanması gereken veya içeriğin kendisini kaldırılması.</p><p>Ayrıca, zorunlu gözatma genellikle bir sunucu dosyaları ve dizinleri listeleme olabildiğince çok URL'leri erişmeye çalışan tarafından bilgi toplamak için deneme yanılma saldırısı teknikleri ile birleştirilir. Saldırganlar genellikle varolan tüm çeşitlemelerini denetleyin dosyaları. Örneğin, bir parola dosya arama psswd.txt, password.htm, password.dat ve diğer Çeşitleme dahil olmak üzere dosyaları kapsayan.</p><p>Bunu azaltmak için deneme yanılma algılanması için özellikleri eklenmelidir.</p>|

## <a id="privileged-server"></a>En az ayrıcalıklı hesapları veritabanı sunucusuna bağlanmak için kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [SQL veritabanı izinleri hiyerarşi](https://msdn.microsoft.com/library/ms191465), [SQL veritabanı güvenliği sağlanabilir öğeler](https://msdn.microsoft.com/library/ms190401) |
| **Adımları** | En az ayrıcalıklı hesapları veritabanına bağlanmak için kullanılması gerekir. Uygulama oturum açma veritabanında sınırlı ve yalnızca seçili saklı yordamları yürütülecek. Uygulamanın oturum açma hiçbir doğrudan tablo erişimi olmalıdır. |

## <a id="rls-tenants"></a>Kiracıların diğer kişilerin veri erişimini engellemek için satır düzeyi güvenlik RLS uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure, OnPrem |
| **Öznitelikleri**              | SQL sürüm - V12, SQL sürümü - MsSQL2016 |
| **Başvuruları**              | [SQL Server satır düzeyi güvenlik (RLS)](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Adımları** | <p>Satır Düzeyi Güvenlik, müşterilerin bir veritabanı tablosundaki satırlara erişimi, sorguyu yürüten kullanıcının özelliklerine göre (grup üyeliği veya yürütme bağlamı) denetlemesini sağlar.</p><p>Satır düzeyi güvenlik (RLS) tasarımı ve uygulamanızda güvenlik kodlama basitleştirir. RLS, veri satırı erişiminde kısıtlama uygulamanızı sağlar. Örneğin çalışanlar yalnızca kendi bölümleriyle ilgili veri satırlarına erişmesi veya müşterilerin yalnızca kendi şirketleriyle ilgili verilere ulaşması sağlanabilir.</p><p>Veritabanı katmanı bulunur yerine başka bir uygulama katmanındaki veriler koymadan erişim kısıtlama mantığı. Veri erişimini herhangi bir katmanı denenir her zaman veritabanı sistem erişim kısıtlamalarını uygular. Bu bir güvenlik sistemi daha güvenli ve sağlam bir güvenlik sistemi'nın yüzey alanını azaltarak hale getirir.</p><p>|

Lütfen unutmayın, RLS Giden kutusu veritabanı özellik olarak yalnızca SQL Server 2016'dan başlayarak ve Azure SQL veritabanı için geçerlidir. Giden kutusu RLS özellik uygulanmıyor varsa, veri erişimi kısıtlanmış kullanarak görünümleri ve yordamları olduğunu güvence altına

## <a id="sysadmin-users"></a>Sysadmin rolünün yalnızca geçerli gerekli kullanıcıların olmalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [SQL veritabanı izinleri hiyerarşi](https://msdn.microsoft.com/library/ms191465), [SQL veritabanı güvenliği sağlanabilir öğeler](https://msdn.microsoft.com/library/ms190401) |
| **Adımları** | SysAdmin sabit sunucu rolünün üyeleri çok sınırlı ve hiçbir zaman uygulamaları tarafından kullanılan hesapları içermesi gerekir.  Lütfen roldeki kullanıcıların listesini gözden geçirin ve tüm gereksiz hesaplarını kaldırın|

## <a id="cloud-least-privileged"></a>Bulut en az ayrıcalıklı belirteçleri kullanarak ağ geçidine bağlanmak

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ağ geçidi seçim - Azure IOT Hub |
| **Başvuruları**              | [IOT hub'ı erişim denetimi](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Adımları** | Bulut ağ geçidi (IOT Hub) bağlanma çeşitli bileşenler için en az ayrıcalık izinleri sağlar. Tipik bir örnektir – aygıt yönetimi ve sağlama bileşeni registryread/yazma kullanıyorsa, hizmet bağlantı olay işlemcisi (ASA) kullanır. Tek bir cihazı Cihaz kimlik bilgilerini kullanarak bağlan|

## <a id="sendonly-sas"></a>Cihaz belirteçleri oluşturmak için bir yalnızca gönderme izinleri SAS anahtarı kullan

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Olay hub'ları kimlik doğrulaması ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | Bir SAS anahtarı tek tek cihaz belirteçleri oluşturmak için kullanılır. Belirtilen yayımcı için cihaz belirteci oluşturulurken yalnızca gönderme izinleri SAS anahtarı kullanın|

## <a id="access-tokens-hub"></a>Olay Hub'ına doğrudan erişim sağlayan erişim belirteçleri kullanmayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Olay hub'ları kimlik doğrulaması ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | Olay hub'ına doğrudan erişim veren bir belirteç cihaza verilmemelidir. Yalnızca bir yayımcı erişmenizi aygıtı belirlemek ve varsa kara yardımcı olacak için en düşük ayrıcalıklı bir belirteci kullanan bir dolandırıcı olacak şekilde bulunamadı veya cihazın güvenliği.|

## <a id="sas-minimum-permissions"></a>Olay gerekli en az izinlere sahip SAS anahtarları kullanarak Hub'ına bağlanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Olay hub'ları kimlik doğrulaması ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | Olay Hub'ına bağlanmak çeşitli arka uç uygulamalar için en az ayrıcalık izinleri sağlar. Her bir arka uç uygulama için ayrı SAS anahtarları oluştur ve Gönder, yalnızca gerekli izinleri - sağlamak alma veya bunları yönetin.|

## <a id="resource-docdb"></a>Cosmos DB mümkün olduğunca bağlanmak için kaynak belirteçleri kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure belge DB | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Kaynak belirteci DocumentDB izni kaynakla ilişkili olan ve ilişki yakalar bir veritabanı kullanıcısı ve izni arasında kullanıcının belirli bir DocumentDB uygulama kaynak için (örneğin, koleksiyon ve belge) sahip. Her zaman istemci ana veya salt okunur anahtarları - son kullanıcı uygulamayı bir mobil veya masaüstü istemcisi gibi gibi işleme ile güvenilir olamazsa DocumentDB erişmek için bir kaynak belirteci kullanın. Ana anahtar veya bu anahtarları güvenli bir şekilde depolayan arka uç uygulamalardan salt okunur tuşlarını kullanın.|

## <a id="grained-rbac"></a>Azure RBAC kullanarak abonelik için ayrıntılı erişim yönetimini etkinleştirme

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure güven sınırı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Adımları** | Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, yalnızca kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını verebilirsiniz.|

## <a id="cluster-rbac"></a>RBAC kullanarak küme işlemleri için istemci erişimi kısıtlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ortam - Azure |
| **Başvuruları**              | [Service Fabric istemciler için rol tabanlı erişim denetimi](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Adımları** | <p>Azure Service Fabric Service Fabric kümeye bağlı istemciler için iki farklı erişim denetim türlerini destekler: Yönetici ve kullanıcı. Erişim denetimi, belirli küme işlemleri farklı küme daha güvenli hale getirme kullanıcı grupları için erişimi sınırlamak Küme Yöneticisi sağlar.</p><p>Yöneticiler için yönetim özellikleri (okuma/yazma özellikleri dahil) tam erişime sahip. Kullanıcıların varsayılan olarak, yalnızca yönetim özellikleri (örneğin, sorgu özellikleri) okuma erişimi ve uygulamaları ve Hizmetleri çözümleme olanağı vardır.</p><p>Küme oluşturma sırasında her biri için ayrı sertifikaların sağlayarak iki istemci rolleri (Yönetici ve istemci) belirtin.</p>|

## <a id="modeling-field"></a>Güvenlik modelleme gerçekleştirmek ve alan düzeyinde güvenliğin kullanmanız gerektiğinde

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Güvenlik modelleme gerçekleştirmek ve alan düzeyinde güvenliğin kullanmanız gerektiğinde|

## <a id="portal-security"></a>Portal için güvenlik modeli CRM geri kalanından farklı olduğunu unutmayın tutma portal hesaplarının güvenlik modelleme gerçekleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM portalı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Portal için güvenlik modeli CRM geri kalanından farklı olduğunu unutmayın tutma portal hesaplarının güvenlik modelleme gerçekleştirin|

## <a id="permission-entities"></a>Azure Table Storage varlıklarda bir dizi hassas izin ver

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | StorageType - tablosu |
| **Başvuruları**              | [SAS kullanarak, Azure depolama hesabındaki nesnelere erişimin nasıl](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Adımları** | Azure Table Storage belirli iş senaryolarda farklı taraflara caters hassas verileri depolamak için gerekir. Farklı ülkelerden ilgili örn., hassas verileri. Verileri belirli bir ülke belirli bir kullanıcının erişebileceği şekilde bu gibi durumlarda SAS imzaları bölüm ve satır anahtarı aralıkları belirterek oluşturulabilir.| 

## <a id="rbac-azure-manager"></a>Rol tabanlı erişim denetimi (RBAC) Azure Resource Manager kullanarak Azure depolama hesabı etkinleştir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Rol tabanlı erişim denetimi (RBAC) ile depolama hesabınızın güvenliğini sağlama](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Adımları** | <p>Yeni bir depolama hesabı oluşturduğunuzda, Klasik veya Azure Resource Manager dağıtım modelini seçin. Azure kaynakları oluşturma Klasik modeli yalnızca abonelik ve ardından, depolama hesabı ya hep ya hiç erişim sağlar.</p><p>Azure Resource Manager modeli ile depolama hesabı Azure Active Directory'yi kullanarak, belirli bir depolama hesabı Yönetim düzeyi için bir kaynak grubu ve Denetim erişim yerleştirin. Örneğin, belirli kullanıcılar diğer kullanıcıların depolama hesabıyla ilgili bilgileri görüntüleyebilirsiniz, ancak depolama hesabı anahtarlarını erişemiyor depolama hesabı anahtarlarını erişim olanağı verebilirsiniz.</p>|

## <a id="rooting-detection"></a>Örtük kaçış veya algılama kök dizini değiştirme uygulanması

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Mobil istemci | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Uygulama kendi yapılandırması ve kullanıcı verilerini telefon kökü varsa durum ya da işletim sistemi engellemeleri korunması. Kök dizini değiştirme/çiğnemekten kısıtlamaları yetkisiz erişim, normal hangi kullanıcıların kendi telefonlarda olmaz anlamına gelir. Bu nedenle uygulama örtük algılama mantığı uygulama başlangıcında telefon kökü algılamak için sahip olmalıdır.</p><p>Algılama mantığı yalnızca normal olarak, yalnızca kök kullanıcı, örneğin erişebilir dosyalara erişme:</p><ul><li>/System/App/Superuser.apk</li><li>/ sbin/su</li><li>/System/bin/su</li><li>/System/xbin/su</li><li>/Data/Local/xbin/su</li><li>/Data/local/bin/su</li><li>/System/SD/xbin/su</li><li>/System/bin/failsafe/su</li><li>/Data/Local/su</li></ul><p>Uygulama bu dosyalar erişebiliyorsanız, uygulama kök kullanıcı olarak çalıştığını gösterir.</p>|

## <a id="weak-class-wcf"></a>WCF'de zayıf sınıf başvurusu

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Adımları** | <p>Sistem, bir saldırganın yetkisiz kod yürütmesine izin verebilir zayıf sınıf başvurusu kullanır. Program benzersiz olarak tanımlanmadığı kullanıcı tanımlı bir sınıf başvurur. Zayıf tanımlanan bu sınıf .NET yüklediğinde, CLR türü yükleyicisi sınıfı belirtilen sırayla aşağıdaki konumlarda arar:</p><ol><li>Yükleyici türü derleme biliniyorsa yapılandırma dosyanın yeniden yönlendirme konumları, GAC, yapılandırma bilgilerini ve uygulamanın ana dizin kullanılarak geçerli derleme arar.</li><li>Derleme bilinmiyorsa, yükleyici geçerli derleme, mscorlib ve TypeResolve olay işleyicisi tarafından döndürülen konumu arar.</li><li>Bu CLR arama sırası, tür iletme mekanizması ve AppDomain.TypeResolve olay gibi kancaları kullanılarak değiştirilebilir.</li></ol><p>Aynı ada sahip başka bir sınıf oluşturarak CLR arama sırası bir saldırganın yararlanan ve CLR ilk olarak, CLR Yükleme farklı bir konumda yerleştirme istemeden saldırgan tarafından sağlanan kodu yürütme</p>|

### <a name="example"></a>Örnek
`<behaviorExtensions/>` WCF yapılandırma dosyasının aşağıdaki öğesinin belirli bir WCF uzantısı için özel davranış sınıfı eklemek için WCF bildirir.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
Tam (tanımlayıcı) adlarının benzersiz olarak kullanarak bir türü tanımlar ve daha da sisteminizin güvenliğini artırır. Machine.config ve app.config dosya türlerini kaydetme tam nitelikli derleme adları kullanın.

### <a name="example"></a>Örnek
`<behaviorExtensions/>` WCF yapılandırma dosyasının aşağıdaki öğesinin belirli bir WCF uzantısı için özel davranış kesinlikle başvurulan sınıfı eklemek için WCF bildirir.
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

## <a id="wcf-authz"></a>WCF uygulayan yetkilendirme denetimi

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Adımları** | <p>Bu hizmet bir yetkilendirme denetimini kullanmaz. Bir istemci belirli bir WCF Hizmeti aradığında, WCF çağıranın sunucuda hizmet yöntemi yürütme izni olduğunu doğrulayın çeşitli Yetkilendirme düzeni sağlar. Kimliği doğrulanmış bir kullanıcı, yetkilendirme denetimleri için WCF hizmetleri etkinleştirilmezse ayrıcalık yükseltme elde edebilirsiniz.</p>|

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma hizmeti yürütülürken istemci yetkilendirme düzeyini denetlemek için WCF bildirir:
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
Hizmet yöntemini çağıran Bunu yapmak için yetkili olup olmadığını doğrulamak için bir hizmet Yetkilendirme düzeni kullanın. WCF iki mod sağlar ve bir özel Yetkilendirme düzeni tanımlanmasına olanak tanır. UseWindowsGroups modu Windows rolleri ve kullanıcıların ve UseAspNetRoles modu kimlik doğrulaması yapmak için SQL Server gibi bir ASP.NET rol sağlayıcıyı kullanır.

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma Ekle hizmet yürütmeden önce istemcinin Administrators grubunun bir parçası olduğundan emin olmak için WCF bildirir:
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
Hizmet, ardından aşağıdaki gibi bildirilmiş:
```
[PrincipalPermission(SecurityAction.Demand,
Role = ""Builtin\\Administrators"")]
public double Add(double n1, double n2)
{
double result = n1 + n2;
return result;
}
```

## <a id="authz-aspnet"></a>ASP.NET Web API'de uygun yetkilendirme mekanizması uygulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, MVC5 |
| **Öznitelikleri**              | Yok, kimlik sağlayıcısı - ADFS, kimlik sağlayıcısı - Azure AD |
| **Başvuruları**              | [Kimlik doğrulama ve yetkilendirme ASP.NET Web API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Adımları** | <p>Uygulama kimlik sağlayıcısı olarak üzerlerinde kullanır veya uygulama olabilir, ADFS talepleri, sağlanan veya uygulama kullanıcıları için rol bilgileri Azure AD'den elde edilebilir. Bu durumların herhangi birinde içinde özel yetkilendirme uygulama kullanıcı rolü bilgilerini doğrulamalıdır.</p><p>Uygulama kimlik sağlayıcısı olarak üzerlerinde kullanır veya uygulama olabilir, ADFS talepleri, sağlanan veya uygulama kullanıcıları için rol bilgileri Azure AD'den elde edilebilir. Bu durumların herhangi birinde içinde özel yetkilendirme uygulama kullanıcı rolü bilgilerini doğrulamalıdır.</p>

### <a name="example"></a>Örnek
```C#
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
Tüm korumalı gereken eylem yöntemlerini ve denetleyicileri ile öznitelik donatılmış.
```C#
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a id="device-permission"></a>Farklı izin düzeyleri gereken çeşitli eylemler destekliyorsa, cihaz yetkilendirme denetimleri gerçekleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT cihaz | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Cihaz çağıran arayan istenen eylemi gerçekleştirmek için gerekli izinlere sahip olup olmadığını denetle yetkilendirmeniz. Örneğin sağlar için deyin buluttan izlenebilir akıllı bir kapısını kilitleme aygıttır artı uzaktan kapısını kilitleme gibi işlevler sağlar.</p><p>Yalnızca birisi fiziksel olarak kapı bir kart ile geldiğinde akıllı kapısını kilitleme kilidini açma işlevselliği sağlar. Bu durumda, uzak komut ve denetim uyarlamasını bulut ağ geçidi kapısının kilidini açmak için bir komut göndermek için yetkili değil olarak kapısının kilidini açmak için herhangi bir işlevsellik sağlamaz şekilde yapılmalıdır.</p>|

## <a id="field-permission"></a>Farklı izin düzeyleri gereken çeşitli eylemler destekliyorsa, alan ağ geçidi yetkilendirme denetimleri gerçekleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Alan ağ geçidi arayan istenen eylemi gerçekleştirmek için gerekli izinlere sahip olup olmadığını denetle çağıran yetkilendirmeniz. İçin bir yönetici kullanıcı arabirimi/kendisine bağlanan bir alan ağ geçidi v/s cihazları yapılandırmak için kullanılan API için farklı izinler örneğin olması gerekir.|
