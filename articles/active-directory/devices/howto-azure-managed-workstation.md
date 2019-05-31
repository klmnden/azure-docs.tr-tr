---
title: Dağıtma Azure yönetilen iş istasyonları - Azure Active Directory
description: Yanlış yapılandırma veya güvenlik ihlali nedeniyle ihlal riskini azaltmak için güvenli Azure tarafından yönetilen iş istasyonlarının dağıtımını yapma hakkında bilgi edinin
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
ms.openlocfilehash: 242926c0821e4951d2a2bd2f858f63691baf1017
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307235"
---
# <a name="deploy-a-secure-workstation"></a>Güvenli bir iş istasyonu dağıtma

Artık anladığınıza göre [önemli olan iş istasyonu erişiminin güvenliğini sağlama neden?](concept-azure-managed-workstation.md) mevcut araçları kullanarak dağıtım işlemini başlatmak için zamanı. Bu kılavuz, tanımlanmış profilleri başından daha güvenli bir iş istasyonu oluşturmak için kullanır.

![Güvenli bir iş istasyonu dağıtımı](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Çözümü dağıtmadan önce kullanacağınız profil seçilmelidir. Seçili profillerinden birini uygulayın ve ihtiyacınıza göre Intune profilinde atayarak diğerine unutulmaması önemlidir. Birden çok profili aynı anda bir dağıtımda kullanılan ve etiketin ya da Grup atamaları kullanarak atanmış.

| Profil | Düşük | Gelişmiş | Yüksek | Özelleştirilmiş | Güvenli | Yalıtılmış |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Azure AD'de kullanıcı | Evet | Evet | Evet | Evet | Evet | Evet |
| Intune ile yönetilen | Evet | Evet | Evet | Evet | Evet | Evet |
| Cihaz Azure AD'ye kayıtlı | Evet |  |  |  |  | |   |
| Azure AD'ye katılmış cihaz |   | Evet | Evet | Evet | Evet | Evet |
| Uygulanan Intune güvenlik temeli |   | Evet <br> (Gelişmiş) | Evet <br> (HighSecurity) | Evet <br> (NCSC) | Evet <br> (Güvenli) |  NA |
| Donanım güvenli Windows 10 standartları karşılar |   | Evet | Evet | Evet | Evet | Evet |
| Microsoft Defender ATP etkin |   | Evet  | Evet | Evet | Evet | Evet |
| Yönetici hakları kaldırma |   |   | Evet  | Evet | Evet | Evet |
| Microsoft Autopilot'ı kullanarak dağıtım |   |   | Evet  | Evet | Evet | Evet |
| Yalnızca Intune tarafından yüklenen uygulamalar |   |   |   | Evet | Evet |Evet |
| Yalnızca onaylanan listesine kısıtlı URL'leri |   |   |   | Evet | Evet |Evet |
| İnternet'e (gelen/engellenen Giden) |   |   |   |  |  |Evet |

## <a name="license-requirements"></a>Lisans gereksinimleri

Bu kılavuzda ele kavramları, Microsoft 365 Kurumsal E5 veya eşdeğer bir SKU varsayar. Bazı öneriler bu kılavuzdaki alt SKU'larıyla uygulanabilir. Ek bilgiler bulunabilir [Microsoft 365 Enterprise lisanslama](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Yapılandırmak istediğiniz [grup tabanlı lisanslama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) kullanıcılarınızın lisanslarını sağlamayı otomatikleştirin.

## <a name="azure-active-directory-configuration"></a>Azure Active Directory yapılandırması

Kullanıcılarınızın yönetecek, Azure Active Directory (Azure AD) dizininizin yapılandırma gruplandırır ve cihazlarının, yönetici iş istasyonları için gerektiren kimlik Hizmetleri ve özellikleri ile etkinleştirme bir [yönetici hesabı](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

Güvenli bir iş istasyonu yönetici hesabı oluşturduğunuzda, hesap için geçerli iş istasyonunuzu bırakıyorsunuz. Bu ilk yapılandırmayı ve bilinen ve güvenli bir CİHAZDAN tüm genel yapılandırma yapmanız önerilir. Kılavuzuna göz önünde bulundurun [kötü amaçlı yazılım bulaşmalarını önlemeye](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection) ilk saldırı ilk kez deneyiminden gösterme riskini azaltmak için.

Kuruluşlar çok faktörlü kimlik doğrulaması, yöneticileri için en az istemeniz gerekir. Bkz: [bulut tabanlı MFA dağıtma](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted) Uygulama Kılavuzu için.

### <a name="azure-ad-users-and-groups"></a>Azure AD kullanıcıları ve grupları

Azure portalından göz atın **Azure Active Directory** > **kullanıcılar** > **yeni kullanıcı**. [Güvenli bir iş istasyonu Kullanıcınızı oluşturma](https://docs.microsoft.com/Intune/quickstart-create-user), kimin cihaz yöneticinize olacaktır.

* **Ad** -güvenli yönetici iş istasyonu
* **Kullanıcı adı** - secure-ws-admin@identityitpro.com
* **Dizin rolü** - **sınırlı yönetici** ve aşağıdaki Yönetici rolü seçin
   * **Intune Yöneticisi**
* **Oluşturma**

İş istasyonu kullanıcıları için iki grup bir ve iş istasyonları kendileri için bir tane oluşturacağız. Azure portalından göz atın **Azure Active Directory** > **grupları** > **yeni Grup**

İş istasyonu kullanıcıları ilk grubu. Yapılandırmak istediğiniz [grup tabanlı lisanslama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) lisanslarını kullanıcılara sağlanmasını otomatikleştirmek için bu gruptaki kullanıcılar için.

* **Grup türü** -güvenlik
* **Grup adı** -güvenli iş istasyonu kullanıcıları
* **Üyelik türü** - atanan
* Güvenli bir iş istasyonu yönetici kullanıcı grubuna ekleyin.
   * secure-ws-admin@identityitpro.com
* Grubu için güvenli iş istasyonlarını yönetme başka bir kullanıcı ekleyebilirsiniz.
* **Oluşturma**

İkinci iş istasyonu cihazlar grubu.

* **Grup türü** -güvenlik
* **Grup adı** -güvenli iş istasyonları
* **Üyelik türü** - atanan
* **Oluşturma**

### <a name="azure-ad-device-configuration"></a>Azure AD cihaz yapılandırması

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Cihazları Azure AD'ye kimin katılabilir belirtin

Cihazlarınızı yapılandırmak Active Directory'de yönetim güvenliği grubunuzun cihazlar Etki Alanınızla birleştirin izin ayarlanamadı. Azure portalından bu ayarı yapılandırmak için Gözat **Azure Active Directory** > **cihazları** > **cihaz ayarları**. Seçin **seçili** altında **kullanıcılar cihazları Azure AD'ye Katıl** ve "Güvenli iş istasyonu kullanıcıları" grubu seçin.

#### <a name="removal-of-local-admin-rights"></a>Yerel yönetici hakları kaldırma

Piyasaya çıkma bir parçası olarak iş istasyonları kullanıcılar VIP, DevOps ve güvenli düzeyi iş istasyonları makinelerinde hiç yönetici haklarına sahip olacaktır. Azure portalından bu ayarı yapılandırmak için Gözat **Azure Active Directory** > **cihazları** > **cihaz ayarları**. Seçin **hiçbiri** altında **ek yerel Yöneticiler Azure AD alanına katılmış cihazlar**.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Cihazları eklemek için çok faktörlü kimlik doğrulaması gerektir

Cihazları Azure AD'ye katılma işlemini daha da güçlendirmek için Gözat **Azure Active Directory** > **cihazları** > **cihaz ayarları** seçin **Evet** altında **cihazları eklemek için multi-Factor Auth gerektir** ardından **Kaydet**.

#### <a name="configure-mdm"></a>MDM yapılandırma

Azure portalından göz atın **Azure Active Directory** > **Mobility (MDM ve MAM)**  > **Intune**. Ayarını **MDM kullanıcı kapsamı** için **tüm** ve **Kaydet** Biz bu senaryoda, Intune tarafından yönetilecek herhangi bir CİHAZDAN izin verdiği. Daha fazla bilgi makalesinde bulunabilir [Intune hızlı başlangıç: Windows 10 cihazları için otomatik kaydını ayarlama](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment). Intune, yapılandırma ve uyumluluk ilkeleri bir sonraki adımda oluşturacağız.

#### <a name="azure-ad-conditional-access"></a>Azure AD koşullu erişim

Azure AD koşullu erişim, uyumlu cihazlarda bu ayrıcalıklı yönetim görevlerini korunmasına yardımcı olabilirsiniz. Tanımladığımız üyeleri olarak kullanıcılar **güvenli iş istasyonu kullanıcıları** grubu gerekir uygulamaları bulut oturum açarken çok faktörlü kimlik doğrulaması gerçekleştirmek için. En iyi uygulama kılavuzunu izleyin ve bizim Acil Durum erişim hesapları ilkeden dışlamak ederiz. Makalesinde ek bilgiler bulunabilir [Azure AD'de Acil Durum erişim hesapları yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)

Azure portalında koşullu erişim yapılandırmak için Gözat **Azure Active Directory** > **koşullu erişim** > **yeni ilke**.

* **Ad** -güvenli cihaz İlkesi gerekli
* Atamalar
   * **Kullanıcılar ve gruplar**
      * Ekle - **kullanıcılar ve gruplar** - seçin **güvenli iş istasyonu kullanıcıları** daha önce oluşturduğunuz Grup
      * Dışlama - **kullanıcılar ve gruplar** -kuruluşunuzun Acil Durum erişim hesapları'nı seçin
   * **Bulut uygulamaları**
      * Ekle - **tüm bulut uygulamaları**
* Erişim denetimleri
   * **GRANT** - seçin **erişim ver** radyo düğmesi
      * **Çok faktörlü kimlik doğrulaması gerektir**
      * **Cihazın uyumlu olarak işaretlenmesini gerektir**
      * Birden fazla denetimin - **seçilen tüm denetimleri gerekli kıl**
* İlke - etkinleştirme **üzerinde**

Kuruluşlar, isteğe bağlı olarak kullanıcıların şirket kaynaklarına burada eriştiğiniz değil blok ülkelere ilkeleri oluşturabilirsiniz. IP konum tabanlı koşullu erişim ilkeleri hakkında daha fazla bilgi makalesinde bulunabilir [konum koşulu Azure Active Directory koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition)

## <a name="intune-configuration"></a>Intune yapılandırma

### <a name="configure-enrollment-status"></a>Kayıt durumu yapılandırma

Tedarik zinciri Yönetimi'nde belirtildiği gibi güvenli iş istasyonudur güvenilir temiz bir cihaz sağlama, cihazları Fabrika kümesine yeni cihazları, satın alırken önerilir [Windows 10 Pro S modunda](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode), maruz kalma riskinizi sınırlar ve tedarik zinciri yönetimi sırasında güvenlik açıklarını. Bir cihaz, tedarikçiden alındıktan sonra cihaza Autopilot'ı kullanarak S modundan kaldırılacak. Aşağıdaki yönergeler dönüştürme süreci uygulama hakkında ayrıntılar sağlar.

Cihaz kullanılmadan önce tam olarak yapılandırıldığından emin olmak istiyoruz. Intune için bir yer sağlar **tüm uygulamalar ve Profiller yüklenene kadar cihaz kullanımını engelle**. 

1. Gelen **Azure portalında** gidin:
1. **Microsoft Intune** > **cihaz kaydı** > **Windows kayıt** > **kayıt durumu sayfası (Önizleme)**  >  **Varsayılan** > **ayarları**.
1. Ayarlama **Göster uygulama profili yükleme ilerleme durumu** için **Evet**.
1. Ayarlama **tüm uygulamalar ve Profiller yüklenene kadar cihaz kullanımını engelle** için **Evet**.

### <a name="create-an-autopilot-deployment-profile"></a>Bir Autopilot dağıtım profili oluşturma

Bir cihaz grubu oluşturduktan sonra Autopilot cihazları yapılandırabilirsiniz. böylece dağıtım profili oluşturmanız gerekir.

1. Azure portalında ıntune'da, **cihaz kaydı** > **Windows kayıt** > **dağıtım profilleri**  >   **Profil oluşturma**.
1. Adı olarak **güvenli iş istasyonu Dağıtım profili**. Açıklama yazın **güvenli iş istasyonlarının dağıtımını**.
1. Dönüştürme tüm hedeflenen cihazların Autopilot Evet'e ayarlayın. Bu ayar, tüm cihazlar listesinde Autopilot dağıtım hizmetine kayıtlı emin olur.  Kaydın işlenmesi için 48 saat kadar bekleyin.
1. Dağıtım modu için seçin **Self (Önizleme) dağıtımı**. Bu profile sahip cihazların kullanıcı cihazı kaydetmeye ile ilişkilidir. Cihazı kaydetmek için kullanıcı kimlik bilgileri gerekir.
1. Azure AD'ye kutusunda birleştirme **Azure AD'ye katılmış** gri ve seçilen gerekir.
1. İlk çalıştırma deneyimi (OOBE) seçin, aşağıdaki seçenek ve diğerleri varsayılan değere ayarlayın ve ardından bırakın yapılandırma **Tamam**:
   1. Kullanıcı hesap türü: **Standart**
1. Profili oluşturmak için **Oluştur**’u seçin. Autopilot dağıtım profili artık cihazlara atanmak üzere hazırdır.
1. Seçin **atamaları** > **atama** > **seçilen gruplar**
   1. **Dahil edilecek grupları seçin** -güvenli iş istasyonu kullanıcıları

### <a name="configure-windows-update"></a>Windows Update'i yapılandırma

Bir kuruluşun en önemli yapabileceğiniz Windows 10 güncel biridir. Windows 10'da güvenli bir duruma korumak için iş istasyonlarına güncelleştirmelerinin uygulanacağı hızını yönetmek için bir güncelleştirme halkası dağıtacağız. Bu yapılandırma bulunabilir **Azure portalında** > **Intune** > **yazılım güncelleştirmelerini**  >  **Windows 10 güncelleştirme halkaları**.

Yapacağız **Oluştur** aşağıdaki ayarlarla yeni bir güncelleştirme halkası varsayılanlarından değiştirildi.

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

Tıklayın **Oluştur** ardından **atamaları** Sekme Ekle **güvenli iş istasyonları** dahil edilen bir grup olarak grup.

Windows Update ilkeleri hakkında daha fazla bilgi bulunabilir [ilke CSP'si - güncelleştirme](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update)

### <a name="windows-defender-atp-intune-integration"></a>Windows Defender ATP'yi Intune tümleştirmesi

Windows Defender ATP Intune güvenlik ihlallerini önlemeye yardımcı olmak için birlikte çalışır ve İhlallerin etkisini sınırlamaya Yardım. Gerçek zamanlı algılama özellikleri sağlar. ATP de bizim dağıtım kapsamlı denetim ve günlüğe kaydetme uç nokta cihazların sağlar.

Azure portalında Windows Defender ATP'yi Intune tümleştirmesini yapılandırmak için Gözat **Intune** > **cihaz uyumluluğu** > **Windows Defender ATP** .

1. Altında 1. adımda **Windows Defender ATP yapılandırma**, tıklayın **Intune Windows Defender Güvenlik Merkezi'nde Windows Defender ATP bağlanma**.
1. Windows Defender Güvenlik Merkezi:
   1. Seçin **ayarları** > **Gelişmiş Özellikler**
   1. İçin **Intune bağlantı**, seçin **üzerinde**
   1. Seçin **Tercihleri Kaydet**
1. Bağlantı kurulduktan sonra ' a tıklayın ve Intune ile iade **Yenile** en üstünde.
1. Ayarlama **Windows cihazları sürüm 10.0.15063 bağlanmak ve üstü için Windows Defender ATP** için **üzerinde**.
1. **Kaydet**

Makalesinde ek bilgiler bulunabilir [Windows Defender Gelişmiş tehdit koruması](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="completing-hardening-of-the-workstation-profile"></a>İş istasyonu profilinin sağlamlaştırma Tamamlanıyor

Çözümün sağlamlaştırma başarıyla tamamlamak için indirin ve istenen üzerinde tabanlı komut dosyası yürütme **profil düzeyi** aşağıdaki hesap.

| Profil | İndirme konumu | Dosya adı |
| --- | --- | --- |
| Düşük güvenlik | Yok |  Yok |
| Geliştirilmiş Güvenlik | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Yüksek güvenlik  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Özelleştirilmiş | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows 10 (1803) SecurityBaseline.ps1 |
| Özel uyumluluk * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Güvenli | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

Özel uyumluluk * NCSC Windows 10 SecurityBaseline içinde sağlanan özel yapılandırma zorlayan bir komut dosyasıdır.

Seçili komut dosyasını başarıyla yürütür sonra güncelleştirmeleri profilleri ve ilkeleri Intune yapılabilir. Betikler geliştirildi ve güvenli profilleri için ilke oluşturma ve profilleri, ancak için ilkeyi atama gerekir, **güvenli iş istasyonları** grubu.

* Intune cihaz yapılandırma profilleri betikler tarafından oluşturulan bulunabilir **Azure portalında** > **Intune** > **cihazyapılandırması**  >  **Profilleri**.
* Intune cihaz uyumluluk ilkeleri betikler tarafından oluşturulan bulunabilir **Azure portalında** > **Intune** > **cihaz uyumluluğu**  >  **İlkeleri**.

Değişiklikleri gözden geçirmek için de profiller dışarı aktarabilirsiniz ve SECCON belirtildiği gibi dışa aktarma dosyasına değişiklikler uygulanırken belgeleri ve ek sağlamlaştırma, dayalı gereklidir.

Intune veri çalışan betik dışarı `DeviceConfiguration_Export.ps1` gelen [cihaz yapılandırması GiuHub depo](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) tüm mevcut Intune profillerini geçerli verilmesini sağlar.

## <a name="additional-configurations-and-hardening-to-consider"></a>Ek yapılandırmalar ve dikkate alınması gereken sağlamlaştırma

Sağlanan yönergeler güvenli bir iş istasyonu etkin olduğundan, ek denetimler de olarak kabul edilmelidir, diğer tarayıcılar erişim gibi giden HTTP izin verilen ve Engellenen Web siteleri ve özel bir PowerShell Betiği çalıştırma olanağı.

### <a name="restrictive-inbound-and-outbound-rules-in-firewall-configuration-service-provider-csp"></a>Güvenlik Duvarı yapılandırma hizmet sağlayıcısı (CSP) kısıtlayıcı gelen ve giden kuralları

Ek yönetim hem gelen hem giden kuralları, izin verilen ve engellenen uç noktalarınızı yansıtacak şekilde güncelleştirilebilir. Güvenli iş istasyonu sağlamlaştırma devam ederken, biz tüm gelen ve giden varsayılan olarak izin verme kısıtlama gitme ve giden genel ve güvenilir web siteleri yansıtacak şekilde için izin verilen siteler ekleyin. Güvenlik Duvarı yapılandırma hizmet sağlayıcısı için ek yapılandırma bilgilerine makalesinde bulunabilir [güvenlik duvarı CSP](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp).

Kısıtlı varsayılan önerilerdir:

* Gelenlerin tümünü Reddet
* Gidenlerin tümünü Reddet

Kuruluşların siteler olarak bilinen engellemek için bir başlangıç noktası olarak kullanabileceği bir iyi sertika listesine Spamhaus proje tutar [etki alanı engelleme listesi (çift)](https://www.spamhaus.org/dbl/).

### <a name="managing-local-applications"></a>Yerel uygulamaları yönetme

Yerel uygulamaları kaldırma, temizleme üretkenlik uygulamaları dahil olmak üzere güvenli iş istasyonu gerçekten sağlamlaştırılmış bir duruma taşınır. Bizim örneğimizde, Chrome varsayılan tarayıcı olarak ekleyin ve ederiz eklentileri de dahil olmak üzere tarayıcı değiştirme becerisini kısıtlamak Intune kullanarak.

#### <a name="deploy-applications-using-intune"></a>Intune kullanarak uygulamaları dağıtma

Bazı durumlarda, Google Chrome tarayıcısının gibi uygulamaları güvenli iş istasyonu gereklidir. Aşağıdaki örnekte, Chrome güvenlik grubundaki cihazlara yüklemek için yönergeler sunulmaktadır **güvenli iş istasyonları** daha önce oluşturduğunuz.

1. Çevrimdışı yükleyiciyi indirmek [Windows 64‑bit Chrome paketi](https://cloud.google.com/chrome-enterprise/browser/download/)
1. Dosyaları ayıklayın ve konumunu not `GoogleChromeStandaloneEnterprise64.msi` Intune kullanarak yüklemek için
1. İçinde **Azure portalında** göz atın **Intune** > **istemci uygulamaları** > **uygulamaları**  >  **Ekleyin**
1. Altında **uygulama türü**, seçin **iş kolu satır**
1. Altında **uygulama paketi dosyası**seçin `GoogleChromeStandaloneEnterprise64.msi` ayıklanan konumu ve **Tamam**
1. Altında **uygulama bilgileri**, yayımcı ve açıklama sağlayın ve seçin **Tamam**
1. Tıklayın **Ekle**
1. Üzerinde **atamaları** seçin **kayıtlı cihazlar için kullanılabilir** altında **atama türü**
1. Altında **bulunan gruplarını**, ekleme **güvenli iş istasyonları** daha önce oluşturduğunuz Grup
1. Tıklayın **Tamam** ardından **Kaydet**

Chrome ayarlarını yapılandırma konusunda ek yönergeler kendi destek makalesinde bulunabilir [yönetme Chrome tarayıcısı Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Özel uygulamalar için şirket Portalı'nı yapılandırma

Güvenli bir modda uygulamaları yükleme Intune şirket Portalı'na sınırlanır. Ancak, Portalı'nı yükleme, Microsoft Store erişim gerektirir. Güvenli Çözümümüzü portalı kullanarak şirket Portalı'nın Çevrimdışı mod tüm cihazlar için kullanılabilir yapacağız.

Bir Intune yüklerken yönetilen kopyasını [Şirket portalı](https://docs.microsoft.com/Intune/store-apps-company-portal-app) ek araçlar isteğe bağlı olarak, güvenli iş istasyonları kullanıcılara anında iletme yeteneği izin verir.

Bazı kuruluşların Windows 32-bit uygulamaları veya diğer hazırlıkları dağıtmak için ihtiyacı olan uygulamaları yüklemek için gerekli. Bu uygulamalar için [Microsoft win32 içerik Hazırlık Aracı](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) kullanmaya hazır sağlayacak `.intunewin` yüklemesi için Biçim dosyası.

### <a name="setting-up-custom-settings-using-powershell"></a>PowerShell kullanarak özel ayarları ayarlama

Konak Yönetimi genişletilebilirlik sağlamak için PowerShell kullanarak bir örnek de kullanırız. Betik, konaktaki varsayılan arka plan ayarlayacaksınız. Bu yetenek profillerinde kullanılabilir ve yalnızca bir özelliği göstermek için kullanılır.

Çözümde güvenli iş istasyonlarında bazı özel denetimler ve ayarları ayarlamak için bir gereksinim olabilir. Bizim örneğimizde, biz kolayca güvenli iş istasyonu kullanıma hazır olarak cihazı tanımlamak için Powershell kullanarak iş istasyonu arka planını değiştirin. Bizim örneğimizde, bu görevi tamamlamak için PowerShell kullanacaksınız olsa da Azure portalında tamamlanabilir.

Bizim örneğimizde, aşağıdaki kullanacağı [ücretsiz genel arka plan resmi](https://i.imgur.com/OAJ28zO.png) ve [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) başlangıç arka planda yüklenecek Windows izin vermek için Microsoft Scripting Center öğesinden.

1. Yerel bir cihaza komut dosyası indir
1. CustomerXXXX ve arka plan dosya ve dağıtımı kullanmak istediğiniz klasör yansıtmak için betik kullanmak isteyen bir arka plan indirme konumunu güncelleştirin. Bizim örneğimizde, biz arka planlarına customerXXXX değiştirin.  
1. Gözat **Azure portalında** > **Intune** > **cihaz Yapılandırması** > **PowerShell betikleri** > **Ekle**
1. Sağlayan bir **adı** komut dosyasını belirtin **betik konumu** indirdiğiniz için
1. Seçin **yapılandırın**
   1. Ayarlama **oturum açmış kimlik bilgilerini kullanarak bu betiği çalıştırın**, **Evet**
   1. **Tamam**’a tıklayın.
1. **Oluştur**'a tıklayın.
1. Seçin **atamaları** > **grupları seçin**
   1. Güvenlik grubuna kullanıcı eklemeniz **güvenli iş istasyonları** tıklayın ve daha önce oluşturduğunuz **Kaydet**

### <a name="using-the-preview-mdm-security-baseline-for-october-2018"></a>Önizlemesi'ni kullanma: Ekim 2018 için MDM güvenlik temeli

Microsoft Intune Yöneticiler genel bir temel güvenlik duruşunu zorlamak için basit bir yol sağlayan güvenlik temel yönetim özelliğini kullanıma sundu. Temel bir kilitli geliştirilmiş profili iş istasyonunu elde etmek için benzer bir yol sağlar.

İçin güvenli iş istasyonu, uygulama bu taban çizgisi kullanılmaz güvenli yapılandırma dağıtımı ile çakışır.

|   |   |   |
| :---: | :---: | :---: |
| Kilit üzerinde | Cihaz yükleme | Uzak Masaüstü Hizmetleri |
| Uygulama çalışma zamanı | Cihaz kilidi | Uzaktan Yönetim |
| Uygulama Yönetimi | Olay Günlüğü hizmeti | Uzak yordam çağrısı |
| Otomatik Yürüt | Deneyimi | Ara |
| BitLocker | Exploit Guard | Akıllı Ekran |
| Tarayıcı | Dosya Gezgini | Sistem gereksinimi|
| Bağlantı | Internet Explorer | Wi-Fi |
| Kimlik temsilcisi | Yerel İlkeler Güvenlik Seçenekleri | Windows Bağlantı Yöneticisi |
| Kimlik bilgileri kullanıcı Arabirimi | MS Güvenlik Kılavuzu | Windows Defender|
| Veri Koruma | MSS eski | Windows Ink çalışma |
| Device Guard | Güç | Windows PowerShell |

Bu önizleme özelliği hakkında daha fazla bilgi makalesinde bulunabilir [Intune için Windows Güvenlik taban çizgisi ayarlarını](https://docs.microsoft.com/Intune/security-baseline-settings-windows).

## <a name="enroll-and-validate-your-first-device"></a>Kaydolma ve ilk Cihazınızı doğrula

1. Cihazınızı kaydetmek için aşağıdaki bilgiler gereklidir:
   * **Seri numarası** - cihaz kasa üzerinde bulunamadı
   * **Windows ürün kimliği** - altında bulunan **sistem** > **hakkında** Windows Ayarlar menüsünden.
   * Çalışan [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) CSV karma dosyası cihaz kaydı için tüm gerekli bilgileri sağlar. 
      * Çalıştırma `Get-WindowsAutoPilotInfo – outputfile device1.csv` bilgileri, Intune'a aktarılabilen bir CSV dosyası olarak çıkarmak için.

   > [!NOTE]
   > Komut dosyasını yükseltilmiş haklara gerektirir ve uzak olarak çalıştırmak imzalanmış. Doğru çalışması komut dosyasının çalışmasını sağlamak için aşağıdaki komutu kullanabilirsiniz. `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

   *  Bilgi toplamak için bir Windows 10 sürüm 1809 veya daha yüksek bir cihaza oturum açarak bu bilgiyi toplamak veya donanım satıcınızla yeni cihazları sıralanırken bu bilgiler sağlayabilir.
1. Gerekli bilgileri toplamak ve geri dönüp **Azure portalında**. Gidin **Intune** > **cihaz kaydı** > **Windows kayıt** > **cihazlar - Windows Autopilot cihazlarını yönetin**seçin **alma**, oluşturduğunuz veya sağlanan CSV dosyasını seçin.
1. Artık kayıtlı cihazı güvenlik grubuna ekleme **güvenli iş istasyonları** daha önce oluşturduğunuz.
1. Yapılandırmak istediğiniz Windows 10 cihazınızdan göz atın **Windows ayarları** > **güncelleştirme ve güvenlik** > **kurtarma**. Seçin **başlama** altında **bu PC sıfırlama** ve sıfırlama ve yapılandırılmış profili ve uyumluluk ilkelerini kullanarak cihaz yeniden yapılandırmak için istemleri izleyin.

Cihaz yapılandırıldıktan sonra bir gözden geçirmeyi tamamlayın ve yapılandırmayı kontrol edin. Dağıtımınıza devam etmeden önce ilk cihazın düzgün yapılandırıldığını onaylayın.

## <a name="assignment-and-monitoring"></a>Atama ve izleme

Cihaz ve kullanıcıları atama eşleşmesini gerektirir [Seçili profilleri](https://docs.microsoft.com/intune/device-profile-assign) grup ve hizmet izin verilen tüm yeni kullanıcılar güvenlik için de güvenlik grubuna eklenecek gerekli olacaktır.

Profilleri izleme yapılabilir izleme kullanarak [Intune profilleri](https://docs.microsoft.com/intune/device-profile-monitor).

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Intune](https://docs.microsoft.com/intune/index) belgeleri

[Azure AD](https://docs.microsoft.com/azure/active-directory/index) belgeleri