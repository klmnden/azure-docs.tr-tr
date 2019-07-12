---
title: Azure mfa'yı özellikleri - Azure Active Directory sağlamak için mevcut NPS sunucuları kullanın.
description: Mevcut kimlik doğrulama altyapınızı bulutta yer alan iki aşamalı doğrulama özellikleri ekleyin
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca6f79b5febdbf12c80ab85d07117bf937babef0
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798202"
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Mevcut NPS altyapınızı Azure multi-Factor Authentication ile tümleştirme

Azure MFA için ağ ilkesi sunucusu (NPS) uzantısı, var olan sunucuları kullanarak kimlik doğrulaması altyapınız için bulut tabanlı MFA özellikleri ekler. NPS uzantısı ile yükleyin, yapılandırın ve yeni sunucuların bakımını yapmak zorunda kalmadan, mevcut kimlik doğrulama akışı için telefon araması, SMS mesajı ve telefon uygulaması doğrulama ekleyebilirsiniz. 

Bu uzantı, Azure MFA Sunucusu'nu dağıtmadan VPN bağlantıları korumak istediğiniz kuruluşlar için oluşturuldu. Bir bağdaştırıcı RADIUS ve Azure MFA bulut tabanlı bir ikinci faktör kimlik doğrulaması sağlamak için arasında federasyona eklenenler veya eşitlenmiş kullanıcıları NPS uzantısı işlevi görür.

Azure MFA için NPS uzantısı kullanarak, kimlik doğrulaması akışı aşağıdaki bileşenleri içerir: 

1. **NAS/VPN sunucusu** VPN istemcilerinden gelen istekleri alan ve NPS sunucuları için RADIUS isteklerini dönüştürür. 
2. **NPS sunucusu** birincil kimlik doğrulaması için RADIUS isteklerini gerçekleştirmek için Active Directory'ye bağlanır ve başarılı olduktan sonra yüklenen tüm uzantıları istek geçirir.  
3. **NPS uzantısı** ikincil kimlik doğrulaması için Azure mfa'yı istek tetikler. Uzantı yanıtı alır ve MFA testini başarılı olursa, MFA talebi içeren güvenlik belirteçleri ile NPS sunucusu sağlayarak kimlik doğrulama isteği tamamlandıktan sonra Azure STS tarafından verilen.  
4. **Azure mfa'yı** Azure kullanıcının ayrıntılarını almak için Active Directory'ye ile iletişim kurar ve kullanıcı için yapılandırılan bir doğrulama yöntemiyle ikincil kimlik doğrulaması gerçekleştirir.

Aşağıdaki diyagramda bu üst düzey kimlik doğrulama isteği akışı gösterilmektedir: 

![Kimlik doğrulaması akışı diyagramı](./media/howto-mfa-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Dağıtımınızı planlama

NPS uzantısı otomatik olarak yedeklilik, işler, böylece özel bir yapılandırma gerekmez.

İhtiyacınız kadar etkin Azure MFA NPS sunucusu oluşturabilirsiniz. Birden çok sunucuya yüklerseniz, her biri için bir fark istemci sertifikasını kullanmanız gerekir. Her sunucu için bir sertifika oluşturma, tek tek her sertifika güncelleştirmesi ve kapalı kalma süresi, tüm sunucularınız genelinde kaygılanmayın anlamına gelir.

Yeni Azure mfa'yı etkin NPS sunucularında bilmeniz gerekir böylece VPN sunucusu kimlik doğrulama isteklerini, yönlendirir.

## <a name="prerequisites"></a>Önkoşullar

NPS uzantısı, var olan altyapınızla çalışacak şekilde tasarlanmıştır. Başlamadan önce aşağıdaki önkoşulların karşılandığından emin olun.

### <a name="licenses"></a>Lisansları

Azure MFA NPS uzantısı ile müşteriler için kullanılabilir [Azure multi-Factor Authentication için lisans](multi-factor-authentication.md) (Azure AD Premium, EMS veya tek başına bir MFA lisans dahil). Tüketim tabanlı lisans kullanıcı başına veya kimlik doğrulaması lisans başına gibi Azure mfa NPS uzantısı ile uyumlu değildir. 

### <a name="software"></a>Yazılım

Windows Server 2008 R2 SP1 veya üstü.

### <a name="libraries"></a>Kitaplıklar

Bu kitaplıklar uzantısı ile otomatik olarak yüklenir.

- [Visual Studio 2013 (X64) için Visual C++ yeniden dağıtılabilir paketleri](https://www.microsoft.com/download/details.aspx?id=40784)
- [Microsoft Azure Active Directory için Windows PowerShell modülü sürüm 1.1.166.0](https://www.powershellgallery.com/packages/MSOnline/1.1.166.0)

Zaten bir yapılandırma betiği Kurulum işleminin bir parçası olarak çalıştırmak aracılığıyla, mevcut değilse Microsoft Azure Active Directory için Windows PowerShell Modülü yüklü. Zaten yüklü değilse, önceden bu modülünü yüklemek için gerek yoktur.

### <a name="azure-active-directory"></a>Azure Active Directory

NPS uzantısı kullanan herkesin, Azure Active Azure AD Connect'i kullanarak dizin eşitlenmesi gerekir ve MFA için kayıtlı olması gerekir.

Uzantı yükleme sırasında Azure AD kiracınız için dizin kimliği ve yönetici kimlik bilgileri gerekir. Dizin Kimliğinizi bulabilirsiniz [Azure portalında](https://portal.azure.com). Bir yönetici olarak oturum açın, select **Azure Active Directory** soldaki simgesini seçip **özellikleri**. GUID kopyalama **dizin kimliği** kutusunda ve kaydedin. NPS uzantısını yüklediğinizde bu GUID Kiracı Kimliğini kullanın.

![Dizin Kimliğinizle Azure Active Directory özelliklerini Bul](./media/howto-mfa-nps-extension/find-directory-id.png)

### <a name="network-requirements"></a>Ağ gereksinimleri

NPS sunucusu aşağıdaki URL'ler ile 80 ve 443 bağlantı noktaları üzerinden iletişim kurabilmesi gerekir.

- https:\//adnotifications.windowsazure.com
- https:\//login.microsoftonline.com

Ayrıca, aşağıdaki URL'ler bağlantısını tamamlamak için gereken [PowerShell betiğini kullanarak bağdaştırıcısı Kurulumu](#run-the-powershell-script)

- https:\//login.microsoftonline.com
- https:\//provisioningapi.microsoftonline.com
- https:\//aadcdn.msauth.net

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

NPS uzantısını yüklemeden önce kimlik doğrulama trafiğini işlemek için ortamı hazırlama istiyorsunuz.

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>Etki alanına katılmış bir sunucudaki NPS rolü etkinleştir

NPS sunucusu, Azure Active Directory'ye bağlanır ve MFA istekleri doğrular. Bu rol için bir sunucu seçin. Hataları, RADIUS olmayan tüm istekler için NPS uzantısı oluşturur çünkü diğer hizmetlerden gelen istekleri işleyemez bir sunucu seçme öneririz. Ortamınız için birincil ve ikincil kimlik doğrulama sunucusu olarak NPS sunucusunu ayarlanmış olması gerekir; başka bir sunucuya proxy RADIUS isteklerini bu işlem gerçekleştirilemiyor.

1. Sunucunuzda açık **Ekle roller ve Özellikler Sihirbazı** Server Manager Hızlı Başlangıç menüsünde.
2. Seçin **rol tabanlı veya özellik tabanlı yükleme** yükleme türünüz için.
3. Seçin **Ağ İlkesi ve Erişim Hizmetleri** sunucu rolü. Bir pencere bu rolü çalıştırmak için gerekli özellikleri hakkında bilgilendirmek için açılır.
4. Sihirbaza onay sayfası kadar devam edin. **Yükle**’yi seçin.

Bir sunucu için NPS atanmış olduğuna göre VPN çözümünden gelen RADIUS isteklerini işlemek için bu sunucuyu da yapılandırmanız gerekir.

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>VPN çözümünüzün NPS sunucusu ile iletişim kurmak için yapılandırma

Kullandığınız VPN çözümüne bağlı olarak, RADIUS kimlik doğrulaması ilkenizi yapılandırma adımları değişiklik gösterir. Bu ilke RADIUS NPS sunucunuzu işaret edecek şekilde yapılandırın.

### <a name="sync-domain-users-to-the-cloud"></a>Bulut eşitleme etki alanı kullanıcıları

Bu adım zaten kiracınızda tam olabilir, ancak Azure AD Connect'in son veritabanlarınızı eşitlendiğini denetleyin daha uygundur.

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Seçin **Azure Active Directory** > **Azure AD'ye bağlanma**
3. Eşitleme durumunuzu doğrulayın **etkin** ve bu, son eşitleme bir saatten önce gerçekleştirildi.

Eşitleme, bize yönergeleri yeni bir gidiş istiyorsanız gerekiyorsa, [Azure AD Connect eşitleme: Scheduler](../hybrid/how-to-connect-sync-feature-scheduler.md#start-the-scheduler) makalesindeki adımlara bakın.

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Kullanıcılarınızın hangi kimlik doğrulama yöntemlerini belirleme

Hangi kimlik doğrulama yöntemleri ile bir NPS uzantısı dağıtımı kullanılabilir etkileyen iki faktör vardır:

1. RADIUS istemcisi arasında kullanılan parola şifreleme algoritması (VPN, Netscaler sunucu veya diğer) ve NPS sunucularını.
   - **PAP** bulutta Azure MFA'ın tüm kimlik doğrulama yöntemleri destekler: telefon araması, tek yönlü SMS mesajı, mobil uygulama bildirimi ve mobil uygulama doğrulama kodu.
   - **CHAPV2** ve **EAP** telefon araması ve mobil uygulama bildirimi destekler.

      > [!NOTE]
      > NPS uzantısı dağıttığınızda, kullanıcılarınız için hangi yöntemlerin kullanılabilir değerlendirmek için bu faktörlerin kullanın. RADIUS istemcinizi PAP destekler, ancak istemci UX bir doğrulama kodu için giriş alanlarını yok ardından telefon araması ve mobil uygulama bildirimi desteklenen iki seçenek vardır.
      >
      > Ağ erişim ilkesi - VPN istemcinizi UX girdi desteklemiyor ve yapılandırdıysanız, ayrıca, kimlik doğrulaması, ancak hiçbir ağ erişim cihazı için Ağ İlkesi'nde yapılandırılan RADIUS özniteliklerini hiçbiri uygulanacak başarılı olabilir, RRAS sunucusu ya da VPN istemcisi gibi. Sonuç olarak, VPN istemcisi, istenen veya hiçbir erişim için daha fazla erişime sahip olabilir.
      >

2. Giriş yöntemleri, istemci uygulaması (VPN, Netscaler sunucu veya diğer) işleyebilir. Örneğin, VPN istemcisi, bir metin veya mobil uygulama doğrulama kodu yazmak izin vermek için bazı araçlar var mı?

Yapabilecekleriniz [desteklenmeyen kimlik doğrulama yöntemleri devre dışı](howto-mfa-mfasettings.md#verification-methods) azure'da.

### <a name="register-users-for-mfa"></a>Kullanıcıları MFA'ya kaydetmeniz

Dağıtma ve NPS uzantısı'ı kullanmadan önce iki aşamalı doğrulamayı gerçekleştirmek için gerekli olan kullanıcılar için mfa'yı kaydedilmesi gerekir. Daha fazla hemen, dağıtım sırasında bu uzantıyı test etmek için çok faktörlü kimlik doğrulaması için tam olarak kayıtlı en az bir test hesabı gerekir.

Başlatılan bir test hesabı almak için aşağıdaki adımları kullanın:

1. Oturum [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) test hesapla.
2. Bir doğrulama yöntemi ayarlamak için yönergeleri izleyin.
3. [Koşullu erişim ilkesi oluşturma](howto-mfa-getstarted.md#create-conditional-access-policy) test hesap için çok faktörlü kimlik doğrulaması istemek için.

## <a name="install-the-nps-extension"></a>NPS uzantısını yükleme

> [!IMPORTANT]
> VPN erişim noktası farklı bir sunucuda NPS uzantısını yükleyin.

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>İndirin ve Azure MFA için NPS uzantısını yükleme

1. [NPS uzantısını indirin](https://aka.ms/npsmfa) Microsoft İndirme Merkezi'nden.
2. İkili, yapılandırmak istediğiniz ağ ilkesi sunucusuna kopyalayın.
3. Çalıştırma *setup.exe* ve yükleme yönergelerini izleyin. Hatalarla karşılaşırsanız, konusunun önkoşul bölümüne iki kitaplıklarından başarıyla yüklendiğini denetleyin.

#### <a name="upgrade-the-nps-extension"></a>NPS uzantısını Yükselt

Mevcut bir NPS uzantısı yükseltme yüklediğinizde önlemek için temel alınan sunucunun yeniden başlatılmasını aşağıdaki adımları tamamlayın:

1. Var olan sürümü kaldırın
1. Yeni yükleyiciyi çalıştırın
1. Ağ İlkesi Sunucusu (IAS) hizmetini yeniden başlatın

### <a name="run-the-powershell-script"></a>PowerShell betiğini çalıştırma

Yükleyici, bu konumda bir PowerShell Betiği oluşturur: `C:\Program Files\Microsoft\AzureMfa\Config` (burada, yükleme sürücüsünü, C:\). Bu PowerShell Betiği her çalıştırıldığında aşağıdaki eylemleri gerçekleştirir:

- Otomatik olarak imzalanan bir sertifika oluşturun.
- Hizmet sorumlusu Azure AD üzerinde sertifikanın ortak anahtarı ile ilişkilendirin.
- Sertifika yerel makine sertifika deposunda Store.
- Sertifikanın özel anahtarı ağ kullanıcıya erişim izni verin.
- NPS yeniden başlatın.

Kendi sertifikalarınızı (yerine PowerShell betiğini oluşturur, otomatik olarak imzalanan sertifikaları) kullanmak istemiyorsanız, yüklemeyi tamamlamak için bu PowerShell Betiği çalıştırın. Uzantı birden çok sunucuya yüklerseniz, her biri kendi sertifika olması gerekir.

1. Windows PowerShell'i yönetici olarak çalıştırın.
2. Dizinleri değiştirin.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Yükleyici tarafından oluşturulan PowerShell betiğini çalıştırın.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. Azure AD'de yönetici olarak oturum açın.
5. Kiracı kimliğiniz için PowerShell ister Önkoşullar bölümündeki Azure portaldan kopyaladığınız dizin kimliği GUID kullanın.
6. Betik tamamlandığında, PowerShell, bir başarı iletisi gösterilir.  

Yük Dengeleme için ayarlamak istediğiniz herhangi bir ek NPS sunucularında bu adımları yineleyin.

Önceki bilgisayar sertifikanızın süresi doldu ve oluşturulan yeni bir sertifika, süresi dolmuş sertifikalarını silmeniz gerekir. Süresi dolmuş sertifikaları sorunlarına neden olabilir NPS uzantısı ile başlayan sahip.

> [!NOTE]
> PowerShell betiğiyle sertifikalarını üretmek yerine kendi sertifikalarınızı kullanırsanız, bunlar için NPS adlandırma kuralı hizalama emin olun. Konu adı olmalıdır **CN =\<Tenantıd\>, OU = Microsoft NPS uzantı**. 

### <a name="certificate-rollover"></a>Sertifika geçişi

Sürüm ile birden çok sertifika okunurken NPS uzantısı 1.0.1.32 artık desteklenmektedir. Bu özellik, sıralı sertifika güncelleştirmelerini tarihlerinden önce kolaylaştırmak yardımcı olur. Kuruluşunuz NPS uzantısı'nın önceki bir sürümü çalıştırıyorsa 1.0.1.32 sürümüne yükseltmeniz gerekir ya da daha yüksek.

Tarafından oluşturulan sertifikaları `AzureMfaNpsExtnConfigSetup.ps1` betik 2 yıl boyunca geçerlidir. BT kuruluşları, sona erme için sertifikaları izlemeniz gerekir. Sertifikaları NPS uzantısı için yerel bilgisayar sertifika deposunda kişisel altına yerleştirilir ve verilen için Kiracı kimliği için betik sağlanır.

Sertifika sona erme tarihini yaklaşırken değiştirmek için yeni bir sertifika oluşturulması gerekir.  Bu işlem kullanılarak elde edilir `AzureMfaNpsExtnConfigSetup.ps1` yeniden ve istendiğinde aynı Kiracı Kimliğini de saklayabilirsiniz. Bu işlem, ortamınızdaki her bir NPS sunucusu üzerinde yinelenmelidir.

## <a name="configure-your-nps-extension"></a>NPS uzantısı yapılandırma

Bu bölümde, tasarım konuları ve başarılı NPS uzantısı dağıtımlar için öneriler içerir.

### <a name="configuration-limitations"></a>Yapılandırma sınırlamaları

- Azure MFA NPS uzantısı kullanıcılarınız ve ayarlarınız MFA Sunucusu'ndan buluta geçirmek için araçları içermez. Bu nedenle, mevcut dağıtım yerine yeni dağıtımlar için uzantıyı kullanmanızı öneririz. Uzantı üzerinde var olan bir dağıtım kullanırsanız, kullanıcılarınızın bulutta MFA ayrıntılarını yeniden doldurmak için kavram yukarı gerçekleştirmek sahip.  
- NPS uzantısı şirket içi Active Directory'den Azure MFA kullanıcı ikincil kimlik doğrulaması gerçekleştirmek için tanımlamak üzere UPN kullanır Uzantı, alternatif bir oturum açma kimliği veya UPN dışında özel Active Directory alan gibi farklı bir kimlik kullanmak için yapılandırılabilir. Daha fazla bilgi için bkz [Gelişmiş multi-Factor Authentication için NPS uzantısı için yapılandırma seçenekleri](howto-mfa-nps-extension-advanced.md).
- Tüm şifreleme protokolleri tüm doğrulama yöntemlerini destekler.
   - **PAP** telefon araması, tek yönlü SMS mesajı, mobil uygulama bildirimi ve mobil uygulama doğrulama kodu destekler
   - **CHAPV2** ve **EAP** telefon araması ve mobil uygulama bildirimi desteği

### <a name="control-radius-clients-that-require-mfa"></a>Mfa'yı gerekli denetimi RADIUS istemcileri

MFA NPS uzantısı kullanarak bir RADIUS istemcisi için etkinleştirdikten sonra MFA gerçekleştirmek için bu istemci için tüm kimlik doğrulamaları gerekir. Bazı RADIUS istemcileri ve diğerleri için mfa'yı etkinleştirmek istiyorsanız, iki NPS sunucularını yapılandırın ve yalnızca bunlardan birine uzantıyı yükleyin. Mfa'yı uzantısıyla yapılandırılmış NPS sunucusuna istekleri göndermek gerekli istediğiniz RADIUS istemcileri ve diğer uzantısı ile yapılandırılmamış NPS sunucusunun RADIUS istemcileri yapılandırın.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>MFA için kayıtlı olmayan kullanıcılar için hazırlama

MFA için kayıtlı olmayan kullanıcılar varsa, kimlik doğrulaması çalıştıklarında ne belirleyebilirsiniz. Kayıt defteri ayarını kullanın *REQUIRE_USER_MATCH* kayıt defteri yolunda *HKLM\Software\Microsoft\AzureMFA* özellik davranışını denetlemek için. Bu ayar bir yapılandırma seçeneği vardır:

| Anahtar | Value | Varsayılan |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | TRUE/FALSE | (TRUE eşdeğer) ayarlanmadı |

Bu ayar amacı bir kullanıcı için mfa'yı kayıtlı ne yapılacağını belirlemektir. Zaman anahtarı yok, ayarlı değil veya olan TRUE olarak ayarlanmış ve kullanıcı kayıtlı değilse, ardından MFA testini uzantısı başarısız olur. Anahtar FALSE olarak ayarlanırsa kullanıcı kayıtlı değilse, kimlik doğrulaması, MFA yapmadan devam eder. Kullanıcı MFA kaydedildiyse REQUIRE_USER_MATCH FALSE olarak ayarlanmış olsa bile MFA ile kimlik doğrulaması gerekir.

Bu anahtarı oluşturun ve bunu FALSE olarak ekleme, kullanıcılar ve tüm henüz Azure MFA için kaydedilebilir ancak ayarlama seçebilirsiniz. Ancak, anahtar ayarlama oturum açmak MFA için kayıtlı olmayan kullanıcılar verdiğinden üretime geçmeden önce bu anahtarı kaldırmanız gerekir.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="nps-extension-health-check-script"></a>NPS uzantısı sistem durumu Denetim betiği

Aşağıdaki betiği TechNet galerisinde NPS uzantıyı gidermede temel sistem durumu onay adımları gerçekleştirmek için kullanılabilir.

[MFA_NPS_Troubleshooter.ps1](https://gallery.technet.microsoft.com/Azure-MFA-NPS-Extension-648de6bb)

---

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>İstemci sertifikasının beklendiği gibi yüklü olduğunu nasıl doğrularım?

Sertifika deposu ve özel anahtarı kullanıcıya verilen izinlere sahip olup olmadığını kontrol edin yükleyici tarafından oluşturulan kendinden imzalı bir sertifika arayın **ağ hizmeti**. Sertifikanın bir konu adını taşıyan **CN \<tenantıd\>, OU = Microsoft NPS uzantısı**

Tarafından oluşturulan otomatik olarak imzalanan sertifikalar *AzureMfaNpsExtnConfigSetup.ps1* betiği Ayrıca iki yıllık bir geçerlilik ömrü vardır. Sertifika yüklü olduğu doğrulanıyor, sertifika süresinin sona ermediğini denetlemeniz gerekir.

---

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>My istemci sertifikası için Azure Active Directory kiracımdaki ilişkili olduğunu nasıl doğrulayabilirim?

PowerShell komut istemi açın ve aşağıdaki komutları çalıştırın:

``` PowerShell
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1
```

Bu komutlar, kiracınız NPS uzantısı örneğinizle PowerShell oturumunuzda ilişkilendirme tüm sertifikaları yazdırın. Sertifikanız için istemci sertifikası özel anahtar olmadan "Base-64 kodlanmış X.509(.cer)" dosyası olarak dışa aktararak bakın ve PowerShell listeden karşılaştırır.

Aşağıdaki komut, biçim .cer, "C:" sürücünüzde "npscertificate" adlı bir dosya oluşturur.

``` PowerShell
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 | select -ExpandProperty "value" | out-file c:\npscertficicate.cer
```

Bu komutu çalıştırdıktan sonra C sürücüsüne dosyasını bulun ve çift tıklayın gidin. Ayrıntılarına gidin ve "parmak izi" aşağı kaydırın, bu sunucuya yüklenen sertifikanın parmak izini karşılaştırın. Sertifika parmak izleri eşleşmesi gerekir.

Geçerli-başlangıç ve geçerli-kadar okunabilir formda bulunan, zaman damgalarını komut birden fazla sertifika döndürürse, açık misfits filtrelemek için kullanılabilir.

---

### <a name="why-cant-i-sign-in"></a>Yapılamıyor neden oturum açmalıyım?

Parolanızın süresinin dolmadığından denetleyin. NPS uzantısı, oturum açma iş akışının bir parçası parola değiştirmeyi desteklemez. Kuruluşunuzun BT personeli, daha fazla yardım almak için iletişime geçin.

---

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Neden isteklerim ADAL belirteç hatası ile başarısız oluyor?

Bu hata çeşitli nedenlerden biri nedeniyle olabilir. Sorun gidermek için aşağıdaki adımları kullanın:

1. NPS sunucunuzu yeniden başlatın.
2. Bu istemci sertifikası, beklendiği gibi yüklendiğini doğrulayın.
3. Sertifikanın Azure AD kiracınız ile ilişkili olduğunu doğrulayın.
4. Uzantıyı çalıştıran sunucudan https://login.microsoftonline.com/ adresine erişilebildiğini doğrulayın.

---

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Neden kimlik doğrulaması, HTTP günlüklerinde kullanıcı bulunamadı belirten bir hata ile başarısız?

AD Connect'in çalıştığını ve kullanıcının hem Windows Active Directory hem de Azure Active Directory'de mevcut olduğunu doğrulayın.

---

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Günlüklerde başarısız olan tüm my kimlik doğrulamaları ile bağlanma HTTP neden görüyorum?

NPS uzantısını çalıştıran sunucudan https://adnotifications.windowsazure.com adresine ulaşılabildiğini doğrulayın.

---

### <a name="why-is-authentication-not-working-despite-a-valid-certificate-being-present"></a>Neden kimlik doğrulaması, geçerli bir sertifika bulunmasına rağmen çalışmıyor?

Önceki bilgisayar sertifikanızın süresi doldu ve oluşturulan yeni bir sertifika, süresi dolmuş sertifikalarını silmeniz gerekir. Süresi dolmuş sertifikaları sorunlarına neden olabilir NPS uzantısı ile başlayan sahip.

Geçerli bir sertifika varsa denetlemek için yerel bilgisayar hesabının sertifika MMC kullanarak Store denetleyin ve sertifika, sona erme tarihi geçmemiş emin olun. Yeni geçerli bir sertifika oluşturmak için bir bölüm altında adımlarını yeniden çalıştıracaktır "[PowerShell betiğini çalıştırma](#run-the-powershell-script)"

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>TLS/SSL Protokollerini ve Şifre Paketlerini yönetme

Eski ve daha zayıf şifre paketleri devre dışı bırakılabilir veya kuruluşunuz tarafından gerekli kılınmadıkça kaldırılması önerilir. Bu görevin nasıl gerçekleştirileceği hakkında bilgiler [AD FS için SSL/TLS Protokollerini ve Şifre Paketlerini Yönetme](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-protocols-in-ad-fs) makalesine bakın

### <a name="additional-troubleshooting"></a>Ek sorun giderme

Ek sorun giderme kılavuzunu ve olası çözümlerini makalesinde bulunabilir [Azure multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme](howto-mfa-nps-extension-errors.md).

## <a name="next-steps"></a>Sonraki adımlar

- Alternatif oturum açma kimliklerini yapılandırabilir veya iki aşamalı doğrulamayı gerçekleştirmek olmamalıdır IP'ler için bir özel durum listesi ayarlamak [Gelişmiş multi-Factor Authentication için NPS uzantısı için yapılandırma seçenekleri](howto-mfa-nps-extension-advanced.md)

- Nasıl tümleştireceğinizi öğrenin [Uzak Masaüstü Ağ Geçidi](howto-mfa-nps-extension-rdg.md) ve [VPN sunucuları](howto-mfa-nps-extension-vpn.md) NPS uzantısını kullanma

- [Azure Multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme](howto-mfa-nps-extension-errors.md)
