---
title: Azure AD parola koruması Önizleme dağıtma
description: Yanlış parolalar şirket içi bYönlendirme için Azure AD parola koruması Önizleme dağıtma
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 6fda373f832d6e24d1252587a19c88b0f464dda6
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232537"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Önizleme: Azure AD parola koruması dağıtma

|     |
| --- |
| Azure AD parola koruması ve özel Engellenenler parola listesini Azure Active Directory genel Önizleme özellikleri verilmiştir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları'nı Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Biz bir anlayış sahip olduğunuza [Windows Server Active Directory için Azure AD parola koruması zorlama yapma](concept-password-ban-bad-on-premises.md), planlama ve dağıtım yürütmek için sonraki adımdır.

## <a name="deployment-strategy"></a>Dağıtım stratejisi

Microsoft, tüm dağıtım denetim modunda başlatmak önerir. Denetleme modunda, burada parolaları ayarlanacak devam edebilirsiniz ve engelleneceğini herhangi olay günlüğü'nde girişleri oluşturmak varsayılan ilk ayardır. Proxy sunucusu ve DC aracıları tam denetim modunda dağıtılan sonra normal izleme yapılmalıdır ilke zorlanmış hangi etkisi parola ilkesi belirlemek için zorlama kullanıcıları ve ortam gerekir.

Denetim aşamasında, birçok kuruluş bulun:

* Daha güvenli parolalar kullanmak için mevcut işlem süreçlerini geliştirmek ihtiyaç duyar.
* Güvenli olmayan parolaları düzenli olarak seçmek için kullanıcıların alışmanızı
* Kullanıcılar güvenlik zorlama, bunlar üzerinde sahip ve daha iyi yardımcı etkisi yaklaşan değişikliği hakkında nasıl daha güvenli parolalar seçebilirsiniz anlamak bildirmeniz gerekir.

Gelen özellik makul bir süre için Denetim modunda çalıştığı sonra zorlama yapılandırması çevrilebilir **denetim** için **zorla** böylece daha güvenli parola gerektirme. Bu süre boyunca odaklanmış izleme iyi bir fikirdir.

## <a name="known-limitation"></a>Bilinen sınırlama

Azure AD parola koruması proxy preview sürümündeki bilinen bir sınırlama yoktur. MFA gerektirecek Kiracı yöneticisi hesaplarını kullanımı desteklenmiyor. Azure AD parola koruması proxy'sinin gelecekteki bir güncelleştirme proxy kaydı MFA gerektirecek yönetici hesaplarını destekler.

## <a name="single-forest-deployment"></a>Çoklu orman dağıtımı

Azure AD parola koruması önizlemesini en fazla iki sunucularda proxy hizmeti ile dağıtılır ve DC Aracısı hizmeti, Active Directory ormanındaki tüm etki alanı denetleyicilerine artımlı olarak dağıtılabilir.

![Azure AD parola koruması bileşenleri birlikte çalışmasını nasıl](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>Yazılımı indirin

Yüklenebilir Azure AD parola koruması için iki gerekli yükleyiciler vardır [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=57071)

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Azure AD parola koruması proxy hizmetini yükleme ve yapılandırma

1. Azure AD parola koruması proxy hizmeti barındırmak için bir veya daha fazla sunucu seçin.
   * Her bir hizmetin yalnızca tek bir orman için parola ilkeleri sağlayabilir ve ana bilgisayar makine etki alanına bir etki alanına katılmış olmalıdır (kök ve alt eşit desteklenir) o ormandaki. Kendi görev gerçekleştirmek Azure AD parola koruması proxy hizmeti için ormandaki her etki alanında en az bir DC ve Azure AD parola koruması Proxy ana makine arasında ağ bağlantısı olmalıdır.
   * Yüklemek ve test amacıyla bir etki alanı denetleyicisinde Azure AD parola koruması proxy hizmeti çalıştırmak için desteklenir, ancak Internet bağlantısı olmasını gerektirir.

   > [!NOTE]
   > Genel Önizleme en fazla iki (2) proxy sunucuları orman başına destekler.

2. Parola İlkesi Proxy Hizmeti yazılımı AzureADPasswordProtectionProxy.msi MSI paketini kullanarak yükleyin.
   * Yazılım yükleme, yeniden başlatma gerektirmez. Yazılım yükleme, standart MSI yordamları, örneğin kullanarak otomatikleştirilmiş: `msiexec.exe /i AzureADPasswordProtectionProxy.msi /quiet /qn`

3. Bir yönetici olarak bir PowerShell penceresi açın.
   * Azure AD parola koruması ara yazılım AzureADPasswordProtection adlı yeni bir PowerShell modülü içerir. Aşağıdaki adımları çeşitli cmdlet'leri bu PowerShell modülünden üzerinde çalışan temel alır ve yeni bir PowerShell penceresi açar ve yeni modül aşağıdaki gibi içeri aktardığınız varsayın:
      * `Import-Module AzureADPasswordProtection`

      > [!NOTE]
      > Yükleme yazılımını ana makinenin PSModulePath ortam değişkenini değiştirir. Böylece AzureADPasswordProtection powershell modülü alınan ve kullanılan etkili olması bu değişikliğin sırayla yepyeni bir PowerShell konsol penceresi açın gerekebilir.

   * Aşağıdaki PowerShell komutunu kullanarak hizmetin çalıştığından emin olun: `Get-Service AzureADPasswordProtectionProxy | fl`.
      * Bir sonuçla sonucu **durum** "Çalışır" sonucu döndürerek.

4. Proxy kaydedin.
   * 3. adım tamamlandıktan sonra Azure AD parola koruması proxy hizmeti makinede çalışıyor, ancak henüz Azure AD ile iletişim kurmak için gerekli kimlik bilgilerine sahip değil. Azure AD ile kayıt kullanarak bu özelliği etkinleştirmek için gereken `Register-AzureADPasswordProtectionProxy` PowerShell cmdlet'i. Cmdlet, genel yönetici Azure kiracınız için kimlik bilgileri yanı sıra şirket içi orman kök etki alanındaki Active Directory etki alanı yöneticisi ayrıcalıkları gerektirir. Belirtilen proxy hizmet için ek çağırmaları başarılı olduktan sonra `Register-AzureADPasswordProtectionProxy` başarılı olmaya devam ancak gereksizdir.
      * Cmdlet aşağıdaki gibi çalıştırabilirsiniz:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionProxy -AzureCredential $tenantAdminCreds
         ```

         Örneğin, yalnızca şu anda oturum açmış olan kullanıcının aynı zamanda kök etki alanı için Active Directory etki alanı yönetici ise çalışır. Aracılığıyla gerekli etki alanı kimlik bilgilerini sağlamanız alternatiftir `-ForestCredential` parametresi.

   > [!TIP]
   > Cmdlet yürütme tamamlanmadan önce belirli bir Azure kiracısı için bu cmdlet'i çalıştırmak ilk kez (birçok saniye) önemli bir gecikme olabilir. Bir hata bildirilir sürece bu gecikme ürkütücü olan ise düşünülmemelidir.

   > [!NOTE]
   > Azure AD parola koruması proxy hizmet kaydının tek seferlik bir adım hizmetin yaşam süresi olması beklenir. Proxy hizmeti, bu noktadan başlayarak diğer gerekli maintainenance otomatik olarak gerçekleştirir. Belirli bir orman için başarılı olduktan sonra ek çağrılarını 'Register-AzureADPasswordProtectionProxy' ın başarılı olmaya devam ancak gereksizdir.

5. Orman kaydedin.
   * Şirket içi Active Directory ormanı Azure kullanarak iletişim kurmak için gerekli kimlik bilgileriyle başlatılmalıdır `Register-AzureADPasswordProtectionForest` Powershell cmdlet'i. Cmdlet, genel yönetici Azure kiracınız için kimlik bilgileri yanı sıra şirket içi orman kök etki alanındaki Active Directory etki alanı yöneticisi ayrıcalıkları gerektirir. Bu adım, orman bir kez çalıştırılır.
      * Cmdlet aşağıdaki gibi çalıştırabilirsiniz:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $tenantAdminCreds
         ```

         Örneğin, yalnızca şu anda oturum açmış olan kullanıcının aynı zamanda kök etki alanı için Active Directory etki alanı yönetici ise çalışır. -ForestCredential parametresi aracılığıyla gerekli etki alanı kimlik bilgilerini sağlamak için kullanılan bir alternatiftir.

         > [!NOTE]
         > Birden çok proxy sunucusu ortamınızda yüklü değilse, yukarıdaki yukarıdaki yordamı sırasında yürütülür belirli proxy sunucusunu bir önemi yoktur.

         > [!TIP]
         > Cmdlet yürütme tamamlanmadan önce belirli bir Azure kiracısı için bu cmdlet'i çalıştırmak ilk kez (birçok saniye) önemli bir gecikme olabilir. Bir hata bildirilir sürece bu gecikme ürkütücü olan ise düşünülmemelidir.

   > [!NOTE]
   > Active Directory ormanı kayıt tek seferlik bir adım ormanın yaşam süresi olması beklenir. Ormandaki çalıştıran etki alanı denetleyicisi aracı, bu noktadan başlayarak diğer gerekli maintainenance otomatik olarak gerçekleştirir. Belirtilen orman için ek çağırmaları başarılı olduktan sonra `Register-AzureADPasswordProtectionForest` başarılı olmaya devam ancak gereksizdir.

6. İsteğe bağlı: belirli bir bağlantı noktasında dinlemek için Azure AD parola koruması proxy hizmetini yapılandırın.
   * TCP üzerinden RPC, Azure AD parola koruması proxy hizmeti ile iletişim kurmak için Azure AD parola koruması DC aracı yazılımı etki alanı denetleyicileri tarafından kullanılır. Varsayılan olarak, tüm kullanılabilir dinamik RPC uç nokta üzerinde Azure AD parola koruması parola ilkesi Proxy Hizmeti dinler. Gerekirse ağ topolojisi veya güvenlik duvarı gereksinimleri nedeniyle, hizmet bunun yerine belirli bir TCP bağlantı noktasında dinleyecek şekilde yapılandırılmış olabilir.
      * Hizmetini statik bağlantı altında çalışacak şekilde yapılandırmak için kullanın `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet'i.
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Durdurun ve bu değişikliklerin etkili olması hizmeti yeniden başlatın.

      * Hizmetini, dinamik bir bağlantı noktası altında çalışacak şekilde yapılandırmak için aynı yordamı ancak StaticPort geri sıfır olarak ayarlanmış sözlüğüdür:
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Durdurun ve bu değişikliklerin etkili olması hizmeti yeniden başlatın.

   > [!NOTE]
   > Herhangi bir bağlantı noktası yapılandırmasını değiştirdikten sonra Azure AD parola koruması proxy hizmeti el ile yeniden başlatma gerektirir. Bu yapı yapılandırma değişiklikleri yaptıktan sonra etki alanı denetleyicilerinde çalışan DC aracısı hizmet yazılımını yeniden başlatmak gerekli değildir.

   * Hizmetin geçerli yapılandırmasını kullanarak sorgulanan `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet'i aşağıdaki örnekte gösterildiği gibi:

      ```
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0 
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Azure AD parola koruması DC Aracısı Hizmeti'ni yükleme

* Yükleme Azure AD parola koruması DC Aracısı hizmeti yazılım kullanarak `AzureADPasswordProtectionDCAgent.msi` MSI paketi:
   * Yazılım yüklemesi yeniden başlatılması yükleyin ve parola filtresi DLL'leri yalnızca yüklenen veya bir yeniden başlatmanın ardından kaldırıldığında, işletim sistemi gereksinimi nedeniyle kaldırın.
   * DC Aracısı hizmeti henüz bir etki alanı denetleyicisi olmayan bir makineye yüklemek için desteklenir. Bu durumda, hizmet başlar ve Çalıştır ancak Aksi durumda olacak makine bir etki alanı denetleyicisi olarak yükseltildikten sonra kadar etkin değil.

   Yazılım yükleme, standart MSI yordamları, örneğin kullanarak otomatikleştirilmiş:  `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > Örnek msiexec komut içinde hemen yeniden neden olur; Bu belirterek önlenebilir `/norestart` bayrağı.

Bir etki alanı denetleyicisinde yüklü ve yeniden sonra Azure AD parola koruması DC Aracısı yazılım yükleme işlemi tamamlanmış olur. Başka bir yapılandırma gerekli veya olası değil.

## <a name="multiple-forest-deployments"></a>Birden çok orman dağıtımı

Azure AD parola koruması birden çok ormanlar arasında dağıtmak için ek gereksinimler yoktur. Her bir orman bağımsız olarak Çoklu orman dağıtımı bölümünde açıklandığı şekilde yapılandırılır. Her Azure AD parola koruması Proxy yalnızca katılmış olduğu ormandan etki alanı denetleyicilerini destekler. Azure AD parola koruma yazılımı belirli bir ormandaki Active Directory güven yapılandırmalar ne olursa olsun başka bir ormanda dağıtılan Azure AD parola koruması yazılım farkında değildir.

## <a name="read-only-domain-controllers"></a>Salt okunur etki alanı denetleyicileri

Parola changes\sets hiçbir zaman işlenir ve salt okunur etki alanı denetleyicileri (RODC); kalıcı Bunun yerine, bu yazılabilir etki alanı denetleyicilerine iletilir. Bu nedenle RODC üzerinde DC aracı yazılımı yüklemek için gerek yoktur.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Yeni ilkeler veya diğer veri Azure'dan indirmek bir ormandaki etki alanı denetleyicileri çalışırken Azure AD parola koruması yüksek kullanılabilirliğini sağlama ile ana proxy sunucuları kullanılabilirliğini konusudur. Genel Önizleme en fazla orman başına iki proxy sunucuları destekler. Her bir DC aracı yanıt vermediği proxy sunucular hangi proxy sunucusuna çağrısı ve atlar karar verirken bir basit hepsini stili algoritması kullanır.
Yüksek kullanılabilirlik ile ilişkili her zamanki sorunlar DC aracı yazılımının tasarım gereği azalır. DC Aracısı en son indirilen parola ilkesi, yerel bir önbelleğini korur. Tüm kayıtlı herhangi bir nedenle kullanılamaz duruma proxy sunucuları olsa bile, DC aracıların önbelleğe alınmış parola ilkelerini zorlamak devam edin. Büyük bir dağıtımda parola ilkeleri için makul güncelleştirme sıklığı gün, saat değil veya daha az sipariş genellikle açıktır.   Bu nedenle proxy sunucularının kısa kesintileri Azure AD parola koruması özelliği ya da güvenlik faydalarını işlemi için önemli etkiye neden olmaz.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi sunucularınızda Azure AD parola koruması için gerekli hizmetleri yüklediğinize tamamlamak [sonrası yükleme yapılandırma ve raporlama bilgileri toplama](howto-password-ban-bad-on-premises-operations.md) dağıtımınızı tamamlamak için.

[Azure AD parola koruması kavramsal genel bakış](concept-password-ban-bad-on-premises.md)
