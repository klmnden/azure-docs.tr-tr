---
title: Azure AD parola koruması - Azure Active Directory sorun giderme
description: Azure AD parola koruması genel sorun giderme anlama
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
ms.openlocfilehash: 108ead982529d2ac6549cceffd9d2177ab6456bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60414775"
---
# <a name="azure-ad-password-protection-troubleshooting"></a>Azure AD parola koruması sorunlarını giderme

Azure AD parola koruması dağıtıldıktan sonra sorun giderme gerekebilir. Bu makalede bazı genel sorun giderme adımlarını anlamanıza yardımcı olması için ayrıntıya gider.

## <a name="the-dc-agent-cannot-locate-a-proxy-in-the-directory"></a>DC aracının bir proxy dizininde bulunamıyor

Ana bu sorunun DC aracı yönetici olay günlüğünde 30017 olayları belirtisidir.

Bu sorunun her zamanki nedeni, bir ara sunucu henüz kaydedilmemiş, ' dir. Kayıtlı bir ara sunucu, belirli bir DC aracı proxy görmeye olana kadar AD çoğaltma gecikmesi nedeniyle bazı gecikme olabilir.

## <a name="the-dc-agent-is-not-able-to-communicate-with-a-proxy"></a>DC Aracısı Ara sunucu ile iletişim kurabildiğini değil

Ana bu sorunun DC aracı yönetici olay günlüğünde 30018 olayları belirtisidir. Bu, birkaç olası nedeni olabilir:

1. DC aracının kayıtlı proxy(s) ağ bağlantısı izin vermeyen ağ yalıtılmış bir kısmında bulunur. Bu sorun, bu nedenle diğer DC aracıları, çoğaltma sysvol paylaşımının ilke dosyaları aracılığıyla yalıtılmış DC tarafından alınır, Azure tarafından sağlanan parola ilkelerini indirilebilmesi proxy(s) ile iletişim sürece expected\benign olabilir.

1. Proxy ana makinenin RPC uç nokta Eşleyici uç noktasını (bağlantı noktası 135) erişimi engelliyor

   Azure AD parola koruması Proxy yükleyici 135 bağlantı noktasına erişime izin veren bir Windows Güvenlik Duvarı gelen kuralı otomatik olarak oluşturur. Bu kural daha sonra silindi veya devre dışı bırakılırsa, DC aracıları proxy'si hizmeti ile iletişim kurmada başarısız olacaktır. Windows Güvenlik Duvarı yerleşik yerine başka bir güvenlik duvarı ürünü devre dışı bırakılırsa, bağlantı noktası 135 erişmesine izin vermek için güvenlik duvarını yapılandırmanız gerekir.

1. Proxy konak makinesi üzerinde Proxy Hizmeti tarafından dinledik RPC uç noktasına (dinamik veya statik) erişimi engelliyor

   Azure AD parola koruması Proxy yükleyiciyi bir Windows Güvenlik Duvarı otomatik olarak oluşturur. herhangi bir gelen bağlantı noktasına erişime izin veren gelen kuralı dinledik için Azure AD parola koruması Proxy Hizmeti tarafından. Bu kural daha sonra silindi veya devre dışı bırakılırsa, DC aracıları proxy'si hizmeti ile iletişim kurmada başarısız olacaktır. Windows Güvenlik Duvarı yerleşik yerine başka bir güvenlik duvarı ürünü devre dışı bırakılırsa, yapılandırmalısınız gelen bağlantı noktalarının erişmesine izin vermek için güvenlik duvarı tarafından Azure AD parola koruması Proxy Hizmeti için yaptık. Bu yapılandırma, Proxy Hizmeti, belirli bir statik RPC bağlantı noktasında dinleyecek şekilde yapılandırılmış olması halinde daha belirli yapılabilir (kullanarak `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet'i).

## <a name="the-proxy-service-can-receive-calls-from-dc-agents-in-the-domain-but-is-unable-to-communicate-with-azure"></a>Proxy hizmet çağrıları etki alanındaki DC aracılardan alabilir, ancak Azure ile iletişim kuramıyor

1. Proxy makine listelenen uç noktalarına bağlantıyı olduğundan [dağıtım gereksinimleri](howto-password-ban-bad-on-premises-deploy.md).

1. Orman ve tüm proxy sunucuları aynı Azure kiracısı karşı kayıtlı emin olun.

   Çalıştırarak bunu denetleyebilirsiniz `Get-AzureADPasswordProtectionProxy` ve `Get-AzureADPasswordProtectionDCAgent` PowerShell cmdlet'lerini, ardından karşılaştırma `AzureTenant` özelliği her öğe döndürdü. Doğru işlem için bunlar aynı orman içindeki tüm DC aracıları ve proxy sunucuları arasında olmalıdır.

   Bir Azure kiracısı kayıt eşleşmiyor durumu mevcut değilse bu çalıştırarak onarılabilir `Register-AzureADPasswordProtectionProxy` ve/veya `Register-AzureADPasswordProtectionForest` tüm kayıtlar için aynı Azure Kiracı kimlik bilgilerini sağlamaktan gerektiği gibi PowerShell cmdlet'leri.

## <a name="the-dc-agent-is-unable-to-encrypt-or-decrypt-password-policy-files-and-other-state"></a>DC Aracısı şifrelemek veya parola ilkesi dosyaları ve diğer durum şifresini çözmek alamıyor

Bu sorun belirtileri çeşitli ile bildirilebilir, ancak genellikle bir ortak kök nedeni vardır.

Azure AD parola koruması, Windows Server 2012 çalıştıran etki alanı denetleyicilerinde ve üzeri olan Microsoft anahtar dağıtım hizmeti tarafından sağlanan şifreleme ve şifre çözme işlevselliği kritik bağımlılığı vardır. KDS hizmeti, etkin ve tüm Windows Server 2012 ve sonraki etki alanı denetleyicilerine de bir işlev olmalıdır.

KDS varsayılan olarak hizmetin hizmet başlangıç modunu el ile (tetikleyiciyi başlatın) olarak yapılandırılır. Bu yapılandırma, bir istemci hizmeti kullanmayı dener ilk kez üzerine başladığını anlamına gelir. Bu varsayılan hizmet başlangıç modu çalışmak Azure AD parola koruması için kabul edilebilir.

KDS hizmeti başlangıç modu devre dışı olarak yapılandırılmışsa, Azure AD parola koruması düzgün çalışması için önce bu yapılandırmayı düzeltilmesi gerekir.

KDS hizmeti, hizmet yönetim MMC konsolu aracılığıyla ya da el ile başlatmak için bu sorun için basit bir test olduğu veya diğer hizmet Yönetim Araçları (örneğin, "net start kdssvc" bir komut istemi konsoldan çalıştırma) kullanarak. KDS hizmeti başarıyla başlatmak ve çalıştırmak için bekleniyor.

Başlangıç erişememe KDS hizmeti için en yaygın kök nedeni, Active Directory etki alanı denetleyicisi nesnesini varsayılan etki alanı denetleyicileri kuruluş dışında bulunan olmasıdır. Bu yapılandırma KDS hizmeti tarafından desteklenmiyor ve Azure AD parola koruması tarafından uygulanan bir kısıtlama değildir. Bu koşul için düzeltme etki alanı denetleyicisi nesnesini varsayılan etki alanı denetleyicileri OU altında bir konuma taşımaktır.

## <a name="weak-passwords-are-being-accepted-but-should-not-be"></a>Zayıf parolalarda kabul edilir ancak olmamalıdır

Bu sorunu çeşitli nedenleri olabilir.

1. DC Aracısı bir ilkesini indiremiyorsunuz veya mevcut ilkeleri şifresi çözülemiyor. Yukarıdaki konularında olası nedenleri kontrol edin.

1. Parola ilkesini zorunlu kıl modunda denetim için hala ayarlanmış durumda. Bu yapılandırma etkinse, Azure AD parola koruması portalı kullanarak zorla için yeniden yapılandırın. Bkz: [etkinleştirme parola koruması](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. Parola ilkesini devre dışı bırakıldı. Bu yapılandırma etkinse, etkin olarak Azure AD parola koruması portalını kullanarak yeniden yapılandırın. Bkz: [etkinleştirme parola koruması](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

1. DC aracı yazılımı, etki alanındaki tüm etki alanı denetleyicilerinde yüklemediniz. Bu durumda, uzak Windows istemcileri bir özel etki alanı denetleyicisine bir parola değişikliği işlemi sırasında hedef olmak zordur. Sahip olabileceğini düşünüyorsanız, başarıyla hedeflenen DC Aracısı yazılımının yüklü olduğu belirli bir DC, DC aracı Yöneticisi olay günlüğü izlemektir tarafından doğrulayabilirsiniz: sonuç bağımsız olarak olacaktır parola sonuçları belgelemek için en az bir olay doğrulama. Hiçbir olay parolasını değiştirildiğinde kullanıcı için mevcut ise, parola değiştirme büyük olasılıkla farklı bir etki alanı denetleyicisi tarafından işlendi.

   Alternatif bir test olarak doğrudan DC Aracısı yazılımının yüklü olduğu bir DC oturumunuz açıkken setting\changing parolaları deneyin. Bu teknik, üretim Active Directory etki alanları için önerilmez.

   Artımlı DC Aracısı yazılım dağıtımını bu sınırlamalara tabi desteklense de, Microsoft DC Aracısı yazılımını bir etki alanındaki tüm etki alanı denetleyicilerinde olabildiğince çabuk yüklendiğini önerir.

1. Parola doğrulama algoritması gerçekten beklendiği gibi çalışıyor olabilir. Bkz: [de parolaları nasıl değerlendirilir](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Dizin Hizmetleri Onarım Modu'nda

Etki alanı denetleyicisi Dizin Hizmetleri Onarım Modu'nda önyüklenir, DC Aracı hizmeti bu durumu algılar ve tüm parola doğrulama veya zorlama etkinlikleri, bağımsız olarak şu anda etkin ilke yapılandırmasını devre dışı bırakılması neden olur.

## <a name="emergency-remediation"></a>Acil Durum düzeltme

Burada, DC Aracı hizmeti sorunlara neden olan bir durum meydana gelirse, DC aracı hizmetini hemen kapatılması. DC aracı parola filtresi DLL'sinin hala çalışan olmayan hizmeti çağırmak çalışır ve uyarı olayları (10012, 10013) günlüğe kaydeder, ancak bu süre boyunca tüm gelen parolaları kabul edilir. DC Aracı hizmeti daha sonra da aracılığıyla Windows Hizmet Denetimi Yöneticisi "Disabled" başlatma türüyle gerektiği şekilde yapılandırılmış olabilir.

Hayır, Azure AD parola koruması portalında etkinleştir modu ayarlamak için başka bir düzeltme ölçü olacaktır. Güncelleştirilmiş ilke İndirildikten sonra her DC Aracısı burada tüm parolalar kabul edildiği olarak gerçekleştirilmeye moduna geçer-olduğu. Daha fazla bilgi için [zorunlu kıl modunda](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Etki alanı denetleyicisinin indirgemesi

Hala DC Aracısı yazılımını çalıştıran bir etki alanı denetleyicisini indirgemek için desteklenir. Yöneticiler ancak DC Aracısı yazılımını indirgeme işlemi sırasında geçerli Şifre politikasını devam ettiğinden bilmeniz gerekir. (İndirme işlemi bir parçası olarak belirtilen) yeni yerel yönetici hesabı parolası gibi başka bir parola doğrulanır. Microsoft, güvenli parolalar DC indirgeme yordamının parçası olarak yerel yönetici hesapları için seçilmesi önerir; ancak yeni yerel yönetici hesabı parolasını DC Aracısı yazılımı tarafından doğrulanmasını indirgeme işlem yordamlarını önceden varolan karışıklığa neden olabilir.

İndirgeme başarılı oldu ve etki alanı denetleyicisi gizleyip gizlemeyeceğini ve normal üye sunucu olarak yeniden çalıştırmayı sonra pasif modunda çalışan DC Aracısı yazılımını döner. Ardından herhangi bir zamanda kaldırılabilir.

## <a name="removal"></a>Çıkarma

Temizleme ve Azure AD parola koruması yazılımı etki alanı ve orman ilgili tüm durum kaldırmanız karar, bu görevi, aşağıdaki adımları kullanarak gerçekleştirilebilir:

> [!IMPORTANT]
> Sırayla bu adımları gerçekleştirmek önemlidir. Proxy Hizmeti herhangi bir örneğini çalıştıran bırakılırsa düzenli aralıklarla serviceConnectionPoint nesne yeniden oluşturur. DC Aracısı hizmeti herhangi bir örneğini çalıştıran bırakılırsa düzenli aralıklarla serviceConnectionPoint nesne ve sysvol durumu yeniden oluşturun.

1. Ara yazılım, tüm makineleri kaldırın. Bu adım mu **değil** yeniden başlatılmasını gerektirir.
2. Tüm etki alanı denetleyicilerinden DC Aracısı yazılımı kaldırın. Bu adım **gerektirir** yeniden başlatma.
3. El ile her etki alanı adlandırma bağlamı tüm Proxy Hizmeti bağlantı noktalarını kaldırın. Bu nesnelerin konumu aşağıdaki Active Directory PowerShell komutuyla bulunan:

   ```powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Yıldız işareti kullanın ("*") sonunda $keywords değişken değeri.

   Aracılığıyla elde edilen nesne bulundu `Get-ADObject` komutu ardından ayrıştırılabileceği `Remove-ADObject`, veya el ile silindi.

4. El ile tüm DC aracı bağlantı noktaları, her etki alanı adlandırma içeriği kaldırın. Olabilir bir yazılım yaygın olarak nasıl dağıtıldığına bağlı olarak ormandaki etki alanı denetleyicisi başına bu nesneler. Bu nesnenin konumu aşağıdaki Active Directory PowerShell komutuyla bulunan:

   ```powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Aracılığıyla elde edilen nesne bulundu `Get-ADObject` komutu ardından ayrıştırılabileceği `Remove-ADObject`, veya el ile silindi.

   Yıldız işareti kullanın ("*") sonunda $keywords değişken değeri.

5. Orman düzeyinde yapılandırma durumunu el ile kaldırın. Orman yapılandırma durumu, Active Directory yapılandırma adlandırma bağlamında bir kapsayıcıda tutulur. Bulunan ve aşağıda silindi:

   ```powershell
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
