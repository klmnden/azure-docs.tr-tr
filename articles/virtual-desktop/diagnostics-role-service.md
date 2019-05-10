---
title: Windows sanal masaüstü Önizleme tanılama özelliği ile - Azure sorunlarını belirleyin
description: Windows sanal masaüstü Önizleme Tanılama özelliğini ve nasıl kullanılacağını açıklar.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 747e177b0fbbfb9049959c3194ee39c3234bba50
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65234021"
---
# <a name="identify-issues-with-the-diagnostics-feature"></a>Tanılama özelliğiyle sorunları belirleme

Windows sanal masaüstü Önizleme yöneticinin tek bir arabirim üzerinden sorunlarını belirlemek bir tanılama özelliği sunar. Bir kullanıcı sistemi ile etkileşime giren her Windows sanal masaüstü rolleri tanılama etkinlik oturum açın. Her günlük işlem, hata iletileri, Kiracı bilgileri ve kullanıcı bilgilerini katılan Windows sanal masaüstü rolleri gibi ilgili bilgileri içerir. Tanılama etkinlikleri, son kullanıcı ve yönetici eylemleri tarafından oluşturulur ve üç ana demetlerin içine kategorilere ayrılabilir:

* Abonelik etkinlikleri akış: kendi akış uygulamaları Microsoft Uzak Masaüstü aracılığıyla bağlanmak istediğinizde, son kullanıcı bu etkinlikler tetikler.
* Bağlantı etkinlikleri: Masaüstü veya RemoteApp için Microsoft Uzak Masaüstü uygulamalar aracılığıyla bağlanmaya çalıştıklarında her son kullanıcı bu etkinlikleri tetikler.
* Yönetim etkinlikleri: sistemdeki rol atamaları oluşturmadan konak havuzları oluşturma ve kullanıcıları gruplara uygulama atama gibi yönetim işlemlerini gerçekleştirdikleri her yönetici bu etkinlikleri tetikler.
  
Windows sanal masaüstü ulaşan olmayan bağlantıları tanılama rol hizmeti kendisi Windows sanal masaüstü bir parçası olduğundan tanılama sonuçlarında görünmez. Son kullanıcı ağ bağlantısı sorunları yaşıyor Windows Sanal Masaüstü bağlantı sorunları oluşabilir.

Başlamak için [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

## <a name="diagnose-issues-with-powershell"></a>PowerShell ile ilgili sorunları tanılayın

Windows sanal masaüstü tanılama yalnızca bir PowerShell cmdlet'ini kullanır ancak daraltın Yardım ve sorunları yalıtmak için birçok isteğe bağlı parametreler içeriyor. Aşağıdaki bölümlerde, sorunların tanılanması için faydalanılan çalıştırabileceğiniz cmdlet'leri listelenmektedir. Filtrelerin çoğu birlikte uygulanabilir. Değerleri gibi köşeli ayraç içinde listelenen `<tenantName>`, sizin durumunuz için değerleri ile değiştirilmelidir.

### <a name="retrieve-diagnostic-activities-in-your-tenant"></a>Kiracınızdaki tanılama etkinlikleri alın

Tanılama etkinlikleri girerek alabilirsiniz **Get-RdsDiagnosticActivities** cmdlet'i. Aşağıdaki örnek cmdlet için en fazla sıralanmış tanılama etkinliklerin listesini en eski döndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName>
```

Diğer Windows sanal masaüstü PowerShell cmdlet'leri gibi kullanmalısınız **- Kiracıadı** sorgunuz için kullanmak istediğiniz Kiracı adını belirtmek için parametre. Kiracı adı, neredeyse tüm tanılama etkinlik sorgular için geçerlidir.

### <a name="retrieve-detailed-diagnostic-activities"></a>Ayrıntılı tanılama etkinlikleri alın

**-Ayrıntılı** parametresi, döndürülen her tanılama etkinlik için ek ayrıntılar sağlar. Biçimi her etkinlik için etkinlik türünü bağlı olarak değişir. **-Ayrıntılı** herhangi parametresi eklenebilir **Get-RdsDiagnosticActivities** aşağıdaki örnekte gösterildiği gibi sorgu.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Detailed
```

### <a name="retrieve-a-specific-diagnostic-activity-by-activity-id"></a>Belirli bir tanılama etkinlik tarafından etkinlik kimliği alınamıyor

**- ActivityID** parametresi, belirli bir döndürür, aşağıdaki örnek cmdlet'te gösterildiği varsa tanılama etkinlik.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityId <ActivityIdGuid>
```

### <a name="filter-diagnostic-activities-by-user"></a>Kullanıcı tarafından filtre tanılama etkinlikleri

**- UserName** parametresi, aşağıdaki örnek cmdlet'te gösterildiği gibi belirtilen kullanıcı tarafından başlatılan tanılama etkinliklerin listesini döndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -UserName <UserUPN>
```

**- UserName** parametresi isteğe bağlı diğer filtre parametreleriyle da birleştirilebilir.

### <a name="filter-diagnostic-activities-by-time"></a>Tanılama etkinlikleri saate göre filtreleme

Döndürülen tanılama etkinlik listeyi filtreleyebilirsiniz **- StartTime** ve **- EndTime** parametreleri. **- StartTime** parametresi aşağıdaki örnekte gösterildiği gibi belirli bir tarihten itibaren tanılama etkinlik listesini döndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -StartTime "08/01/2018"
```

**- EndTime** parametresi, bir cmdlet ile eklenebilir **- StartTime** parametre sonuçları almak için istediğiniz zaman belirli bir süre belirtin. Aşağıdaki örnek cmdlet 1 Ağustos ve 10 Ağustos arasındaki tanılama etkinliklerin listesini döndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -StartTime "08/01/2018" -EndTime "08/10/2018"
```

**- StartTime** ve **- EndTime** parametreleri isteğe bağlı diğer filtre parametreleriyle da birleştirilebilir.

### <a name="filter-diagnostic-activities-by-activity-type"></a>Tanılama etkinlikleri etkinlik türüne göre filtrele

Tanılama etkinlikleri ile etkinlik türüne göre de filtreleyebilirsiniz **- ActivityType** parametresi. Aşağıdaki cmdlet, son kullanıcı bağlantılarının bir listesini döndürür:

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityType Connection
```

Aşağıdaki cmdlet'i yönetici yönetim görevlerinin listesini döndürür:

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityType Management
```

**Get-RdsDiagnosticActivities** cmdlet'i, akış ActivityType belirten şu anda desteklemiyor.

### <a name="filter-diagnostic-activities-by-outcome"></a>Tanılama etkinlikleri sonucuna göre filtrele

Sonucu ile tarafından döndürülen tanılama etkinlik listeyi filtreleyebilirsiniz **-sonucu** parametresi. Aşağıdaki örnek cmdlet başarılı tanılama etkinliklerin listesini döndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Outcome Success
```

Aşağıdaki örnek cmdlet başarısız tanılama etkinliklerin listesini döndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Outcome Failure
```

**-Sonucu** parametresi isteğe bağlı diğer filtre parametreleriyle da birleştirilebilir.

## <a name="common-error-scenarios"></a>Yaygın hata senaryoları

Hata senaryolarını hizmete iç ve dış sanal Windows Masaüstü için ayrılır.

* İç sorun: bir destek sorunu çözülmesi gerekir ve Kiracı Yöneticisi tarafından edilemeyecek senaryolar belirtir. Geri bildirim sağlanırken [Windows sanal masaüstü teknoloji topluluğuna](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop), etkinlik kimliği içerir ve yaklaşık, sorunun gerçekleştiği zaman çerçevesi.
* Dış sorun: Sistem Yöneticisi tarafından azaltılabilir senaryolarda ilgilidir. Windows sanal masaüstüne dış şunlardır.

Aşağıdaki tabloda, yöneticilerinize karşılaşabileceğiniz genel hatalar listelenmektedir.

>[!NOTE]
>Bu önizleme sürümü, bir tam hataların kategorilere ayrılması içermez ve düzenli olarak güncelleştirilir. En güncel bilgilere sahip olmak için bu makaleye geri aylık en az bir kez kontrol edin. emin olun.

### <a name="external-management-error-codes"></a>Dış yönetim hata kodları

|Sayısal kodu|Hata kodu|Önerilen çözüm|
|---|---|---|
|3|UnauthorizedAccess|Yönetici PowerShell cmdlet'ini çalıştırmaya çalıştığı kullanıcı bunu yapmak için izinlere sahip değil ya da kullanıcı adı yanlış yazılmış.|
|1000|TenantNotFound|Girdiğiniz Kiracı adı, var olan tüm kiracılar eşleşmiyor. Kiracı adı yazım hataları gözden geçirip yeniden deneyin.|
|1006|TenantCannotBeRemovedHasSessionHostPools|Bir kiracı, uzun nesneler içerdiğinden silinemiyor. Oturum ana bilgisayarı havuzları silin ve yeniden deneyin.|
|2000|HostPoolNotFound|Girdiğiniz ana makine havuzu adı herhangi bir mevcut konak havuzları eşleşmiyor. Yazım hatası konak havuzu adını gözden geçirin ve yeniden deneyin.|
|2005|HostPoolCannotBeRemovedHasApplicationGroups|Nesneleri içerdiği sürece bir ana bilgisayar havuzunu silemezsiniz. Tüm uygulama grupları, konak havuzda ilk kaldırın.|
|2004|HostPoolCannotBeRemovedHasSessionHosts|İlk önce oturum ana havuzuna silinmesi tüm oturumları konakları kaldırın.|
|5001|SessionHostNotFound|Sorgulanan oturumu ana bilgisayarı çevrimdışı olabilir. Konak havuzun durumunu kontrol edin.|
|5008|SessionHostUserSessionsExist |Hedeflenen yönetim etkinliklerinizi yürütülmeden önce tüm kullanıcıların oturumu ana bilgisayarı üzerinde kullanıma imzalamanız gerekir.|
|6000|AppGroupNotFound|Girdiğiniz uygulama grubu adı, mevcut tüm uygulama grupları eşleşmiyor. Uygulama grubu adı için yazım hataları gözden geçirip yeniden deneyin.|
|6022|RemoteAppNotFound|Girdiğiniz RemoteApp adı tüm RemoteApps eşleşmiyor. RemoteApp adı için yazım hataları gözden geçirip yeniden deneyin.|
|6010|PublishedItemsExist|Yayımlama çalıştığınız kaynak adı zaten var olan bir kaynak ile aynıdır. Kaynak adı değiştirin ve yeniden deneyin.|
|7002|NameNotValidWhiteSpace|Ad boşluk kullanmayın.|
|8000|InvalidAuthorizationRoleScope|Girilen rol adı, tüm mevcut rol adları eşleşmiyor. Rol adı için yazım hataları gözden geçirip yeniden deneyin. |
|8001|UserNotFound |Girdiğiniz kullanıcı adı, var olan tüm kullanıcı adları eşleşmiyor. Yazım hatası adını gözden geçirin ve yeniden deneyin.|
|8005|UserNotFoundInAAD |Girdiğiniz kullanıcı adı, var olan tüm kullanıcı adları eşleşmiyor. Yazım hatası adını gözden geçirin ve yeniden deneyin.|
|8008|TenantConsentRequired|Yönergeleri izleyerek [burada](tenant-setup-azure-active-directory.md#grant-azure-active-directory-permissions-to-the-windows-virtual-desktop-preview-service) kiracınız için rıza sağlamanın.|

### <a name="external-connection-error-codes"></a>Dış bağlantı hata kodları

|Sayısal kodu|Hata kodu|Önerilen çözüm|
|---|---|---|
|-2147467259|ConnectionFailedAdTrustedRelationshipFailure|Oturumu ana bilgisayarı doğru Active Directory'ye katılmamış.|
|-2146233088|ConnectionFailedUserHasValidSessionButRdshIsUnhealthy|Bağlantı oturumu ana bilgisayarı kullanılamadığından başarısız oldu. Oturum ana bilgisayarın sistem durumunu denetleyin.|
|-2146233088|ConnectionFailedClientDisconnect|Bu hata sık bakın, kullanıcının bilgisayarının ağa bağlı olduğundan emin olun.|
|-2146233088|ConnectionFailedNoHealthyRdshAvailable|Konak kullanıcı bağlanmaya çalıştı oturumu iyi durumda değil. Sanal makine hata ayıklayın.|
|-2146233088|ConnectionFailedUserNotAuthorized|Kullanıcı yayımlanan uygulama veya Masaüstü erişim iznine sahip değil. Yönetici yayımlanan kaynaklar kaldırıldıktan sonra hata görüntülenebilir. Uzak Masaüstü uygulama akışındaki yenilemek için kullanıcıya sor.|
|2|FileNotFound|Kullanıcının erişmeye uygulama hatalı yüklenen veya hatalı bir yola ayarlayın.|
|3|InvalidCredentials|Kullanıcı adı veya parola girilen kullanıcı mevcut kullanıcı adlarını veya parolaları eşleşmiyor. Kimlik bilgilerini yazım hataları gözden geçirip yeniden deneyin.|
|8|ConnectionBroken|İstemci ve ağ geçidi veya sunucu arasında bağlantı kesildi. Beklenmedik bir şekilde gerçekleşir sürece gereken herhangi bir işlem.|
|14|UnexpectedNetworkDisconnect|Ağ bağlantı kesildi. Yeniden bağlanmak için kullanıcıya sor.|
|24|ReverseConnectFailed|Konak sanal makine, hiçbir doğrudan görebilmesi için RD Ağ Geçidi sahiptir. Ağ geçidi IP adresini çözülebilir emin olun.|

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal masaüstü içinde rolleri hakkında daha fazla bilgi için bkz: [Windows sanal masaüstü Önizleme ortamı](environment-setup.md).

Windows sanal masaüstü için kullanılabilen PowerShell cmdlet'lerin bir listesi görmek için bkz [PowerShell başvurusu](/powershell/windows-virtual-desktop/overview).
