---
title: Azure AD parola koruması Önizleme dağıtma
description: Dağıtın yasaklamak yanlış parolalar şirket içi Azure AD parola koruması önizlemesi
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 5774af4e0550ceb7a51e399fcab203a503a7f23f
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54033613"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Önizleme: Azure AD parola korumasını dağıtma

|     |
| --- |
| Azure AD parola koruması, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Biz bir anlayışa sahip olduğunuza göre [Windows Server Active Directory için Azure AD parola koruması zorlama yapma](concept-password-ban-bad-on-premises.md), planlama ve dağıtım yürütmek için sonraki adımdır.

## <a name="deployment-strategy"></a>Dağıtım stratejisi

Microsoft, herhangi bir dağıtıma denetim modunda başlatmak önerir. Denetleme modunda, burada parolalar ayarlamak devam edebilirsiniz engelleneceğini tüm girişleri oluşturup olay günlüğünde varsayılan ilk ayardır. Proxy sunucusu ve DC aracıları tam denetim modunda dağıtılan sonra normal izleme yapılmalıdır ilke zorlandı hangi etkisi parola ilkesi belirlemek için zorlama kullanıcılar ve ortam üzerinde gerekir.

Denetim aşamasında pek çok kuruluş bulun:

* Daha güvenli parolalar kullanmak için mevcut işlem süreçlerini iyileştirmek gerekir.
* Kullanıcılar, güvenli parolaları düzenli olarak seçme alışmanızı sağlamak
* Kullanıcılar güvenlik zorlama, etkisi olan ve daha iyi yardımcı yaklaşan değişikliği hakkında nasıl daha güvenli parolalar seçebilirsiniz anlamak bildirmeniz gerekir.

Gelen özellik için makul bir süre denetim modunda çalıştırıldıktan sonra uygulama yapılandırma çevrilebilir **denetim** için **zorla** böylece daha güvenli bir parola gerektirme. Odaklanmış bu süre boyunca izleme iyi bir fikirdir.

## <a name="deployment-requirements"></a>Dağıtım gereksinimleri

* Azure AD parola DC koruma aracısı yüklü olduğu tüm etki alanı denetleyicileri Windows Server 2012 veya sonraki sürümünü çalıştırmalıdır.

   > [!NOTE]
   > Sunucu Çekirdek tabanlı işletim sistemleri, şu anda desteklenmemektedir. Bu destek için büyüyecek planlanmaktadır.

* Azure AD parola koruması Proxy Hizmeti yükleneceği tüm makineler, Windows Server 2012 R2 çalıştırmalıdır veya üzeri.

   > [!NOTE]
   > Sunucu Çekirdek tabanlı işletim sistemleri, şu anda desteklenmemektedir. Bu destek için büyüyecek planlanmaktadır.

* Etki alanı denetleyicileri de dahil olmak üzere Azure AD parola koruması bileşenlerinin yüklendiği tüm makinelerde yüklü olan evrensel C çalışma zamanı olması gerekir.
Bu tercihen Windows Update aracılığıyla makine tam olarak düzeltme eki uygulama tarafından gerçekleştirilir. Aksi takdirde uygun işletim sistemine özgü güncelleştirme paketi olabilir yüklü - [Windows Evrensel C çalışma zamanı güncelleştirmesi](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows)
* En az bir sunucu Azure AD parola koruması proxy hizmetini barındıran her etki alanında en az bir etki alanı denetleyicisi arasında ağ bağlantısı olmalıdır. Bu RPC uç nokta Eşleyici bağlantı noktası (135) ve RPC sunucusu bağlantı noktası proxy hizmetine erişmek etki alanı denetleyicisi bağlanmaya izin vermelidir.  RPC sunucusu bağlantı noktası varsayılan olarak dinamik bir RPC bağlantı noktasıdır, ancak yapılandırılabilir (aşağıya bir statik bağlantı noktasını kullanacak şekilde bakın).
* Azure AD parola koruması proxy hizmeti barındıran tüm makinelerde aşağıdaki uç noktalarına ağ erişimi olması gerekir:

    |Uç Nokta |Amaç|
    | --- | --- |
    |`https://login.microsoftonline.com`|Kimlik doğrulama istekleri|
    |`https://enterpriseregistration.windows.net`|Azure AD parola koruması işlevi|

* Azure AD parola koruması proxy hizmeti ve orman Azure AD'ye kaydetmeniz için bir genel yönetici hesabı.
* Azure AD ile Windows Server Active Directory orman kaydetmek için orman kök etki alanındaki Active Directory etki alanı yönetici ayrıcalıklarına sahip bir hesap.
* DC çalıştıran herhangi bir Active Directory etki alanı Aracı hizmeti yazılımı DFSR sysvol çoğaltma için kullanmanız gerekir.

## <a name="single-forest-deployment"></a>Tek ormanlı dağıtımı

Azure AD parola koruması önizlemesi, en fazla iki sunucularda proxy hizmeti ile dağıtılır ve DC Aracı hizmeti, Active Directory ormanındaki tüm etki alanı denetleyicilerine artımlı olarak dağıtılabilir.

![Azure AD parola koruması bileşenleri birlikte nasıl çalıştığını](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>Yazılımı indirin

Adresinden indirilip Azure AD parola koruması için gerekli iki yükleyiciler vardır [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=57071)

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Azure AD parola koruması proxy hizmetini yükleme ve yapılandırma

1. Azure AD parola koruması proxy hizmeti barındırmak için bir veya daha fazla sunucu seçin.
   * Her bir hizmet bir tek orman için yalnızca parola ilkeleri sağlayabilir ve ana makine etki alanına bir etki alanına katılmış olmalıdır (kök ve alt her ikisi de eşit derecede desteklenir) o ormandaki. Kendi görevi yerine getirmek Azure AD parola koruması proxy hizmeti için en az bir DC ormandaki her etki alanında ve Azure AD parola koruması Proxy konak makine arasında ağ bağlantısı bulunmalıdır.
   * Yüklemek ve Azure AD parola koruması proxy hizmeti amacıyla ancak etki alanı denetleyicisi test etmek için bir etki alanı denetleyicisinde çalıştırmak için desteklenen ardından Internet bağlantısı olmasını gerektirir.

   > [!NOTE]
   > Genel Önizleme iki (2) proxy sunucular orman başına en fazla destekler.

2. Parola İlkesi Proxy Hizmeti yazılımı AzureADPasswordProtectionProxy.msi MSI paketini kullanarak yükleyin.
   * Yazılım yüklemesi yeniden başlatma gerektirmez. Yazılım yükleme, örneğin standart MSI yordamları kullanarak otomatik: `msiexec.exe /i AzureADPasswordProtectionProxy.msi /quiet /qn`

      > [!NOTE]
      > AzureADPasswordProtectionProxy.msi MSI paketini yüklemeden önce Windows Güvenlik Duvarı hizmeti çalışıyor olmalıdır; aksi takdirde yükleme hatası meydana gelir. Çalıştırma için Windows Güvenlik Duvarı yapılandırılmışsa, geçici olarak etkinleştirin ve Windows Güvenlik Duvarı hizmeti yükleme işlemi sırasında başlatmak için çözüm olabilir. Ara yazılım, yüklemeden sonra Windows Güvenlik duvarı yazılımı belirli hiçbir bağımlılığı vardır. Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, yine de dağıtım gereksinimlerini karşılamak için yapılandırılması gerekir (bağlantı noktası 135 gelen erişimi ve RPC proxy sunucusu bağlantı noktası dinamik veya statik izin ver). [Bkz: dağıtım gereksinimleri](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements)

3. Yönetici olarak bir PowerShell penceresi açın.
   * Azure AD parola koruması ara yazılım AzureADPasswordProtection adlı yeni bir PowerShell modülü içerir. Aşağıdaki adımlar, bu PowerShell modülünden çeşitli cmdlet'ler çalıştıran temel alır ve yeni bir PowerShell penceresi açar ve yeni modül aşağıdaki gibi içe varsayalım:
      * `Import-Module AzureADPasswordProtection`

      > [!NOTE]
      > Yükleme yazılımını ana makinenin PSModulePath ortam değişkeni değiştirir. AzureADPasswordProtection powershell modülünü içeri aktarıldı ve kullanılan etkili olması bu değişikliğin sırayla yeni bir PowerShell konsol penceresi açmanız gerekebilir.

   * Aşağıdaki PowerShell komutunu kullanarak hizmetinin çalıştığını kontrol edin: `Get-Service AzureADPasswordProtectionProxy | fl`.
      * Sonucu bir sonuç ile üretmelidir **durumu** "Çalışıyor" sonucu döndürerek.

4. Proxy kaydedin.
   * 3. adım tamamlandıktan sonra Azure AD parola koruması proxy hizmeti makinede çalışıyor, ancak henüz Azure AD ile iletişim kurmak için gerekli kimlik bilgilerine sahip değil. Azure AD ile kayıt kullanarak bu özelliği etkinleştirmek için gerekli `Register-AzureADPasswordProtectionProxy` PowerShell cmdlet'i. Cmdlet, genel yönetici olmak üzere Azure kiracısı için kimlik bilgileri hem de şirket içinde orman kök etki alanındaki Active Directory etki alanı yöneticisi ayrıcalıkları gerektirir. Belirli bir proxy hizmeti için ek çağrıları başarılı oldu sonra `Register-AzureADPasswordProtectionProxy` başarılı olması devam eder ancak gereksizdir.
      * Cmdlet şu şekilde çalıştırılabilir:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionProxy -AzureCredential $tenantAdminCreds
         ```

         Örneğin, yalnızca şu anda oturum açmış kullanıcı aynı zamanda kök etki alanı için Active Directory etki alanı yönetici ise çalışır. İle gerekli etki alanı kimlik bilgilerini sağlamanız alternatiftir `-ForestCredential` parametresi.

   > [!NOTE]
   > Internet Explorer Artırılmış Güvenlik Yapılandırması etkinse, bu işlemi başarısız olabilir. Geçici çözüm olan IESC devre dışı bırakmak, kayıt proxy IESC daha sonra yeniden etkinleştirin.

   > [!NOTE]
   > Azure AD parola koruması proxy hizmet kaydının, tek seferlik bir adım hizmetinin yaşam süresi olması beklenir. Proxy hizmeti, gerekli herhangi bir maintainenance ve sonraki sürümlerde bu noktadan itibaren otomatik olarak gerçekleştirir. Belirli bir orman için başarılı oldu sonra ek çağrıları 'Register-AzureADPasswordProtectionProxy' başarılı olmaya devam ancak gereksizdir.

   > [!TIP]
   > İlk kez cmdlet'ini yürütme tamamlanmadan önce bu cmdlet için belirli bir Azure kiracısı çalıştırdığınızda önemli bir gecikme (birçok saniye) olabilir. Bir hata bildirdi sürece bu gecikme açılan kutuyla düşünülmemelidir.

5. Orman kaydedin.
   * Şirket içi Active Directory ormanında, Azure'ı kullanarak iletişim kurmak için gerekli kimlik bilgileriyle başlatılmalıdır `Register-AzureADPasswordProtectionForest` Powershell cmdlet'i. Cmdlet, genel yönetici olmak üzere Azure kiracısı için kimlik bilgileri hem de şirket içinde orman kök etki alanındaki Active Directory etki alanı yöneticisi ayrıcalıkları gerektirir. Bu adım, orman bir kez çalıştırın.
      * Cmdlet şu şekilde çalıştırılabilir:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $tenantAdminCreds
         ```

         Örneğin, yalnızca şu anda oturum açmış kullanıcı aynı zamanda kök etki alanı için Active Directory etki alanı yönetici ise çalışır. -ForestCredential parametresi ile gerekli etki alanı kimlik bilgilerini sağlamak için kullanılan bir alternatiftir.

         > [!NOTE]
         > Internet Explorer Artırılmış Güvenlik Yapılandırması etkinse, bu işlemi başarısız olabilir. Geçici çözüm olan IESC devre dışı bırakmak, kayıt proxy IESC daha sonra yeniden etkinleştirin.

         > [!NOTE]
         > Birden fazla ara sunucuyu ortamınızda yüklü değilse, hangi proxy sunucusu yukarıdaki yordamın belirtilmiş bir önemi yoktur.

         > [!TIP]
         > İlk kez cmdlet'ini yürütme tamamlanmadan önce bu cmdlet için belirli bir Azure kiracısı çalıştırdığınızda önemli bir gecikme (birçok saniye) olabilir. Bir hata bildirdi sürece bu gecikme açılan kutuyla düşünülmemelidir.

   > [!NOTE]
   > Active Directory ormanı kayıt, tek seferlik bir adım ormanın yaşam süresi olması beklenir. Ormanda çalıştıran etki alanı denetleyicisi aracıları, gerekli herhangi bir maintainenance ve sonraki sürümlerde bu noktadan itibaren otomatik olarak gerçekleştirir. Bu işlem, ek çağrıları gibi belirli bir orman için başarılı `Register-AzureADPasswordProtectionForest` başarılı olması devam eder ancak gereksizdir.

   > [!NOTE]
   > Sırayla `Register-AzureADPasswordProtectionForest` en az bir Windows Server 2012 veya üzeri etki alanı başarılı olması için denetleyici proxy sunucunun etki alanında bulunmalıdır. Ancak bu adımı öncesinde herhangi bir etki alanı denetleyicilerinde DC Aracısı yazılımının yüklenmesi gereksinimi yoktur.

6. İsteğe bağlı: Belirli bir bağlantı noktasında dinleyecek şekilde Azure AD parola koruması proxy hizmeti yapılandırın.
   * TCP üzerinden RPC, Azure AD parola koruması proxy hizmeti ile iletişim kurmak için Azure AD parola koruması'nı etki alanı denetleyicilerinde DC Aracısı yazılım tarafından kullanılır. Varsayılan olarak, Azure AD parola koruması parola ilkesi Proxy Hizmeti tüm kullanılabilir dinamik RPC uç nokta üzerinde dinler. Gerekirse ağ topolojisi veya güvenlik duvarı gereksinimleri nedeniyle, hizmet yerine belirli bir TCP bağlantı noktasında dinleyecek şekilde yapılandırılabilir.
      * Hizmetini, statik bir bağlantı noktası altında çalışacak şekilde yapılandırmak için kullanın `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet'i.
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Durdur ve bu değişikliklerin devreye girmesi hizmeti yeniden başlatmanız gerekir.

      * Hizmetini, dinamik bir bağlantı noktası altında çalışacak şekilde yapılandırmak için aynı yordamı ancak StaticPort geri sıfır olarak ayarlanmış kullanım şu şekilde:
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Durdur ve bu değişikliklerin devreye girmesi hizmeti yeniden başlatmanız gerekir.

   > [!NOTE]
   > Herhangi bir bağlantı noktası yapılandırmasını değiştirdikten sonra Azure AD parola koruması proxy hizmeti el ile yeniden başlatma gerektirir. Bu niteliği yapılandırma değişikliklerini yaptıktan sonra etki alanı denetleyicilerinde çalışan DC Aracı hizmeti yazılımı yeniden başlatmak gerekli değildir.

   * Hizmet geçerli yapılandırmasını kullanarak sorgulanan `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet'i aşağıdaki örnekte gösterildiği gibi:

      ```
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0 
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Azure AD parola koruması DC Aracısı Hizmeti'ni yükleme

* Azure AD parola koruması DC Aracısı hizmeti yazılım kullanılarak `AzureADPasswordProtectionDCAgent.msi` MSI paketi:
   * Yazılım yüklemesi üzerinde yükleme yeniden başlatmayı gerektirdiği ve parola filtresi DLL'leri yalnızca yüklenen veya bir yeniden başlatmanın kaldırıldığında, işletim sistemi gereksinimi nedeniyle kaldırın.
   * DC Aracısı henüz bir etki alanı denetleyicisi olmayan bir makineye yüklemek için desteklenir. Bu durumda, hizmet başlar ve çalışma ancak aksi makine bir etki alanı denetleyicisi olarak yükseltildikten sonra kadar etkin değil.

   Yazılım yükleme, örneğin standart MSI yordamları kullanarak otomatik:  `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > Örnek msiexec komut bir hemen yeniden başlatılmasına neden olur; Bu belirterek önlenebilir `/norestart` bayrağı.

Bir etki alanı denetleyicisinde yüklü ve yeniden sonra Azure AD parola koruması aracı DC yazılım yüklemesi tamamlanır. Başka bir yapılandırma gerekli veya olanaklı.

## <a name="multiple-forest-deployments"></a>Birden çok ormanlı dağıtımlarda

Azure AD parola koruması birden çok orman içinde dağıtmak için ek gereksinim yoktur. Her bir orman, Çoklu orman dağıtımı bölümünde açıklandığı gibi bağımsız olarak yapılandırılır. Her Azure AD parola koruması Proxy yalnızca katılmış olduğu ormandan etki alanı denetleyicilerini destekler. Azure AD parola koruması yazılımı belirli bir ormanda, başka bir ormandaki Active Directory güven yapılandırmalar ne olursa olsun dağıtılan Azure AD parola koruması yazılım farkında değil.

## <a name="read-only-domain-controllers"></a>Salt okunur etki alanı denetleyicileri

Parola changes\sets hiçbir zaman işlenir ve salt okunur etki alanı denetleyicileri (RODC);'üzerinde kalıcı Bunun yerine, bu yazılabilir etki alanı denetleyicilerine iletilir. Bu nedenle RODC üzerinde DC Aracısı yazılımı yüklemek için gerek yoktur.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Bir ormandaki etki alanı denetleyicilerinin yeni ilkeler veya diğer verileri Azure'dan yüklemeye çalışırken ile Azure AD parola koruması yüksek kullanılabilirliğini sağlama kaygısı proxy sunucuların bir kullanılabilirlik kümesidir. Genel Önizleme iki proxy sunucu orman başına en fazla destekler. Her bir DC aracı proxy sunucusunu çağrısı ve atlar, yanıt vermeyen proxy sunucuları verirken basit hepsini bir kez deneme stil algoritması kullanır.
Yüksek kullanılabilirlik ile ilişkili her zamanki sorunlar DC Aracısı yazılımının tasarım gereği azalır. DC aracının en son indirilen parola ilkesini yerel önbelleğini korur. Proxy sunucuları için herhangi bir nedenle kullanılamaz duruma tüm kayıtlı olsa bile, DC aracılar önbelleğe alınmış parola ilkelerini zorlamak devam edin. Bir parola ilkelerini büyük bir dağıtımın makul güncelleştirme sıklığını gün, saat değil ya da daha az sırasını genellikle açıktır.   Bu nedenle kısa kesintiler proxy sunucuları, Azure AD parola koruması özelliği ya da güvenlik avantajlarından işlemi için önemli bir etkisi neden olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi sunucularınızı Azure AD parola koruması için gerekli hizmetleri yüklediğinize tamamlamak [sonrası yapılandırma ve raporlama bilgileri toplama yükleyin](howto-password-ban-bad-on-premises-operations.md) dağıtımınızı tamamlamak için.

[Azure AD parola koruması kavramsal genel bakış](concept-password-ban-bad-on-premises.md)
