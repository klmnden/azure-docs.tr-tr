---
title: Azure MFA yetenekleri sağlamak için mevcut NPS sunucuları kullanın | Microsoft Docs
description: Varolan kimlik doğrulama altyapınız için bulut tabanlı iki aşamalı vericiation yetenekleri ekleme
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 05/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 2b08c3adb0c638cdfa0ccd9ae4c5beacac822eb4
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37018315"
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Varolan NPS altyapınızı Azure multi-Factor Authentication ile tümleştirme

Azure MFA için ağ ilkesi sunucusu (NPS) uzantısı, var olan sunucuları kullanarak kimlik doğrulaması altyapınız için bulut tabanlı MFA özellikleri ekler. NPS uzantısıyla yüklemeniz, yapılandırmanız ve yeni sunucuların bakımını yapmak zorunda kalmadan, var olan kimlik doğrulama akışı telefon araması, SMS mesajı ya da telefon uygulama doğrulama ekleyebilirsiniz. 

Bu uzantı, Azure MFA sunucusu dağıtmadan VPN bağlantıları korumak istediğiniz kuruluşlar için oluşturuldu. NPS uzantısı arasındaki RADIUS Azure MFA bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için bir bağdaştırıcı federe veya kullanıcıları gibi davranır.

NPS uzantısı için Azure MFA kullanıldığında, kimlik doğrulaması akışı aşağıdaki bileşenleri içerir: 

1. **NAS/VPN sunucusu** VPN istemcilerinden gelen istekleri alır ve bunları NPS sunucuları için RADIUS isteklerini dönüştürür. 
2. **NPS sunucusu** birincil kimlik doğrulaması için RADIUS isteklerini gerçekleştirmek için Active Directory bağlanır ve başarı istek için yüklü uzantılar geçirir.  
3. **NPS uzantısı** ikincil kimlik doğrulaması için Azure MFA istek tetikler. Uzantı yanıtı alır ve MFA testini başarılı olursa, MFA talep dahil güvenlik belirteçleri ile NPS sunucusu sağlayarak kimlik doğrulama isteği tamamlandıktan sonra Azure STS tarafından verilmiş.  
4. **Azure MFA** Azure kullanıcının ayrıntılarını almak için Active Directory'ye ile iletişim kurar ve kullanıcı için yapılandırılan bir doğrulama yöntemiyle ikincil kimlik doğrulaması gerçekleştirir.

Aşağıdaki diyagram bu üst düzey kimlik doğrulaması istek akışının gösterir: 

![Kimlik doğrulama akışı diyagramı](./media/howto-mfa-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Dağıtımınızı planlama

Özel bir yapılandırma gerekmez şekilde NPS uzantısı artıklık, otomatik olarak yönetir.

Gereksinim duyduğunuz kadar Azure MFA etkin NPS sunucusu oluşturabilirsiniz. Birden çok sunucuya yüklerseniz, bunların her biri için bir fark istemci sertifikası kullanmanız gerekir. Her sunucu için bir sertifika oluşturmak, her sertifika tek tek güncelleştirin ve kapalı kalma süresi hakkında tüm sunucularınız üzerinde endişelenmeyin anlamına gelir.

Yeni Azure MFA etkin NPS sunucularını bilmeniz gereken şekilde VPN sunucularını kimlik doğrulama isteklerini yönlendirir.

## <a name="prerequisites"></a>Önkoşullar

NPS uzantısı, mevcut altyapınızı ile çalışmak için tasarlanmıştır. Başlamadan önce aşağıdaki önkoşullara sahip olduğunuzdan emin olun.

### <a name="licenses"></a>Lisanslar

Azure MFA için NPS uzantısı sahip müşteriler için kullanılabilir [Azure multi-Factor Authentication için lisans](multi-factor-authentication.md) (Azure AD Premium, EMS veya MFA tek başına lisans dahil). Tüketim tabanlı lisansları kullanıcı başına veya başına kimlik doğrulama lisansı gibi Azure MFA için NPS uzantısı ile uyumlu değildir. 

### <a name="software"></a>Yazılım

Windows Server 2008 R2 SP1 veya üstü.

### <a name="libraries"></a>Kitaplıkları

Bu kitaplıklar uzantısı ile otomatik olarak yüklenir.

-   [Visual Studio 2013 (X64) için Visual C++ yeniden dağıtılabilir paketleri](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory için Windows PowerShell modülü sürümü 1.1.166.0](https://www.powershellgallery.com/packages/MSOnline/1.1.166.0)

Microsoft Azure Active Directory için Windows PowerShell modülü, zaten var, Kurulum işleminin bir parçası olarak çalışan bir yapılandırma komut dosyası aracılığıyla değilse yüklenir. Zaten yüklü değilse bu modül önceden yüklemeye gerek yoktur.

### <a name="azure-active-directory"></a>Azure Active Directory

Herkes NPS uzantısı kullanılarak Azure Active Azure AD Connect'i kullanarak dizin eşitlenen ve MFA için kayıtlı olması gerekir.

Uzantı yüklediğinizde, Azure AD kiracınız için dizin kimliği ve yönetici kimlik bilgileri gerekir. Dizin Kimliğinizi bulabilirsiniz [Azure portal](https://portal.azure.com). Bir yönetici olarak oturum açın, select **Azure Active Directory** Simge solda, ardından **özellikleri**. GUID kopyalama **dizin kimliği** kutusunda ve kaydedin. NPS uzantısını yüklediğinizde bu GUID Kiracı kimliği olarak kullanın.

![Azure Active Directory özellikleri'nin altında dizin kimliği bulunamıyor](./media/howto-mfa-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

NPS uzantısını yüklemeden önce kimlik doğrulama trafiğini işlemek için ortam hazırlama istiyor.

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>Etki alanına katılmış bir sunucudaki NPS rolü etkinleştir

NPS sunucusu, Azure Active Directory'ye bağlanır ve MFA isteklerin kimliğini doğrular. Bu rol için bir sunucu seçin. Hataları RADIUS olmayan tüm istekler için NPS uzantısı oluşturur çünkü diğer hizmetler gelen istekleri işleyemez bir sunucu seçme öneririz. Ortamınız için birincil ve ikincil kimlik doğrulama sunucusu olarak NPS sunucusunu ayarlanmış olması gerekir; proxy RADIUS istekleri başka bir sunucuya uygulanamaz.

1. Sunucunuzda açmak **Ekle roller ve Özellikler Sihirbazı** Sunucu Yöneticisi'ni Hızlı Başlangıç menüsünde.
2. Seçin **rol tabanlı veya özellik tabanlı yükleme** için yükleme türü.
3. Seçin **Ağ İlkesi ve Erişim Hizmetleri** sunucu rolü. Bir pencere bu rolü çalıştırmak için gerekli özellikleri hakkında bilgilendirmek için açılır.
4. Sihirbaza onay sayfasında kadar devam edin. **Yükle**’yi seçin.

NPS için belirlenmiş bir sunucunuz varsa, VPN çözümden gelen RADIUS isteklerini işlemek için bu sunucuyu yapılandırmanız gerekir.

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>VPN çözümünüzün NPS sunucusu ile iletişim kurmak için yapılandırma

Kullandığınız VPN çözümüne bağlı olarak, RADIUS kimlik doğrulama İlkesi yapılandırmak için gereken adımları farklılık gösterir. Bu ilke RADIUS NPS sunucuya işaret edecek şekilde yapılandırın.

### <a name="sync-domain-users-to-the-cloud"></a>Bulut için eşitleme etki alanı kullanıcıları

Bu adım zaten kiracınız üzerinde tam olabilir, ancak Azure AD Connect veritabanlarınızı yakın zamanda eşitlendi iki kez kontrol iyidir.

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Seçin **Azure Active Directory** > **Azure AD Connect**
3. Eşitleme durumunuzu doğrulayın **etkin** ve son eşitleme değerinden bir saatten önce gerçekleştirildi.

Yeni bir eşitleme, bize yönergeleri gidiş kazandırın gerekiyorsa, [Azure AD Connect eşitleme: Zamanlayıcı](../connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Kullanıcılarınızın kullanabilirsiniz hangi kimlik doğrulama yöntemlerini belirleme

NPS uzantısı dağıtımı ile hangi kimlik doğrulama yöntemleri kullanılabilir etkileyen iki Etkenler vardır:

1. RADIUS istemcisi arasında kullanılan parola şifreleme algoritması (VPN, Netscaler sunucu veya diğer) ve NPS sunucularını.
   - **PAP** bulutta Azure mfa tüm kimlik doğrulama yöntemlerini destekler: telefon araması, tek yönlü SMS mesajı, mobil uygulama bildirimi ve mobil uygulama doğrulama kodu.
   - **CHAPV2** ve **EAP** telefon araması ve mobil uygulama bildirimi destekler.
2. Giriş yöntemleri, istemci uygulaması (VPN, Netscaler sunucu veya diğer) işleyebilir. Örneğin, VPN istemcisi, bir metin veya mobil uygulama doğrulama kodu yazın yapmalarına izin vermek için bazı araçlar var mı?

NPS uzantısı dağıttığınızda, hangi yöntemler, kullanıcılarınız için kullanılabilir değerlendirmek için bu Etkenler kullanın. RADIUS istemciniz PAP destekler, ancak istemci UX bir doğrulama kodu için girdi alanlarının yok, ardından telefon araması ve mobil uygulama bildirimi iki desteklenen seçeneklerdir.

Yapabilecekleriniz [desteklenmeyen kimlik doğrulama yöntemleri devre dışı](howto-mfa-mfasettings.md#selectable-verification-methods) azure'da.

### <a name="register-users-for-mfa"></a>MFA için kullanıcıları kaydetme

Dağıtma ve NPS uzantısı kullanmaya başlamadan önce iki aşamalı doğrulamayı gerçekleştirmek için gereken kullanıcıların MFA'ya kayıtlı olması gerekir. Daha hemen bu dağıtırken uzantısı sınamak için çok faktörlü kimlik doğrulaması için tam olarak kayıtlı en az bir sınama hesabı gerekir.

Başlatılan bir sınama hesabı almak için aşağıdaki adımları kullanın:
1. Oturum [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) bir test hesabıyla. 
2. Bir doğrulama yöntemi ayarlamak için yönergeleri izleyin.
3. Ya da bir koşullu erişim ilkesi oluşturun veya [kullanıcı durumunu değiştirme](howto-mfa-userstates.md) test hesap için iki aşamalı doğrulama gerektirecek şekilde. 

Kullanıcılarınızın, ayrıca NPS uzantısı ile kimlik doğrulama gerçekleştirmeden önce kaydetmek için aşağıdaki adımları izleyin gerekir.

## <a name="install-the-nps-extension"></a>NPS uzantısını yükleyin

> [!IMPORTANT]
> NPS uzantısı VPN erişim noktası başka bir sunucuya yükleyin.

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>Karşıdan yükleyip NPS uzantısı için Azure MFA

1.  [NPS uzantısını indirin](https://aka.ms/npsmfa) Microsoft İndirme Merkezi'nden.
2.  İkili yapılandırmak istediğiniz ağ ilkesi sunucusuna kopyalayın.
3.  Çalıştırma *setup.exe* ve yükleme yönergelerini izleyin. Hatalarla karşılaşırsanız, önkoşul bölümüne iki kitaplıklarından başarıyla yüklendiğini denetleyin.

### <a name="run-the-powershell-script"></a>PowerShell betiğini çalıştırma

Yükleyici, bu konumda bir PowerShell Betiği oluşturur: `C:\Program Files\Microsoft\AzureMfa\Config` (C:\ olduğu yükleme sürücüsü). Bu PowerShell Betiği aşağıdaki eylemleri gerçekleştirir:

-   Kendinden imzalı bir sertifika oluşturun.
-   Üzerinde Azure AD asıl hizmet sertifikasının ortak anahtarı ilişkilendirin.
-   Sertifika yerel makine sertifika deposunda saklar.
-   Sertifikanın özel anahtarı ağ kullanıcı izni verin.
-   NPS yeniden başlatın.

(Yerine PowerShell Betiği oluşturur otomatik olarak imzalanan sertifikalar) kendi sertifikaları kullanmak istemiyorsanız, yüklemeyi tamamlamak için PowerShell betiğini çalıştırın. Birden çok sunucu üzerinde uzantısı yüklerseniz, her biri kendi sertifikanın olması gerekir.

1. Windows PowerShell'i yönetici olarak çalıştırın.
2. Dizinleri değiştirin.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Yükleyici tarafından oluşturulan PowerShell komut dosyasını çalıştırın.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. Azure AD yönetici olarak oturum açın.
5. Kiracı kimliğinizi PowerShell sorar Önkoşullar bölümünde Azure portalından kopyaladığınız dizini kimliği GUID kullanın.
6. PowerShell betik tamamlandığında, bir başarı iletisi gösterilir.  

Yük Dengeleme için ayarlamak istediğiniz herhangi bir ek NPS sunucularında bu adımları yineleyin.

>[!NOTE]
>PowerShell Betiği sertifikalarla üretmek yerine kendi sertifikaları kullanıyorsanız, NPS adlandırma kuralı Hizala emin olun. Konu adı olmalıdır **CN =\<Tenantıd\>, OU = Microsoft NPS uzantı**. 

## <a name="configure-your-nps-extension"></a>NPS uzantınızı yapılandırın

Bu bölümde, tasarım konuları ve başarılı NPS uzantısı dağıtımlar için öneriler içerir.

### <a name="configuration-limitations"></a>Yapılandırma sınırlamaları

- Azure MFA için NPS uzantısı kullanıcılar ve ayarlarını MFA sunucusundan buluta geçirmek için araçları içermez. Bu nedenle, varolan dağıtım yerine yeni dağıtımlar için uzantısını kullanarak öneririz. Uzantı üzerinde var olan bir dağıtıma kullanırsanız, kullanıcılarınızın bulutta MFA ayrıntılarını yeniden doldurmak için kanıt Yukarı yapmanız gerekir.  
- NPS uzantısı şirket içi Active Directory'den ikincil kimlik doğrulaması gerçekleştirmek için Azure MFA kullanıcı tanımlamak için kullanır Uzantı alternatif oturum açma kimliği veya özel Active Directory alan UPN dışında gibi farklı bir kimlik kullanacak şekilde yapılandırılabilir. Bkz: [Gelişmiş çok faktörlü kimlik doğrulaması için NPS uzantısı için yapılandırma seçenekleri](howto-mfa-nps-extension-advanced.md) daha fazla bilgi için.
- Tüm şifreleme protokolleri tüm doğrulama yöntemlerini destekler.
   - **PAP** telefon araması, tek yönlü SMS mesajı, mobil uygulama bildirimi ve mobil uygulama doğrulama kodu destekler
   - **CHAPV2** ve **EAP** telefon araması ve mobil uygulama bildirimi desteği

### <a name="control-radius-clients-that-require-mfa"></a>MFA gerektirecek denetim RADIUS istemcileri

NPS uzantısını kullanarak bir RADIUS istemcisi için MFA etkinleştirdikten sonra MFA gerçekleştirmek için bu istemci için tüm kimlik doğrulamaları gerekir. MFA bazı RADIUS istemcileri ve diğerleri için etkinleştirmek istiyorsanız, iki NPS sunucularını yapılandırın ve yalnızca bunlardan biri üzerinde uzantısını yükleyin. Uzantısı ile yapılandırılmış NPS sunucusuna istekleri göndermek MFA gerektirecek şekilde istediğiniz RADIUS istemcileri ve diğer uzantısı ile yapılandırılmamış NPS sunucusunun RADIUS istemcileri yapılandırın.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>MFA için kayıtlı olmayan kullanıcılar için hazırlama

MFA için kayıtlı olmayan kullanıcılar varsa, kimlik doğrulaması yapmaya çalıştıklarında ne olacağını belirleyebilirsiniz. Kayıt defteri ayarını kullanın *REQUIRE_USER_MATCH* kayıt defteri yolunda *HKLM\Software\Microsoft\AzureMFA* özelliği davranışını denetlemek için. Bu ayar tek yapılandırma seçeneği vardır:

| Anahtar | Değer | Varsayılan |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | TRUE/FALSE | (TRUE eşdeğer) ayarlanmadı |

Bu ayarın amacı kullanıcı MFA'ya kayıtlı olmayan ne yapacakları belirlemektir. Ne zaman anahtarı yok, ayarlı değil veya olan TRUE olarak ayarlayın ve kullanıcı kayıtlı olmayan, ardından uzantısı MFA sınama başarısız olur. Anahtar FALSE olarak ayarlayın ve kullanıcının kayıtlı olmayan, kimlik doğrulaması MFA yapmadan devam eder. Bir kullanıcı MFA'kaydedilmişse REQUIRE_USER_MATCH FALSE olarak ayarlansa bile MFA ile kimlik doğrulaması gerekir.

Bu anahtarı oluşturun ve FALSE, kullanıcılar ekleme ve tüm henüz Azure MFA için kaydedilebilir sırada ayarlayın seçebilirsiniz. Ancak, anahtarı ayarı oturum açmak mfa kayıtlı olmayan kullanıcılar verdiğinden üretime geçmeden önce bu anahtarı kaldırmanız gerekir.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>İstemci sertifikası beklendiği gibi yüklü olduğunu nasıl doğrularım?

Sertifika deposu ve kullanıcıya verilen izinler özel anahtara sahip onay yükleyici tarafından oluşturulan otomatik olarak imzalanan sertifika arayın **ağ hizmeti**. Sertifika bir konu adına sahip **CN \<tenantıd\>, OU = Microsoft NPS uzantı**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>My istemci sertifikası Azure Active Directory'de my Kiracı için ilişkili olup olmadığını nasıl doğrulayabilirsiniz?

PowerShell komut istemi açın ve aşağıdaki komutları çalıştırın:

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Bu komutlar, Kiracı PowerShell oturumunuzda NPS uzantısı'nın örneğinizle ilişkilendirme tüm sertifikaları yazdırın. Sertifikanız için istemci sertifikası özel anahtarı olmayan bir "X.509(.cer) Base-64 Kodlamalı" dosyası olarak dışa aktararak arayın ve PowerShell listeden ile karşılaştırır.

Geçerli-başlangıç ve geçerli-kadar okunabilir formda bulunan, zaman damgaları komut birden fazla sertifika döndürürse belirgin misfits filtrelemek için kullanılabilir.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Neden isteklerim ADAL belirteci hatasıyla başarısız oluyor?

Bu hata çeşitli nedenlerden biri olabilir. Gidermenize yardımcı olması için aşağıdaki adımları kullanın:

1. NPS sunucusunu yeniden başlatın.
2. Bu istemci sertifikası beklendiği gibi yüklü doğrulayın.
3. Sertifika üzerinde Azure AD Kiracı ile ilişkili olduğunu doğrulayın.
4. Doğrulayın https://login.microsoftonline.com/ uzantısı'nı çalıştıran sunucudan erişilebilir.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Neden kimlik doğrulaması, HTTP günlükleri kullanıcı bulunamadığında bildiren bir hata ile başarısız?

AD Connect çalışır durumda olduğunu ve kullanıcının Windows Active Directory ve Azure Active Directory içinde mevcut olduğunu doğrulayın.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>HTTP hataları günlüklerinde başarısız olan tüm my kimlik doğrulamaları ile bağlanmak neden görüyor musunuz?

Doğrulayın https://adnotifications.windowsazure.com NPS uzantısı çalıştıran sunucudan erişilebilir.


## <a name="next-steps"></a>Sonraki adımlar

- İki aşamalı doğrulamayı gerçekleştirmek döndürmemelidir IP'ler için bir özel durum listesi ayarlamak veya alternatif kimlikleri oturum açma için yapılandırma [Gelişmiş çok faktörlü kimlik doğrulaması için NPS uzantısı için yapılandırma seçenekleri](howto-mfa-nps-extension-advanced.md)

- Nasıl tümleştireceğinizi öğrenin [Uzak Masaüstü Ağ Geçidi](howto-mfa-nps-extension-rdg.md) ve [VPN sunucuları](howto-mfa-nps-extension-vpn.md) NPS uzantısını kullanarak

- [Azure Multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme](howto-mfa-nps-extension-errors.md)
