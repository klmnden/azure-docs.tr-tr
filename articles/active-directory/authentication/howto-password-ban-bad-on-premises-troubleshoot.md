---
title: Azure AD parola koruması önizlemede sorunlarını giderme
description: Azure AD parola koruması Önizleme genel sorun giderme anlama
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
ms.openlocfilehash: 5727965373752d40e3ce508c1bc79046c2b3b70b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177760"
---
# <a name="preview-azure-ad-password-protection-troubleshooting"></a>Önizleme: Azure AD parola koruması sorunlarını giderme

|     |
| --- |
| Azure AD parola koruması, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD parola koruması dağıtıldıktan sonra sorun giderme gerekebilir. Bu makalede bazı genel sorun giderme adımlarını anlamanıza yardımcı olması için ayrıntıya gider.

## <a name="weak-passwords-are-not-getting-rejected-as-expected"></a>Zayıf parolalarda beklendiği gibi reddedilir değil

Bu, birkaç olası nedeni olabilir:

1. DC aracılar henüz bir ilkesini yükleyip yüklemediğinizi değil. 30001 olayları DC aracı yönetici olay günlüğünde bu belirtisidir.

    Bu sorunun olası nedenleri şunlardır:

    1. Orman henüz kayıtlı değil
    2. Proxy henüz kayıtlı değil
    3. Ağ bağlantısı sorunları (onay HTTP Proxy gereksinimlerini) Azure ile iletişim kurmasını Proxy hizmeti engelliyor

2. Parola ilkesini zorunlu kıl modunda denetim için hala ayarlanmış durumda. Bu durumda, Azure AD parola koruması portalı kullanarak zorla için yeniden yapılandırın. Lütfen [etkinleştirme parola koruması](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

3. Parola ilkesini devre dışı bırakıldı. Bu durumda, etkin olarak Azure AD parola koruması portalını kullanarak yeniden yapılandırın. Lütfen [etkinleştirme parola koruması](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

4. Parola doğrulama algoritması beklendiği gibi çalışıyor olabilir. Lütfen [de parolaları nasıl değerlendirilir](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Dizin Hizmetleri Onarım Modu'nda

Etki alanı denetleyicisi Dizin Hizmetleri Onarım Modu'nda önyüklenir, DC Aracı hizmeti bunu algılar ve tüm parola doğrulama veya zorlama etkinlikleri, bağımsız olarak şu anda etkin ilke yapılandırmasını devre dışı bırakılması neden olur.

## <a name="emergency-remediation"></a>Acil Durum düzeltme

Burada, DC Aracı hizmeti sorunlara neden olan bir durum meydana gelirse, DC aracı hizmetini hemen kapatılması. DC aracı parola filtresi DLL'sinin hala çalışan olmayan hizmeti çağırmak çalışır ve uyarı olayları (10012, 10013) günlüğe kaydeder, ancak bu süre boyunca tüm gelen parolaları kabul edilir. DC Aracı hizmeti daha sonra da aracılığıyla Windows Hizmet Denetimi Yöneticisi "Disabled" başlatma türüyle gerektiği şekilde yapılandırılmış olabilir.

Hayır, Azure AD parola koruması portalında etkinleştir modu ayarlamak için başka bir düzeltme ölçü olacaktır. Güncelleştirilmiş ilke İndirildikten sonra her DC Aracısı burada tüm parolalar kabul edildiği olarak gerçekleştirilmeye moduna geçer-olduğu. Daha fazla bilgi için [zorunlu kıl modunda](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Etki alanı denetleyicisinin indirgemesi

Hala DC Aracısı yazılımını çalıştıran bir etki alanı denetleyicisini indirgemek için desteklenir. Yöneticiler ancak DC Aracısı yazılımını indirgeme işlemi sırasında geçerli Şifre politikasını devam ettiğinden bilmeniz gerekir. (İndirme işlemi bir parçası olarak belirtilen) yeni yerel yönetici hesabı parolası gibi başka bir parola doğrulanır. Microsoft, güvenli parolalar DC indirgeme yordamının parçası olarak yerel yönetici hesapları için seçilmesi önerir; ancak yeni yerel yönetici hesabı parolasını DC Aracısı yazılımı tarafından doğrulanmasını indirgeme işlem yordamlarını önceden varolan karışıklığa neden olabilir.

İndirgeme başarılı oldu ve etki alanı denetleyicisi gizleyip gizlemeyeceğini ve normal üye sunucu olarak yeniden çalıştırmayı sonra pasif modunda çalışan DC Aracısı yazılımını döner. Ardından herhangi bir zamanda kaldırılabilir.

## <a name="removal"></a>Çıkarma

Etki alanı ve orman ilgili tüm durum temizleme ve genel Önizleme yazılımını kaldırmak için karar verilir, bu görevi, aşağıdaki adımları kullanarak gerçekleştirilebilir:

> [!IMPORTANT]
> Sırayla bu adımları gerçekleştirmek önemlidir. Proxy Hizmeti herhangi bir örneğini çalıştıran bırakılırsa düzenli aralıklarla serviceConnectionPoint nesne yeniden oluşturur. DC Aracısı hizmeti herhangi bir örneğini çalıştıran bırakılırsa düzenli aralıklarla serviceConnectionPoint nesne ve sysvol durumu yeniden oluşturun.

1. Ara yazılım, tüm makineleri kaldırın. Bu adım mu **değil** yeniden başlatılmasını gerektirir.
2. Tüm etki alanı denetleyicilerinden DC Aracısı yazılımı kaldırın. Bu adım **gerektirir** yeniden başlatma.
3. El ile her etki alanı adlandırma bağlamı tüm Proxy Hizmeti bağlantı noktalarını kaldırın. Bu nesnelerin konumu aşağıdaki Active Directory PowerShell komutuyla bulunan:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Yıldız işareti kullanın ("*") sonunda $keywords değişken değeri.

   Aracılığıyla elde edilen nesne bulundu `Get-ADObject` komutu ardından ayrıştırılabileceği `Remove-ADObject`, veya el ile silindi. 

4. El ile tüm DC aracı bağlantı noktaları, her etki alanı adlandırma içeriği kaldırın. Olabilir bir genel Önizleme yazılımını yaygın olarak nasıl dağıtıldığına bağlı olarak ormandaki etki alanı denetleyicisi başına bu nesneler. Bu nesnenin konumu aşağıdaki Active Directory PowerShell komutuyla bulunan:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Aracılığıyla elde edilen nesne bulundu `Get-ADObject` komutu ardından ayrıştırılabileceği `Remove-ADObject`, veya el ile silindi.

   Yıldız işareti kullanın ("*") sonunda $keywords değişken değeri.

5. Orman düzeyinde yapılandırma durumunu el ile kaldırın. Orman yapılandırma durumu, Active Directory yapılandırma adlandırma bağlamında bir kapsayıcıda tutulur. Bulunan ve aşağıda silindi:

   ```PowerShell
   $passwordProtectionConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectionConfigContainer
   ```

6. El ile tüm sysvol durumu tarafından el ile ilgili kaldırın, aşağıdaki klasör ve tüm içeriğini silme:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   Gerekirse, bu yolu yerel olarak belirtilen etki alanı denetleyicisinde erişilebilecek; Varsayılan konum şu yol şöyle olacaktır:

   `%windir%\sysvol\domain\Policies\AzureADPasswordProtection`

   Sysvol paylaşımının varsayılan olmayan bir konumda yapılandırdıysanız, bu yolu farklıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola koruması için sık sorulan sorular](howto-password-ban-bad-on-premises-faq.md)

Genel ve özel yasaklı parola listelerini hakkında daha fazla bilgi için bkz [yasaklamak hatalı parola](concept-password-ban-bad.md)
