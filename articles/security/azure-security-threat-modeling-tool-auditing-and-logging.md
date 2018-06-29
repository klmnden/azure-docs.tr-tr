---
title: Denetim ve - Microsoft tehdit modelleme aracı - Azure günlük | Microsoft Docs
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
ms.openlocfilehash: 8837dfaf156e5a4d07598f2c58694663a9ff5580
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029990"
---
# <a name="security-frame-auditing-and-logging--mitigations"></a>Güvenlik çerçevesi: Denetim ve günlük | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Dynamics CRM**    | <ul><li>[Çözümünüzdeki hassas varlıkları tanımlamak ve değişiklik denetimi uygulama](#sensitive-entities)</li></ul> |
| **Web uygulaması** | <ul><li>[Denetim ve günlük uygulamada zorlanan emin olun](#auditing)</li><li>[Günlük rotasyonunun ve ayırma yerinde olduğundan emin olun](#log-rotation)</li><li>[Uygulama hassas kullanıcı verilerini günlüğe kaydetmez emin olun](#log-sensitive-data)</li><li>[Denetim ve günlük dosyalarını sınırlı erişimi olduğundan emin olun](#log-restricted-access)</li><li>[Kullanıcı Yönetimi olayları oturum açtığınızdan emin olun](#user-management)</li><li>[Sistemi olduğundan emin olun kötüye karşı yerleşik savunma](#inbuilt-defenses)</li><li>[Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme](#diagnostics-logging)</li></ul> |
| **Veritabanı** | <ul><li>[SQL Server'da oturum açma denetimi etkin olduğundan emin olun](#identify-sensitive-entities)</li><li>[Azure SQL tehdit algılamayı etkinleştir](#threat-detection)</li></ul> |
| **Azure Depolama** | <ul><li>[Azure Storage erişimi denetlemek için Azure Storage Analytics kullanın](#analytics)</li></ul> |
| **WCF** | <ul><li>[Yeterli günlük kaydı uygulayın](#sufficient-logging)</li><li>[Uygulama yeterli denetim hata işleme](#audit-failure-handling)</li></ul> |
| **Web API** | <ul><li>[Denetim ve günlük, Web API uygulanmasını sağlamak](#logging-web-api)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[Uygun denetim ve günlük alan ağ geçidinde zorlanır emin olun](#logging-field-gateway)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Uygun denetim ve günlük bulut ağ geçidinde zorlanır emin olun](#logging-cloud-gateway)</li></ul> |

## <a id="sensitive-entities"></a>Çözümünüzdeki hassas varlıkları tanımlamak ve değişiklik denetimi uygulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Dynamics CRM | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | Hassas veri içeren çözümünüzdeki varlıkları tanımlamak ve o varlıklar ile alanları değişikliğini denetleme uygulamak |

## <a id="auditing"></a>Denetim ve günlük uygulamada zorlanan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | Denetim ve tüm bileşenlerini günlük etkinleştirin. Denetim günlüklerini kullanıcı bağlamı yakalama. Tüm önemli olayları belirleyin ve bu olayları günlüğe kaydedin. Merkezi günlük kaydı uygulayın |

## <a id="log-rotation"></a>Günlük rotasyonunun ve ayırma yerinde olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Günlük rotasyonunun tarihli günlük dosyalarını arşivlenmiş sistem yönetiminde kullanılan otomatik bir işlemdir. Büyük uygulamalar genellikle çalıştırdığı sunucuları oturum her istek: hantal günlükleri karşısında günlük rotasyonunun hala son olayların analizini verirken günlükleri toplam boyutunu sınırlamak için bir yoldur. </p><p>Oturum ayrımı temelde günlüğünüzün depolamak için kullandığınız anlamına gelir, işletim sistemi/uygulama bir hizmet reddi saldırısına ya da uygulamanızın veya eski sürüme düşürmeyi avert için çalıştığı olarak farklı bir bölüme performansını dosyaları</p>|

## <a id="log-sensitive-data"></a>Uygulama hassas kullanıcı verilerini günlüğe kaydetmez emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Siteniz için bir kullanıcının gönderdiğini herhangi bir duyarlı veri günlük tutma denetleyin. Tasarım sorunları neden yan etkileri yanı sıra kasıtlı günlüğünü denetleyin. Gizli verilerin örnekleri şunlardır:</p><ul><li>Kullanıcı Kimlik Bilgileri</li><li>Sosyal Güvenlik numarası veya başka tanımlayıcı bilgiler</li><li>Kredi kartı numaraları veya diğer finansal bilgileri</li><li>Sistem durumu bilgileri</li><li>Özel anahtarlar veya şifrelenmiş bilgilerin şifresini çözmek için kullanılabilecek diğer veri</li><li>Uygulama daha etkili bir şekilde saldırmak için kullanılan sistem veya uygulama bilgileri</li></ul>|

## <a id="log-restricted-access"></a>Denetim ve günlük dosyalarını sınırlı erişimi olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Günlük dosyalarına erişim hakları uygun şekilde ayarlandığından emin olun. Uygulama hesapları yalnızca yazma erişimi ve işleçler olması gerekir ve destek personeli, gerektiği gibi salt okunur erişimi olması gerekir.</p><p>Yöneticiler, tam erişimi yalnızca hesapları hesaplarıdır. Günlük dosyaları düzgün kısıtlı olduklarından emin olmak için Windows ACL denetleyin:</p><ul><li>Uygulama hesapları yalnızca yazma erişimi olması gerekir</li><li>İşleçler ve destek personeli salt okunur erişim gerektiğinde olmalıdır</li><li>Yöneticiler tam erişime sahip olması gereken yalnızca hesaplardır</li></ul>|

## <a id="user-management"></a>Kullanıcı Yönetimi olayları oturum açtığınızdan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Uygulama kullanıcı başarılı ve başarısız oturum açma bilgileri gibi kullanıcı yönetimi olayları izler, parolasını sıfırlar, parola değişiklikleri, hesap kilitleme, kullanıcı kaydı emin olun. Algılamak ve şüpheli olabilecek davranışları tepki vermek için bu yardımcı yapılıyor. Ayrıca operations veri toplamak üzere sağlar; Uygulama erişen izlemek için örnek için</p>|

## <a id="inbuilt-defenses"></a>Sistemi olduğundan emin olun kötüye karşı yerleşik savunma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları**                   | <p>Denetimler, uygulama kötüye kullanılması durumunda güvenlik özel durum yerinde olmalıdır. Örneğin, giriş doğrulaması yerinde olduğundan ve bir saldırgan regex eşleşmiyor kötü amaçlı kod ekleme girişiminde, bir güvenlik özel durumu, sistemin kötüye kullanımı bir vurgulayan olabilen atılabilen</p><p>Örneğin, güvenlik özel durumları günlüğe ve aşağıdaki sorunlar için yapılan eylemler için önerilir:</p><ul><li>Giriş doğrulaması</li><li>CSRF ihlalleri</li><li>Deneme yanılma saldırısı (kullanıcı kaynak başına istek sayısı için üst sınır)</li><li>Dosya karşıya yükleme ihlalleri</li><ul>|

## <a id="diagnostics-logging"></a>Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | EnvironmentType - Azure |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Azure, bir App Service web uygulamasının hatalarını ayıklamaya yardımcı olmak üzere yerleşik tanılama sağlar. Ayrıca, API uygulamaları ve mobil uygulamalar için de geçerlidir. App Service web uygulamalarının web sunucusu ve web uygulamasının içinden bilgileri günlüğe kaydetme için tanılama işlevsellik sağlar.</p><p>Bu web sunucusu tanılama ve uygulama tanılama mantıksal olarak ayrılır</p>|

## <a id="identify-sensitive-entities"></a>SQL Server'da oturum açma denetimi etkin olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Oturum açma denetimi yapılandırma](https://msdn.microsoft.com/library/ms175850.aspx) |
| **Adımları** | <p>Veritabanı sunucusu oturum açma denetimi algılamak ve Onayla parola saldırılarını tahmin etkinleştirilmesi gerekir. Başarısız oturum açma denemesi yakalamak önemlidir. Hem başarılı ve başarısız oturum açma denemesi yakalama sırasında adli incelemelere ek bir fayda sağlar</p>|

## <a id="threat-detection"></a>Azure SQL tehdit algılamayı etkinleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure |
| **Öznitelikleri**              | SQL sürümü - V12 |
| **Başvuruları**              | [SQL veritabanı tehdit algılama ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| **Adımları** |<p>Tehdit algılama veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algılar. Yeni bir katman müşterilerine algılamak ve anormal etkinlikler güvenlik uyarıları sağlayarak göründüklerinde olası risklere yanıt sağlayan bir güvenlik sağlar.</p><p>Kullanıcılar, bunlar erişim, ihlal ya da veritabanındaki verileri yararlanma girişimi sonucu olmadığını belirlemek için Azure SQL veritabanı denetimi kullanarak şüpheli olayları gözden geçirebilirsiniz.</p><p>Tehdit algılama, veritabanına Uzman güvenlik olması veya sistemleri izleme Gelişmiş Güvenlik yönetmek zorunda kalmadan adresi olası tehditlere kolaylaştırır</p>|

## <a id="analytics"></a>Azure Storage erişimi denetlemek için Azure Storage Analytics kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok |
| **Başvuruları**              | [Storage Analytics Yetkilendirme türü izlemek için kullanma](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| **Adımları** | <p>Her Depolama hesabı için bir günlük kaydı gerçekleştirmek ve ölçüm verilerini depolamak Azure Storage Analytics etkinleştirebilirsiniz. Storage analytics günlükleri depolama eriştiklerinde birisi tarafından kullanılan kimlik doğrulama yöntemi gibi önemli bilgileri sağlar.</p><p>Bu depolama alanına erişimi sıkı bir şekilde kullanılan koruyarak gerçekten yardımcı olabilir. Örneğin, Blob depolama alanına tüm kapsayıcıların özel ayarlayın ve uygulamalarınızı bir SAS hizmet kullanımını uygulayın. Ardından, düzenli olarak bloblarınızın bir güvenlik açığını gösterebilir, depolama hesabı anahtarları kullanılarak erişilir mı yoksa BLOB ortak ancak bunlar olmamalıdır görmek için günlüklere denetleyebilirsiniz.</p>|

## <a id="sufficient-logging"></a>Yeterli günlük kaydı uygulayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_insufficient_logging) |
| **Adımları** | <p>Bir güvenlik olayı sonra uygun denetim izi eksikliği adli çaba engel olabilir. Windows Communication Foundation (WCF) başarılı ve/veya başarısız kimlik doğrulama girişimlerini oturum olanağı sunar.</p><p>Başarısız kimlik doğrulama girişimlerini günlüğe olası kaba kuvvet saldırıları yöneticileri uyarır. Yasal bir hesap güvenliği aşıldığında benzer şekilde, başarılı kimlik doğrulama olaylarını günlüğe yararlı denetim izi sağlayabilir. WCF'ın hizmeti güvenlik denetim özelliğini etkinleştir |

### <a name="example"></a>Örnek
Denetim etkin olan bir örnek yapılandırma aşağıda verilmiştir
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

## <a id="audit-failure-handling"></a>Uygulama yeterli denetim hata işleme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Krallık Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_insufficient_audit_failure_handling) |
| **Adımları** | <p>Geliştirilmiş çözüm bir denetim günlüğüne yazılması başarısız olduğunda bir özel durum oluşturmak değil üzere yapılandırılmıştır. WCF bir denetim günlüğüne yazamadığını olduğunda bir özel durum değil için yapılandırılmışsa, program hatasını bildirilmez ve kritik güvenlik olaylarının denetlenmesi gerçekleşmeyebilir.</p>|

### <a name="example"></a>Örnek
`<behavior/>` Öğesi aşağıdaki WCF yapılandırma dosyasının bir denetim günlüğüne yazılması WCF başarısız olduğunda uygulamayı bilgilendirecek değil WCF bildirir.
````
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
````
Bir denetim günlüğüne yazamadığını olduğunda program bildirmek için WCF yapılandırın. Program bir alternatif bildirim düzeni denetim izlemeyi kuruluş korunmaz uyarı yerinde olması gerekir. 

## <a id="logging-web-api"></a>Denetim ve günlük, Web API uygulanmasını sağlamak

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Denetim ve günlük Web API'leri etkinleştirin. Denetim günlüklerini kullanıcı bağlamı yakalama. Tüm önemli olayları belirleyin ve bu olayları günlüğe kaydedin. Merkezi günlük kaydı uygulayın |

## <a id="logging-field-gateway"></a>Uygun denetim ve günlük alan ağ geçidinde zorlanır emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | <p>Birden çok aygıt bir alan ağ geçidi bağladığınızda, bağlantı denemeleri ve bireysel aygıtlar için kimlik doğrulama durumu (başarı veya başarısızlık) oturum açmış ve alan ağ geçidi üzerinde tutulan emin olun.</p><p>Ayrıca, alan ağ geçidi tek bir cihazı IOT Hub kimlik bilgileri burada koruma durumlarda bu kimlik bilgileri alınırken denetim özelliğinin gerçekleştirildiğinden emin olun. Uzun vadeli bekletme için Azure IOT hub'ı / depolama için düzenli aralıklarla günlükleri karşıya yüklemek için bir işlem geliştirin.</p> |

## <a id="logging-cloud-gateway"></a>Uygun denetim ve günlük bulut ağ geçidinde zorlanır emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [IOT hub'ı operations izleme giriş](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| **Adımları** | <p>Toplama ve IOT Hub işlemlerini izleme ile toplanan denetim verilerini depolamak için tasarlayın. Aşağıdaki izleme kategorileri etkinleştirin:</p><ul><li>Aygıt Kimliği işlemleri</li><li>Cihaz bulut iletişimleri</li><li>Bulut-cihaz iletişimi</li><li>Bağlantılar</li><li>Dosya yüklemeleri</li></ul>|