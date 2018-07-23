---
title: Sorun giderme ve Azure AD parola koruması önizlemede günlüğe kaydetme
description: Azure AD parola koruması anlamak Önizleme günlüğe kaydetme ve genel sorun giderme
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 1eea6380d4276644db0c7681f23a4b0c5e79ff09
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187358"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Önizleme: Azure AD parola koruması izleme, raporlama ve sorun giderme

|     |
| --- |
| Azure AD parola koruması ve özel yasaklı parola listesi Azure Active Directory genel Önizleme özellikleri şunlardır. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD parola koruması dağıtıldıktan sonra izleme ve raporlama temel görevleridir. Bu makalede olduğu her hizmet bilgilerini kaydeder ve Azure AD parola koruması kullanımını bildirme anlamanıza yardımcı olmak üzere ayrıntıya gider.

## <a name="on-premises-logs-and-events"></a>Şirket içi günlüklerini ve olayları

### <a name="dc-agent-service"></a>DC Aracısı hizmeti

Her etki alanı denetleyicisinde DC Aracı hizmeti yazılımı kendi parola doğrulamaları (ve diğer durum) sonuçlarının bir yerel olay günlüğüne yazar: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin

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

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Örnek olay günlüğü iletisi için olay kimliği 10014 başarılı parola ayarlama

Değiştirilen parolayı belirtilen kullanıcı için geçerli Azure parola ilkesiyle uyumlu olarak doğrulandı.

 Kullanıcı adı: BPL_02885102771 tam adı:

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Örnek olay günlüğü iletisi için olay kimliği 10017 ve 30003 başarısız parola ayarlama

10017:

Geçerli Azure parola ilkesiyle uymadığından belirtilen kullanıcı için parolayı Sıfırla bağlantısı reddedildi. Bağıntılı olay günlüğü iletisi daha fazla ayrıntı için lütfen bkz.

 Kullanıcı adı: BPL_03283841185 tam adı:

30003:

En az bir geçerli Azure parola ilkesi Kiracı başına yasaklanmış parola listesinde mevcut belirteçlerin uymadığı için belirtilen kullanıcı için parolayı Sıfırla bağlantısı reddedildi.

 Kullanıcı adı: BPL_03283841185 tam adı:

Diğer bazı önemli olay günlüğü iletilerini dikkat edilmesi gereken şunlardır:

#### <a name="sample-event-log-message-for-event-id-30001"></a>Olay Kimliği 30001 örnek olay günlüğü iletisi

Belirtilen kullanıcının parolasını Azure parola ilkesi henüz kullanılabilir olmadığı için kabul edildi

Kullanıcı adı: <user> tam adı: <user>

Bu durum, bir veya daha fazla aşağıdaki nedenlerle: % n neden olabilir

1. Azure ile orman henüz kaydedilmemiş.

   Çözüm adımları: yönetici kaydı AzureADPasswordProtectionForest cmdlet'ini kullanarak orman kaydetmeniz gerekir.

2. Bir Azure AD parola koruması Proxy henüz geçerli ormandaki en az bir makine üzerinde kullanılabilir değil.

   Çözüm adımları: yönetici yüklemeli ve kayıt AzureADPasswordProtectionProxy cmdlet'ini kullanarak bir proxy kaydedin.

3. Bu etki alanı Denetleyicisinin herhangi bir Azure AD parola koruması Proxy örneği için ağ bağlantısı yok.

   Çözüm adımları: en az bir Azure AD parola koruması Proxy örneği için ağ bağlantısı olduğundan emin olun.

4. Bu etki alanı Denetleyicisinin, etki alanındaki diğer etki alanı denetleyicilerine bağlantı yok.

   Çözüm adımları: etki alanına ağ bağlantısı olduğundan emin olun.

#### <a name="sample-event-log-message-for-event-id-30006"></a>Olay Kimliği 30006 örnek olay günlüğü iletisi

Hizmet artık aşağıdaki Azure parola ilkesini zorunlu kıldığı.

 AuditOnly: 1

 Genel ilke tarih: 2018-05-15T00:00:00.000000000Z

 Kiracı İlkesi tarih: 2018-06-10T20:15:24.432457600Z

 Kiracı İlkesi Uygula: 1

#### <a name="dc-agent-log-locations"></a>DC aracı günlük konumları

DC Aracısı hizmeti de ilgili işletimsel olayları aşağıdaki günlüğüne: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational

Ayrıca hata ayıklama düzeyinde ayrıntılı izleme olayları DC Aracısı hizmeti aşağıdaki günlük kaydı için oturum açabilir: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace

> [!WARNING]
> İzleme günlüğü varsayılan olarak devre dışıdır. Etkin olduğunda, bu günlük yüksek hacimli olayları alır ve etki alanı denetleyicisi performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu Gelişmiş günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

### <a name="azure-ad-password-protection-proxy-service"></a>Azure AD parola koruması proxy hizmeti

Parola koruması Proxy Hizmeti aşağıdaki olay günlüğüne olayları en az bir dizi yayar: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational

Parola koruması Proxy Hizmeti ayrıca aşağıdaki günlük kaydı için ayrıntılı hata ayıklama düzeyinde izleme olayları kaydedebilirsiniz: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace

> [!WARNING]
> İzleme günlüğü varsayılan olarak devre dışıdır. Etkin olduğunda, bu günlük yüksek hacimli olayları alır ve bu proxy konağını performansını etkileyebilir. Bir sorunun daha kapsamlı bir araştırma gerektirdiğinde bu nedenle, bu günlük yalnızca etkinleştirilmiş olmalıdır ve yalnızca en az bir süre için.

### <a name="dc-agent-discovery"></a>DC Aracısı bulma

`Get-AzureADPasswordProtectionDCAgent` Cmdlet'i, bir etki alanı veya orman çalışan çeşitli DC aracılarla ilgili temel bilgileri görüntülemek için kullanılabilir. Bu bilgiler, çalışan DC aracı hizmetleri tarafından kayıtlı serviceConnectionPoint nesnelerden alınır. Bu cmdlet'in bir örnek çıktısı aşağıdaki gibidir:

```
PS C:\> Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

Çeşitli özellikleri, her DC Aracısı yaklaşık bir saatlik aralıklarla güncelleştirilir. Yine de Active Directory çoğaltma gecikmesine verilerdir.

Cmdlet'in sorgu kapsamı kullanarak etkilenebilir orman veya – etki alanı parametreleri.

### <a name="emergency-remediation"></a>Acil Durum düzeltme

Burada DC Aracı hizmeti sorunlarına neden olmaktan talihsiz bir durum meydana gelirse, DC aracı hizmetini hemen kapatılması. DC aracı parola filtresi DLL'sinin çalışan olmayan hizmeti çağırmak çalışır ve uyarı olayları (10012, 10013) günlüğe kaydeder, ancak bu süre boyunca tüm gelen parolaları kabul edilir. DC Aracı hizmeti daha sonra da aracılığıyla Windows Hizmet Denetimi Yöneticisi "Disabled" başlatma türüyle gerektiği şekilde yapılandırılmış olabilir.

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
> Sırayla bu adımları gerçekleştirmek önemlidir. Herhangi bir örneğini Proxy Hizmeti parola koruması bırakılması durumunda çalışan serviceConnectionPoint nesne düzenli aralıklarla yeniden oluşturmanız hem de düzenli aralıklarla sysvol durumu yeniden oluşturun.

1. Parola koruması ara yazılım, tüm makineleri kaldırın. Bu adım mu **değil** yeniden başlatılmasını gerektirir.
2. Tüm etki alanı denetleyicilerinden DC Aracısı yazılımı kaldırın. Bu adım **gerektirir** yeniden başlatma.
3. El ile her etki alanı adlandırma bağlamı tüm proxy hizmeti bağlantı noktalarını kaldırın. Bu nesnelerin konumu aşağıdaki Active Directory Powershell komutuyla bulunan:
   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{EBEFB703-6113-413D-9167-9F8DD4D24468}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Yıldız işareti kullanın ("*") sonunda $keywords değişken değeri.

   Elde edilen nesnenin aracılığıyla bulunan `Get-ADObject` komutu ardından ayrıştırılabileceği `Remove-ADObject`, veya el ile silindi. 

4. El ile tüm DC aracı bağlantı noktaları, her etki alanı adlandırma içeriği kaldırın. Olabilir bir genel Önizleme yazılımını yaygın olarak nasıl dağıtıldığına bağlı olarak ormandaki etki alanı denetleyicisi başına bu nesneler. Bu nesnenin konumu aşağıdaki Active Directory Powershell komutuyla bulunan:

   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{B11BB10A-3E7D-4D37-A4C3-51DE9D0F77C9}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Elde edilen nesnenin aracılığıyla bulunan `Get-ADObject` komutu ardından ayrıştırılabileceği `Remove-ADObject`, veya el ile silindi.

5. Orman düzeyinde yapılandırma durumunu el ile kaldırın. Orman yapılandırma durumu, Active Directory yapılandırma adlandırma bağlamında bir kapsayıcıda tutulur. Bulunan ve aşağıda silindi:

   ```
   $passwordProtectonConfigContainer = "CN=Azure AD password protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. El ile tüm sysvol durumu tarafından el ile ilgili kaldırın, aşağıdaki klasör ve tüm içeriğini silme:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Gerekirse, bu yolu yerel olarak belirtilen etki alanı denetleyicisinde erişilebilecek; Varsayılan konum şu yol şöyle olacaktır:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Sysvol paylaşımının varsayılan olmayan bir konumda yapılandırdıysanız, bu yolu farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

Genel ve özel yasaklı parola listelerini hakkında daha fazla bilgi için bkz [yasaklamak hatalı parola](concept-password-ban-bad.md)
