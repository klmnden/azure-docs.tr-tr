---
title: Azure AD parola koruması - Azure Active Directory dağıtma
description: Dağıtın yasaklamak yanlış parolalar şirket içi Azure AD parola koruması
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1c24ec49652cfe9105aa66fd1d5e26c81afcd14
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904636"
---
# <a name="deploy-azure-ad-password-protection"></a>Azure AD parola korumasını dağıtma

Artık anladığınıza göre [Windows Server Active Directory için Azure AD parola koruması zorlama yapma](concept-password-ban-bad-on-premises.md), planlama ve dağıtım yürütmek için sonraki adımdır.

## <a name="deployment-strategy"></a>Dağıtım stratejisi

Denetleme modunda, dağıtımları başlamanızı öneririz. Denetim modu varsayılan ilk ayarlamak burada parolaları devam ayarıdır. Engelleneceğini parolalar, olay günlüğüne kaydedilir. Denetleme modunda proxy sunucuları ve aracıların DC dağıttıktan sonra İlkesi zorunlu tutulduysa kullanıcılar ve ortam üzerinde parola ilkesine sahip olacak etki izlemeniz gerekir.

Denetim aşamasında pek çok kuruluş bulmak:

* Daha güvenli parolalar kullanmak için mevcut işlem süreçlerini iyileştirmek gerekir.
* Kullanıcılar, genellikle güvenli parolalar kullanın.
* Bunlar, yaklaşan değişikliği güvenlik zorlama, bunları etkileyebilir ve daha güvenli parolalar seçme hakkında kullanıcılara bildirmeniz gerekir.

Özellik makul bir süre için Denetim modunda çalıştırıldıktan sonra yapılandırmayı üst verilerden geçebilirsiniz *denetim* için *zorla* daha güvenli bir parola gerektirme. Odaklanmış bu süre boyunca izleme iyi bir fikirdir.

## <a name="deployment-requirements"></a>Dağıtım gereksinimleri

* DC Aracı hizmeti Azure AD parola koruması yüklü Windows Server 2012 çalıştırmanız gerekir veya sonraki bir sürümü Al tüm etki alanı denetleyicileri. Bu gereksinim Active Directory etki alanı veya orman Windows Server 2012 etki alanı veya orman işlev düzeyinde de olmalıdır göstermez. Belirtildiği gibi [tasarım ilkeleri](concept-password-ban-bad-on-premises.md#design-principles), en düşük DFL veya çalıştırmak her iki DC aracı veya ara yazılım için gerekli FFL yoktur.
* Alma DC aracısı hizmetinin yüklü olduğu tüm makineler, .NET 4. 5'in yüklü olması gerekir.
* Proxy hizmet için Azure AD parola koruması yüklü Windows Server 2012 R2 çalıştırmalıdır veya sonraki bir sürümü alın, tüm makineler.
* Azure AD parola koruması Proxy hizmeti yüklü olduğu tüm makineler .NET 4.7 yüklü olması gerekir.
  .NET 4.7 zaten tamamen güncelleştirilmiş bir Windows sunucusuna yüklenmesi gerekir. Durum bu değilse, indirmek ve bulunan yükleyiciyi çalıştırın [Windows için .NET Framework 4.7 çevrimdışı yükleyici](https://support.microsoft.com/en-us/help/3186497/the-net-framework-4-7-offline-installer-for-windows).
* Tüm makineler, Azure AD parola koruması bileşenleri yüklü etki alanı denetleyicileri, dahil olmak üzere, Evrensel C çalışma zamanı yüklü olması gerekir. Tüm güncelleştirmeleri Windows Update'ten olmasını sağlayarak çalışma zamanı elde edebilirsiniz. Veya bir işletim sistemine özgü güncelleştirme paketinde alabilirsiniz. Daha fazla bilgi için [Windows Evrensel C çalışma zamanı güncelleştirmesi](https://support.microsoft.com/help/2999226/update-for-uniersal-c-runtime-in-windows).
* Ağ bağlantısı her etki alanında en az bir etki alanı denetleyicisi ile en az bir sunucu arasında parola koruması için proxy hizmeti barındıran mevcut olması gerekir. Bu RPC uç nokta Eşleyici bağlantı noktası 135 ve RPC sunucusu bağlantı noktası proxy hizmetine erişmek etki alanı denetleyicisi bağlanmaya izin vermelidir. Varsayılan olarak, dinamik bir RPC bağlantı noktası RPC sunucusu bağlantı noktası olduğu halde şekilde yapılandırılabilir [statik bağlantı noktasını](#static).
* Proxy hizmeti barındıran tüm makinelerin aşağıdaki uç noktalarına ağ erişimi olması gerekir:

    |**Uç Nokta**|**Amaç**|
    | --- | --- |
    |`https://login.microsoftonline.com`|Kimlik doğrulama istekleri|
    |`https://enterpriseregistration.windows.net`|Azure AD parola koruması işlevi|

* Parola koruması için proxy hizmeti barındıran tüm makinelerin giden TLS 1.2 HTTP trafiğine izin verecek şekilde yapılandırılması gerekir.
* Azure AD ile parola koruması ve orman için proxy hizmeti kaydetmek için bir genel yönetici hesabı.
* Azure AD ile Windows Server Active Directory orman kaydetmek için orman kök etki alanındaki Active Directory etki alanı yönetici ayrıcalıklarına sahip bir hesap.
* DC Aracı hizmeti yazılımı çalıştıran herhangi bir Active Directory etki alanı için sysvol çoğaltma dağıtılmış dosya sistemi çoğaltma (DFSR) kullanmanız gerekir.
* Anahtar Dağıtım Hizmeti, Windows Server 2012 çalıştıran tüm etki alanı denetleyicilerinde etki alanındaki etkinleştirilmesi gerekir. Varsayılan olarak, bu hizmeti el ile tetikleyici başlangıç etkinleştirilir.

## <a name="single-forest-deployment"></a>Tek ormanlı dağıtım

Aşağıdaki diyagramda, Azure AD parola koruması temel bileşenleri şirket içi Active Directory ortamında birlikte nasıl çalıştığı gösterilmektedir.

![Azure AD parola koruması bileşenleri birlikte nasıl çalıştığını](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Dağıtmadan önce yazılımın nasıl çalıştığını gözden geçirmek iyi bir fikirdir. Bkz: [Azure AD parola koruması kavramsal genel bakış](concept-password-ban-bad-on-premises.md).

### <a name="download-the-software"></a>Yazılımı indirin

Azure AD parola koruması için gerekli iki yükleyiciler vardır. Kullanılabilir oldukları [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071).

### <a name="install-and-configure-the-proxy-service-for-password-protection"></a>Parola koruması için proxy hizmetini yükleme ve yapılandırma

1. Parola koruması için proxy hizmeti barındırmak için bir veya daha fazla sunucu seçin.
   * Her bir hizmet, tek bir orman için yalnızca parola ilkeleri sağlayabilir. Konak makine o ormandaki etki alanına katılması gerekir. Kök ve alt etki alanları, her ikisi de desteklenir. Ormandaki her etki alanında en az bir DC ve parola koruması makine arasında ağ bağlantısı gerekir.
   * Proxy hizmeti test etmek için bir etki alanı denetleyicisi üzerinde çalıştırabilirsiniz. Ancak, bu etki alanı denetleyicisi daha sonra bir güvenlik sorunu olabilecek internet bağlantısı gerektirir. Yalnızca test için bu yapılandırmayı öneririz.
   * Artıklık için en az iki proxy sunucu öneririz. Bkz: [yüksek kullanılabilirlik](howto-password-ban-bad-on-premises-deploy.md#high-availability).

1. Azure AD parola koruması Proxy Hizmeti kullanılarak `AzureADPasswordProtectionProxySetup.exe` yazılım yükleyicisi.
   * Yazılım yüklemesi yeniden başlatma gerektirmez. Yazılım yükleme, örneğin standart MSI yordamları kullanarak otomatik:

      `AzureADPasswordProtectionProxySetup.exe /quiet`

      > [!NOTE]
      > Bir yükleme hatasını önlemek için AzureADPasswordProtectionProxySetup.msi paketini yüklemeden önce Windows Güvenlik Duvarı hizmetini çalıştırması gerekir. Çalıştırma için Windows Güvenlik Duvarı yapılandırılmışsa, yükleme sırasında Güvenlik Duvarı hizmeti çalıştırma ve geçici olarak etkinleştirmek için çözüm olabilir. Ara yazılım, Windows Güvenlik Duvarı'nı yükleme sonrasında belirli bir bağımlılığı yoktur. Bir üçüncü taraf güvenlik duvarı kullanıyorsanız, dağıtım gereksinimlerini karşılamak için yine de yapılandırılması gerekir. Bunlar, 135 ve RPC sunucusu bağlantı noktası proxy bağlantı noktasına gelen erişime izin vermeyi içerir. Bkz: [dağıtım gereksinimleri](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements).

1. Yönetici olarak bir PowerShell penceresi açın.
   * Parola koruması ara yazılım içeren yeni bir PowerShell Modülü *AzureADPasswordProtection*. Aşağıdaki adımlar, bu PowerShell modülünden çeşitli cmdlet'lerini çalıştırın. Yeni modül gibi içeri aktarın:

      ```powershell
      Import-Module AzureADPasswordProtection
      ```

   * Hizmetinin çalışıp çalışmadığını denetlemek için aşağıdaki PowerShell komutunu kullanın:

      `Get-Service AzureADPasswordProtectionProxy | fl`.

     Sonuç göstermelidir bir **durumu** "Çalışır."

1. Proxy kaydedin.
   * Adım 3 tamamlandıktan sonra proxy hizmeti makinede çalışıyor. Ancak, hizmeti henüz Azure AD ile iletişim kurmak için gerekli kimlik bilgilerine sahip değil. Azure AD ile kayıt gereklidir:

     `Register-AzureADPasswordProtectionProxy`

     Bu cmdlet, Azure kiracınızın genel yönetici kimlik bilgileri gerektirir. Orman kök etki alanında şirket içi Active Directory etki alanı yöneticisi ayrıcalıkları da gerekir. Bu komut, bir proxy hizmeti için bir kez başarılı olduktan sonra ek çağrıları başarılı olur ancak gereksizdir.

      `Register-AzureADPasswordProtectionProxy` Cmdlet aşağıdaki üç kimlik doğrulama modlarını destekler.

     * Etkileşimli kimlik doğrulaması modu:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Bu mod, Sunucu Çekirdeği işletim sistemlerinde çalışmaz. Bunun yerine, aşağıdaki kimlik doğrulama modlarından birini kullanın. Internet Explorer Artırılmış Güvenlik Yapılandırması etkinse, bu mod ayrıca başarısız olabilir. Bu yapılandırmayı devre dışı bırakmak, proxy kaydedin ve yeniden etkinleştirmek için çözüm olabilir.

     * Cihaz kodu kimlik doğrulama modu:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        Ardından farklı bir cihazda görüntülenen yönergeleri izleyerek kimlik doğrulaması tamamlayın.

     * Sessiz (parola tabanlı) kimlik doğrulama modu:

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionProxy -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Azure çok faktörlü kimlik doğrulaması gerekiyorsa, bu mod başarısız olur. Bu durumda, önceki iki kimlik doğrulama modlarından birini kullanın.

       Şu anda belirtmeniz gerekmez *- ForestCredential* gelecekteki işlevselliği için ayrılmış olan parametre.

   Parola koruması için proxy hizmet kaydının hizmeti kullanım ömrü içinde yalnızca bir kere gereklidir. Bundan sonra proxy hizmeti otomatik olarak gerekli herhangi bir bakım gerçekleştirir.

   > [!TIP]
   > Bu cmdlet, belirli bir Azure kiracısı için çalıştırılan ilk kez tamamlanmadan önce fark edilebilir bir gecikme olabilir. Bir hata bildirdi sürece bu gecikmenin endişelenmeyin.

1. Orman kaydedin.
   * Şirket içi Active Directory ormanını kullanarak Azure ile iletişim kurmak için gerekli kimlik bilgileriyle başlatmalıdır `Register-AzureADPasswordProtectionForest` PowerShell cmdlet'i. Cmdlet, Azure kiracınızın genel yönetici kimlik bilgileri gerektirir. Ayrıca, orman kök etki alanında şirket içi Active Directory etki alanı yöneticisi ayrıcalıkları gerektirir. Bu adım, orman bir kez çalıştırın.

      `Register-AzureADPasswordProtectionForest` Cmdlet aşağıdaki üç kimlik doğrulama modlarını destekler.

     * Etkileşimli kimlik doğrulaması modu:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Bu mod, Sunucu Çekirdeği işletim sistemlerinde çalışmaz. Bunun yerine aşağıdaki iki kimlik doğrulama modlarından birini kullanın. Internet Explorer Artırılmış Güvenlik Yapılandırması etkinse, bu mod ayrıca başarısız olabilir. Bu yapılandırmayı devre dışı bırakmak, proxy kaydedin ve yeniden etkinleştirmek için çözüm olabilir.  

     * Cihaz kodu kimlik doğrulama modu:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        Ardından farklı bir cihazda görüntülenen yönergeleri izleyerek kimlik doğrulaması tamamlayın.

     * Sessiz (parola tabanlı) kimlik doğrulama modu:

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Azure çok faktörlü kimlik doğrulaması gerekiyorsa, bu mod başarısız olur. Bu durumda, önceki iki kimlik doğrulama modlarından birini kullanın.

       Bu örneklerde, şu anda oturum açmış kullanıcı aynı zamanda kök etki alanı için Active Directory etki alanı yönetici ise yalnızca başarılı. Bu durumda değilse, diğer etki alanı kimlik bilgileri aracılığıyla sağlayabilirsiniz *- ForestCredential* parametresi.

   > [!NOTE]
   > Birden fazla ara sunucuyu ortamınızda yüklü değilse, ormanı kaydetmek için kullandığınız proxy sunucusunu önemi yoktur.
   >
   > [!TIP]
   > Bu cmdlet, belirli bir Azure kiracısı için çalıştırılan ilk kez tamamlanmadan önce fark edilebilir bir gecikme olabilir. Bir hata bildirdi sürece bu gecikmenin endişelenmeyin.

   Active Directory orman kaydı, yalnızca bir kez ormanın yaşam süresi gereklidir. Bundan sonra ormandaki etki alanı denetleyicisi aracıları otomatik olarak gerekli herhangi bir bakım gerçekleştirir. Sonra `Register-AzureADPasswordProtectionForest` başarıyla bir orman için çalışır, ek çağrıları cmdlet'inin başarılı ancak gereksizdir.

   İçin `Register-AzureADPasswordProtectionForest` başarılı olmak için Windows Server 2012 veya sonraki sürümünü çalıştıran en az bir etki alanı denetleyicisi proxy sunucunun etki alanında bulunmalıdır. Ancak, DC Aracısı yazılımını bu adımdan önce herhangi bir etki alanı denetleyicilerinde yüklü gerekmez.

1. Bir HTTP proxy üzerinden iletişim kurmak parola koruması için proxy hizmeti yapılandırın.

   Ortamınızı Azure ile iletişim kurmak için belirli bir HTTP proxy'sinin kullanılmasını gerektiriyorsa, bu yöntemi kullanın: Oluşturma bir *AzureADPasswordProtectionProxy.exe.config* %ProgramFiles%\Azure AD parola koruması Proxy\Service klasöründeki dosya. Aşağıdaki içeriği içerir:

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

   HTTP Ara sunucunuz kimlik doğrulaması gerektiriyorsa, ekleme *useDefaultCredentials* etiketi:

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

   Her iki durumda da değiştirin `http://yourhttpproxy.com:8080` belirli HTTP proxy sunucunuzun bağlantı noktası ve adresi.

   HTTP proxy yapılandırılmışsa, bize bir yetkilendirme ilkesi parola koruması için proxy hizmeti barındıran makinenin Active Directory bilgisayar hesabına erişim sağlamanız gerekir.

   Durdur ve oluşturma veya güncelleştirme sonra proxy hizmetini yeniden başlatmanız önerilir *AzureADPasswordProtectionProxy.exe.config* dosya.

   Proxy hizmeti bir HTTP proxy sunucuya bağlanmak için belirli kimlik bilgilerinin kullanımını desteklemez.

1. İsteğe bağlı: Belirli bir bağlantı noktasında dinleyecek biçimde parola koruması için proxy hizmeti yapılandırın.
   * Etki alanı denetleyicilerinde parola koruması DC Aracısı yazılımı proxy'si hizmeti ile iletişim kurmak için TCP üzerinden RPC kullanır. Varsayılan olarak, tüm kullanılabilir dinamik RPC uç nokta üzerinde proxy hizmeti dinler. Ancak, ağ topolojisi veya ortamınızda güvenlik duvarı gereksinimleri nedeniyle gerekli olması durumunda belirli bir TCP bağlantı noktasında dinleyecek şekilde hizmeti yapılandırabilirsiniz.
      * <a id="static" /></a>Hizmetini, statik bir bağlantı noktası altında çalışacak şekilde yapılandırmak için kullanın `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet'i.

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Durdur ve bu değişikliklerin devreye girmesi hizmeti yeniden başlatmanız gerekir.

      * Hizmetini, dinamik bir bağlantı noktası altında çalışacak şekilde yapılandırmak için aynı yordamı kullanın ancak ayarlanmış *StaticPort* sıfır geri dön:

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Durdur ve bu değişikliklerin devreye girmesi hizmeti yeniden başlatmanız gerekir.

   > [!NOTE]
   > Herhangi bir bağlantı noktası yapılandırmasını değiştirdikten sonra parola koruması için proxy hizmeti el ile yeniden başlatma gerektirir. Ancak, bu yapılandırma değişikliklerini yaptıktan sonra etki alanı denetleyicilerinde DC Aracı hizmeti yazılımı yeniden başlatmanız gerekmez.

   * Hizmetin geçerli yapılandırmasını sorgulamak için aşağıdaki komutu kullanın `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet:

      ```powershell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### <a name="install-the-dc-agent-service"></a>DC Aracısı Hizmeti'ni yükleme

   Parola koruması için DC aracı hizmetini kullanarak yükleme `AzureADPasswordProtectionDCAgentSetup.msi` paket.

   Yazılım yükleme veya yüklemeyi, yeniden başlatma gerektirir. Parola filtresi DLL'leri yalnızca yüklenen veya yeniden başlatma tarafından yüklenmemiş olmasıdır.

   DC Aracı hizmeti henüz bir etki alanı denetleyicisi olmayan bir makineye yükleyebilirsiniz. Bu durumda, hizmet Başlat ve Çalıştır ancak makine bir etki alanı denetleyicisi olarak yükseltilir kadar etkin kalır.

   Standart MSI yordamları kullanarak yazılım yüklemeyi otomatik hale getirebilirsiniz. Örneğin:

   `msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn`

   > [!WARNING]
   > Örnek msiexec komut hemen yeniden neden olur. Bunu önlemek için kullanın `/norestart` bayrağı.

Bir etki alanı denetleyicisinde DC Aracısı yazılım yüklendikten sonra bilgisayar yeniden yükleme tamamlanır. Başka bir yapılandırma gerekli veya olanaklı.

## <a name="multiple-forest-deployments"></a>Birden çok ormanlı dağıtımlarda

Azure AD parola koruması birden çok orman içinde dağıtmak için ek gereksinim yoktur. Her bir orman, "tek ormanlı dağıtımı" bölümünde açıklandığı gibi bağımsız olarak yapılandırılır. Her parola koruması proxy yalnızca katılmış olduğu ormandan etki alanı denetleyicilerini destekler. Parola koruması yazılımı herhangi bir ormandaki Active Directory güven yapılandırmalar ne olursa olsun diğer ormanlardaki dağıtılan parola koruması yazılımı habersizdir.

## <a name="read-only-domain-controllers"></a>Salt okunur etki alanı denetleyicileri

Parola değişiklikleri/ayarlar değil işlenir ve salt okunur etki alanı denetleyicileri (RODC) kalıcı. Bunlar, yazılabilir etki alanı denetleyicilerine iletilir. Bu nedenle, RODC üzerinde DC Aracısı yazılımını yüklemeniz gerekmez.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Bir ormandaki etki alanı denetleyicilerinin yeni ilkeler veya diğer verileri Azure'dan indirilmeye çalışılırken ana kullanılabilirlik parola koruması için proxy sunucuları kullanılabilirliğini konusudur. Her bir DC aracı çağırmak için proxy sunucusunu verirken basit bir hepsini bir kez deneme stili algoritması kullanır. Aracı yanıt olmayan proxy sunucuları atlar. Dizin hem sysvol klasörü durumunun sağlıklı çoğaltmanın en tam olarak bağlı Active Directory dağıtımları için iki proxy sunucu olduğu yeterli kullanılabilir olmasını sağlamak için. Bu yeni ilkeleri zamanında indirilmesini ve diğer verileri sonuçlanır. Ancak, ek bir proxy sunucuları dağıtabilirsiniz.

DC Aracısı yazılım tasarımı, yüksek kullanılabilirlik ile ilişkili her zamanki sorunlar azaltır. DC aracının en son indirilen parola ilkesini yerel önbelleğini korur. Tüm kullanılabilir duruma proxy sunucuları kayıtlı olsa bile, önbelleğe alınmış parola ilkelerini zorlamak DC aracıları devam eder. Genellikle büyük bir dağıtımın parola ilkelerini makul güncelleştirme sıklığı olan *gün*, olmayan bir saat veya daha az. Bu nedenle, proxy sunucuları kısa kesintiler önemli ölçüde Azure AD parola koruması etkisini.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi sunucularınız üzerinde Azure AD parola koruması için gereksinim duyduğunuz Hizmetleri yüklediğinize göre [yükleme sonrası yapılandırma ve raporlama bilgileri toplama işlemleri](howto-password-ban-bad-on-premises-operations.md) dağıtımınızı tamamlamak için.

[Azure AD parola koruması kavramsal genel bakış](concept-password-ban-bad-on-premises.md)
