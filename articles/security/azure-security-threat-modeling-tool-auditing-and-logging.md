---
title: Denetim ve günlüğe kaydetme - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
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
ms.openlocfilehash: 4843828c89b04e36b0bcc73dcedf9c5735b73729
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610851"
---
# <a name="security-frame-auditing-and-logging--mitigations"></a>Güvenlik çerçevesi: Denetim ve günlüğe kaydetme | Risk azaltma işlemleri 

| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Dynamics CRM**    | <ul><li>[Çözümünüzdeki hassas varlıkları tanımlamak ve değişiklik denetimi uygulayın](#sensitive-entities)</li></ul> |
| **Web uygulaması** | <ul><li>[Denetim ve günlüğe kaydetme, uygulamada zorlanan emin olun.](#auditing)</li><li>[Günlük rotasyonu ve ayırma yerinde olduğundan emin olun](#log-rotation)</li><li>[Uygulama hassas kullanıcı verilerini günlüğe kaydetmez emin olun.](#log-sensitive-data)</li><li>[Denetim ve günlük dosyaları sınırlı erişimi olduğundan emin olun](#log-restricted-access)</li><li>[Kullanıcı yönetimi olaylarını açtığınızdan emin olun](#user-management)</li><li>[Sistemi olduğundan emin olun kötüye karşı yerleşik savunma](#inbuilt-defenses)</li><li>[Azure App Service'te web apps için tanılama günlüğünü etkinleştirme](#diagnostics-logging)</li></ul> |
| **Veritabanı** | <ul><li>[SQL Server'da oturum açma denetimi etkin olduğundan emin olun](#identify-sensitive-entities)</li><li>[Azure SQL tehdit algılamayı etkinleştirme](#threat-detection)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure depolama erişimi denetlemek için Azure depolama analizi kullanma](#analytics)</li></ul> |
| **WCF** | <ul><li>[Yeterli günlük kaydı uygulayın](#sufficient-logging)</li><li>[Yeterli denetim hata işleme uygulama](#audit-failure-handling)</li></ul> |
| **Web API** | <ul><li>[Web API, Denetim ve günlük uygulandığını emin olun.](#logging-web-api)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[Alan ağ geçidi üzerinde uygun denetim ve günlük uygulandığını emin olun.](#logging-field-gateway)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Bulut ağ geçidi, uygun denetim ve günlük uygulandığını emin olun.](#logging-cloud-gateway)</li></ul> |

## <a id="sensitive-entities"></a>Çözümünüzdeki hassas varlıkları tanımlamak ve değişiklik denetimi uygulayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | Hassas veriler içeren çözümünüzdeki varlıkları tanımlamak ve bu varlıklarını ve alanlarını değişiklik denetimi uygulayın |

## <a id="auditing"></a>Denetim ve günlüğe kaydetme, uygulamada zorlanan emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | Denetim ve tüm bileşenlere günlük etkinleştirin. Denetim günlükleri, kullanıcı bağlamı yakalamalısınız. Tüm önemli olayları tanımlamak ve olayları günlüğe kaydedin. Merkezi günlük kaydı uygulayın |

## <a id="log-rotation"></a>Günlük rotasyonu ve ayırma yerinde olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Günlük rotasyonu tarihli günlük dosyalarını arşivlenen sistem yönetiminde kullanılan otomatik bir işlemdir. Büyük uygulamalar genellikle çalışan sunucular oturum her istek: hantal günlükleri karşılaşıldığında, günlük analizi en son olayların hala izin verirken günlükleri toplam boyutunu sınırlamak için bir yoldur. </p><p>Oturum ayrımı temelde, günlüğünüzün depolamak için sahip olduğunuz anlamına gelir işletim sistemi/uygulama için bir hizmet reddi saldırısına veya uygulamanızın veya eski sürüme düşürme engellemeye çalıştığı olarak farklı bir bölüme performansını dosyaları</p>|

## <a id="log-sensitive-data"></a>Uygulama hassas kullanıcı verilerini günlüğe kaydetmez emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Sitenize bir kullanıcının gönderdiğini hassas verileri günlüğe kaydetme onay. Kasıtlı günlüğe kaydetme ve bunun yanı sıra yan etkileri tasarım sorunları neden olup olmadığını denetleyin. Hassas verilerin örnekleri şunlardır:</p><ul><li>Kullanıcı Kimlik Bilgileri</li><li>Sosyal Güvenlik numarası veya diğer tanımlama bilgileri</li><li>Kredi kartı numaraları veya diğer finansal bilgi</li><li>Sistem durumu bilgileri</li><li>Özel anahtarlar veya şifrelenmiş bilgilerin şifresini çözmek için kullanılabilecek diğer veri</li><li>Uygulama daha etkili bir şekilde saldırmak için kullanılacak sistem veya uygulama bilgileri</li></ul>|

## <a id="log-restricted-access"></a>Denetim ve günlük dosyaları sınırlı erişimi olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Günlük dosyalarına erişim hakları uygun şekilde ayarlandığından emin olun. Salt yazma erişimi ve işleçleri uygulama hesapları olmalıdır ve destek personeli, gerektiği şekilde salt okunur erişimine sahip olmalıdır.</p><p>Yöneticiler, tam erişimi olması gereken tek hesapları hesaplarıdır. Günlük dosyaları düzgün şekilde kısıtlı olduklarından emin olmak için Windows ACL denetleyin:</p><ul><li>Uygulama hesaplarını salt yazma erişimine sahip olmalıdır</li><li>İşleçler ve destek personelinin gerektiği şekilde salt okunur erişimi olmalıdır</li><li>Yöneticiler tam erişimi olması gereken yalnızca hesaplarıdır</li></ul>|

## <a id="user-management"></a>Kullanıcı yönetimi olaylarını açtığınızdan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Uygulama, başarılı ve başarısız kullanıcı oturumu açma gibi kullanıcı yönetimi olayları izler ve parola sıfırlamaları parola değişiklikleri, hesap kilitleme, kullanıcı kaydı emin olun. Bu yardımcı olan algılama ve tepki vermek için şüpheli olabilecek davranışları yapılıyor. Ayrıca operations veri toplamak üzere sağlar; Uygulama erişen izlemek için örneğin,</p>|

## <a id="inbuilt-defenses"></a>Sistemi olduğundan emin olun kötüye karşı yerleşik savunma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Denetimler, hangi uygulamanın kötüye kullanılması durumunda güvenlik durum yerinde olmalıdır. Örneğin, giriş doğrulaması yerinde olduğundan ve bir saldırganın normal ifade eşleşmiyor kötü amaçlı kod ekleme girişiminde, bir güvenlik özel durumu, sistemin kötüye kullanımı bir vurgulayan olabilen atılabilir</p><p>Örneğin, güvenlik özel durumları günlüğe ve aşağıdaki sorunlar için gerçekleştirilen eylemleri önerilir:</p><ul><li>Giriş doğrulaması</li><li>CSRF ihlalleri</li><li>Deneme yanılma (kaynak başına kullanıcı başına istek sayısı için üst sınır)</li><li>Dosya karşıya yükleme ihlali</li><ul>|

## <a id="diagnostics-logging"></a>Azure App Service'te web apps için tanılama günlüğünü etkinleştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | EnvironmentType - Azure |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Azure, bir App Service web uygulamasının hatalarını ayıklamaya yardımcı olmak üzere yerleşik tanılama sağlar. Ayrıca, API uygulamaları ve mobil uygulamalar için de geçerlidir. App Service web apps için web sunucusunu hem web uygulamasının içinden bilgileri günlüğe kaydetme tanılama işlevi sağlar.</p><p>Bu web sunucusu tanılama ve uygulama tanılama mantıksal olarak ayrılır</p>|

## <a id="identify-sensitive-entities"></a>SQL Server'da oturum açma denetimi etkin olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Oturum açma denetimi yapılandırma](https://msdn.microsoft.com/library/ms175850.aspx) |
| **Adımları** | <p>Veritabanı sunucusu oturum açma denetimi algılamak ve onaylayın parola saldırılarını tahmin etkinleştirilmesi gerekir. Başarısız oturum açma girişimlerini yakalamak önemlidir. Hem başarılı ve başarısız oturum açma denemesi yakalama adli araştırma sırasında ek bir fayda sağlar</p>|

## <a id="threat-detection"></a>Azure SQL tehdit algılamayı etkinleştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure |
| **Öznitelikler**              | SQL sürümü - V12 |
| **Başvuruları**              | [SQL veritabanı tehdit algılamayı kullanmaya başlama](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| **Adımları** |<p>Tehdit algılama, veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. Bu yeni katmanı müşterilerin anormal etkinliklerde güvenlik uyarıları sağlayarak oluşunca potansiyel tehditleri algılayıp güvenlik sağlar.</p><p>Kullanıcılar, erişim, güvenlik ihlali veya veritabanındaki verileri açığından yararlanma girişimi sonucunda varsa belirlemek için Azure SQL veritabanı denetimini kullanarak şüpheli etkinlikleri araştırabilirsiniz.</p><p>Tehdit algılama olmadan Uzman güvenlik veya Gelişmiş Güvenlik izleme sistemlerine ihtiyaç duymadan potansiyel tehditlerle kolaylaştırır</p>|

## <a id="analytics"></a>Azure depolama erişimi denetlemek için Azure depolama analizi kullanma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok |
| **Başvuruları**              | [Yetkilendirme türünü izlemek için depolama analizi kullanma](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| **Adımları** | <p>Her Depolama hesabı için bir Azure depolama analizi, günlük gerçekleştirmek ve ölçüm verilerini depolamak etkinleştirebilirsiniz. Depolama analizi günlüklerinde depolama eriştiklerinde birisi tarafından kullanılan kimlik doğrulama yöntemi gibi önemli bilgileri sağlar.</p><p>Bu, depolama alanına erişimi sıkı bir şekilde kullanılan koruyarak, gerçekten çok yardımcı olabilir. Örneğin, Blob Depolama alanında tüm kapsayıcıları private olarak ayarlayın ve bir SAS hizmet kullanımını uygulamalarınız uygulayın. Ardından bir güvenlik ihlalini gösterebilir, depolama hesabı anahtarlarını kullanarak bloblarınızın erişilen veya bloblar herkese açık, ancak bunlar olmamalıdır düzenli olarak görmek için günlükleri gözden geçirebilirsiniz.</p>|

## <a id="sufficient-logging"></a>Yeterli günlük kaydı uygulayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_insufficient_logging) |
| **Adımları** | <p>Bir güvenlik olayı sonra bir uygun denetim izi eksikliği adli çalışmalarını engel olabilir. Windows Communication Foundation (WCF) başarılı ve/veya başarısız kimlik doğrulama girişimlerini de günlüğe olanağı sunar.</p><p>Başarısız kimlik doğrulama girişimlerini günlüğe olası deneme yanılma saldırıları yöneticileri uyarır. Yasal bir hesap tehlikede benzer şekilde, başarılı kimlik doğrulama olaylarını günlüğe bir faydalı bir denetim izi sağlayabilir. WCF'ın hizmeti güvenlik denetim özelliğini etkinleştir |

### <a name="example"></a>Örnek
Denetimin etkin örnek bir yapılandırma verilmiştir
```
<system.serviceModel>
    <behaviors>
        <serviceBehaviors>
            <behavior name=""NewBehavior"">
                <serviceSecurityAudit auditLogLocation=""Default""
                suppressAuditFailure=""false"" 
                serviceAuthorizationAuditLevel=""SuccessAndFailure""
                messageAuthenticationAuditLevel=""SuccessAndFailure"" />
                ...
            </behavior>
        </servicebehaviors>
    </behaviors>
</system.serviceModel>
```

## <a id="audit-failure-handling"></a>Yeterli denetim hata işleme uygulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_insufficient_audit_failure_handling) |
| **Adımları** | <p>Geliştirilmiş çözüm, bir denetim günlüğüne yazmak başarısız olduğunda bir özel durum oluşturmamanız için yapılandırılır. WCF bir denetim günlüğüne yazamadığını olduğunda bir özel durum oluşturması beklenmiyor yapılandırılmışsa, programın hatasını bildirilmez ve kritik güvenlik olaylarının denetlenmesini algılanamayabilir.</p>|

### <a name="example"></a>Örnek
`<behavior/>` WCF yapılandırma dosyasına aşağıdaki öğesi bir denetim günlüğüne yazmak WCF başarısız olduğunda uygulamaya bildirmek değil için WCF bildirir.
```
<behaviors>
    <serviceBehaviors>
        <behavior name="NewBehavior">
            <serviceSecurityAudit auditLogLocation="Application"
            suppressAuditFailure="true"
            serviceAuthorizationAuditLevel="Success"
            messageAuthenticationAuditLevel="Success" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Bir denetim günlüğüne yazamadığını olduğunda programın bildirmek için WCF yapılandırın. Programın denetim izlemeyi kuruluş korunmaz uyarı yerinde bir alternatif bildirim şeması olmalıdır. 

## <a id="logging-web-api"></a>Web API, Denetim ve günlük uygulandığını emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Denetim ve Web API'leri günlük etkinleştirin. Denetim günlükleri, kullanıcı bağlamı yakalamalısınız. Tüm önemli olayları tanımlamak ve olayları günlüğe kaydedin. Merkezi günlük kaydı uygulayın |

## <a id="logging-field-gateway"></a>Alan ağ geçidi üzerinde uygun denetim ve günlük uygulandığını emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Bir alan ağ geçidi için birden çok cihazı bağladığınızda, bağlantı denemeleri ve tek tek cihazlar için kimlik doğrulama durumu (başarı veya hata) günlüğe kaydedilir ve alan ağ geçidi üzerinde saklanır emin olun.</p><p>Ayrıca, alan ağ geçidi tek tek cihazlar IOT Hub kimlik bilgilerini nerede koruma durumlarda, bu kimlik bilgileri alınırken denetim yapıldığından emin olun. Uzun süreli saklama için Azure IOT hub'ı / depolama için düzenli aralıklarla günlükleri karşıya yüklemek için bir işlem geliştirin.</p> |

## <a id="logging-cloud-gateway"></a>Bulut ağ geçidi, uygun denetim ve günlük uygulandığını emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [IOT Hub işlemlerini izleme giriş](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| **Adımları** | <p>IOT Hub işlemlerini izleme yoluyla toplanan denetim verileri toplama ve depolama için tasarlayın. Aşağıdaki izleme kategorileri etkinleştir:</p><ul><li>Cihaz kimlik işlemleri</li><li>CİHAZDAN buluta iletişimleri</li><li>Bulut-cihaz iletişimi</li><li>Bağlantılar</li><li>Dosya yüklemeleri</li></ul>|