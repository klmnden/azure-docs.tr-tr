---
title: Ayarlar ve veri dolaşımı hakkında SSS | Microsoft Docs
description: BT yöneticileri bazı soruların yanıtlarını ayarları ve uygulama verilerini eşitleme hakkında olabilir sağlar.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f9270aff6bc2aab7e210716ffe3e21efb07b8ed
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481965"
---
# <a name="settings-and-data-roaming-faq"></a>Ayarlar ve veri dolaşımı hakkında SSS
Bu makalede, BT yöneticileri, ayarları ve uygulama verilerini eşitleme hakkında olabilir. bazı sorular yanıtlanmaktadır.

## <a name="what-data-roams"></a>Hangi verilerin Dolaşım yapar?
**Windows ayarları**: Windows işletim sisteminde yerleşik PC Ayarları. Genellikle bunlar Bilgisayarınızı kişiselleştirme ayarlarını ve aşağıdaki kategoriden içerirler:

* *Tema*, Masaüstü tema ve görev ayarları gibi özellikler içerir.
* *Internet Explorer ayarlarını*en son açılan sekmeleri ve Sık Kullanılanlar dahil olmak üzere.
* *Microsoft Edge tarayıcı ayarlarını*Sık Kullanılanlar ve okuma listesi gibi.
* *Parolalar*Internet parolaları, Wi-Fi profilleri ve diğerleri dahil olmak üzere.
* *Dil Tercihleri*, klavye düzenleri, Sistem dili, tarih ve saat ve daha fazlası için ayarları içerir.
* *Özelliklere erişim kolaylığı*, yüksek karşıtlıklı tema ve ekran okuyucusu Büyüteç gibi.
* *Diğer Windows ayarları*, fare ayarları gibi.

**Uygulama verileri**: Evrensel Windows uygulamaları için gezici bir klasör ayarları veri yazabilirsiniz ve bu klasöre yazılan tüm veriler otomatik olarak eşitlenir. Bu özellikten faydalanmak için bir uygulama tasarlamak için tek tek uygulama geliştiriciye aittir. Dolaşım kullanan bir evrensel Windows uygulaması geliştirme hakkında daha fazla ayrıntı için bkz. [appdata depolama API'si](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) ve [Geliştirici blog Dolaşım Windows 8 appdata](https://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Hangi hesap ayarlarını eşitleme için kullanılır?
Ayarları eşitleme, Windows 8.1 tüketici Microsoft hesapları her zaman kullanılır. Kurumsal kullanıcıların bir Microsoft hesabı ayarlarının eşitlenmesinin erişim elde etmek için Active Directory etki alanı hesabına bağlanma yeteneğini gerekiyordu. Windows 10'da bu işlevselliği bir birincil/ikincil hesap çerçevesiyle değiştirilmekte olan bir Microsoft hesabı bağlı.

Birincil hesap için Windows oturum açmak için kullanılan hesap olarak tanımlanır. Bu, bir Microsoft hesabı, bir Azure Active Directory (Azure AD) hesabı, bir şirket içi Active Directory hesabı veya yerel hesap olabilir. Birincil hesap ek olarak, Windows 10 kullanıcıları, kullanıcının cihazında bir veya daha fazla ikincil bulut hesapları ekleyebilirsiniz. Bir ikincil genellikle bir Microsoft hesabı, bir Azure AD hesabı veya başka bir hesap Gmail veya Facebook gibi hesabıdır. Bu ikincil hesaplar çoklu oturum açma gibi ek hizmetlerden ve Windows Store için erişim sağlar, ancak ayarları eşitleme destekleyen yeteneğine sahip değildir.

Windows 10, yalnızca cihaz için birincil hesap ayarlarını eşitleme için kullanılabilir (bkz [nasıl yükseltebilirim Windows 8'de Microsoft hesap ayarlarını Eşitleme'den Azure AD'ye Windows 10 ayarları eşitleme?](enterprise-state-roaming-faqs.md#how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10)).

Veri cihazda farklı kullanıcı hesaplarını arasında hiçbir zaman karma. Ayarları eşitleme için iki kural vardır:

* Windows ayarları her zaman birincil hesap dolaşır.
* Uygulama verilerini uygulama almak için kullanılan hesap ile etiketlenir. Birincil hesap ile etiketlenmiş yalnızca uygulamaları eşitler. Dışarıdan yüklenen Windows Store veya mobil cihaz Yönetimi (MDM) ile bir uygulama olduğunda uygulama sahipliği etiketleme belirlenir.

Bir uygulamanın sahibi belirtilmemesi durumunda, birincil hesap dolaşır. Windows 10'a bir cihaz Windows 8 veya Windows 8.1 yükseltilmiş ise, tüm uygulamalar Microsoft hesabı tarafından alınan olarak etiketlenir. Bu kullanıcıların çoğu Windows Store uygulamaları alma ve Azure AD hesapları önce Windows 10 için Windows Store destek yoktu olmasıdır. Çevrimdışı bir lisansı bir uygulama yüklediyseniz, uygulama cihazdaki birincil hesap kullanarak etiketlenir.

> [!NOTE]
> Kuruluşa ait olan ve Azure AD'ye bağlı Windows 10 cihazları, bir etki alanı hesabı için artık Microsoft hesaplarına bağlanabilir. Bir Microsoft hesabı bir etki alanı hesabına bağlanın ve kullanıcının tüm veri eşitleme (diğer bir deyişle, gezici bağlı bir Microsoft hesabı ve Active Directory işlevi Microsoft hesabı) Microsoft hesabına sahip olanağı, Windows 10'dan kaldırıldı bağlı bir Active Directory veya Azure AD ortamına katılmış cihazlar.
>
>

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Nasıl yükseltebilirim Windows 8'de Microsoft hesap ayarlarını Eşitleme'den Azure AD'ye Windows 10 ayarları eşitleme?
Bağlantılı bir Microsoft hesabı ile Windows 8.1 çalıştıran Active Directory etki alanına katılmış ise Microsoft hesabınız üzerinden ayarları eşitleme yapar. Windows 10'a yükselttikten sonra etki alanına katılmış bir kullanıcı olduğunuz ve Azure AD ile Active Directory etki alanına bağlanmaz sürece Microsoft hesabı ile kullanıcı ayarlarını eşitlemek devam eder.

Şirket içi Active Directory etki alanı ile Azure AD connect, cihazınız bağlı kullanarak ayarlarını eşitlemek deneyecek Azure AD hesabı. Azure AD Yöneticisi, Kurumsal durumda dolaşım, uygulamanızın bağlı etkinleştirmez, Azure AD hesap ayarlarını eşitleme durdurur. Windows 10 kullanıcıysanız ve bir Azure AD kimlik bilgilerinizle oturum açın, windows ayarları Azure AD üzerinden ayarları eşitleme yöneticinize sağlar hemen sonra eşitleme başlar.

Kişisel verileri şirket Cihazınızda depolanan, Windows işletim sistemi ve uygulama verileri için Azure AD eşitleme başlayacağımızı farkında olmalıdır. Bu, aşağıdaki anlamlara geliyor:

* Kişisel Microsoft hesap ayarlarınızı dışında ayarları üzerinde çalışmanız kayma veya Okul Azure AD hesapları. Microsoft hesabı ve Azure AD ayarları eşitleme olmasıdır artık ayrı hesaplar kullanıyorsunuz.
* Wi-Fi parolalar, web kimlik bilgilerini ve bağlı bir Microsoft hesabı ile önceden eşitlenmiş olan Internet Explorer Sık Kullanılanlar gibi kişisel veriler Azure AD'ye eşitlenecektir.

## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Microsoft hesabınız ve Azure AD kuruluş durumu gezici birlikte çalışabilirlik iş nasıl musunuz?
Windows 10 Kasım 2015 veya üzeri sürümlerde, Kurumsal durumda Dolaşım yalnızca tek bir hesap için aynı anda desteklenir. Windows için bir iş kullanarak oturum açın ya da Okul hesabı Azure AD, tüm verileri Azure AD ile eşitler. Windows için kişisel bir Microsoft hesabı kullanarak oturum açarsanız, tüm verileri Microsoft hesabı ile eşitler. Cihazda yalnızca birincil oturum açma hesabı kullanarak evrensel appdata dolaşır ve yalnızca birincil hesap tarafından uygulamanın lisansı aitse dolaşır. Tüm İkincil hesapları tarafından sahip olunan uygulamaları için evrensel appdata eşitlenmez.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Ayarları birden çok kiracıdan Azure AD hesapları için eşitliyor musunuz?
Birden çok Azure AD'yi farklı hesaplarını Azure AD kiracılarıyla aynı cihazda, her Azure AD kiracısı için Azure Rights Management hizmeti ile iletişim kurmak için cihaz kayıt defterini güncelleştirmeniz gerekir.  

1. Her Azure AD kiracınız için GUID'i bulun. Azure portalını açın ve Azure AD kiracısı seçin. Kiracı için Özellikler sayfasında seçilen Kiracı için GUID'dir (https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties), etiketlenmiş **dizin kimliği**. 
2. GUID sonra kayıt defteri anahtarını eklemeniz gerekecektir **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<Kiracı kimliği GUID >** .
   Gelen **Kiracı kimliği GUID** anahtar, adlı yeni Çoklu dize değeri (REG çoklu SZ) oluşturma **AllowedRMSServerUrls**. Verileri için cihaz erişen diğer Azure kiracılar lisans dağıtım noktası URL'leri belirtin.
3. Lisans dağıtım noktası URL'lerini çalıştırarak bulabilirsiniz **Get-AadrmConfiguration** AADRM modülünden cmdlet'i. Varsa değerlerini **Licensingıntranetdistributionpointurl** ve **LicensingExtranetDistributionPointUrl** farklıysa, her iki değeri belirtin. Değerler aynıysa değeri yalnızca bir kez belirtin.

## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Var olan Windows Masaüstü uygulamaları için Dolaşım ayarları seçenekleri nelerdir?
Dolaşım, yalnızca evrensel Windows uygulamaları için çalışır. Mevcut bir Windows Masaüstü uygulama dolaşımı etkinleştirmek için kullanılabilen iki seçenek vardır:

* [Masaüstü köprüsü](https://aka.ms/desktopbridge) Evrensel Windows platformu için var olan Windows Masaüstü uygulamalarınızın getirmenize yardımcı olur. Buradan, çok az kod değişiklikleri, Azure AD uygulama verileri dolaşımı avantajlarından yararlanmak için gerekli olacaktır. Masaüstü köprüsü uygulama verileri dolaşımı mevcut Masaüstü uygulamaları için etkinleştirmek için gereken bir uygulama kimliği ile uygulamalarınızı sağlar.
* [Kullanıcı deneyimi sanallaştırma (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) var olan Windows Masaüstü uygulamaları için özel ayarlar şablon oluşturmak ve Win32 uygulamaları için Dolaşım etkinleştirmek yardımcı olur. Bu seçenek, uygulamanın kodunu değiştirmek uygulama geliştiricisinin gerektirmez. UE-V, Microsoft Masaüstü iyileştirme paketi satın alan müşteriler için şirket içi Active Directory için Dolaşım sınırlıdır.

Yöneticiler, Windows işletim sistemi ayarları ve evrensel uygulama verileri dolaşımı değiştirerek Windows Masaüstü uygulama verilerini gerekmeden UE-V yapılandırabilir [UE-V grup ilkeleri](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2)de dahil olmak üzere:

* Windows ayarlarını Grup İlkesi Dolaşımda
* Windows uygulamaları Grup İlkesi eşitlemeyin
* Internet Explorer'ın uygulamalar bölümünde Dolaşım

Gelecekte, Microsoft Windows ile tamamen tümleştirilmiştir UE-V olun ve ayarları Azure AD bulut üzerinden Dolaşımda olabilen UE-V genişletmek için yol araştırabilir.

## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Şirket içi eşitlenmiş ayarları ve verileri depolayabilir?
Kurumsal durumda Dolaşım tüm eşitlenmiş veriler, Microsoft bulutunda yer depolar. UE-V sunan bir şirket içi çözüm Dolaşım.

## <a name="who-owns-the-data-thats-being-roamed"></a>Dolaşımda verilerin sahibi kim?
Kuruluşların kendi veri Kurumsal durumda Dolaşım dolaşıma açıldı. Veriler bir Azure veri merkezinde depolanır. Hem aktarım hem de Azure Information protection'dan Azure Rights Management hizmetini kullanarak bulutta bekleyen tüm kullanıcı verileri şifrelenir. Cihaz bırakmadan önce belirli kullanıcı kimlik bilgileri gibi yalnızca hassas verileri şifreler Microsoft hesabı tabanlı ayarları eşitlemeye izin, karşılaştırıldığında bir geliştirme budur.

Microsoft müşteri verilerini koruma için taahhüt eder. Başka bir kullanıcı bu verileri okuyabilmesi için bir Windows 10 cihaz bırakır. önce bir kurumsal kullanıcı ayarları verileri Azure Rights Management hizmeti tarafından otomatik olarak şifrelenir. Kuruluşunuzda Azure Rights Management hizmeti için ücretli bir abonelik varsa, izleme gibi diğer koruma özelliklerini kullanmak ve belgeleri iptal edebilir, otomatik olarak, hassas bilgiler içeren bir e-postaları korumak ve kendi anahtarlarınızı yönetme ("Getir kendi anahtarınızı"Çözüm, BYOK olarak da bilinir). Bu özellikler ve bu koruma hizmetinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Rights Management nedir](/azure/information-protection/what-is-information-protection).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Eşitleme ayarı veya belirli bir uygulama için yönetebilirim?
Windows 10'da için tek bir uygulamayı dolaşımı devre dışı bırakmak için Grup İlkesi veya MDM ayar yoktur. Kiracı yöneticileri, yönetilen bir cihazda tüm uygulamalar için appdata eşitleme devre dışı bırakabilirsiniz, ancak bir uygulama başına veya içinde uygulama düzeyinde daha hassas bir denetim yoktur.

## <a name="how-can-i-enable-or-disable-roaming"></a>Nasıl etkinleştirmek veya dolaşımı devre dışı?
İçinde **ayarları** uygulama, Git **hesapları** > **ayarlarınızı eşitleme**. Bu sayfada, hangi hesap ayarları gerekmeden kullanılıyor görebilir ve etkinleştirebilir veya dolaşıma için ayarları tek tek gruplar devre dışı bırakın.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Windows 10 Dolaşım etkinleştirmek için Microsoft'un öneri nedir?
Microsoft, gezici kullanıcı profilleri, UE-V ve kurumsal durumu Dolaşım dahil olmak üzere, kullanılabilir çözümler Dolaşım birkaç farklı ayarları vardır.  Microsoft gelecekteki Windows sürümlerinde gezici durumu Kurumsal yatırım yapmayı taahhüt eder. Ardından, kuruluşunuzun verileri buluta taşıma ile rahat veya hazır değilse, UE-V birincil Dolaşım teknolojinizi kullanmanızı öneririz. Kuruluşunuz var olan Windows Masaüstü uygulamaları için destek Dolaşım gerektirir, ancak buluta taşımak için Kurumsal durumu Dolaşım ve UE-V kullanmanızı öneririz. UE-V ve kurumsal durumda Dolaşım çok benzer teknolojiler olsa da, bunlar birbirini dışlamaz. Bunlar birbirini kuruluşunuz, kullanıcılarınızın gereken Dolaşım hizmetleri sağlayan sağlamaya yardımcı olmak için tamamlar.  

Kurumsal durumda Dolaşım ve UE-V kullanırken aşağıdaki kurallar geçerlidir:

* Kurumsal durumda Dolaşım birincil Dolaşım cihazda aracısıdır. UE-V "Win32 gap." desteklemek için kullanılıyor
* UE-V grubunu kullanarak ilkeler, UE-V için Windows ayarları ve modern UWP uygulama verileri dolaşımı devre dışı bırakılması gerekir. Bunlar zaten Kurumsal durumda Dolaşım tarafından ele alınmaktadır.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Sanal Masaüstü Altyapısı (VDI) Kurumsal durumda Dolaşım nasıl destekler?
Kurumsal durumda dolaşım, Windows 10 İstemci SKU'ları, ancak sunucu SKU'ları desteklenir. Bir istemci VM'nin hiper yönetici makinede barındırılan ve sanal makinede uzaktan oturum açın, verilerinizi dolaşır. Birden çok kullanıcı aynı işletim sistemi paylaşın ve kullanıcıların uzaktan bir sunucu tam masaüstü deneyimi için oturum açın, dolaşım çalışmayabilir. İkinci oturum tabanlı senaryosu resmi olarak desteklenmez.

## <a name="what-happens-when-my-organization-purchases-a-subscription-that-includes-azure-rights-management-after-using-roaming"></a>Kuruluşum, bir abonelik satın aldığında neler, dolaşım kullandıktan sonra Azure Rights Management içeren?
Kuruluşunuz zaten Azure Rights Management kullanımı sınırlı ücretsiz aboneliği ile Windows 10 Dolaşım kullanıyorsa, satın alma bir [Ücretli aboneliği](https://azure.microsoft.com/pricing/details/information-protection/) Azure Rights Management koruma hizmetini sahip olmaz içerir Dolaşım özelliğin işlevleri, herhangi bir etkisi ve hiçbir yapılandırma değişikliği, BT yöneticiniz tarafından gerekli olacaktır.

## <a name="known-issues"></a>Bilinen sorunlar
Lütfen belgeye bakın [sorun giderme](enterprise-state-roaming-troubleshooting.md) bilinen sorunların bir listesi için bölüm. 

## <a name="next-steps"></a>Sonraki adımlar 

Genel bakış için bkz. [Kurumsal durumda dolaşıma genel bakış](enterprise-state-roaming-overview.md)
