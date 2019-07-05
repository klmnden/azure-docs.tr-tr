---
title: Azure tarafından yönetilen iş istasyonları - Azure Active Directory dağıtma
description: Yanlış yapılandırma veya güvenlik ihlali nedeniyle ihlal riskini azaltmak için güvenli, Azure tarafından yönetilen iş istasyonları dağıtmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51d8bbc8b8be9679fbf024d7c51de53c430dc493
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67550479"
---
# <a name="deploy-a-secure-azure-managed-workstation"></a>Güvenli, Azure tarafından yönetilen bir iş istasyonu dağıtma

Şimdi, [güvenli iş istasyonları anlamak](concept-azure-managed-workstation.md), dağıtım işlemini başlatmak için zamanı. Bu kılavuz ile baştan daha güvenli bir iş istasyonu oluşturmak için tanımlanmış profilleri kullanın.

![Güvenli bir iş istasyonu dağıtımı](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Çözümü dağıtmadan önce bir profil seçmelisiniz. Birden çok profil bir dağıtımda aynı anda kullanmak ve etiketleri veya gruplarla atayın.
> [!NOTE]
> Profillerinden birini gereksinimlerinize göre gerektiğinde de geçerlidir. Intune'da atayarak, başka bir profile taşıyabilirsiniz.

| Profil | Düşük | Gelişmiş | Yüksek | Özelleştirilmiş | Güvenli | Yalıtılmış |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Azure AD'de kullanıcı | Evet | Evet | Evet | Evet | Evet | Evet |
| Intune tarafından yönetilen | Evet | Evet | Evet | Evet | Evet | Evet |
| Cihaz - Azure AD kayıtlı | Evet |  |  |  |  | |   |
| Cihaz - Azure AD'ye katıldı |   | Evet | Evet | Evet | Evet | Evet |
| Uygulanan Intune güvenlik temeli |   | Evet <br> (Gelişmiş) | Evet <br> (HighSecurity) | Evet <br> (NCSC) | Evet <br> (Güvenli) |  NA |
| Donanım güvenli Windows 10 standartları karşılar |   | Evet | Evet | Evet | Evet | Evet |
| Microsoft Defender ATP etkin |   | Evet  | Evet | Evet | Evet | Evet |
| Yönetici hakları kaldırma |   |   | Evet  | Evet | Evet | Evet |
| Microsoft Autopilot'ı kullanarak dağıtım |   |   | Evet  | Evet | Evet | Evet |
| Yalnızca Intune tarafından yüklenen uygulamalar |   |   |   | Evet | Evet |Evet |
| Onaylı listeye kısıtlı URL'leri |   |   |   | Evet | Evet |Evet |
| Internet (gelen/giden) engellendi |   |   |   |  |  |Evet |

## <a name="license-requirements"></a>Lisans gereksinimleri

Bu kılavuzda ele kavramları, Microsoft 365 Kurumsal E5 veya eşdeğer bir SKU sahip olduğunuz varsayılır. Bazı öneriler bu kılavuzdaki alt SKU'larıyla uygulanabilir. Daha fazla bilgi için [Microsoft 365 Enterprise lisanslama](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Lisans sağlanmasını otomatikleştirmek için göz önünde bulundurun [grup tabanlı lisanslama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) kullanıcılarınız için.

## <a name="azure-active-directory-configuration"></a>Azure Active Directory yapılandırması

Azure Active Directory (Azure AD), kullanıcılara, gruplara veya, bir yönetici iş istasyonları için cihazları yönetir. Kimlik Hizmetleri ve özellikleri ile etkinleştirmek gereken bir [yönetici hesabı](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

Güvenli bir iş istasyonu yönetici hesabı oluşturduğunuzda, geçerli istasyonunuza hesap kullanıma sunar. Bu ilk yapılandırmayı ve tüm genel yapılandırmasını yapmak için bilinen güvenli bir cihaz kullandığınızdan emin olun. İlk kez deneyimi için saldırı ifşa riskini azaltmak için göz önünde bulundurun aşağıdaki [kötü amaçlı yazılım bulaşmalarını önlemeye yönelik kılavuz](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection).

Çok faktörlü kimlik doğrulaması, ayrıca en az ve yöneticileriniz için gerektiriyorsa. Bkz: [bulut tabanlı MFA dağıtma](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted) Uygulama Kılavuzu için.

### <a name="azure-ad-users-and-groups"></a>Azure AD kullanıcıları ve grupları

1. Azure portalından göz atın **Azure Active Directory** > **kullanıcılar** > **yeni kullanıcı**.
1. İçindeki adımları izleyerek, cihaz Yöneticisi oluşturma [kullanıcı öğretici oluşturma](https://docs.microsoft.com/Intune/quickstart-create-user).
1. Girin:
   * **Ad** -güvenli yönetici iş istasyonu
   * **Kullanıcı adı** - `secure-ws-admin@identityitpro.com`
   * **Dizin rolü** - **sınırlı yönetici** seçip **Intune yönetici** rol.
1. **Oluştur**’u seçin.

Ardından, iki grubu oluşturun: iş istasyonu kullanıcıları ve cihazları iş istasyonu.

Azure portalından göz atın **Azure Active Directory** > **grupları** > **yeni grup**.

1. İş istasyonu kullanıcıları grubuna yapılandırmak isteyebilirsiniz [grup tabanlı lisanslama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) lisanslarını kullanıcılara sağlanmasını otomatikleştirmek için.
1. İş istasyonu kullanıcıları grubuna girin:
   * **Grup türü** -güvenlik
   * **Grup adı** -güvenli iş istasyonu kullanıcıları
   * **Üyelik türü** - atanan
1. Güvenli bir iş istasyonu yönetici kullanıcı Ekle: `secure-ws-admin@identityitpro.com`
1. Güvenli iş istasyonlarını yönetme başka bir kullanıcı ekleyebilirsiniz.
1. **Oluştur**’u seçin.

1. İş istasyonu cihazlar grubu girin:
   * **Grup türü** -güvenlik
   * **Grup adı** -güvenli iş istasyonları
   * **Üyelik türü** - atanan
1. **Oluştur**’u seçin.

### <a name="azure-ad-device-configuration"></a>Azure AD cihaz yapılandırması

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Cihazları Azure AD'ye kimin katılabilir belirtin

Cihazlarınızı cihazlar, etki alanına eklemek, Yönetim güvenlik grubunun izin vermek için Active Directory'de ayarını yapılandırın. Azure portalından bu ayarı yapılandırmak için:

1. Git **Azure Active Directory** > **cihazları** > **cihaz ayarları**.
1. Seçin **seçili** altında **kullanıcılar cihazları Azure AD'ye Katıl**, ardından "Güvenli iş istasyonu kullanıcıları" grubu seçin.

#### <a name="removal-of-local-admin-rights"></a>Yerel yönetici hakları kaldırma

Bu yöntem VIP, DevOps ve güvenli düzeyinde iş istasyonu kullanıcıları makinelerinde hiç yönetici haklarına sahip olmasını gerektirir. Azure portalından bu ayarı yapılandırmak için:

1. Git **Azure Active Directory** > **cihazları** > **cihaz ayarları**.
1. Seçin **hiçbiri** altında **ek yerel Yöneticiler Azure AD alanına katılmış cihazlar**.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Cihazları eklemek için çok faktörlü kimlik doğrulaması gerektir

Cihazları Azure AD'ye katılma işlemini daha da güçlendirmek için:

1. Git **Azure Active Directory** > **cihazları** > **cihaz ayarları**.
1. Seçin **Evet** altında **cihazları eklemek için multi-Factor Auth gerektir**.
1. **Kaydet**’i seçin.

#### <a name="configure-mdm"></a>MDM yapılandırma

Azure portalından:

1. Gözat **Azure Active Directory** > **Mobility (MDM ve MAM)**  > **Microsoft Intune**.
1. Değişiklik **MDM kullanıcı kapsamı** ayarını **tüm**.
1. **Kaydet**’i seçin.

Bu adımları herhangi bir cihaz Intune ile yönetmenizi sağlar. Daha fazla bilgi için [Intune hızlı başlangıç: Windows 10 cihazları için otomatik kaydını ayarlama](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment). Intune, yapılandırma ve uyumluluk ilkelerini gelecekteki bir adımda oluşturacağınız.

#### <a name="azure-ad-conditional-access"></a>Azure AD Koşullu Erişim

Azure AD koşullu erişim, ayrıcalıklı yönetim görevleri için uyumlu cihazları kısıtlamak yardımcı olabilir. Önceden tanımlanmış üyeleri **güvenli iş istasyonu kullanıcıları** grubu, bulut uygulamaları oturum açarken çok faktörlü kimlik doğrulaması gerçekleştirmek için gereklidir. Acil Durum erişim hesapları ilkeden dışlamak en iyi bir uygulamadır. Daha fazla bilgi için [Azure AD'de Acil Durum erişim hesapları yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access).

Azure portalında koşullu erişim yapılandırmak için:

1. Git **Azure Active Directory** > **koşullu erişim** > **yeni ilke**.
1. Girin:
   * **Ad** -güvenli cihaz İlkesi gerekli
   * Atamalar
     * **Kullanıcılar ve gruplar**
       * Ekle - **kullanıcılar ve gruplar** - seçin **güvenli iş istasyonu kullanıcıları** daha önce oluşturduğunuz grup.
       * Dışlama - **kullanıcılar ve gruplar** -kuruluşunuzun Acil Durum erişim hesapları'nı seçin.
     * **Bulut uygulamaları** -dahil **tüm bulut uygulamaları**.
    * Erişim denetimleri
      * **GRANT** - seçin **erişim ver** radyo düğmesi.
        * **Çok faktörlü kimlik doğrulaması gerektiren**.
        * **Cihazın uyumlu olarak işaretlenmesini gerektir**.
        * Birden fazla denetimin - **seçilen tüm denetimleri gerekli**.
    * İlke - etkinleştirme **üzerinde**.

Kullanıcıların şirket kaynaklarına burada eriştiğiniz olmayan ülkelerde engelleme ilkeleri oluşturma seçeneğiniz vardır. IP konum tabanlı koşullu erişim ilkeleri hakkında daha fazla bilgi için bkz. [Azure Active Directory koşullu erişimde konum koşulu](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition).

## <a name="intune-configuration"></a>Intune yapılandırma

### <a name="configure-enrollment-status"></a>Kayıt durumu yapılandırma

Güvenli iş istasyonunuzu güvenilir temiz bir cihaz olduğundan emin olmak önemlidir. Yeni cihazlar satın alırken ısrar Fabrika kümesine oldukları [Windows 10 Pro S modunda](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode), tedarik zinciri yönetimi sırasında güvenlik açıklarına maruz kalma riskinizi sınırlar. Bir cihaz, tedarikçiden aldıktan sonra S moduna geçmek için Autopilot'ı kullanabilirsiniz. Aşağıdaki yönergeler dönüştürme süreci uygulama hakkında ayrıntılar sağlar.

Cihazları kullanılmadan önce tam olarak yapılandırıldığından emin olmak için Intune için bir yol sağlar **tüm uygulamalar ve Profiller yüklenene kadar cihaz kullanımını engelle**.

Gelen **Azure portalında**:
1. Git **Intune** > **cihaz kaydı** > **Windows kayıt** > **kayıt durumu Sayfa** > **varsayılan** > **ayarları**.
1. Ayarlama **Göster uygulama profili yükleme ilerleme durumu** için **Evet**.
1. Ayarlama **tüm uygulamalar ve Profiller yüklenene kadar cihaz kullanımını engelle** için **Evet**.

### <a name="create-an-autopilot-deployment-profile"></a>Bir Autopilot dağıtım profili oluşturma

Bir cihaz grubu oluşturduktan sonra Autopilot cihazları yapılandırmak için dağıtım profili oluşturmanız gerekir.

Azure portalında Intune'da:

1. Seçin **cihaz kaydı** > **Windows kayıt** > **dağıtım profilleri** > **profili oluştur** .
1. Girin:
   * Adı - **güvenli iş istasyonu Dağıtım profili**.
   * Açıklama - **güvenli iş istasyonlarının dağıtımını**.
   * Ayarlama **Autopilot için hedeflenen tüm cihazlara dönüştürmek** için **Evet**. Bu ayar, tüm cihazlar listesinde Autopilot dağıtım hizmetine kayıtlı emin olur. Kaydın işlenmesi için 48 saat kadar bekleyin.
1. **İleri**’yi seçin.
   * İçin **dağıtım modu**, seçin **Self (Önizleme) dağıtımı**. Bu profile sahip cihazların cihazı kaydeden kullanıcı ile ilişkilendirilmiş. Cihazı kaydetmek için kullanıcı kimlik bilgileri gerekir.
   * **Azure AD'ye Katıl** kutusu göstermelidir **Azure AD'ye katılmış** ve gri renkte.
   * Langugage (bölge), kullanıcı hesabı türü seçin **standart**. 
1. **İleri**’yi seçin.
   * Bir önceden yapılandırılmış durumunda bir kapsam etiketi seçin.
1. **İleri**’yi seçin.
1. Seçin **atamaları** > **atama** > **seçilen grupları**. İçinde **dahil edilecek grupları seçin**, seçin **güvenli iş istasyonu kullanıcıları**.
1. **İleri**’yi seçin.
1. Profili oluşturmak için **Oluştur**’u seçin. Autopilot dağıtım profili artık cihazlara atanmak üzere hazırdır.

### <a name="configure-windows-update"></a>Windows Update'i yapılandırma

Windows 10 güncel tutarak yapabileceğiniz en önemli şeylerden biridir. Windows güvenli bir durumda korumak için uygun bir hızda yönetmek için bir güncelleştirme halkası dağıtma güncelleştirmeleri iş istasyonları için uygulanır. 

Bu kılavuz, yeni bir güncelleştirme kademesi oluştur ve aşağıdaki varsayılan ayarlarının değiştirilmesi önerir:

Azure portalında:

1. Git **Intune** > **yazılım güncelleştirmelerini** > **Windows 10 güncelleştirme halkaları**.
1. Girin:
   * Adı - **Azure yönetilen iş istasyonu güncelleştirmeleri**
   * Hizmet kanalı - **Windows Insider - hızlı**
   * Kalite güncelleştirmesi erteleme (gün) - **3**
   * Özellik güncelleştirmesi erteleme süresi (gün) - **3**
   * Otomatik Güncelleştirme davranışı - **otomatik olarak yükle ve yeniden başlatma, son kullanıcı denetimi olmadan**
   * Kullanıcıyı engelle duraklatma Windows Update'ten - **bloğu**
   * İş saatleri dışında - yeniden başlatmayı kullanıcının onayını iste **gerekli**
   * (Meşgul) - yeniden izin ver **gerekli**
   * Sonra otomatik yeniden başlatma bir geçiş katılımcı yeniden kullanıcılara (gün) - **3**
   * Erteleme katılımcı yeniden anımsatıcı (gün) - **3**
   * Son tarih için bekleyen ayarlamak getirin (gün) - yeniden **3**

1. **Oluştur**’u seçin.
1. Üzerinde **atamaları** sekmesinde, ekleme **güvenli iş istasyonları** grubu.

Windows Update ilkeleri hakkında ek bilgi için bkz. [ilke CSP'si - güncelleştirme](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update).

### <a name="windows-defender-atp-intune-integration"></a>Windows Defender ATP'yi Intune tümleştirmesi

Windows Defender ATP ve Microsoft Intune güvenlik ihlallerini önlemeye yardımcı olmak için birlikte çalışır. Bunlar ayrıca İhlallerin etkisini sınırlayabilirsiniz. ATP yetenekleri sağlayan gerçek zamanlı tehdit algılama yanı sıra kapsamlı denetim ve günlüğe kaydetme uç nokta cihazları etkinleştirme.

Windows Defender ATP ve Intune tümleştirmesini yapılandırmak için Azure portalına gidin.

1. Gözat **Intune** > **cihaz uyumluluğu** > **Windows Defender ATP**.
1. Altında 1. adımda **Windows Defender ATP yapılandırma**seçin **Intune Windows Defender Güvenlik Merkezi'nde Windows Defender ATP bağlanma**.
1. Windows Defender Güvenlik Merkezi:
   1. Seçin **ayarları** > **Gelişmiş Özellikler**.
   1. İçin **Intune bağlantı**, seçin **üzerinde**.
   1. Seçin **Tercihleri Kaydet**.
1. Bağlantı kurulduktan sonra seçin ve Intune ile iade **Yenile** en üstünde.
1. Ayarlama **Windows cihazları sürüm 10.0.15063 bağlanmak ve üstü için Windows Defender ATP** için **üzerinde**.
1. **Kaydet**’i seçin.

Daha fazla bilgi için [Windows Defender Gelişmiş tehdit koruması](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="finish-workstation-profile-hardening"></a>Son profil iş istasyonu sağlamlaştırma

Çözümün sağlamlaştırma başarıyla tamamlamak için indirin ve uygun betiği yürütün. İndirme bağlantıları, istenen Bul **profil düzeyi**:

| Profil | İndirme konumu | Dosya adı |
| --- | --- | --- |
| Düşük güvenlik | Yok |  Yok |
| Geliştirilmiş Güvenlik | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Yüksek güvenlik  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Özelleştirilmiş | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows 10 (1803) SecurityBaseline.ps1 |
| Özel uyumluluk * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Güvenli | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

\* Özel uyumluluk NCSC Windows 10 SecurityBaseline içinde sağlanan özel yapılandırma zorlayan bir betiktir.

Betik başarıyla yürütüldükten sonra profilleri ve ilkeleri ıntune güncelleştirmeleri yapabilirsiniz. Betikler geliştirildi ve güvenli profilleri için ilkeler ve Profiller sizin için oluşturur, ancak ilkeye atayabileceğiniz gerekir, **güvenli iş istasyonları** grubu.

* Betikler tarafından oluşturulan Intune cihaz yapılandırma profilleri bulabileceğiniz yerler şöyledir: **Azure portalında** > **Intune** > **cihaz Yapılandırması** > **profilleri**.
* Intune cihaz uyumluluk ilkeleri betikler tarafından oluşturulan bulabileceğiniz yerler şöyledir: **Azure portalında** > **Intune** > **cihaz uyumluluğu** > **ilkeleri**.

Komut dosyaları tarafından yapılan değişiklikleri gözden geçirmek için profiller dışarı aktarabilirsiniz. Bu şekilde SECCON belgelerinde belirtildiği gibi gerekli olabilecek ek sağlamlaştırma belirleyebilirsiniz.

Intune veri dışa aktarma betiği çalıştırma `DeviceConfiguration_Export.ps1` gelen [cihaz yapılandırması GiuHub depo](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) tüm geçerli Intune profillerini dışarı aktarmak için.

## <a name="additional-configurations-and-hardening-to-consider"></a>Ek yapılandırmalar ve dikkate alınması gereken sağlamlaştırma

Buradaki yönergeleri izleyerek, güvenli bir iş istasyonu dağıttınız. Ancak, ek denetimler dikkate almanız. Örneğin:

* diğer tarayıcılar erişimi sınırlayın
* Giden HTTP izin ver
* Select siteleri engelle
* Özel PowerShell betikleri çalıştırmak için izinleri ayarlama

### <a name="set-rules-in-the-firewall-configuration-service-provider-csp"></a>Güvenlik Duvarı yapılandırma hizmet sağlayıcısı (CSP) kurallar

Gelen ve giden kurallar Yönetim için izin verilen ve engellenen uç gerektiği gibi ek değişiklikler yapabilirsiniz. Güvenli iş istasyonu sağlamlaştırma devam ederken, tüm gelen ve giden trafiği engelleyen kısıtlamayı çözmek. Ortak ve güvenilir Web siteleri dahil etmek için izin verilen giden siteler ekleyebilirsiniz. Daha fazla bilgi için [Güvenlik Duvarı Yapılandırma hizmeti](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp).

Kısıtlı varsayılan önerilerdir:

* Gelenlerin tümünü Reddet
* Gidenlerin tümünü Reddet

Spamhaus proje tutar [etki alanı engelleme listesi (çift)](https://www.spamhaus.org/dbl/): siteleri engellemek için bir başlangıç noktası olarak kullanmak iyi bir kaynaktır.

### <a name="manage-local-applications"></a>Yerel uygulamaları yönetme

Üretkenlik uygulamaları dahil olmak üzere yerel uygulamalar kaldırıldığında, güvenli iş istasyonu gerçekten sağlamlaştırılmış bir duruma taşınır. Burada, Chrome varsayılan tarayıcı olarak ekleyin ve Intune, kullanıcının tarayıcı ve eklentilerini değiştirme olanağı sınırlamak için kullanın.

#### <a name="deploy-applications-using-intune"></a>Intune kullanarak uygulamaları dağıtma

Bazı durumlarda, Google Chrome tarayıcısının gibi uygulamaları güvenli iş istasyonu gereklidir. Aşağıdaki örnekte, Chrome güvenlik grubundaki cihazlara yüklemek için yönergeler sunulmaktadır **güvenli iş istasyonları**.

1. Çevrimdışı yükleyiciyi indirmek [Windows 64‑bit Chrome paketi](https://cloud.google.com/chrome-enterprise/browser/download/).
1. Dosyaları ayıklayın ve konumunu not edin `GoogleChromeStandaloneEnterprise64.msi` dosya.
1. İçinde **Azure portalında** göz atın **Intune** > **istemci uygulamaları** > **uygulamaları**  >  **Ekleme**.
1. Altında **uygulama türü**, seçin **satır iş kolu**.
1. Altında **uygulama paketi dosyası**seçin `GoogleChromeStandaloneEnterprise64.msi` seçin ve dosya ayıklanan konumdan **Tamam**.
1. Altında **uygulama bilgileri**, yayımcı ve bir açıklama sağlayın. **Tamam**’ı seçin.
1. **Add (Ekle)** seçeneğini belirleyin.
1. Üzerinde **atamaları** sekmesinde **kayıtlı cihazlar için kullanılabilir** altında **atama türü**.
1. Altında **bulunan gruplarını**, ekleme **güvenli iş istasyonları** grubu.
1. Seçin **Tamam**ve ardından **Kaydet**.

Chrome ayarlarını yapılandırma hakkında daha fazla rehberlik için bkz [yönetme Chrome tarayıcısı Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Özel uygulamalar için şirket Portalı'nı yapılandırma

Güvenli bir modda Intune Şirket portalı uygulama yüklemesi sınırlıdır. Ancak, Portalı'nı yükleme, Microsoft Store erişim gerektirir. Güvenli çözümünüzde, Şirket portalı aracılığıyla Çevrimdışı mod tüm cihazlar için kullanılabilir yapabilirsiniz.

Intune tarafından yönetilen bir kopyasını [Şirket portalı](https://docs.microsoft.com/Intune/store-apps-company-portal-app) güvenli iş istasyonları kullanıcılara anında ek araçlar isteğe bağlı erişim sağlar.

Windows 32-bit uygulamalar ya da diğer uygulamalarda, dağıtım gerektiren özel hazırlıkları yüklemeniz gerekebilir. Bu gibi durumlarda [Microsoft win32 içerik Hazırlık Aracı](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) bir hazır kullanımı sağlayabilir `.intunewin` yüklemesi için Biçim dosyası.

### <a name="use-powershell-to-create-custom-settings"></a>Özel ayarlar oluşturmak için PowerShell kullanma

Konak yönetim özelliklerinizi genişletmek için PowerShell de kullanabilirsiniz. Bu örnek betik bir varsayılan arka plan konak üzerindeki ayarlar. Ayrıca Azure portalı ve profilleri aracılığıyla kullanılabilir olan bir özelliktir. Örnek betik, yalnızca PowerShell işlevselliğini göstermek için kullanılır.

Bazı özel denetimler ve ayarları, güvenli iş istasyonlarında ayarlamanız gerekebilir. Bu örnek, kullanıma hazır, güvenli bir iş istasyonu olarak cihaz kolayca belirlemek için PowerShell'in özelliğini kullanarak iş istasyonu arka planını değiştirir.

[SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) Microsoft Scripting Center betikten sağlayan bu yüklemek Windows [ücretsiz, genel arka plan resmi](https://i.imgur.com/OAJ28zO.png) üzerinde başlatın.

1. Yerel bir cihaza komut dosyasını indirin.
1. CustomerXXXX ve arka plan görüntüsü indirme konumunu güncelleştirin. Bizim örneğimizde, biz arka planlarına customerXXXX değiştirin.  
1. Gözat **Azure portalında** > **Intune** > **cihaz Yapılandırması** > **PowerShell betikleri** > **Ekle**.
1. Sağlayan bir **adı** komut dosyasını belirtin **betik konumu**.
1. Seçin **yapılandırma**.
   1. Ayarlama **oturum açmış kimlik bilgilerini kullanarak bu betiği çalıştırın** için **Evet**.
   1. **Tamam**’ı seçin.
1. **Oluştur**’u seçin.
1. Seçin **atamaları** > **grupları seçin**.
   1. Güvenlik grubuna kullanıcı eklemeniz **güvenli iş istasyonları**.
   1. **Kaydet**’i seçin.

## <a name="enroll-and-validate-your-first-device"></a>Kaydolma ve ilk Cihazınızı doğrula

1. Cihazınızı kaydetmek için aşağıdaki bilgiler gereklidir:
   * **Seri numarası** - cihaz kasa üzerinde bulunamadı.
   * **Windows ürün kimliği** - altında bulunan **sistem** > **hakkında** Windows Ayarlar menüsünden.
   * Çalıştırabileceğiniz [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) cihaz kaydı için tüm gerekli bilgileri içeren CSV karma dosyalarını almak için.
   
     Çalıştırma `Get-WindowsAutoPilotInfo – outputfile device1.csv` bilgileri Intune'a de içeri aktarabileceğiniz bir CSV dosyası olarak çıkarmak için.

     > [!NOTE]
     > Komut dosyasını yükseltilmiş haklara gerektirir. Uzak olarak çalıştığı imzalanmış. `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` Komutu, doğru çalıştırılacak betik sağlar.

   * Bir Windows 10 sürüm 1809 veya daha yüksek bir cihaza oturum açarak bu bilgi toplayabilirsiniz. Donanım satıcınızla, bu bilgileri de sağlayabilirsiniz.
1. İçinde **Azure portalında**Git **Intune** > **cihaz kaydı** > **Windows kayıt**  >  **Cihazlar - yönetme Windows Autopilot cihazları**.
1. Seçin **alma** ve CSV dosyanızı seçin.
1. Aygıt ekleme **güvenli iş istasyonları** güvenlik grubu.
1. Yapılandırmak istediğiniz Windows 10 cihazında Git **Windows ayarları** > **güncelleştirme ve güvenlik** > **kurtarma**.
   1. Seçin **başlama** altında **bu PC sıfırlama**.
   1. Sıfırlayabilir ve cihazın yapılandırılan profili ve uyumluluk ilkeleri ile yeniden yönergeleri izleyin.

Cihaz yapılandırdıktan sonra bir gözden geçirmeyi tamamlayın ve yapılandırmayı kontrol edin. İlk cihaz devam etmeden önce dağıtımınızı doğru şekilde yapılandırıldığını onaylayın.

## <a name="assign-and-monitor"></a>Atama ve izleme

Cihazlara ve kullanıcılara atamak için eşlemeniz [Seçili profilleri](https://docs.microsoft.com/intune/device-profile-assign) için güvenlik grubunuza. Hizmet izinleri isteyen tüm yeni kullanıcılar de güvenlik grubuna eklenmesi gerekir.

Profilleriyle izleyebilirsiniz [Intune profili izleme](https://docs.microsoft.com/intune/device-profile-monitor).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Intune](https://docs.microsoft.com/intune/index).
* Anlamak [Azure AD'ye](https://docs.microsoft.com/azure/active-directory/index).
