---
title: İzleme ve Azure AD parola koruması - Azure Active Directory günlüğe kaydetme
description: İzleme ve günlüğe kaydetme Azure AD parola koruması anlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae2d18541788e769e4f1b44319aa1be200921b88
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437569"
---
# <a name="azure-ad-password-protection-monitoring-and-logging"></a>Azure AD parola izleme ve günlüğe kaydetme koruması

Azure AD parola koruması dağıtıldıktan sonra izleme ve raporlama temel görevleridir. Bu makalede olduğu her hizmet bilgilerini kaydeder ve Azure AD parola koruması kullanımını bildirme dahil olmak üzere çeşitli izleme teknikleri anlamanıza yardımcı olmak üzere ayrıntıya gider.

İzleme ve raporlama ya da olay günlüğü iletilerini veya PowerShell cmdlet'lerini çalıştırarak gerçekleştirilir. DC aracı ve proxy hizmetleri her iki olay günlüğü iletilerini günlüğe kaydet. Aşağıda açıklanan tüm PowerShell cmdlet'leri yalnızca ara sunucusunda kullanılabilir (AzureADPasswordProtection PowerShell modülüne bakın). Bir PowerShell modülü DC Aracısı yazılım yüklemez.

## <a name="dc-agent-event-logging"></a>DC agent olay günlüğü

Her etki alanı denetleyicisinde DC aracısı hizmet yazılımının her bireysel parola doğrulama işlemi (ve diğer durum) sonuçlarının bir yerel olay günlüğüne yazar:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

DC aracı yönetim günlüğü yazılımı nasıl davrandığını bilgileri birincil kaynağıdır.

İzleme günlüğü varsayılan olarak devre dışı olduğuna dikkat edin.

Çeşitli DC aracı bileşenler tarafından günlüğe kaydedilen olayları aşağıdaki aralıklar ayrılır:

|Bileşen |Olay Kimliği aralığı|
| --- | --- |
|DC aracı parola filtresi DLL'sinin| 10000-19999|
|DC Aracısı hizmeti barındırma işlemi| 20000-29999|
|DC aracısı hizmet İlkesi Doğrulama mantığı| 30000-39999|

## <a name="dc-agent-admin-event-log"></a>DC aracı Yöneticisi olay günlüğü

### <a name="password-validation-outcome-events"></a>Parola doğrulama sonucu olayları

Her etki alanı denetleyicisinde DC aracısı hizmet yazılımının her bireysel parola doğrulama sonuçlarını DC aracı yönetici olay günlüğüne yazar.

Başarılı parola doğrulama işlemi için genellikle DC aracı parola filtresi DLL'den günlüğe bir olay yok. İçin başarısız parola doğrulama işlemi vardır genellikle iki olayları günlüğe, bir DC Aracı hizmeti ve DC aracı parola filtresi DLL'sinin birinden.

Geçici bir çözüm aşağıdaki etkenlere göre bu durumlar yakalamak için ayrı olaylar günlüğe kaydedilir:

* Belirli bir parola olup ettiğinden ayarlayın veya değiştirildi.
* Olup verilen parola doğrulaması geçti veya başarısız oldu.
* Doğrulama Microsoft Genel ilke, kuruluş ilkesi veya bir bileşimi nedeniyle başarısız oldu.
* Yalnızca denetim modunda şu anda açıp geçerli parola ilkesini olup olmadığı.

Anahtar parolası doğrulama ile ilgili olayları aşağıdaki gibidir:

|   |Parola değiştirme |Parola ayarlama|
| --- | :---: | :---: |
|Geçiş |10014 |10015|
|Başarısız (müşteri parola ilkesi nedeniyle)| 10016, 30002| 10017, 30003|
|(Microsoft parola ilkesi nedeniyle) başarısız| 10016, 30004| 10017, 30005|
|(Birleşik Microsoft ve müşteri parola ilkeleri nedeniyle) başarısız| 10016, 30026| 10017, 30027|
|Yalnızca denetim geçişi (müşteri parola ilkesi başarısız)| 10024, 30008| 10025, 30007|
|Yalnızca denetim geçişi (Microsoft parola ilkesi başarısız)| 10024, 30010| 10025, 30009|
|Yalnızca denetim geçişi (Birleşik Microsoft ve müşteri parola ilkeleri başarısız olurdu)| 10024, 30028| 10025, 30029|

"İlkeleri birleştirilmiş" bakın yukarıdaki tabloda durumlarda, hem Microsoft yasaklanmış parola listesi, hem de müşteri yasaklanmış parola listesi en az bir belirteci içeren bir kullanıcının parolasını bulunduğu durumlar için başvuruyordur.

Olay çifti birlikte günlüğe kaydedildiğinde, her iki olay açıkça aynı Correlationıd sağlayarak ilişkilendirilir.

### <a name="password-validation-summary-reporting-via-powershell"></a>PowerShell raporlama parolası doğrulama özeti

`Get-AzureADPasswordProtectionSummaryReport` Cmdlet'i, parola doğrulama etkinliği bir Özet görünümünü oluşturmak için kullanılabilir. Bu cmdlet'in bir örnek çıktısı aşağıdaki gibidir:

```PowerShell
Get-AzureADPasswordProtectionSummaryReport -DomainController bplrootdc2
DomainController                : bplrootdc2
PasswordChangesValidated        : 6677
PasswordSetsValidated           : 9
PasswordChangesRejected         : 10868
PasswordSetsRejected            : 34
PasswordChangeAuditOnlyFailures : 213
PasswordSetAuditOnlyFailures    : 3
PasswordChangeErrors            : 0
PasswordSetErrors               : 1
```

Cmdlet'in raporlama kapsamı kullanarak etkilenebilir orman, - etki alanı veya – DomainController parametre. Bir parametre belirtmeden gelir – orman.

`Get-AzureADPasswordProtectionSummaryReport` DC aracı Yöneticisi olay günlüğü sorgulama ve ardından her görüntülenen sonuç kategoriye karşılık gelen olayların toplam sayısını sayma cmdlet'i çalışır. Aşağıdaki tabloda her sonucunu ve ilgili olay kimliği arasındaki eşlemeleri içerir:

|Get-AzureADPasswordProtectionSummaryReport özelliği |İlgili olay kimliği|
| :---: | :---: |
|PasswordChangesValidated |10014|
|PasswordSetsValidated |10015|
|PasswordChangesRejected |10016|
|PasswordSetsRejected |10017|
|PasswordChangeAuditOnlyFailures |10024|
|PasswordSetAuditOnlyFailures |10025|
|PasswordChangeErrors |10012|
|PasswordSetErrors |10013|

Unutmayın `Get-AzureADPasswordProtectionSummaryReport` cmdlet'i, PowerShell komut dosyası biçiminde gönderildiği ve Mayıs gerekirse başvuru doğrudan şu konumda:

`%ProgramFiles%\WindowsPowerShell\Modules\AzureADPasswordProtection\Get-AzureADPasswordProtectionSummaryReport.ps1`

> [!NOTE]
> Bu cmdlet her etki alanı denetleyicisi için bir PowerShell oturumu açarak çalışır. Başarılı olması için her etki alanı denetleyicisinde PowerShell uzak oturum desteği etkinleştirilmeli ve istemci yeterli ayrıcalıklara sahip olmalıdır. PowerShell uzak oturum gereksinimleri hakkında daha fazla bilgi için 'Get-Help about_Remote_Troubleshooting' bir PowerShell penceresinde çalıştırın.

> [!NOTE]
> Bu cmdlet her DC aracı hizmetinin yönetici olay günlüğünü uzaktan sorgulayarak çalışır. Olay günlükleri, olaylar çok sayıda içeriyorsa, cmdlet tamamlanması uzun zaman alabilir. Ayrıca, toplu ağ sorguları büyük veri kümeleri, etki alanı denetleyicisi performansını etkileyebilir. Bu nedenle, bu cmdlet üretim ortamlarında dikkatli kullanılmalıdır.

### <a name="sample-event-log-message-for-event-id-10014-successful-password-change"></a>Örnek olay günlüğü iletisi olay kimliği 10014 (başarılı parola değiştirme)

```text
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Örnek olay günlüğü iletisi olay kimliği 10017 ve 30003 (başarısız parola küme)

10017:

```text
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```text
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Örnek olay günlüğü iletisi olay kimliği 30001 (hiçbir ilke kullanılabilir nedeniyle kabul parola)

```text
The password for the specified user was accepted because an Azure password policy is not available yet

UserName: SomeUser
FullName: Some User

This condition may be caused by one or more of the following reasons:%n

1. The forest has not yet been registered with Azure.

   Resolution steps: an administrator must register the forest using the Register-AzureADPasswordProtectionForest cmdlet.

2. An Azure AD password protection Proxy is not yet available on at least one machine in the current forest.

   Resolution steps: an administrator must install and register a proxy using the Register-AzureADPasswordProtectionProxy cmdlet.

3. This DC does not have network connectivity to any Azure AD password protection Proxy instances.

   Resolution steps: ensure network connectivity exists to at least one Azure AD password protection Proxy instance.

4. This DC does not have connectivity to other domain controllers in the domain.

   Resolution steps: ensure network connectivity exists to the domain.
```

### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Örnek olay günlüğü iletisi olay kimliği 30006 (yeni ilke tarafından zorunlu kılınmayı)

```text
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

### <a name="sample-event-log-message-for-event-id-30019-azure-ad-password-protection-is-disabled"></a>Örnek olay günlüğü iletisi olay kimliği 30019 (Azure AD parola koruması devre dışı)

```text
The most recently obtained Azure password policy was configured to be disabled. All passwords submitted for validation from this point on will automatically be considered compliant with no processing performed.

No further events will be logged until the policy is changed.%n

```

## <a name="dc-agent-operational-log"></a>DC Aracısı işlem günlüğü

DC Aracısı hizmeti de ilgili işletimsel olayları aşağıdaki günlüğüne:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

## <a name="dc-agent-trace-log"></a>DC aracı izleme günlükleri

DC aracısı aşağıdaki günlük kaydı için ayrıntılı hata ayıklama düzeyinde izleme olaylarını günlüğe kaydedebilirsiniz:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

İzleme günlüğü kaydının varsayılan olarak devre dışıdır.

> [!WARNING]
> Etkinleştirildiğinde, izleme günlüğü yüksek hacimli olayları alır ve etki alanı denetleyicisi performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu Gelişmiş günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

## <a name="dc-agent-text-logging"></a>DC aracı metin günlüğü

DC Aracı hizmeti, aşağıdaki kayıt defteri değerini ayarlayarak bir metin günlüğüne yazmak için yapılandırılabilir:

```text
HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters!EnableTextLogging = 1 (REG_DWORD value)
```

Metin günlüğü varsayılan olarak devre dışıdır. Değişikliklerin etkili olması için bu değer için DC aracı hizmetini yeniden başlatılması gerekiyor. DC etkinleştirildiğinde aracısı hizmetinin altında yer alan bir günlük dosyasına yazar:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> Metin günlüğü izleme günlüğüne kaydedilebilir debug düzeyi girişleri alır, ancak genellikle gözden geçirin ve analiz etmek için daha kolay bir biçimde olacaktır.

> [!WARNING]
> Etkin olduğunda, bu günlük yüksek hacimli olayları alır ve etki alanı denetleyicisi performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu Gelişmiş günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

## <a name="dc-agent-performance-monitoring"></a>Performans izleme aracısını DC

Adlı bir performans sayacı nesne DC Aracısı hizmeti yazılımını yükler **Azure AD parola koruması**. Aşağıdaki performans sayaçları şu anda kullanılabilir:

|Performans sayaç adı | Açıklama|
| --- | --- |
|İşlenen parolaları |Bu sayaç, son yeniden başlatmadan sonra (kabul veya reddedildi) toplam işlenen parola sayısını görüntüler.|
|Kabul edilen parolalar |Bu sayaç, son yeniden başlatmadan sonra kabul edildi parola toplam sayısını görüntüler.|
|Parolaları reddetti |Bu sayaç, son yeniden başlatmadan sonra Reddedilen Parola toplam sayısını görüntüler.|
|Devam eden parola filtresi istekleri |Bu sayaç, parola filtresi istek sayısı şu anda devam eden görüntüler.|
|Parola en yüksek istek filtreleme |Bu sayaç, son yeniden başlatmadan sonra eşzamanlı parola filtresi istekleri en yüksek sayısını görüntüler.|
|Parola filtresi istek hataları |Bu sayaç son yeniden başlatmadan sonra bir hata nedeniyle başarısız parola filtresi isteklerinin toplam sayısını görüntüler. Azure AD parola koruması DC Aracı hizmeti çalışmadığı zaman hataları oluşabilir.|
|Parola filtresi İsteği/sn |Bu sayaç, işlenmekte olan parolalar hızını görüntüler.|
|Parola filtresi istek işleme süresi |Bu sayaç, bir parola filtresi isteği işlemek için gereken ortalama süreyi görüntüler.|
|Yoğun parola filtresi istek işleme süresi |Bu sayaç, yoğun parola filtresi istek son yeniden başlatma işleminden işleme görüntüler.|
|Denetim modu nedeniyle kabul edilen parolalar |Bu sayaç, normalde reddedilen, ancak parola ilkesi (son yeniden başlatmadan sonra) denetim modunda olacak şekilde yapılandırılmadığından kabul edildi parola toplam sayısını görüntüler.|

## <a name="dc-agent-discovery"></a>DC Aracısı bulma

`Get-AzureADPasswordProtectionDCAgent` Cmdlet'i, bir etki alanı veya orman çalışan çeşitli DC aracılarla ilgili temel bilgileri görüntülemek için kullanılabilir. Bu bilgiler, çalışan DC aracı hizmetleri tarafından kayıtlı serviceConnectionPoint nesnelerden alınır.

Bu cmdlet'in bir örnek çıktısı aşağıdaki gibidir:

```PowerShell
Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
PasswordPolicyDateUTC : 2/16/2018 8:35:01 AM
HeartbeatUTC          : 2/16/2018 8:35:02 AM
```

Çeşitli özellikleri, her DC Aracısı yaklaşık bir saatlik aralıklarla güncelleştirilir. Yine de Active Directory çoğaltma gecikmesine verilerdir.

Cmdlet'in sorgu kapsamı kullanarak etkilenebilir orman veya – etki alanı parametreleri.

HeartbeatUTC değeri eski alırsa, bu Azure AD parola koruması DC aracı bu etki alanı denetleyicisi üzerinde çalışmadığı veya kaldırılmış veya makine yetkisi ve artık etki alanı denetleyicisi bir belirtisi olabilir.

Bu Azure AD parola koruma DC aracısının bu makinede olduğu bir belirti olabilir, eski PasswordPolicyDateUTC değeri alır, düzgün çalışmıyor.

## <a name="proxy-service-event-logging"></a>Proxy hizmeti olay günlüğü

Proxy Hizmeti, olayları aşağıdaki olay günlüklerinden en az bir dizi yayar:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

İzleme günlüğü varsayılan olarak devre dışı olduğuna dikkat edin.

> [!WARNING]
> Etkin olduğunda, yüksek hacimli olay izleme günlüğü alır ve bu proxy konağını performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

Olayları aşağıdaki aralıklarını kullanarak çeşitli Proxy bileşenleri tarafından kaydedilir:

|Bileşen |Olay Kimliği aralığı|
| --- | --- |
|Proxy hizmet barındırma işlemi| 10000-19999|
|Proxy hizmet ana iş mantığı| 20000-29999|
|PowerShell cmdlet'leri| 30000-39999|

## <a name="proxy-service-text-logging"></a>Proxy hizmet metin günlüğü

Proxy Hizmeti, aşağıdaki kayıt defteri değerini ayarlayarak bir metin günlüğüne yazmak için yapılandırılabilir:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters! EnableTextLogging = 1 (REG_DWORD değeri)

Metin günlüğü varsayılan olarak devre dışıdır. Değişikliklerin etkili olması için bu değer için Proxy Hizmeti yeniden başlatma gerekiyor. Hizmetinin altında yer alan bir günlük dosyasına yazar Proxy etkin olduğunda:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> Metin günlüğü izleme günlüğüne kaydedilebilir debug düzeyi girişleri alır, ancak genellikle gözden geçirin ve analiz etmek için daha kolay bir biçimde olacaktır.

> [!WARNING]
> Etkin olduğunda, bu günlük yüksek hacimli olayları alır ve makinenin performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu Gelişmiş günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

## <a name="powershell-cmdlet-logging"></a>PowerShell cmdlet günlükleri

Bir durum değişikliği (örneğin, Register-AzureADPasswordProtectionProxy) neden PowerShell cmdlet'leri normal işlem günlüğü için bir sonuç olayı günlüğe kaydeder.

Ayrıca, Azure AD parola koruması PowerShell cmdlet'lerinin en altında yer alan bir metin günlüğü Yazar:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Bir cmdlet hatası oluşur ve neden ve/veya özetleri çözüm kolaylıkla görünebilir değil, bu metin günlükleri de consulted.

## <a name="proxy-discovery"></a>Ara sunucu bulma

`Get-AzureADPasswordProtectionProxy` Cmdlet'i, bir etki alanı veya orman içinde çalışan çeşitli Azure AD parola koruması Proxy hizmetleri hakkında temel bilgileri görüntülemek için kullanılabilir. Bu bilgiler, çalışan Proxy Hizmetleri tarafından kayıtlı serviceConnectionPoint nesnelerden alınır.

Bu cmdlet'in bir örnek çıktısı aşağıdaki gibidir:

```PowerShell
Get-AzureADPasswordProtectionProxy
ServerFQDN            : bplProxy.bplchild2.bplRootDomain.com
Domain                : bplchild2.bplRootDomain.com
Forest                : bplRootDomain.com
HeartbeatUTC          : 12/25/2018 6:35:02 AM
```

Çeşitli özellikleri, yaklaşık bir saatlik olarak her bir Proxy Hizmeti tarafından güncelleştirilir. Yine de Active Directory çoğaltma gecikmesine verilerdir.

Cmdlet'in sorgu kapsamı kullanarak etkilenebilir orman veya – etki alanı parametreleri.

HeartbeatUTC değeri eski alırsa, bu, Azure AD parola koruması Proxy bu makinede çalışmıyor veya kaldırılmış bir belirtisi olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola koruması için sorun giderme](howto-password-ban-bad-on-premises-troubleshoot.md)

Genel ve özel yasaklı parola listelerini hakkında daha fazla bilgi için bkz [yasaklamak hatalı parola](concept-password-ban-bad.md)
