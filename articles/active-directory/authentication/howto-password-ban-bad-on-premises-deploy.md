---
title: Azure AD parola koruması Önizleme dağıtma
description: Dağıtın yasaklamak yanlış parolalar şirket içi Azure AD parola koruması önizlemesi
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 824bedf782d6d227f2fa3adcf52492bb5a3eb478
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696872"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Önizleme: Azure AD parola koruması dağıtma

|     |
| --- |
| Azure AD parola koruması, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Biz bir anlayışa sahip olduğunuza göre [Azure AD parola koruması için Windows Server Active Directory zorlama yapma](concept-password-ban-bad-on-premises.md), planlama ve dağıtım yürütmek için sonraki adımdır.

## <a name="deployment-strategy"></a>Dağıtım stratejisi

Microsoft, herhangi bir dağıtıma denetim modunda başlatmak önerir. Denetleme modunda, burada parolalar ayarlamak devam edebilirsiniz engelleneceğini tüm girişleri oluşturup olay günlüğünde varsayılan ilk ayardır. Proxy sunucusu ve DC aracıları tam denetim modunda dağıtılan sonra normal izleme yapılmalıdır ilke zorlandı hangi etkisi parola ilkesi belirlemek için zorlama kullanıcılar ve ortam üzerinde gerekir.

Denetim aşamasında pek çok kuruluş bulun:

* Daha güvenli parolalar kullanmak için mevcut işlem süreçlerini iyileştirmek gerekir.
* Kullanıcılar, güvenli parolaları düzenli olarak seçme alışmanızı sağlamak
* Kullanıcılar güvenlik zorlama, etkisi olan ve daha iyi yardımcı yaklaşan değişikliği hakkında nasıl daha güvenli parolalar seçebilirsiniz anlamak bildirmeniz gerekir.

Gelen özellik için makul bir süre denetim modunda çalıştırıldıktan sonra uygulama yapılandırma çevrilebilir **denetim** için **zorla** böylece daha güvenli bir parola gerektirme. Odaklanmış bu süre boyunca izleme iyi bir fikirdir.

## <a name="deployment-requirements"></a>Dağıtım gereksinimleri

* Azure AD parola koruması DC aracısı hizmetinin yüklü olduğu tüm etki alanı denetleyicileri Windows Server 2012 veya sonraki sürümünü çalıştırmalıdır.
* Azure AD parola koruması Proxy Hizmeti yükleneceği tüm makineler, Windows Server 2012 R2 çalıştırmalıdır veya üzeri.
* Etki alanı denetleyicileri de dahil olmak üzere Azure AD parola koruması bileşenlerinin yüklendiği tüm makinelerde yüklü olan evrensel C çalışma zamanı olması gerekir.
Bu tercihen Windows Update aracılığıyla makine tam olarak düzeltme eki uygulama tarafından gerçekleştirilir. Aksi takdirde uygun işletim sistemine özgü güncelleştirme paketi olabilir yüklü - [Windows Evrensel C çalışma zamanı güncelleştirmesi](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows)
* En az bir sunucu Azure AD parola koruması Proxy hizmetini barındıran her etki alanında en az bir etki alanı denetleyicisi arasında ağ bağlantısı olmalıdır. Bu RPC uç nokta Eşleyici bağlantı noktası (135) ve RPC sunucusu bağlantı noktası proxy hizmetine erişmek etki alanı denetleyicisi bağlanmaya izin vermelidir. RPC sunucusu bağlantı noktası varsayılan olarak dinamik bir RPC bağlantı noktasıdır, ancak yapılandırılabilir (aşağıya bir statik bağlantı noktasını kullanacak şekilde bakın).
* Azure AD parola koruması Proxy hizmetini barındıran tüm makinelerde aşağıdaki uç noktalarına ağ erişimi olması gerekir:

    |Uç Nokta |Amaç|
    | --- | --- |
    |`https://login.microsoftonline.com`|Kimlik doğrulama istekleri|
    |`https://enterpriseregistration.windows.net`|Azure AD parola koruması işlevi|

* Azure AD parola koruması Proxy Hizmeti ve orman Azure AD'ye kaydetmeniz için bir genel yönetici hesabı.
* Azure AD ile Windows Server Active Directory orman kaydetmek için orman kök etki alanındaki Active Directory etki alanı yönetici ayrıcalıklarına sahip bir hesap.
* DC çalıştıran herhangi bir Active Directory etki alanı Aracı hizmeti yazılımı DFSR sysvol çoğaltma için kullanmanız gerekir.

## <a name="single-forest-deployment"></a>Tek ormanlı dağıtımı

Aşağıdaki diyagramda, Azure AD parola koruması temel bileşenleri şirket içi Active Directory ortamında birlikte nasıl çalıştığı gösterilmektedir.  

![Azure AD parola koruması bileşenleri birlikte nasıl çalışır](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>Yazılımı indirin

Adresinden indirilip Azure AD parola koruması için gerekli iki yükleyiciler vardır [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=57071)

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Azure AD parola koruması proxy hizmetini yükleme ve yapılandırma

1. Azure AD parola koruması Proxy Hizmeti barındırmak için bir veya daha fazla sunucu seçin.
   * Her bir hizmet bir tek orman için yalnızca parola ilkeleri sağlayabilir ve ana makine etki alanına bir etki alanına katılmış olmalıdır (kök ve alt etki alanları desteklenir) o ormandaki. Kendi görevi yerine getirmek Azure AD parola koruması Proxy Hizmeti için sırayla ormanda her etki alanında en az bir DC ve Azure AD parola koruması Proxy makine arasında ağ bağlantısı bulunmalıdır.
   * Yükleme ve test amacıyla, bir etki alanı denetleyicisinde Azure AD parola koruması Proxy hizmeti çalıştırmak için desteklenir. olumsuz tarafı, etki alanı denetleyicisidir sonra bir güvenlik sorunu olabilecek Internet bağlantısı olmasını gerektirir. Microsoft, test amacıyla, bu yapılandırma yalnızca kullanılmasını önerir.
   * En az iki proxy sunucu yedeklilik amaçlar için önerilir. [Yüksek kullanılabilirlik bakın](howto-password-ban-bad-on-premises-deploy.md#high-availability)

2. AzureADPasswordProtectionProxySetup.msi MSI paketini kullanarak Azure AD parola koruması Proxy hizmetini yükleyin.
   * Yazılım yüklemesi yeniden başlatma gerektirmez. Yazılım yükleme, örneğin standart MSI yordamları kullanarak otomatik: `msiexec.exe /i AzureADPasswordProtectionProxySetup.msi /quiet /qn`

      > [!NOTE]
      > AzureADPasswordProtectionProxySetup.msi MSI paketini yüklemeden önce Windows Güvenlik Duvarı hizmeti çalışıyor olmalıdır; aksi takdirde yükleme hatası meydana gelir. Çalıştırma için Windows Güvenlik Duvarı yapılandırılmışsa, geçici olarak etkinleştirin ve Windows Güvenlik Duvarı hizmeti yükleme işlemi sırasında başlatmak için çözüm olabilir. Ara yazılım, yüklemeden sonra Windows Güvenlik duvarı yazılımı belirli hiçbir bağımlılığı vardır. Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, yine de dağıtım gereksinimlerini karşılamak için yapılandırılması gerekir (bağlantı noktası 135 gelen erişimi ve RPC proxy sunucusu bağlantı noktası dinamik veya statik izin ver). [Bkz: dağıtım gereksinimleri](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements)

3. Yönetici olarak bir PowerShell penceresi açın.
   * Azure AD parola koruması ara yazılımı AzureADPasswordProtection adlı yeni bir PowerShell modülü içerir. Aşağıdaki adımlar, bu PowerShell modülünden çeşitli cmdlet'ler çalıştıran temel alır ve yeni bir PowerShell penceresi açar ve yeni modül aşağıdaki gibi içe varsayalım:

      ```PowerShell
      Import-Module AzureADPasswordProtection
      ```

   * Aşağıdaki PowerShell komutunu kullanarak hizmetinin çalıştığını kontrol edin: `Get-Service AzureADPasswordProtectionProxy | fl`.
     Sonucu bir sonuç ile üretmelidir **durumu** "Çalışıyor" sonucu döndürerek.

4. Proxy kaydedin.
   * 3. adım tamamlandıktan sonra Azure AD parola koruması Proxy Hizmeti makinede çalışıyor, ancak henüz Azure AD ile iletişim kurmak için gerekli kimlik bilgilerine sahip değil. Azure AD ile kayıt kullanarak bu özelliği etkinleştirmek için gerekli `Register-AzureADPasswordProtectionProxy` PowerShell cmdlet'i. Cmdlet, genel yönetici olmak üzere Azure kiracısı için kimlik bilgileri hem de şirket içinde orman kök etki alanındaki Active Directory etki alanı yöneticisi ayrıcalıkları gerektirir. Belirli bir proxy hizmeti için ek çağrıları başarılı oldu sonra `Register-AzureADPasswordProtectionProxy` başarılı olması devam eder ancak gereksizdir.

      Register-AzureADPasswordProtectionProxy cmdlet, üç farklı kimlik doğrulama modu şu şekilde destekler.

      * Etkileşimli kimlik doğrulaması modu:

         ```PowerShell
         Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
         ```
         > [!NOTE]
         > Bu mod, Sunucu Çekirdeği işletim sistemlerinde çalışmaz. Bunun yerine alternatif bir kimlik doğrulama modlarından birini aşağıda özetlendiği gibi kullanın.

         > [!NOTE]
         > Bu mod, Internet Explorer Artırılmış Güvenlik Yapılandırması etkinse, başarısız olabilir. Geçici çözüm olan IESC devre dışı bırakmak, kayıt proxy IESC daha sonra yeniden etkinleştirin.

      * Cihaz kodu kimlik doğrulama modu:

         ```PowerShell
         Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceMode
         To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
         ```

         Ardından, farklı bir cihazda görüntülenen yönergeleri izleyerek kimlik doğrulaması tamamlayabilirsiniz.

      * Sessiz (parola tabanlı) kimlik doğrulama modu:

         ```PowerShell
         $globalAdminCredentials = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
         ```

         > [!NOTE]
         > MFA için herhangi bir nedenle kimlik doğrulaması gerektiriyorsa, bu mod başarısız olur. Bu durumda, Lütfen önceki iki moddan birini MFA tabanlı kimlik doğrulaması gerçekleştirmek için kullanın.

      Bunu şu anda gelecekteki işlevselliği için ayrılmış olan - ForestCredential parametresini belirtmeniz gerekli değildir.

   > [!NOTE]
   > Azure AD parola koruması proxy hizmet kaydının, tek seferlik bir adım hizmetinin yaşam süresi olması beklenir. Proxy hizmeti, gerekli herhangi bir bakım ve sonraki sürümlerde bu noktadan itibaren otomatik olarak gerçekleştirir. Belirli bir ara sunucu için başarılı oldu sonra ek çağrıları 'Register-AzureADPasswordProtectionProxy' başarılı olmaya devam ancak gereksizdir.

   > [!TIP]
   > İlk kez cmdlet'ini yürütme tamamlanmadan önce bu cmdlet için belirli bir Azure kiracısı çalıştırdığınızda önemli bir gecikme (birçok saniye) olabilir. Bir hata bildirdi sürece bu gecikme açılan kutuyla düşünülmemelidir.

5. Orman kaydedin.
   * Şirket içi Active Directory ormanında, Azure'ı kullanarak iletişim kurmak için gerekli kimlik bilgileriyle başlatılmalıdır `Register-AzureADPasswordProtectionForest` PowerShell cmdlet'i. Cmdlet, genel yönetici olmak üzere Azure kiracısı için kimlik bilgileri hem de şirket içinde orman kök etki alanındaki Active Directory etki alanı yöneticisi ayrıcalıkları gerektirir. Bu adım, orman bir kez çalıştırın.

      Register-AzureADPasswordProtectionForest cmdlet, üç farklı kimlik doğrulama modu şu şekilde destekler.

      * Etkileşimli kimlik doğrulaması modu:

         ```PowerShell
         Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
         ```
         > [!NOTE]
         > Bu mod, Sunucu Çekirdeği işletim sistemlerinde çalışmaz. Bunun yerine alternatif bir kimlik doğrulama modlarından birini aşağıda özetlendiği gibi kullanın.

         > [!NOTE]
         > Bu mod, Internet Explorer Artırılmış Güvenlik Yapılandırması etkinse, başarısız olabilir. Geçici çözüm olan IESC devre dışı bırakmak, kayıt proxy IESC daha sonra yeniden etkinleştirin.  

      * Cihaz kodu kimlik doğrulama modu:

         ```PowerShell
         Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceMode
         To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
         ```

         Ardından, farklı bir cihazda görüntülenen yönergeleri izleyerek kimlik doğrulaması tamamlayabilirsiniz.

      * Sessiz (parola tabanlı) kimlik doğrulama modu:
         ```PowerShell
         $globalAdminCredentials = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
         ```

         > [!NOTE]
         > MFA kimlik doğrulaması gerektiriyorsa, bu mod başarısız olur. Bu durumda, Lütfen önceki iki moddan birini MFA tabanlı kimlik doğrulaması gerçekleştirmek için kullanın.

      Yukarıdaki örneklerde, şu anda oturum açmış kullanıcı aynı zamanda kök etki alanı için Active Directory etki alanı yönetici ise yalnızca başarılı olur. Durum bu değilse, diğer etki alanı kimlik bilgileri - ForestCredential parametresi aracılığıyla sağlayabilir.

   > [!NOTE]
   > Birden fazla ara sunucuyu ortamınızda yüklü değilse, ormanı kaydetmek için kullandığı proxy sunucusunu olduğu önemli değildir.

   > [!TIP]
   > İlk kez cmdlet'ini yürütme tamamlanmadan önce bu cmdlet için belirli bir Azure kiracısı çalıştırdığınızda önemli bir gecikme (birçok saniye) olabilir. Bir hata bildirdi sürece bu gecikme açılan kutuyla düşünülmemelidir.

   > [!NOTE]
   > Active Directory ormanı kayıt, tek seferlik bir adım ormanın yaşam süresi olması beklenir. Ormanda çalıştıran etki alanı denetleyicisi aracıları, gerekli herhangi bir maintainenance ve sonraki sürümlerde bu noktadan itibaren otomatik olarak gerçekleştirir. Bu işlem, ek çağrıları gibi belirli bir orman için başarılı `Register-AzureADPasswordProtectionForest` başarılı olması devam eder ancak gereksizdir.

   > [!NOTE]
   > Sırayla `Register-AzureADPasswordProtectionForest` en az bir Windows Server 2012 veya üzeri etki alanı başarılı olması için denetleyici proxy sunucunun etki alanında bulunmalıdır. Ancak bu adımı öncesinde herhangi bir etki alanı denetleyicilerinde DC Aracısı yazılımının yüklenmesi gereksinimi yoktur.

6. Bir HTTP proxy üzerinden iletişim kurmak için Azure AD parola koruması Proxy hizmetini yapılandırma

   Ortamınızı Azure ile iletişim kurmak için belirli bir HTTP proxy'sinin kullanılmasını gerektiriyorsa, bu şekilde gerçekleştirilebilir.

   Adlı bir dosya oluşturun `proxyservice.exe.config` dosyası `%ProgramFiles%\Azure AD Password Protection Proxy\Service` klasöründe aşağıdaki içeriğe sahip:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   HTTP Ara sunucunuz kimlik doğrulaması gerektiriyorsa, useDefaultCredentials etiketi aşağıdaki gibi ekleyin:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   Her iki durumda da, değiştirirler `http://yourhttpproxy.com:8080` belirli HTTP proxy sunucunuzun bağlantı noktası ve adresi.

   HTTP Ara sunucunuz ile bir yetkilendirme ilkesi yapılandırdıysanız Azure AD parola koruması Proxy hizmetini barındıran bilgisayarın Active Directory bilgisayar hesabına erişim verilmesi gerekir.

   Durdur ve Azure AD parola koruması Proxy hizmeti oluşturma veya güncelleştirme sonrasında yeniden `proxyservice.exe.config` dosya.

   Azure AD parola koruması Proxy Hizmeti, bir HTTP proxy sunucuya bağlanmak için belirli kimlik bilgilerinin kullanımını desteklemiyor.

7. İsteğe bağlı: Azure AD parola koruması Proxy hizmetini, belirli bir bağlantı noktasında dinleyecek şekilde yapılandırın.
   * TCP üzerinden RPC, Azure AD parola koruması proxy'si hizmeti ile iletişim kurmak için Azure AD parola koruması DC Aracısı yazılımı etki alanı denetleyicileri tarafından kullanılır. Varsayılan olarak, Azure AD parola koruması Proxy Hizmeti tüm kullanılabilir dinamik RPC uç nokta üzerinde dinler. Gerekirse ağ topolojisi veya güvenlik duvarı gereksinimleri nedeniyle, hizmet yerine belirli bir TCP bağlantı noktasında dinleyecek şekilde yapılandırılabilir.
      * Hizmetini, statik bir bağlantı noktası altında çalışacak şekilde yapılandırmak için kullanın `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet'i.
         ```PowerShell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Durdur ve bu değişikliklerin devreye girmesi hizmeti yeniden başlatmanız gerekir.

      * Hizmetini, dinamik bir bağlantı noktası altında çalışacak şekilde yapılandırmak için aynı yordamı ancak StaticPort geri sıfır olarak ayarlanmış kullanım şu şekilde:
         ```PowerShell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Durdur ve bu değişikliklerin devreye girmesi hizmeti yeniden başlatmanız gerekir.

   > [!NOTE]
   > Herhangi bir bağlantı noktası yapılandırmasını değiştirdikten sonra Azure AD parola koruması Proxy hizmetini el ile yeniden başlatma gerektirir. Bu niteliği yapılandırma değişikliklerini yaptıktan sonra etki alanı denetleyicilerinde çalışan DC Aracı hizmeti yazılımı yeniden başlatmak gerekli değildir.

   * Hizmet geçerli yapılandırmasını kullanarak sorgulanan `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet'i aşağıdaki örnekte gösterildiği gibi:

      ```PowerShell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Azure AD parola koruması DC Aracısı Hizmeti'ni yükleme

   Azure AD parola koruması DC Yükleme Aracısı yazılımını hizmet kullanarak `AzureADPasswordProtectionDCAgent.msi` MSI paketi

   Yazılım yüklemesi üzerinde yükleme yeniden başlatmayı gerektirdiği ve parola filtresi DLL'leri yalnızca yüklenen veya bir yeniden başlatmanın kaldırıldığında, işletim sistemi gereksinimi nedeniyle kaldırın.

   DC Aracısı henüz bir etki alanı denetleyicisi olmayan bir makineye yüklemek için desteklenir. Bu durumda, hizmet başlar ve çalışma ancak aksi makine bir etki alanı denetleyicisi olarak yükseltildikten sonra kadar etkin değil.

   Yazılım yükleme, örneğin standart MSI yordamları kullanarak otomatik:

   `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > Yukarıdaki örnek MSIEXEC komutu bir hemen yeniden başlatılmasına neden olur; Bu belirterek önlenebilir `/norestart` bayrağı.

Azure AD parola koruması DC aracı yazılım yüklemesini, bir etki alanı denetleyicisinde yüklü ve yeniden sonra tamamlanmıştır. Başka bir yapılandırma gerekli veya olanaklı.

## <a name="multiple-forest-deployments"></a>Birden çok ormanlı dağıtımlarda

Azure AD parola koruması birden çok orman içinde dağıtmak için ek gereksinim yoktur. Her bir orman, Çoklu orman dağıtımı bölümünde açıklandığı gibi bağımsız olarak yapılandırılır. Her Azure AD parola koruması Proxy yalnızca katılmış olduğu ormandan etki alanı denetleyicilerini destekler. Azure AD parola koruması yazılımı belirli bir ormanda, başka bir ormandaki Active Directory güven yapılandırmalar ne olursa olsun dağıtılan Azure AD parola koruması yazılım farkında değil.

## <a name="read-only-domain-controllers"></a>Salt okunur etki alanı denetleyicileri

Parola changes\sets hiçbir zaman işlenir ve salt okunur etki alanı denetleyicileri (RODC);'üzerinde kalıcı Bunun yerine, bu yazılabilir etki alanı denetleyicilerine iletilir. Bu nedenle RODC üzerinde DC Aracısı yazılımı yüklemek için gerek yoktur.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Bir ormandaki etki alanı denetleyicilerinin yeni ilkeler veya diğer verileri Azure'dan yüklemeye çalışırken ile Azure AD parola koruması yüksek kullanılabilirliğini sağlama kaygısı proxy sunucuların bir kullanılabilirlik kümesidir. Her bir DC aracı proxy sunucusunu çağrısı ve atlar, yanıt vermeyen proxy sunucuları verirken basit hepsini bir kez deneme stil algoritması kullanır. İki (2) proxy sunucuları tam olarak bağlı Active Directory dağıtımların çoğunluğu için sağlıklı çoğaltması (durumunun hem directory ve sysvol'de), kullanılabilirlik ve bu nedenle zamanında yüklemeleri yeni ilkeleri ve diğer verileri emin olmak yeterli olur. İstenen. ek proxy sunucuları olarak dağıtılabilir.

Yüksek kullanılabilirlik ile ilişkili her zamanki sorunlar DC Aracısı yazılımının tasarım gereği azalır. DC aracının en son indirilen parola ilkesini yerel önbelleğini korur. Proxy sunucuları için herhangi bir nedenle kullanılamaz duruma tüm kayıtlı olsa bile, DC aracılar önbelleğe alınmış parola ilkelerini zorlamak devam edin. Bir parola ilkelerini büyük bir dağıtımın makul güncelleştirme sıklığını gün, saat değil ya da daha az sırasını genellikle açıktır. Bu nedenle kısa kesintiler proxy sunucuları, Azure AD parola koruması özelliği ya da güvenlik avantajlarından işlemi için önemli bir etkisi neden olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi sunucularınızı Azure AD parola koruması için gerekli hizmetleri yüklediğinize tamamlamak [sonrası yapılandırma ve raporlama bilgileri toplama yükleyin](howto-password-ban-bad-on-premises-operations.md) dağıtımınızı tamamlamak için.

[Azure AD parola koruması kavramsal genel bakış](concept-password-ban-bad-on-premises.md)
