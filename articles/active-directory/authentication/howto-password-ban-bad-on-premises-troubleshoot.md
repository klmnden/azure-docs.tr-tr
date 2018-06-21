---
title: Sorun giderme ve Azure AD parola koruması önizlemede günlüğe kaydetme
description: Azure AD parola koruması anlamak Önizleme günlüğe kaydetme ve ortak sorun giderme
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: e5b3dc1bfa7c7890be83529e863907ec056f188f
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295662"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Önizleme: Azure AD parola koruması izleme, raporlama ve sorun giderme

|     |
| --- |
| Azure AD parola koruması ve özel Engellenenler parola listesini Azure Active Directory genel Önizleme özellikleri verilmiştir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları'nı Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD parola koruması dağıtımdan sonra izleme ve raporlama temel görevlerdir. Bu makalede, burada her hizmet bilgileri günlüğe kaydeder ve Azure AD parola koruması kullanımını bildirme anlamanıza yardımcı ayrıntıya gider.

## <a name="on-premises-logs-and-events"></a>Şirket içi günlüklerini ve olayları

### <a name="dc-agent-service"></a>DC Aracısı hizmeti

Her etki alanı denetleyicisinde DC Aracısı hizmeti yazılım parola doğrulama (ve diğer durum) sonuçlarını yerel bir olay günlüğüne yazar: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin

Olayları aşağıdaki aralıkları kullanarak çeşitli DC Aracısı bileşenleri tarafından kaydedilir:

|Bileşen |Olay Kimliği aralığını|
| --- | --- |
|DC Aracısı parola Filtre DLL'si| 10000 19999|
|DC aracısı hizmet barındırma işlemi| 20000 29999|
|DC Aracısı hizmeti ilke Doğrulama mantığı| 30000 39999|

Başarılı parola doğrulama işlemi için genellikle DC Aracısı parola filtresi DLL'den günlüğe bir olay yok. İçin başarısız parola doğrulama işlemi, genellikle vardır iki olayları günlüğe, bir DC Aracısı hizmeti ve DC Aracısı parola filtresi DLL'sinin birinden.

Bu durumlarda yakalamak için ayrık olayları aşağıdaki etmenlere dayalı kaydedilir:

* Verilen parola olup yapılıyor ayarlamak veya değiştirildi.
* Verilen parola doğrulaması geçirilen olup başarısız oldu.
* Microsoft Genel ilke vs nedeniyle mi kuruluş ilkesi doğrulaması başarısız oldu.
* Yalnızca denetim modunda şu anda açmak veya kapatmak için geçerli parola ilkesi olup olmadığı.

Anahtar parolası doğrulama ilgili olayları aşağıdaki gibidir:

|   |Parola değiştirme |Parola ayarlama|
| --- | :---: | :---: |
|Geçiş |10014 |10015|
|Başarısız (müşteri parola ilkesi geçemedi)| 10016, 30002| 10017, 30003|
|Başarısız (Microsoft parola ilkesi geçemedi)| 10016, 30004| 10017, 30005|
|Yalnızca denetim geçişi (müşteri parola ilkesi başarısız olurdu)| 10024, 30008| 10025, 30007|
|Yalnızca denetim geçişi (Microsoft parola ilkesi başarısız olurdu)| 10024, 30010| 10025, 30009|

> [!TIP]
> Gelen parolaları Microsoft genel parola listesine karşı ilk doğrulanır; başarısız olursa, başka işlem gerçekleştirilir. Parola değişiklikleri Azure üzerinde gerçekleştirilen gibi aynı davranışı budur.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Olay Kimliği 10014 başarılı parola kümesi için örnek olay günlüğü iletisi

Değiştirilen parola belirtilen kullanıcı için geçerli Azure parola ilkesiyle uyumlu olarak doğrulandı.

 Kullanıcı adı: BPL_02885102771 FullName:

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Olay Kimliği 10017 ve 30003 başarısız parola kümesi için örnek olay günlüğü iletisi

10017:

Belirtilen kullanıcı için parola sıfırlama geçerli Azure parola ilkesiyle uymadığından reddedildi. Daha fazla ayrıntı için ilişkili olay günlüğü iletisine bakın.

 Kullanıcı adı: BPL_03283841185 FullName:

30003:

Belirtilen kullanıcı için parola sıfırlama en az bir geçerli Azure parola ilkesi Kiracı başına yasaklanan parola listesinde belirteçleri eşleşen nedeniyle reddedildi.

 Kullanıcı adı: BPL_03283841185 FullName:

Diğer bazı anahtar olay günlüğü iletilerini dikkat edilmesi gereken şunlardır:

#### <a name="sample-event-log-message-for-event-id-30001"></a>Olay Kimliği 30001 örnek olay günlüğü iletisi

Bir Azure parola ilkesi henüz kullanılabilir olmadığından belirtilen kullanıcı için parola kabul edildi

Kullanıcı adı: <user> FullName: <user>

Bu durum, bir veya daha fazla aşağıdaki nedenlerden: % n neden olabilir

1. Orman henüz Azure ile kayıtlı değil.

   Çözüm adımları: bir yönetici kaydı AzureADPasswordProtectionForest cmdlet'ini kullanarak orman kaydetmeniz gerekir.

2. Bir Azure AD parola koruması Proxy henüz geçerli ormandaki en az bir makinede kullanılabilir değil.

   Çözüm adımları: yönetici yüklemeli ve kayıt AzureADPasswordProtectionProxy cmdlet'ini kullanarak bir proxy kaydedin.

3. Bu DC hiç Azure AD parola koruması Proxy örneği için ağ bağlantısı yok.

   Çözüm adımları: en az bir Azure AD parola koruması Proxy örneği için ağ bağlantısı olduğundan emin olun.

4. Bu etki alanı Denetleyicisinin etki alanındaki diğer etki alanı denetleyicilerine bağlantısı yok.

   Çözüm adımları: ağ bağlantısı etki alanında olduğundan emin olun.

#### <a name="sample-event-log-message-for-event-id-30006"></a>Olay Kimliği 30006 örnek olay günlüğü iletisi

Hizmet artık aşağıdaki Azure parola ilkesi zorlama.

 AuditOnly: 1

 Genel ilke tarihi: 2018-05-15T00:00:00.000000000Z

 Kiracı İlkesi tarihi: 2018-06-10T20:15:24.432457600Z

 Kiracı politikasını: 1

#### <a name="dc-agent-log-locations"></a>DC aracı günlük konumları

DC Aracısı hizmeti de ilgili işletimsel olayları aşağıdaki günlüğüne: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational

DC Aracısı hizmeti de ayrıntılı hata ayıklama düzeyi izleme olayları aşağıdaki günlüğüne: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace

> [!WARNING]
> İzleme günlüğü varsayılan olarak devre dışıdır. Etkinleştirildiğinde, bu günlük yüksek hacimli olayları alır ve etki alanı denetleyicisi performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu Gelişmiş günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca için en az bir zaman miktarıdır.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD parola koruması proxy hizmeti

Parola koruması Proxy Hizmeti olayları aşağıdaki olay günlüğü için en az sayıda yayar: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational

Parola koruması Proxy hizmeti de ayrıntılı hata ayıklama düzeyi izleme olayları aşağıdaki günlüğüne: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace

> [!WARNING]
> İzleme günlüğü varsayılan olarak devre dışıdır. Etkinleştirildiğinde, bu günlük yüksek hacimli olayları alır ve bu proxy konağını performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca için en az bir zaman miktarıdır.

### <a name="dc-agent-discovery"></a>DC Aracısı bulma

`Get-AzureADPasswordProtectionDCAgent` Cmdlet, bir etki alanı veya orman içinde çalışan çeşitli DC aracılar ile ilgili temel bilgileri görüntülemek için kullanılabilir. Bu bilgiler çalışan DC aracısı hizmet (ler) tarafından kayıtlı serviceConnectionPoint nesneleri alınır. Bu cmdlet örnek çıktısı şu şekildedir:

```
PS C:\> Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

Çeşitli özellikleri, yaklaşık bir saatlik olarak her DC Aracısı hizmeti tarafından güncelleştirilir. Veri hala Active Directory çoğaltma gecikmesi tabi değildir.

Cmdlet'in sorgusunun kapsamındaki kullanarak etkilenebilir orman veya – etki alanı parametreleri.

### <a name="emergency-remediation"></a>Acil Durum düzeltme

Bir talihsiz DC Aracısı hizmeti sorunlara neden olduğu durumda, DC Aracısı hizmeti hemen kapatılması. DC Aracısı parola filtresi DLL'sinin çalışan olmayan hizmetini çağırmak çalışır ve uyarı olayları (10012, 10013) günlüğe kaydeder, ancak bu süre boyunca tüm gelen parolaları kabul edilir. DC Aracısı hizmeti sonra da aracılığıyla Windows Hizmet Denetimi Yöneticisi "Disabled" başlatma türüyle gerektiği şekilde yapılandırılmış olabilir.

### <a name="performance-monitoring"></a>Performans izleme

Adlı bir performans sayacı nesne DC aracısı hizmet yazılımını yükler **Azure AD parola koruması**. Aşağıdaki performans sayaçları şu anda kullanılabilir:

|Performans sayaç adı | Açıklama|
| --- | --- |
|İşlenen parolaları |Bu sayaç, son yeniden başlatmadan sonra (kabul veya reddedilen) toplam işlenen parola sayısını görüntüler.|
|Kabul parolaları |Bu sayaç son yeniden başlatmadan sonra kabul edildi parola toplam sayısını görüntüler.|
|Parolalar reddetti |Bu sayaç son yeniden başlatmadan sonra Reddedilen Parola toplam sayısını görüntüler.|
|Parola filtresi isteklerini sürüyor |Bu sayaç ediyor şu anda parola filtresi isteklerini sayısını görüntüler.|
|Yoğun parola filtresi istekleri |Bu sayaç, son yeniden başlatmadan sonra eşzamanlı parola filtresi isteklerini en yüksek sayısını görüntüler.|
|Parola filtre istek hataları |Bu sayaç son yeniden başlatmadan sonra bir hata nedeniyle başarısız parola filtresi isteklerinin toplam sayısını görüntüler. Azure AD parola koruması DC Aracısı hizmeti çalışmadığı zaman hatalar oluşabilir.|
|Parola filtresi İsteği/sn |Bu sayaç işlenmekte olan parolaları hızını görüntüler.|
|Parola filtresi isteği işleme süresi |Bu sayaç, bir parola filtresi isteği işlemek için gereken ortalama süreyi görüntüler.|
|Yoğun parola filtresi istek işleme süresi |Bu sayaç, en son yeniden başlatıldığından beri işleme yoğun parola filtresi isteği görüntüler.|
|Denetleme modu nedeniyle kabul parolaları |Bu sayaç, normalde reddedilmiş, ancak parola ilkesi (son yeniden başlatmadan sonra) denetim modunda olacak şekilde yapılandırılmış olduğundan kabul edildi parolaları toplam sayısını görüntüler.|

## <a name="directory-services-repair-mode"></a>Dizin Hizmetleri onarım modunda

Etki alanı denetleyicisi Dizin Hizmetleri Onarım Modu'nda önyüklenir varsa, bu tüm parola doğrulama veya zorlama etkinlikleri, bağımsız olarak şu anda etkin ilke yapılandırması devre dışı bırakılmasına neden DC Aracısı hizmeti algılar.

## <a name="domain-controller-demotion"></a>Etki alanı denetleyicisi indirgeme

Hala DC aracı yazılımını çalıştıran bir etki alanı denetleyicisini indirgemek için desteklenir. Yöneticiler ancak DC aracı yazılımı çalıştıran tutar ve indirgeme işlemi sırasında geçerli parola ilkesi zorlama devam bilmeniz gerekir. (İndirgeme işleminin bir parçası olarak belirtilen) yeni yerel yönetici hesabı parola gibi herhangi bir parola doğrulanır. Microsoft, güvenli parolalar DC indirgeme yordamının parçası olarak yerel yönetici hesapları seçilmesi önerir; ancak yeni yerel yönetici hesabı parolasını DC Aracısı yazılımı tarafından doğrulanmasını indirgeme işlemsel yordamlara önceden varolan kesintiye uğratan olabilir.
İndirgeme başarılı oldu ve etki alanı denetleyicisini yeniden başlattı ve normal üye sunucu olarak yeniden çalıştırmayı sonra edilgen modda çalışan DC Aracısı yazılım döner. Ardından herhangi bir zamanda kaldırılabilir.

## <a name="removal"></a>Çıkarma

Genel Önizleme yazılım ve temizleme ilgili tüm durum etki alanı ve orman kaldırmaya karar verdiyseniz, bu görev aşağıdaki adımları kullanarak gerçekleştirilebilir:

> [!IMPORTANT]
> Bu adımları sırayla gerçekleştirmek önemlidir. Parola koruması Proxy Hizmeti herhangi bir örneğine bırakılırsa çalışan düzenli aralıklarla kendi serviceConnectionPoint nesneyi yeniden oluşturmak yanı düzenli aralıklarla sysvol durumu yeniden oluşturun.

1. Parola koruması Proxy yazılım tüm makinelerden kaldırın. Bu adım mu **değil** yeniden başlatılması gerekir.
2. Tüm etki alanı denetleyicilerinden DC Aracısı yazılımı kaldırın. Bu adım **gerektirir** yeniden başlatma.
3. El ile her etki alanı adlandırma bağlamını tüm proxy hizmet bağlantı noktalarını kaldırın. Bu nesnelerin konumu aşağıdaki Active Directory Powershell komutuyla bulunan:
   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{EBEFB703-6113-413D-9167-9F8DD4D24468}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -eq $keywords }
   ```

   Yıldız işareti kullanın ("*") $keywords değişken değeri sonunda.

   Sonuç nesnesi aracılığıyla bulunamadı `Get-ADObject` komutu sonra yöneltilen için `Remove-ADObject`, veya el ile silindi. 

4. El ile tüm DC Aracısı bağlantı noktaları, her etki alanı adlandırma bağlamını kaldırın. Olabilir bir genel Önizleme yazılım nasıl yaygın olarak dağıtılan bağlı olarak ormandaki etki alanı denetleyicisi başına bu nesneler. Bu nesnenin konumu aşağıdaki Active Directory Powershell komutuyla bulunan:

   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{B11BB10A-3E7D-4D37-A4C3-51DE9D0F77C9}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -eq $keywords }
   ```

   Sonuç nesnesi aracılığıyla bulunamadı `Get-ADObject` komutu sonra yöneltilen için `Remove-ADObject`, veya el ile silindi.

5. Orman düzeyinde yapılandırma durumunu el ile kaldırın. Orman yapılandırma durumu, Active Directory yapılandırma adlandırma bağlamında kapsayıcısında korunur. Bulunmalı ve aşağıdaki gibi silindi:

   ```
   $passwordProtectonConfigContainer = "CN=Azure AD password protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. El ile Kaldır tüm sysvol durumuna göre el ile ilgili aşağıdaki klasörü ve tüm içeriğini silme:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Gerekirse, bu yol yerel olarak verilen etki alanı denetleyicisinde erişilebilir; varsayılan konumu şu yol şöyle olacaktır:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Sysvol paylaşımının varsayılan olmayan konumda yapılandırdıysanız, bu yol farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

Genel ve özel Engellenenler parola listeleri hakkında daha fazla bilgi için bkz: [bYönlendirme bozuk parola](concept-password-ban-bad.md)
