---
title: Sorun giderme ve Azure AD parola koruması önizlemede günlüğe kaydetme
description: Azure AD parola koruması anlamak Önizleme günlüğe kaydetme ve genel sorun giderme
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 1e5782ce3421cc5f0d2e0e51484d4bbe6b9eb6ab
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978647"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Önizleme: Azure AD parola koruması izleme, raporlama ve sorun giderme

|     |
| --- |
| Azure AD parola koruması, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD parola koruması dağıtıldıktan sonra izleme ve raporlama temel görevleridir. Bu makalede olduğu her hizmet bilgilerini kaydeder ve Azure AD parola koruması kullanımını bildirme anlamanıza yardımcı olmak üzere ayrıntıya gider.

## <a name="on-premises-logs-and-events"></a>Şirket içi günlüklerini ve olayları

### <a name="dc-agent-admin-log"></a>DC aracı yönetim günlüğü

Her etki alanı denetleyicisinde DC Aracı hizmeti yazılımı kendi parola doğrulamaları (ve diğer durum) sonuçlarının bir yerel olay günlüğüne yazar:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

Olayları aşağıdaki aralıklarını kullanarak çeşitli DC aracı bileşenleri tarafından kaydedilir:

|Bileşen |Olay Kimliği aralığı|
| --- | --- |
|DC aracı parola filtresi DLL'sinin| 10000 19999|
|DC Aracısı hizmeti barındırma işlemi| 20000 29999|
|DC aracısı hizmet İlkesi Doğrulama mantığı| 30000 39999|

Başarılı parola doğrulama işlemi için genellikle DC aracı parola filtresi DLL'den günlüğe bir olay yok. İçin başarısız parola doğrulama işlemi vardır genellikle iki olayları günlüğe, bir DC Aracı hizmeti ve DC aracı parola filtresi DLL'sinin birinden.

Geçici bir çözüm aşağıdaki etkenlere göre bu durumlar yakalamak için ayrı olaylar günlüğe kaydedilir:

* Belirli bir parola olup ettiğinden ayarlayın veya değiştirildi.
* Olup verilen parola doğrulaması geçti veya başarısız oldu.
* Olup olmadığını doğrulama kuruluş ilkesinin Microsoft Genel ilke vs nedeniyle başarısız oldu.
* Yalnızca denetim modunda şu anda açıp geçerli parola ilkesini olup olmadığı.

Anahtar parolası doğrulama ile ilgili olayları aşağıdaki gibidir:

|   |Parola değiştirme |Parola ayarlama|
| --- | :---: | :---: |
|Geçiş |10014 |10015|
|Başarısız (müşteri parola ilkesi geçemedi)| 10016, 30002| 10017, 30003|
|Başarısız (Microsoft parola ilkesi geçemedi)| 10016, 30004| 10017, 30005|
|Yalnızca denetim geçişi (müşteri parola ilkesi başarısız)| 10024, 30008| 10025, 30007|
|Yalnızca denetim geçişi (Microsoft parola ilkesi başarısız)| 10024, 30010| 10025, 30009|

> [!TIP]
> Gelen parolaların Microsoft genel parola listesine göre ilk doğrulanır; Bu başarısız olursa başka işlem gerçekleştirilir. Parola değişikliklerini azure'da gerçekleştirilen gibi aynı davranışı budur.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Örnek olay günlüğü iletisi olay kimliği 10014 (başarılı parola küme)

```
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Örnek olay günlüğü iletisi olay kimliği 10017 ve 30003 (başarısız parola küme)

10017:

```
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Örnek olay günlüğü iletisi olay kimliği 30001 (hiçbir ilke kullanılabilir nedeniyle kabul parola)

```
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

#### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Örnek olay günlüğü iletisi olay kimliği 30006 (yeni ilke tarafından zorunlu kılınmayı)

```
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

#### <a name="dc-agent-operational-log"></a>DC Aracısı işlem günlüğü

DC Aracısı hizmeti de ilgili işletimsel olayları aşağıdaki günlüğüne:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

#### <a name="dc-agent-trace-log"></a>DC aracı izleme günlükleri

DC aracısı aşağıdaki günlük kaydı için ayrıntılı hata ayıklama düzeyinde izleme olaylarını günlüğe kaydedebilirsiniz:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

İzleme günlüğü kaydının varsayılan olarak devre dışıdır.

> [!WARNING]
>  Etkinleştirildiğinde, izleme günlüğü yüksek hacimli olayları alır ve etki alanı denetleyicisi performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu Gelişmiş günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

#### <a name="dc-agent-text-logging"></a>DC aracı metin günlüğü

DC Aracı hizmeti, aşağıdaki kayıt defteri değerini ayarlayarak bir metin günlüğüne yazmak için yapılandırılabilir:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters! EnableTextLogging = 1 (REG_DWORD değeri)

Metin günlüğü varsayılan olarak devre dışıdır. Değişikliklerin etkili olması için bu değer için DC aracı hizmetini yeniden başlatılması gerekiyor. DC etkinleştirildiğinde aracısı hizmetinin altında yer alan bir günlük dosyasına yazar:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> Metin günlüğü izleme günlüğüne kaydedilebilir debug düzeyi girişleri alır, ancak genellikle gözden geçirin ve analiz etmek için daha kolay bir biçimde olacaktır.

> [!WARNING]
> Etkin olduğunda, bu günlük yüksek hacimli olayları alır ve etki alanı denetleyicisi performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu Gelişmiş günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD parola koruması proxy hizmeti

#### <a name="proxy-service-event-logs"></a>Proxy hizmeti olay günlükleri

Proxy Hizmeti, olayları aşağıdaki olay günlüklerinden en az bir dizi yayar:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

Proxy Hizmeti aşağıdaki günlük kaydı için ayrıntılı hata ayıklama düzeyinde izleme olaylarını günlüğe kaydedebilirsiniz:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

İzleme günlüğü kaydının varsayılan olarak devre dışıdır.

> [!WARNING]
> Etkin olduğunda, yüksek hacimli olay izleme günlüğü alır ve bu proxy konağını performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

#### <a name="proxy-service-text-logging"></a>Proxy hizmet metin günlüğü

Proxy Hizmeti, aşağıdaki kayıt defteri değerini ayarlayarak bir metin günlüğüne yazmak için yapılandırılabilir:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters! EnableTextLogging = 1 (REG_DWORD değeri)

Metin günlüğü varsayılan olarak devre dışıdır. Değişikliklerin etkili olması için bu değer için Proxy Hizmeti yeniden başlatma gerekiyor. Hizmetinin altında yer alan bir günlük dosyasına yazar Proxy etkin olduğunda:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> Metin günlüğü izleme günlüğüne kaydedilebilir debug düzeyi girişleri alır, ancak genellikle gözden geçirin ve analiz etmek için daha kolay bir biçimde olacaktır.

> [!WARNING]
> Etkin olduğunda, bu günlük yüksek hacimli olayları alır ve etki alanı denetleyicisi performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu Gelişmiş günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

#### <a name="powershell-cmdlet-logging"></a>PowerShell cmdlet günlükleri

Azure AD parola koruması Powershell cmdlet'lerinin çoğu altında yer alan bir metin günlüğü için yazılacaktır:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Bir cmdlet hatası oluşur ve neden ve/veya özetleri çözüm kolaylıkla görünebilir değil, bu metin günlükleri de consulted.

### <a name="emergency-remediation"></a>Acil Durum düzeltme

Burada, DC Aracı hizmeti sorunlara neden olan bir durum meydana gelirse, DC aracı hizmetini hemen kapatılması. DC aracı parola filtresi DLL'sinin hala çalışan olmayan hizmeti çağırmak çalışır ve uyarı olayları (10012, 10013) günlüğe kaydeder, ancak bu süre boyunca tüm gelen parolaları kabul edilir. DC Aracı hizmeti daha sonra da aracılığıyla Windows Hizmet Denetimi Yöneticisi "Disabled" başlatma türüyle gerektiği şekilde yapılandırılmış olabilir.

### <a name="performance-monitoring"></a>Performans izleme

Adlı bir performans sayacı nesne DC Aracısı hizmeti yazılımını yükler **Azure AD parola koruması**. Aşağıdaki performans sayaçları şu anda kullanılabilir:

|Performans sayaç adı | Açıklama|
| --- | --- |
|İşlenen parolaları |Bu sayaç, son yeniden başlatmadan sonra (kabul veya reddedildi) toplam işlenen parola sayısını görüntüler.|
|Kabul edilen parolalar |Bu sayaç, son yeniden başlatmadan sonra kabul edildi parola toplam sayısını görüntüler.|
|Parolaları reddetti |Bu sayaç, son yeniden başlatmadan sonra Reddedilen Parola toplam sayısını görüntüler.|
|Devam eden parola filtresi istekleri |Bu sayaç, parola filtresi istek sayısı şu anda devam eden görüntüler.|
|Parola en yüksek istek filtreleme |Bu sayaç, son yeniden başlatmadan sonra eşzamanlı parola filtresi istekleri en yüksek sayısını görüntüler.|
|Parola filtresi istek hataları |Bu sayaç son yeniden başlatmadan sonra bir hata nedeniyle başarısız parola filtresi isteklerinin toplam sayısını görüntüler. Azure AD parola koruması DC Aracısı hizmeti çalışmadığı zaman hataları oluşabilir.|
|Parola filtresi İsteği/sn |Bu sayaç, işlenmekte olan parolalar hızını görüntüler.|
|Parola filtresi istek işleme süresi |Bu sayaç, bir parola filtresi isteği işlemek için gereken ortalama süreyi görüntüler.|
|Yoğun parola filtresi istek işleme süresi |Bu sayaç, yoğun parola filtresi istek son yeniden başlatma işleminden işleme görüntüler.|
|Denetim modu nedeniyle kabul edilen parolalar |Bu sayaç, normalde reddedilen, ancak parola ilkesi (son yeniden başlatmadan sonra) denetim modunda olacak şekilde yapılandırılmadığından kabul edildi parola toplam sayısını görüntüler.|

## <a name="directory-services-repair-mode"></a>Dizin Hizmetleri Onarım Modu'nda

Etki alanı denetleyicisi Dizin Hizmetleri Onarım Modu'nda önyüklenir, DC Aracı hizmeti bu tüm parola doğrulama veya zorlama etkinlikleri, bağımsız olarak şu anda etkin ilke yapılandırmasını devre dışı bırakılması neden algılar.

## <a name="domain-controller-demotion"></a>Etki alanı denetleyicisinin indirgemesi

Hala DC Aracısı yazılımını çalıştıran bir etki alanı denetleyicisini indirgemek için desteklenir. Yöneticiler ancak DC Aracısı yazılımını çalışmaya devam eder ve geçerli parola ilkesini zorlama indirgeme işlemi sırasında devam bilmeniz gerekir. (İndirme işlemi bir parçası olarak belirtilen) yeni yerel yönetici hesabı parolası gibi başka bir parola doğrulanır. Microsoft, güvenli parolalar DC indirgeme yordamının parçası olarak yerel yönetici hesapları için seçilmesi önerir; ancak yeni yerel yönetici hesabı parolasını DC Aracısı yazılımı tarafından doğrulanmasını indirgeme işlem yordamlarını önceden varolan karışıklığa neden olabilir.

İndirgeme başarılı oldu ve etki alanı denetleyicisi gizleyip gizlemeyeceğini ve normal üye sunucu olarak yeniden çalıştırmayı sonra pasif modunda çalışan DC Aracısı yazılımını döner. Ardından herhangi bir zamanda kaldırılabilir.

## <a name="removal"></a>Çıkarma

Etki alanı ve orman ilgili tüm durum temizleme ve genel Önizleme yazılımını kaldırmak için karar verilir, bu görevi, aşağıdaki adımları kullanarak gerçekleştirilebilir:

> [!IMPORTANT]
> Sırayla bu adımları gerçekleştirmek önemlidir. Proxy Hizmeti herhangi bir örneğini çalıştıran bırakılırsa düzenli aralıklarla serviceConnectionPoint nesne yeniden oluşturur. DC Aracısı hizmeti herhangi bir örneğini çalıştıran bırakılırsa düzenli aralıklarla serviceConnectionPoint nesne ve sysvol durumu yeniden oluşturun.

1. Parola koruması ara yazılım, tüm makineleri kaldırın. Bu adım mu **değil** yeniden başlatılmasını gerektirir.
2. Tüm etki alanı denetleyicilerinden DC Aracısı yazılımı kaldırın. Bu adım **gerektirir** yeniden başlatma.
3. El ile her etki alanı adlandırma bağlamı tüm Proxy Hizmeti bağlantı noktalarını kaldırın. Bu nesnelerin konumu aşağıdaki Active Directory Powershell komutuyla bulunan:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Yıldız işareti kullanın ("*") sonunda $keywords değişken değeri.

   Aracılığıyla elde edilen nesne bulundu `Get-ADObject` komutu ardından ayrıştırılabileceği `Remove-ADObject`, veya el ile silindi. 

4. El ile tüm DC aracı bağlantı noktaları, her etki alanı adlandırma içeriği kaldırın. Olabilir bir genel Önizleme yazılımını yaygın olarak nasıl dağıtıldığına bağlı olarak ormandaki etki alanı denetleyicisi başına bu nesneler. Bu nesnenin konumu aşağıdaki Active Directory Powershell komutuyla bulunan:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Aracılığıyla elde edilen nesne bulundu `Get-ADObject` komutu ardından ayrıştırılabileceği `Remove-ADObject`, veya el ile silindi.

5. Orman düzeyinde yapılandırma durumunu el ile kaldırın. Orman yapılandırma durumu, Active Directory yapılandırma adlandırma bağlamında bir kapsayıcıda tutulur. Bulunan ve aşağıda silindi:

   ```Powershell
   $passwordProtectonConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. El ile tüm sysvol durumu tarafından el ile ilgili kaldırın, aşağıdaki klasör ve tüm içeriğini silme:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Gerekirse, bu yolu yerel olarak belirtilen etki alanı denetleyicisinde erişilebilecek; Varsayılan konum şu yol şöyle olacaktır:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Sysvol paylaşımının varsayılan olmayan bir konumda yapılandırdıysanız, bu yolu farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

Genel ve özel yasaklı parola listelerini hakkında daha fazla bilgi için bkz [yasaklamak hatalı parola](concept-password-ban-bad.md)
