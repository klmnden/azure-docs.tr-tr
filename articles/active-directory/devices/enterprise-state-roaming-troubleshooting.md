---
title: Azure Active Directory'de Kurumsal durumda Dolaşım ayarları sorunlarını giderme | Microsoft Docs
description: BT yöneticileri bazı soruların yanıtlarını ayarları ve uygulama verilerini eşitleme hakkında olabilir sağlar.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: tanning
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4cceae17b06e8b631dd530b0408008a8222bccbf
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481863"
---
# <a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>Azure Active Directory'de Kurumsal durumda Dolaşım ayarları sorunlarını giderme

Bu konu, Kurumsal durumda Dolaşım ile ilgili sorunları tanılayın ve sorunlarını giderme hakkında bilgi sağlar ve bilinen sorunların bir listesini sağlar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="preliminary-steps-for-troubleshooting"></a>Sorun giderme için başlangıç adımları 

Sorunlarını gidermeye başlamadan önce kullanıcı ve cihaz düzgün yapılandırıldığından emin ve kurumsal durumda dolaşım, tüm gereksinimlerin cihaz ve kullanıcı tarafından karşılandığını doğrulayın. 

1. Windows 10, cihazda en son güncelleştirmeleri ve en düşük sürüm 1511 (işletim sistemi derleme 10586 veya sonraki bir sürümü) ile yüklenir. 
1. Cihaz, Azure AD alanına katılmış veya hibrit Azure AD'ye katıldı. Daha fazla bilgi için [, Azure AD denetimi altında bir aygıt alma](overview.md).
1. Emin **Kurumsal durumda Dolaşım** Kiracı için Azure AD'de açıklandığı etkin [Kurumsal durumda Dolaşım etkinleştirmek için](enterprise-state-roaming-enable.md). Tüm kullanıcılar için veya yalnızca seçili bir grup kullanıcı için gezici etkinleştirebilirsiniz.
1. Kullanıcı zaten bir Azure Active Directory Premium lisansı atanmış olması gerekir.  
1. Cihazın yeniden başlatılması gerekiyor ve kullanıcı yeniden Kurumsal durumda Dolaşım özelliklerine erişmek oturum açmanız gerekir.

## <a name="information-to-include-when-you-need-help"></a>Yardıma ihtiyacınız olduğunda eklenecek bilgiler
Aşağıdaki yönergeleri ile sorununuzu çözmek olamaz, destek mühendislerimizle başvurabilirsiniz. Bunları başvurduğunuzda, aşağıdaki bilgileri ekleyin:

* **Genel hata açıklamasını**: Hata iletisi kullanıcı tarafından görülen var mı? Hata iletisi varsa, ayrıntılı olarak fark beklenmeyen davranışı tanımlayın. Hangi özellikler eşitleme için etkindir ve hangi kullanıcının eşitlenecek bekleniyor mi? Birden çok özelliği değil eşitleniyor veya bir izole edilmiş?
* **Etkilenen kullanıcılar** – eşitleme çalışma/bir kullanıcı veya birden çok kullanıcı için başarısız olan? Kullanıcı başına cihaz sayısını rol oynayan? Tüm bunları değil eşitleniyor veya bunlardan bazıları eşitlemeye ve bazı eşitleniyor değil misiniz?
* **Kullanıcı hakkındaki bilgileri** – hangi aygıta bağlanmayı kullanarak kullanıcı kimliğidir? Nasıl kullanıcı cihaza oturum açmak için? Seçili güvenlik grubu eşitlemesine izin bir parçası olduklarından? 
* **Cihaz hakkındaki bilgileri** – bu cihazı Azure AD'ye katılmış veya etki alanına katılmış mı? Hangi derleme cihaz üzerinde mu? En son güncelleştirmeler nelerdir?
* **Tarih / saat / saat dilimi** – kesin tarih ve saat, gördüğünüz hata neydi (saat dilimi dahil)?

Bu bilgiler dahil olmak üzere mümkün olan en kısa sürede sorununuzu çözmenize yardımcı olur.

## <a name="troubleshooting-and-diagnosing-issues"></a>Sorunları tanılama ve giderme
Bu bölüm, Kurumsal durumda Dolaşım için ilgili sorunları tanılayın ve sorunlarını giderme hakkında öneriler sağlar.

## <a name="verify-sync-and-the-sync-your-settings-settings-page"></a>Eşitleme ve ayarları "ayarlarınızı eşitleme" Sayfa doğrulayın 

1. Windows 10 bilgisayarınıza Kurumsal durumda Dolaşım izin verecek şekilde yapılandırılmış bir etki alanına katıldıktan sonra iş hesabınızla oturum açın. Git **ayarları** > **hesapları** > **eşitleme ayarlarınızı** ve eşitleme ve ayrı ayrı ayarlar üzerinde olduğundan emin olun ve üst Ayarlar sayfasında, iş hesabınızla eşitleniyor gösterir. Oturum açma hesabınız olarak de aynı hesabı kullanılır onaylayın **ayarları** > **hesapları** > **bilgisayarınızı bilgisi**. 
1. Ekranın sağ veya üst kenarına görev taşımak gibi özgün makine üzerinde bazı değişiklikler yaparak eşitleme birden çok makinede çalıştığını doğrulayın. Beş dakika içinde ikinci bir makinede yaymak değişiklik izleyin. 

   * Kilitleme ve kilit ekranı (Win + L), bir eşitleme tetikleme yardımcı olabilir.
   * Kurumsal durumda Dolaşım kullanıcı hesabı ve makine hesabı için bağlı gibi çalışmaya – her iki bilgisayar eşitleme için aynı hesapla oturum gerekir.

**Olası sorun**: Varsa denetimlerinde **ayarları** sayfa kullanılabilir değil ve "bazı Windows özellikleri yalnızca bir Microsoft hesabı veya iş hesabı kullanıyorsanız kullanılabilir." iletisini görürsünüz Şekilde ayarlamanız cihazlarda Bu sorun ortaya çıkabilir etki alanına katılmış ve Azure AD'ye kayıtlı ancak cihaz henüz başarılı bir şekilde Azure AD'ye kimlik doğrulaması. Olası bir nedeni, cihaz İlkesi uygulanmış olması gerekir, ancak bu uygulama, zaman uyumsuz olarak yapılır ve olarak birkaç saat Gecikmeli olmasıdır. 

### <a name="verify-the-device-registration-status"></a>Cihaz kayıt durumu doğrulayın

Kurumsal durumda dolaşım, cihazın Azure AD'ye kayıtlı olması gerekir. Kurumsal durumda Dolaşım için özel olmayan olsa da aşağıdaki yönergeleri izleyerek Windows 10 istemci kayıtlı onaylamak parmak izi, Azure AD ayarları URL'sini NGC durum olduğunu onaylayın ve yardımcı olabilir ve diğer bilgiler.

1. Uzantıda yükseltilmemiş komut istemini açın. Windows bunu yapmak için çalışma Başlatıcısı (Win + R) açın ve "cmd" yazın açın.
1. Komut istemi açıldıktan sonra yazın "*dsregcmd.exe/status*".
1. Beklenen Çıktı **AzureAdJoined** alan değeri olmalıdır. "Evet" **WamDefaultSet** alan değeri olmalıdır. "Evet" ve **WamDefaultGUID** alan değerinin bir GUID olmalıdır "(AzureAd)" ile sonunda.

**Olası sorun**: **WamDefaultSet** ve **AzureAdJoined** hem "Hayır" alan değeri olması, cihaz etki alanına katılmış ve Azure AD ile kaydedilen ve cihaz eşitleme. Bu gösteriyor, cihaz ilkesinin uygulanması için beklemeniz gerekebilir veya Azure AD'ye bağlanma cihaz için kimlik doğrulaması başarısız oldu. Kullanıcı, uygulanacak ilke için birkaç saat beklemeniz gerekebilir. Diğer sorun giderme adımları kapatıp arka planda otomatik kaydı yeniden denemeden veya Görev Zamanlayıcı görevi başlatma içerebilir. Bazı durumlarda, çalışan "*dsregcmd.exe /leave*" Bu sorunla yeniden başlatarak ve kaydı yeniden denemeden bir yükseltilmiş komut istemi penceresinde yardımcı olabilir.

**Olası sorun**: Alan için **SettingsUrl** boştur ve cihazı eşitleyebilir değil. Kurumsal durumda Dolaşım Azure Active Directory portalında etkinleştirilmeden önce kullanıcının son cihaza oturum açmış. Cihazı yeniden başlatın ve kullanıcı oturum açma bilgileriniz yok. İsteğe bağlı olarak, portalda, BT yöneticisi gitmek zorunda deneyin **Azure Active Directory** > **cihazları** > **Kurumsal durumda Dolaşım** devre dışı bırakıp yeniden etkinleştirin **kullanıcılar eşitleme ayarları ve uygulama verilerini cihazlarda**. Yeniden etkinleştirildikten sonra cihazı yeniden başlatın ve kullanıcı oturum açma bilgileriniz yok. Bu sorunu çözmezse **SettingsUrl** söz konusu olduğunda hatalı cihaz sertifika boş olabilir. Bu durumda, çalışan "*dsregcmd.exe /leave*" Bu sorunla yeniden başlatarak ve kaydı yeniden denemeden bir yükseltilmiş komut istemi penceresinde yardımcı olabilir.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>Kurumsal durumda Dolaşım ve çok faktörlü kimlik doğrulaması 

Belirli koşullar altında Azure multi-Factor Authentication yapılandırılmışsa veri eşitlemesine izin Kurumsal durumda Dolaşım devredebilirsiniz. Destek belgesi bu belirtiler ilgili ek ayrıntılar için bkz [KB3193683](https://support.microsoft.com/kb/3193683). 

**Olası sorun**: Cihazınız Azure Active Directory portalında çok faktörlü kimlik doğrulamasını gerektirecek şekilde yapılandırılmışsa, ayarları bir Windows 10 cihazda bir parola kullanarak oturum açma sırasında eşitlenecek başarısız olabilir. Bu tür bir multi-Factor Authentication yapılandırmasına, Azure yönetici hesabı korumak için tasarlanmıştır. Yönetici kullanıcıların Windows 10 cihazlarını kendi Microsoft Passport for Work PIN oturum açarak veya Office 365 gibi diğer Azure hizmetlerine erişirken multi-Factor Authentication tamamlayarak eşitleyebildiklerinden olabilir.

**Olası sorun**: Eşitleme, yönetim Active Directory Federasyon Hizmetleri çok faktörlü kimlik doğrulaması koşullu erişim ilkesini yapılandırır ve cihazın erişim belirtecinin süresi başarısız olabilir. Office 365 gibi diğer Azure hizmetlerine erişirken çok faktörlü kimlik doğrulamasını tamamlamak ya da oturum açın ve Microsoft Passport for Work PIN kullanarak oturumunuzu emin olun.

### <a name="event-viewer"></a>Olay Görüntüleyicisi

Gelişmiş sorun giderme için Olay Görüntüleyicisi'ni belirli hataları bulmak için kullanılabilir. Bunlar aşağıdaki tabloda belirtilmiştir. Olayları Olay Görüntüleyicisi'ni altında bulunabilir > Uygulama ve hizmet günlükleri > **Microsoft** > **Windows** > **SettingSync Azure** ve kimlikle ilgili sorunları ile eşitleme için **Microsoft** > **Windows** > **AAD**.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>MDM yazılımınız aracılığıyla dışarıdan yüklenen uygulamaların bulunduğu cihazlara eşitleme çalışmıyor

Windows 10 Yıldönümü Güncelleştirmesi (sürüm 1607) çalıştıran cihazlar etkiler. Azure SettingSync günlükleri altındaki Olay Görüntüleyicisi'nde olay kimliği 6013 80070259 hatasıyla sık görülür.

**Önerilen eylem**  
Windows 10 v1607 istemci 23 Ağustos 2016 sahip olduğundan emin olun toplu güncelleştirme ([KB3176934](https://support.microsoft.com/kb/3176934) işletim sistemi derleme 14393.82). 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Internet Explorer sık kullanılanları Eşitle değil

Windows 10 Kasım Güncelleştirmesi (sürüm 1511) çalıştıran cihazlar etkiler.

**Önerilen eylem**  
Windows 10 v1511 istemci Temmuz 2016 sahip olduğundan emin olun toplu güncelleştirme ([KB3172985](https://support.microsoft.com/kb/3172985) işletim sistemi derleme 10586.494).

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Tema, Windows Information Protection ile korunan verileri yanı sıra eşitleniyor değil 

İle korunan veriler, veri sızıntısını önlemek için [Windows Information Protection](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) Kurumsal durumda Dolaşım Windows 10 Yıldönümü Güncelleştirmesi'ni kullanan cihazlar için eşitlenmez.

**Önerilen eylem**  
Yok. Windows için Gelecekteki güncelleştirmelerin bu sorunu çözebilir.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Tarih, saat ve bölge ayarları etki alanına katılmış cihazda eşitliyor musunuz 
  
Etki alanına katılmış olan cihazlar değil deneyimi tarih ayarı için eşitleme, saat ve bölge: otomatik saat. Otomatik saat kullanarak diğer tarih, saat ve bölge ayarları geçersiz kılar ve eşitleme bu ayarları neden. 

**Önerilen eylem**  
Yok. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>Parolaları eşitlerken UAC istemleri

Windows 10 Kasım Güncelleştirmesi (sürüm 1511) çalıştıran cihazlar, parolalarını eşitlemek için yapılandırılan kablosuz bir NIC ile etkiler.

**Önerilen eylem**  
Windows 10 v1511 istemcisi toplu güncelleştirme sahip olduğundan emin olun ([KB3140743](https://support.microsoft.com/kb/3140743) işletim sistemi derleme 10586.494).

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Akıllı kart oturum açma için kullanan cihazlarda eşitleme çalışmıyor

Windows cihazınıza bir akıllı kart ya da sanal akıllı kart kullanarak oturum açmak çalışırsanız, ayarları eşitleme çalışmayı durdurur.     

**Önerilen eylem**  
Yok. Windows için Gelecekteki güncelleştirmelerin bu sorunu çözebilir.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Etki alanına katılmış cihaz şirket ağına bırakarak sonra eşitleniyor değil     

Etki alanına katılan cihazları Azure AD'ye kayıtlı, cihaz uzun süre için şirket dışı ise ve etki alanı kimlik doğrulaması tamamlanamıyor eşitleme hatayla karşılaşabilir.

**Önerilen eylem**  
Eşitleme devam edebilmeniz için cihazın şirket ağına bağlayın.

---

### <a name="azure-ad-joined-device-is-not-syncing-and-the-user-has-a-mixed-case-user-principal-name"></a>Azure AD katıldı cihaz değil eşitleme ve kullanıcı karışık bir servis talebi kullanıcı asıl adı.

Karma bir durumda (örneğin kullanıcı adı yerine kullanıcıadı) UPN kullanıcının sahip ve Windows 10 derleme 10586 14393 için yükseltti bir Azure AD katıldı cihazında kullanıcı ise, kullanıcının cihaz eşitleme başarısız olabilir. 

**Önerilen eylem**  
Kullanıcı ayrılma ve CİHAZDAN buluta katılabilir gerekecektir. Bu, yerel yönetici kullanıcı olarak oturum açın ve cihaz giderek ayrılma **ayarları** > **sistem** > **hakkında** ve "Yönet'i seçin veya iş yeri veya okuldan bağlantıyı kes". Aşağıdaki dosyaları ve Azure AD'ye katılımı daha sonra yeniden cihazı temizlemek **ayarları** > **sistem** > **hakkında** tıklayıp "işe Bağlan veya Okul". Cihaz Azure Active Directory'ye katılmasını ve akışını tamamlamak devam edin.

Temizleme adımda, aşağıdaki temizleme dosyaları:
- İçinde Settings.dat `C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Klasör altındaki tüm dosyaları `C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>Olay Kimliği 6065: 80070533 bu hesap şu anda devre dışı bırakıldığından bu kullanıcı oturum açamaz  

Kullanıcının kimlik bilgilerinin süresi dolmuş olduğunda bu hata SettingSync/hata ayıklama günlükleri altındaki Olay Görüntüleyicisi'nde görülebilir. Ayrıca, Kiracı otomatik olarak sağlanan AzureRMS olmadığı oluşabilir. 

**Önerilen eylem**  
Bu durumda, kullanıcının kendi kimlik bilgilerini ve oturum açma için yeni kimlik bilgileriyle cihazda güncelleştirme vardır. AzureRMS sorunu çözmek için listelenen adımlarla devam edin [KB3193791](https://support.microsoft.com/kb/3193791). 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>Olay Kimliği 1098: Hata: 0xCAA5001C belirteci Aracısı işlemi başarısız oldu  

Günlükleri AAD/Operational altındaki Olay Görüntüleyicisi'nde bu hata ile olay 1104 görülebilir: AAD bulut AP eklentisi çağrı alma belirteci döndürülen hata: 0xC000005F. İzinleri veya sahipliği öznitelikler eksik durumunda bu sorun oluşur.  

**Önerilen eylem**  
Listelenen adımlarla devam edin [KB3196528](https://support.microsoft.com/kb/3196528).  

## <a name="next-steps"></a>Sonraki adımlar

Genel bakış için bkz. [Kurumsal durumda dolaşıma genel bakış](enterprise-state-roaming-overview.md).
