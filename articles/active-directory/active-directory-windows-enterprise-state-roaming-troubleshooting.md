---
title: Azure Active Directory'de Kurumsal durumda Dolaşım ayarları sorunlarını giderme | Microsoft Docs
description: BT yöneticileri bazı sorulara yanıtlar ayarları ve uygulama veri eşitleme hakkında olabilir sağlar.
services: active-directory
keywords: Kurumsal durum Dolaşım ayarları, windows bulut Kurumsal durumda Dolaşım hakkında sık sorulan sorular
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: f45d0515-99f7-42ad-94d8-307bc0d07be5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: tanning
ms.custom: it-pro
ms.openlocfilehash: 25660eef50a0a18d4f404944daeb443133424897
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261160"
---
# <a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>Sorun giderme Azure Active Directory'de Kurumsal durumda Dolaşım ayarları

Bu konu, Kurumsal durumda Dolaşım ile ilgili sorunları tanılamak ve gidermek nasıl hakkında bilgiler sağlar ve bilinen sorunların bir listesini sağlar.

## <a name="preliminary-steps-for-troubleshooting"></a>Sorun giderme için başlangıç adımları 
Sorunlarını gidermeye başlamadan önce kullanıcı ve aygıt düzgün yapılandırıldığını ve tüm gereksinimlerini Kurumsal durumda Dolaşım cihaz ve kullanıcı tarafından karşılandığından doğrulayın. 

1. Windows 10, cihazda en son güncelleştirmeleri ve en düşük sürüm 1511 (işletim sistemi yapı 10586 veya sonrası) ile birlikte yüklenir. 
2. Cihaz, Azure AD alanına veya karma Azure AD alanına katılmış. Daha fazla bilgi için bkz: [Azure ad denetimindeki bir aygıtı alma](device-management-introduction.md).
3. Emin **Kurumsal durumda Dolaşım** bir kiracı için Azure AD içinde açıklandığı şekilde etkinleştirilip [Kurumsal durumda Dolaşım etkinleştirmek için](active-directory-windows-enterprise-state-roaming-enable.md). Tüm kullanıcılar için veya yalnızca seçilen bir grup kullanıcı için gezici etkinleştirebilirsiniz.
4. Kullanıcı zaten bir Azure Active Directory Premium lisansı atanmış olması gerekir.  
25. Cihaz yeniden başlatılması gerekiyor ve kullanıcının kurumsal durumda Dolaşım özelliklerine erişmek yeniden imzalamalısınız.

## <a name="information-to-include-when-you-need-help"></a>Yardıma ihtiyacınız olduğunda dahil edilecek bilgileri
Aşağıdaki yönergeleri ile sorunu çözemiyorsanız, bizim destek mühendisleri başvurabilirsiniz. Bunları başvurduğunuzda, aşağıdaki bilgileri içerir:

* **Genel hata açıklaması**: kullanıcı tarafından görülen hata iletileri vardır? Hata iletisi varsa, ayrıntılı olarak fark beklenmeyen davranışları açıklanmaktadır. Hangi özellikler eşitleme için etkindir ve ne eşitlenmesi bekleniyor kullanıcı nedir? Birden çok özelliği olmayan eşitleniyor veya bir yalıtılmış olduğundan?
* **Etkilenen kullanıcılar** – çalışma/başarısız olan bir kullanıcı veya birden çok kullanıcı için eşitleme olduğu? Her kullanıcı kaç cihaz oynayan? Bunları değil eşitleniyor tümü veya bunlardan bazıları eşitleniyor ve bazı eşitleniyor değil misiniz?
* **Kullanıcı hakkındaki bilgileri** – hangi aygıta oturum açmak için kullanarak kullanıcı kimliğidir? Nasıl kullanıcı cihaza oturum? Bunlar eşitlemesine izin Seçili güvenlik grubunun parçası misiniz? 
* **Aygıt hakkındaki bilgileri** – bu cihazı Azure AD alanına katılmış veya etki alanına katılmış mı? Hangi derleme cihaz üzerinde mu? En son güncelleştirmeleri nelerdir?
- **Tarih / saat / saat dilimi** – kesin bir tarih ve saat hata gördüğünüz neydi (saat dilimi dahil)?

Bu bilgiler dahil olmak üzere mümkün olan en kısa sürede sorunu çözmenize yardımcı olur.

## <a name="troubleshooting-and-diagnosing-issues"></a>Sorunları tanılama ve giderme
Bu bölümde, Kurumsal durumda Dolaşım için ilgili sorunları tanılamak ve gidermek nasıl hakkında öneriler sağlar.

## <a name="verify-sync-and-the-sync-your-settings-settings-page"></a>Eşitleme ve "ayarlarınızı eşitleyin" Ayarları sayfasına doğrulayın 

1. Windows 10 bilgisayarınıza Kurumsal durumda Dolaşım izin verecek şekilde yapılandırılmış bir etki alanına katıldıktan sonra iş hesabınızla oturum açma. Git **ayarları** > **hesapları** > **eşitleme ayarlarınızı** ve eşitleme ve ayrı ayrı ayarlar olan ve sayfanın üst kısmındaki ayarlarını belirten iş hesabınız ile eşitleniyor onaylayın. Aynı hesap, oturum açma hesabı olarak da kullanılır onaylayın **ayarları** > **hesapları** > **bilgisayarınızı bilgisi**. 
2. Ekranın sağ üst veya üst tarafındaki görev taşıma gibi özgün makine üzerinde bazı değişiklikler yaparak eşitleme birden fazla makine arasında çalıştığını doğrulayın. Değişiklik izleme beş dakika içinde ikinci makineye yayılır. 
  * Kilitleme ve kilit ekranı (Win + L) bir eşitlemesi yardımcı olabilir.
  * Kurumsal durumda Dolaşım kullanıcı hesabı ve makine hesabı için bağlı olarak çalışmak için – hem bilgisayarları eşitleme için aynı hesabıyla imzalama gerekir.

**Olası sorun**: varsa denetimlerinde **ayarları** sayfa kullanılabilir değildir ve "bazı Windows özellikleri yalnızca bir Microsoft hesabı veya iş hesabı kullanıyorsanız, kullanılabilir." iletisini görürsünüz Bu sorun için ayarlamanız cihazlar için doğabilecek etki alanına katılmış ve Azure ad kayıtlı ancak aygıt henüz başarılı bir şekilde Azure AD ile kimlik doğrulaması. Olası bir neden, aygıt İlkesi uygulanmış olması gerekir, ancak bu uygulama zaman uyumsuz olarak olur ve tarafından birkaç saat Gecikmeli ' dir. 

### <a name="verify-the-device-registration-status"></a>Cihaz kayıt durumunu doğrulayın
Kurumsal durumda Dolaşım aygıt Azure AD ile kayıtlı olması gerekir. Özel olmayan kurumsal durumda Dolaşım için de, aşağıdaki yönergeleri izleyerek Windows 10 istemci kayıtlı ve parmak izi, Azure AD ayarları URL'si NGC'ye durumunu onaylayın yardımcı olabilir ve diğer bilgileri.

1.  Yetkisiz komut istemi açın. Bu Windows yapmak için çalışma Başlatıcısı (Win + R) açın ve "cmd" yazın açın.
2.  Komut istemi açıldıktan sonra türü "*dsregcmd.exe/status*".
3.  Beklenen çıktı için **AzureAdJoined** alan değeri, "Evet" olmalıdır **WamDefaultSet** alan değeri, "Evet" olmalıdır ve **WamDefaultGUID** alan değeri sonunda "(Azuread'i)" ile bir GUID olması gerekir.

**Olası sorun**: **WamDefaultSet** ve **AzureAdJoined** hem "Hayır" alan değerine sahip, cihazın etki alanına katılmış ve Azure AD ile kaydedilen ve cihaz eşitleme. Bu gösteriyor, cihaz ilkesinin uygulanması için beklemeniz gerekebilir veya Azure AD'ye bağlanma cihaz için kimlik doğrulaması başarısız oldu. Kullanıcı gruplarına ilke için birkaç saat beklemeniz gerekebilir. Diğer sorun giderme adımları oturumunuzu ve arka planda otomatik kaydı yeniden deneniyor veya Görev Zamanlayıcı görevi başlatılıyor olabilir. Bazı durumlarda, çalışan "*dsregcmd.exe /leave*" ile bu sorunu yeniden başlatılıyor ve kayıt yeniden deneme bir yükseltilmiş komut istemi penceresinde yardımcı olabilir.


**Olası sorun**: alan için **AzureAdSettingsUrl** boş ve aygıtı eşitleme değil. Kurumsal durumda Dolaşım Azure Active Directory portalında etkinleştirilmeden önce kullanıcı son cihaza oturum açmış. Cihaz yeniden başlatın ve kullanıcı oturum açma sahip. İsteğe bağlı olarak, portalda, BT devre dışı bırakın ve kullanıcıların olabilir eşitleme ayarları ve kurumsal uygulama verileri yeniden etkinleştirmek yönetici sahip deneyin. Yeniden etkinleştirildiğinde, aygıt yeniden başlatma ve kullanıcı oturum açma sahip. Bu sorunu çözmezse **AzureAdSettingsUrl** hatalı aygıt sertifika durumunda boş olabilir. Bu durumda, çalışan "*dsregcmd.exe /leave*" ile bu sorunu yeniden başlatılıyor ve kayıt yeniden deneme bir yükseltilmiş komut istemi penceresinde yardımcı olabilir.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>Kurumsal durumda Dolaşım ve çok faktörlü kimlik doğrulaması 
Belirli koşullar altında Kurumsal durumda Dolaşım Azure çok faktörlü kimlik doğrulaması yapılandırılmışsa, verileri eşitlemek başarısız olabilir. Bu belirtiler hakkında daha fazla ayrıntı için Destek belgesine bakın [KB3193683](https://support.microsoft.com/kb/3193683). 

**Olası sorun**: Cihazınızı çok faktörlü kimlik doğrulaması Azure Active Directory portalında gerektirecek şekilde yapılandırılmışsa, bir Windows 10 cihazda bir parola kullanarak oturum açma sırasında ayarlarını eşitlemek başarısız olabilir. Bu tür bir çok faktörlü kimlik doğrulamasını yapılandırma, Azure yönetici hesabı korumak için tasarlanmıştır. Yönetici kullanıcılar hala Windows 10 cihazlarını kendi Microsoft Passport for Work PIN oturum açma veya Office 365 gibi diğer Azure hizmetleriyle erişirken çok faktörlü kimlik doğrulamasını tamamlayan eşitleyemez olabilir.

**Olası sorun**: yönetici Active Directory Federasyon Hizmetleri çok faktörlü kimlik doğrulaması koşullu erişim ilkesini yapılandırır ve cihazın erişim belirtecinin süresi eşitleme başarısız olabilir. Oturum açın ve Microsoft Passport for Work PIN kullanarak oturumu emin olun veya Office 365 gibi diğer Azure hizmetleriyle erişirken çok faktörlü kimlik doğrulaması gerçekleştirmez.

### <a name="event-viewer"></a>Olay Görüntüleyici
Gelişmiş sorun giderme için Olay Görüntüleyicisi'ni belirli hataları bulmak için kullanılabilir. Bunlar aşağıdaki tabloda belirtilmiştir. Olayları Olay Görüntüleyicisi'ni altında bulunabilir > Uygulama ve hizmet günlükleri > **Microsoft** > **Windows** > **SettingSync** ve Eşitleme kimlikle ilgili sorunlar **Microsoft** > **Windows** > **AAD**.


## <a name="known-issues"></a>Bilinen sorunlar

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>Eşitleme MDM yazılım kullanarak dışarıdan yüklenen uygulamaların bulunduğu cihazlara çalışmıyor

Windows 10 Anniversary güncelleştirme (sürüm 1607) çalıştıran aygıtları etkiler. SettingSync Azure günlükleri altında Olay Görüntüleyicisi'nde olay kimliği 6013 80070259 hatasıyla sık görülür.

**Önerilen eylem**  
Windows 10 v1607 istemci 23 Ağustos 2016 sahip olduğundan emin olun toplu güncelleştirme ([KB3176934](https://support.microsoft.com/kb/3176934) OS yapı 14393.82). 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Internet Explorer Sık Kullanılanlar eşitleme değil

Windows 10 Kasım güncelleştirme (sürüm 1511) çalıştıran aygıtları etkiler.

**Önerilen eylem**  
Windows 10 v1511 istemci Temmuz 2016 sahip olduğundan emin olun toplu güncelleştirme ([KB3172985](https://support.microsoft.com/kb/3172985) OS yapı 10586.494).

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Tema, Windows Information Protection ile korunan verileri yanı sıra eşitlemiyor 

Veri sızıntısı, ile korunan verileri önlemek için [Windows bilgi koruması](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) Kurumsal durumda Dolaşım Windows 10 Anniversary Update kullanarak cihazları için eşitlenmez.



**Önerilen eylem**  
Yok. Windows için gelecekteki güncelleştirmeleri bu sorun çözülebilir.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Tarih, saat ve bölge ayarları etki alanına katılmış aygıtta eşitliyor musunuz 
  
Etki alanına katılmış aygıtlar eşitleme tarihi ayarı için saat ve bölge değil yararlanır: otomatik saat. Otomatik saat kullanarak diğer tarih, saat ve bölge ayarları geçersiz kılar ve eşitleme bu ayarları neden. 

**Önerilen eylem**  
Yok. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>UAC parolaları eşitlerken ister

Windows 10 Kasım güncelleştirme (sürüm 1511) çalıştıran cihazlarda parola eşitleme için yapılandırılmış bir kablosuz NIC ile etkiler.

**Önerilen eylem**  
Windows 10 v1511 istemcisi toplu güncelleştirme olduğundan emin olun ([KB3140743](https://support.microsoft.com/kb/3140743) OS yapı 10586.494).

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Akıllı kart oturum açma kullanan cihazları eşitleme çalışmıyor
Windows Cihazınızı bir akıllı kart veya sanal akıllı kart kullanarak oturum açmaya çalışırsanız ayarları eşitleme çalışmayı durdurur.     

**Önerilen eylem**  
Yok. Windows için gelecekteki güncelleştirmeleri bu sorun çözülebilir.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Etki alanına katıldı cihazın şirket ağına bırakarak sonra eşitlemiyor     
Etki alanına katılmış cihazları Azure AD'ye kayıtlı cihaz uzun süre için şirket dışı ise ve etki alanı kimlik doğrulaması tamamlayamıyor eşitleme hatası karşılaşabilirsiniz.

**Önerilen eylem**  
Eşitleme devam edebilmeniz için cihazın şirket ağına bağlayın.

---

 ### <a name="azure-ad-joined-device-is-not-syncing-and-the-user-has-a-mixed-case-user-principal-name"></a>Azure AD katılmış aygıt değil eşitleniyor ve kullanıcının karışık bir servis talebi kullanıcı asıl adı.
 Kullanıcının sahip olduğu bir büyük/küçük harf UPN (örn. kullanıcı adı yerine UserName) ve Windows 10 derleme 10586 için 14393 yükseltti bir Azure AD katılmış aygıtında kullanıcı, kullanıcı cihazının eşitleme başarısız olabilir. 

**Önerilen eylem**  
Kullanıcı ayrılma ve bulut aygıta yeniden katılmasına gerekecektir. Bu, yerel yönetici kullanıcı olarak oturum açın ve giderek cihaz ayrılma **ayarları** > **sistem** > **hakkında** seçip "Yönet veya iş veya Okul bağlantısını kesin.". Aşağıdaki dosyaları Azure AD katılım daha sonra yeniden aygıtı Temizleme **ayarları** > **sistem** > **hakkında** ve "Bağlanmak için iş veya Okul" seçme. Cihaz Azure Active Directory'ye katılmasını ve akış tamamlamak devam edin.

Temizleme adımda Temizleme aşağıdaki dosyaları:
- İçinde Settings.dat `C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Klasör altındaki tüm dosyalar `C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>Olay Kimliği 6065:80070533 bu hesap şu anda devre dışı bırakıldığı için bu kullanıcı oturum açamaz  
Kullanıcının kimlik bilgilerinin süresi dolmuş olduğunda bu hata SettingSync/hata ayıklama günlükleri altında Olay Görüntüleyicisi'nde görülebilir. Ayrıca, Kiracı sağlanan AzureRMS otomatik olarak sahip olduğunda oluşabilir. 

**Önerilen eylem**  
İlk durumda, yeni kimlik bilgileri aygıta kendi kimlik bilgilerini ve oturum açma güncelleştirme kullanıcı sahip. AzureRMS sorunu çözmek için listelenen adımları devam [KB3193791](https://support.microsoft.com/kb/3193791). 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>Olay Kimliği 1098: Hata: 0xCAA5001C belirteci Aracısı işlemi başarısız oldu  
Bu hata ile olay 1104 AAD/Operational günlükleri altındaki Olay Görüntüleyicisi'nde görülebilir: AAD bulut AP eklentisi çağrı Get belirteci hata döndürdü: 0xC000005F. İzinleri veya sahipliği öznitelikler var. eksik ise bu sorun oluşur.  

**Önerilen eylem**  
Listelenen adımlarla devam [KB3196528](https://support.microsoft.com/kb/3196528).  



## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [kullanıcı sesi Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/category/158658-enterprise-state-roaming) görüş ve öneriler Kurumsal durumda Dolaşım geliştirme hakkında yapın.

- Daha fazla bilgi için bkz: [Kurumsal durumda Dolaşım genel bakış](active-directory-windows-enterprise-state-roaming-overview.md). 

## <a name="related-topics"></a>İlgili konular
* [Kurumsal durum gezici genel bakış](active-directory-windows-enterprise-state-roaming-overview.md)
* [Azure Active Directory'de gezici Kurumsal durumunu etkinleştir](active-directory-windows-enterprise-state-roaming-enable.md)
* [Ayarlar ve veri dolaşımı SSS](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Grup İlkesi ve ayarları eşitleme için MDM ayarları](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 Dolaşım ayarları başvurusu](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
