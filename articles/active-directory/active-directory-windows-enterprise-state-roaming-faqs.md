---
title: Ayarlar ve veri dolaşımı SSS | Microsoft Docs
description: BT yöneticileri bazı sorulara yanıtlar ayarları ve uygulama veri eşitleme hakkında olabilir sağlar.
services: active-directory
keywords: Kurumsal durum Dolaşım ayarları, windows bulut Kurumsal durumda Dolaşım hakkında sık sorulan sorular
documentationcenter: ''
author: tanning
manager: mtillman
editor: curtand
ms.assetid: c0824f5c-129b-4240-969f-921f6a64eae7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: markvi
ms.openlocfilehash: f33376d5f68d64495a7a90e62870f3ec14f73246
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34258723"
---
# <a name="settings-and-data-roaming-faq"></a>Ayarlar ve veri dolaşımı hakkında SSS
Bu makalede, BT yöneticileri, ayarları ve uygulama veri eşitleme hakkında sorabileceğiniz bazı sorular yanıtlanmaktadır.

## <a name="what-data-roams"></a>Hangi verilerin Dolaşım yapar?
**Windows ayarlarını**: Windows işletim sisteminde yerleşik PC Ayarları. Genellikle, bunlar Bilgisayarınızı kişiselleştirme ayarlarını ve aşağıdaki geniş kategoriler şunlardır:

* *Tema*, masaüstü teması ve görev çubuğunda ayarlar gibi özellikleri içerir.
* *Internet Explorer ayarlarını*son açılan sekmeler ve Sık Kullanılanlar dahil olmak üzere.
* *Kenar tarayıcı ayarlarını*, Sık Kullanılanlar ve okuma listesi gibi.
* *Parolalar*Internet parolaları, Wi-Fi profilleri ve diğerleri dahil olmak üzere.
* *Dil Tercihleri*, klavye düzenleri, Sistem dili, tarih ve saat ve daha fazlası için ayarları içerir.
* *Özelliklere erişim kolaylığı*, yüksek karşıtlık teması, okuyucu ve Büyüteç gibi.
* *Diğer Windows ayarları*komut istemi ayarlarını ve uygulama listesi gibi.

**Uygulama verileri**: Evrensel Windows uygulamaları ayarları veri gezici bir klasöre yazma ve bu klasöre yazılan tüm veriler otomatik olarak senkronize edilir. Bu uygulama bu özellikten yararlanabilir için tasarlamak için tek tek uygulama geliştiricisi hazır. Dolaşım kullanan bir evrensel Windows uygulaması geliştirme hakkında daha fazla ayrıntı için bkz: [appdata depolama API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) ve [Geliştirici blog gezici Windows 8 appdata](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Hangi hesabı ayarları eşitleme için kullanılır?
Windows 8 ve Windows 8.1 ayarları eşitleme tüketici Microsoft hesapları her zaman kullanılır. Kurumsal kullanıcıların ayarları eşitleme erişmek için Active Directory etki alanı hesabı için bir Microsoft hesabı bağlanma özelliği gerekiyordu. Windows 10'da bu işlevselliği birincil/ikincil hesap çerçevesiyle değiştirilmekte olan Microsoft hesabı bağlı.

Birincil hesap, Windows'ta oturum açmak için kullanılan hesap olarak tanımlanır. Bu, bir Microsoft hesabı, bir Azure Active Directory (Azure AD) hesabı, bir şirket içi Active Directory hesabı veya yerel bir hesap olabilir. Birincil hesabının yanı sıra, Windows 10 kullanıcılar cihazlarını için bir veya daha fazla ikincil bulut hesapları ekleyebilirsiniz. Bir ikincil genellikle bir Microsoft hesabı, bir Azure AD hesabının veya başka bir hesap Gmail veya Facebook gibi hesabıdır. Bu ikincil hesapları için çoklu oturum açma gibi ek hizmetleri ve Windows mağazası erişimi sağlasa da, bunlar ayarları eşitleme destekleyen yeteneğine sahip değil.

Windows 10 cihaz için yalnızca birincil hesap ayarları eşitleme için kullanılabilir (bkz [nasıl yükseltebilirim Microsoft hesabı ayarları eşitleme Windows 8'den Azure AD ile Windows 10 ayarları eşitleme?](active-directory-windows-enterprise-state-roaming-faqs.md#how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10)).

Veri cihazda farklı kullanıcı hesapları arasında hiçbir zaman karma. Ayarları eşitleme için iki kural vardır:

* Windows ayarlarını her zaman birincil hesabıyla dolaşan.
* Uygulama verileri uygulama almak için kullanılan hesap ile etiketlenecektir. Birincil hesabıyla etiketli yalnızca uygulamaları eşitler. Bir uygulamayı Windows mağazası veya mobil cihaz Yönetimi (MDM) dışarıdan yüklenen olduğunda uygulama sahipliği etiketleme belirlenir.

Bir uygulamanın sahibi tanımlanamıyor, birincil hesabıyla dolaşan. Windows 10'a bir cihaz Windows 8 veya Windows 8.1 yükseltilmiş ise, tüm uygulamalar Microsoft hesabı tarafından alınan olarak etiketlenecektir. Windows mağazası uygulamaları kullanıcıların çoğunun edinmeli ve Azure AD hesapları önce Windows 10 için Windows mağazası destek yoktu çünkü budur. Bir uygulama çevrimdışı bir lisans yüklüyse, uygulama cihaz üzerinde birincil hesabıyla etiketlenecektir.

> [!NOTE]
> Kuruluşa ait olan ve Azure AD ile bağlı olan Windows 10 cihazları için bir etki alanı hesabı artık Microsoft hesaplarını bağlanabilir. Bir Microsoft hesabı bir etki alanı hesabına bağlanma ve tüm kullanıcı verileri eşitlemek için Microsoft hesabı (diğer bir deyişle, gezici bağlı Microsoft hesabı ve Active Directory işlevselliği Microsoft hesabı) sahip olanağı bağlı bir Active Directory veya Azure AD ortamına katılmış Windows 10 cihazları kaldırılır.
>
>

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Nasıl yükseltebilirim Microsoft hesabı ayarları eşitleme Windows 8'den Azure AD ile Windows 10 ayarları eşitleme?
Bağlı bir Microsoft hesabı ile Windows 8 veya Windows 8.1 çalıştıran Active Directory etki alanına katılırsa, Microsoft hesabınız üzerinden ayarları eşitleme yapar. Windows 10'a yükselttikten sonra etki alanına katılmış bir kullanıcı olduğunuz ve Azure AD ile Active Directory etki alanına bağlanmaz sürece kullanıcı ayarlarını bir Microsoft hesabı ile eşitlemeye devam eder.

Şirket içi Active Directory etki alanı ile Azure AD connect, Cihazınızı bağlı kullanarak ayarlarını eşitleme dener Azure AD hesabı. Azure AD Yöneticisi Kurumsal durumda dolaşım, sizin bağlı etkinleştirmez, Azure AD hesabının ayarları eşitleniyor durdurur. Bir Windows 10 kullanıcı olduğunuz ve Azure AD kimlik bilgilerinizle oturum açın, Azure AD üzerinden ayarları eşitleme yöneticinize etkinleştirir hemen sonra windows ayarları eşitleme başlar.

Kişisel verileri şirket aygıtınızda depolanırsa, Windows işletim sistemi ve uygulama verileri için Azure AD eşitleme başlayacağını farkında olmalıdır. Bu, aşağıdaki etkilere sahiptir:

* Kişisel Microsoft hesap ayarlarınızı dışında ayarları üzerindeki çalışmanızı kayma veya Okul Azure AD hesapları. Microsoft hesabınız ve Azure AD ayarları eşitleme olmasıdır artık ayrı hesaplar kullanma.
* Wi-Fi parolalar, web kimlik bilgilerini ve bağlı bir Microsoft hesabı ile önceden eşitlenmiş olan Internet Explorer Sık Kullanılanlar gibi kişisel verileri Azure AD ile eşitlenir.

## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Microsoft hesabınız ve Azure AD kuruluş durumu gezici birlikte çalışabilirlik iş nasıl musunuz?
Windows 10 Kasım 2015 veya sonraki sürümlerde Kurumsal durumda Dolaşım yalnızca tek bir hesap için aynı anda desteklenir. Bir iş kullanarak Windows'a oturum açın veya Azure AD hesabının Okul varsa, tüm verileri Azure AD eşitleme yapar. Windows kişisel bir Microsoft hesabı kullanarak oturum açarsanız, tüm verileri Microsoft hesabı ile eşitler. Evrensel uygulama verileri cihazdaki yalnızca birincil oturum açma hesabı kullanarak dolaşan ve yalnızca uygulamanın lisans birincil hesabıyla aitse dolaşan. Tüm İkincil hesapları tarafından sahip olunan uygulamalar için evrensel appdata eşitlenmeyecek.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Ayarları birden çok kiracıdan Azure AD hesapları için eşitliyor musunuz?
Birden çok Azure AD farklı hesaplarını Azure AD kiracılarıyla aynı cihaza, her Azure AD kiracısı Azure Rights Management hizmeti ile iletişim kurmak için cihaz kayıt defterini güncelleştirmeniz gerekir.  

1. Her bir Azure AD Kiracı için GUID'i bulun. Azure Portalı'nı açın ve Azure AD kiracısı seçin. Kiracı için seçilen Kiracı için Özellikler sayfasında GUID'dir (https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties), etiketlenmiş **dizin kimliği**. 
2. GUID aldıktan sonra kayıt defteri anahtarını eklemeniz gerekir **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<kimliği GUID Kiracı >**.
   Gelen **kimliği GUID Kiracı** anahtar, adlı yeni bir çok dizeli değer (REG-MULTI-SZ) oluşturma **AllowedRMSServerUrls**. Verileri için aygıt erişen bir Azure kiracılar lisans dağıtım noktası URL'lerini belirtin.
3. Lisans dağıtım noktası URL'leri çalıştırarak bulabileceğiniz **Get-AadrmConfiguration** AADRM modülden cmdlet. Varsa değerlerini **Licensingıntranetdistributionpointurl** ve **LicensingExtranetDistributionPointUrl** farklıysa, her iki değeri belirtin. Değerler aynıysa değeri yalnızca bir kez belirtin.

## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Var olan Windows Masaüstü uygulamaları için Dolaşım ayarları seçenekleri nelerdir?
Dolaşım, yalnızca evrensel Windows uygulamaları için çalışır. Varolan bir Windows Masaüstü uygulama gezici etkinleştirmek için kullanılabilen iki seçenek vardır:

* [Masaüstü köprüsü](https://aka.ms/desktopbridge) var olan Windows Masaüstü uygulamalarınızı Evrensel Windows platformu getirmenize yardımcı olur. Buradan, kodda minimum düzeyde değişiklik Azure AD uygulama verileri dolaşımı avantajlarından yararlanmak için gerekli olacaktır. Masaüstü köprüsü uygulamalarınızı varolan Masaüstü uygulamaları için gezici uygulama verilerini etkinleştirmek için gereken bir uygulama kimliği ile sağlar.
* [Kullanıcı deneyimi sanallaştırma (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) , var olan Windows Masaüstü uygulamaları için özel ayarlar şablonu oluşturmak ve Win32 uygulamaları için dolaşımını etkinleştir yardımcı olur. Bu seçenek uygulamanın kodunu değiştirmek uygulama geliştiricisi gerektirmez. UE-V, Microsoft Masaüstü iyileştirme paketi satın alan müşteriler için şirket içi Active Directory için gezici sınırlıdır.

Yöneticiler, Windows Masaüstü uygulama verileri, Windows işletim sistemi ayarları ve evrensel uygulama verilerine gezici değiştirerek dolaşıma UE-V yapılandırabilir [UE-V grup ilkeleri](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2)dahil:

* Windows ayarlarını Grup İlkesi dolaşan
* Windows uygulamaları Grup İlkesi eşitlemeyin
* Internet Explorer'ın uygulamaları bölümünde Dolaşım

Gelecekte, Microsoft iç Windows'da tümleşik UE-V yapmak ve Azure AD bulut aracılığıyla ayarları dolaştırmayı UE-V genişletmek için yollar araştırabilir.

## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Şirket içinde eşitlenmiş ayarları ve verileri depolayabilir?
Kurumsal durumda Dolaşım tüm eşitlenmiş veriler Azure bulutunda depolar. UE-V sunan bir şirket içi çözüm gezici.

## <a name="who-owns-the-data-thats-being-roamed"></a>Çıkan verilerin sahibi kim?
Kuruluşların kendi Kurumsal durumda Dolaşım veri çıkan. Veriler bir Azure veri merkezinde depolanır. Aktarım sırasında hem Azure Information Protection Azure Rights Management hizmetinden kullanarak bulut bekleyen tüm kullanıcı verileri şifrelenir. Cihaz terk etmeden önce yalnızca kullanıcı kimlik bilgileri gibi hassas belirli verileri şifreler Microsoft hesabı tabanlı ayarları eşitleme için karşılaştırıldığında geliştirme budur.

Microsoft müşteri verilerini koruma için taahhüt eder. Başka bir kullanıcı bu verileri okuyabilmesi için bir Windows 10 cihaz terk etmeden önce bir kurumsal kullanıcının ayarlarını veriler Azure Rights Management hizmeti tarafından otomatik olarak şifrelenir. Kuruluşunuzun Azure Rights Management hizmeti için ücretli bir abonelik varsa, izleme gibi diğer koruma özelliklerini kullanmak ve belgeleri iptal edebilir, otomatik olarak hassas bilgiler içeren bir e-postaları korumak ve kendi anahtarları Yönet ("Getir kendi anahtarınızı"Çözüm, BYOK olarak da bilinir). Bu özellikler ve bu koruma hizmetinin nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure Rights Management nedir](https://docs.microsoft.com/azure/information-protection/understand-explore/what-is-information-protection).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Eşitleme için belirli uygulama veya ayarını yönetebilir miyim?
Windows 10'da tek bir uygulama için gezici devre dışı bırakmak için hiçbir MDM veya Grup İlkesi ayarı yoktur. Kiracı Yöneticiler appdata eşitleme yönetilen bir cihazda tüm uygulamalar için devre dışı bırakabilir, ancak uygulama başına veya uygulama içinde düzeyinde daha hassas bir denetim yok yok.

## <a name="how-can-i-enable-or-disable-roaming"></a>Nasıl etkinleştirebilirim veya gezici devre dışı?
İçinde **ayarları** uygulama, Git **hesapları** > **ayarlarınızı eşitleyin**. Bu sayfadan hangi hesabı ayarları dolaştırmayı kullanılıyor görebilirsiniz ve etkinleştirmek veya bireysel gruplar çıkan ayarlarının devre dışı bırakabilirsiniz.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Windows 10 Dolaşım etkinleştirmek için Microsoft'un öneri nedir?
Microsoft solutions gezici kullanıcı profilleri, UE-V ve kurumsal durumu Dolaşım gibi kullanılabilir gezici birkaç farklı ayarları vardır.  Microsoft Enterprise yatırım gelecekteki Windows sürümlerinde gezici durum hale getireceğini taahhüt eder. Ardından, kuruluşunuzun verilerini buluta taşıma ile hazır veya rahat değilse, birincil gezici teknolojiyi UE-V kullanma öneririz. Kuruluşunuz var olan Windows Masaüstü uygulamaları için destek gezici gerektiriyor, ancak buluta taşımak istekli, hem Enterprise durumu gezici hem de UE-V kullanmanızı öneririz. UE-V ve kurumsal durumda Dolaşım çok benzer teknolojiler olsa da, bunlar birbirini dışlayan değildir. Bunlar diğer kuruluşunuz kullanıcılarınıza gezici hizmetleri sağlayan sağlamaya yardımcı olmak için tamamlar.  

Kurumsal durumda Dolaşım ve UE-V kullanırken, aşağıdaki kurallar geçerlidir:

* Kurumsal durumda Dolaşım birincil gezici cihazda aracısıdır. UE-V "Win32 boşluğu." desteklemek için kullanılıyor
* UE-V grubunu kullanarak ilkeleri zaman UE-V Windows ayarları ve modern UWP uygulama verileri için gezici devre dışı bırakılması gerekir. Bunlar zaten Kurumsal durumda Dolaşım tarafından ele alınmıştır.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Sanal Masaüstü Altyapısı (VDI) Kurumsal durumda Dolaşım nasıl destekler?
Kurumsal durumda dolaşım, Windows 10 İstemci SKU'ları, ancak sunucu SKU'ları desteklenir. Bir istemci VM hiper yönetici makinesinde barındırılır ve sanal makineye uzaktan oturum açın, verilerinizi dolaşıma. Birden çok kullanıcı aynı işletim sistemi paylaşma ve kullanıcıların uzaktan bir sunucu tam masaüstü deneyimi için oturum açın, gezici çalışmayabilir. İkinci oturum tabanlı senaryo resmi olarak desteklenmez.

## <a name="what-happens-when-my-organization-purchases-a-subscription-that-includes-azure-rights-management-after-using-roaming"></a>Kuruluşum, bir abonelik satın ne olur, Azure Rights Management gezici kullandıktan sonra içerir?
Kuruluşunuz zaten Windows 10'da Azure Rights Management sınırlı kullanım ücretsiz aboneliğe gezici kullanıyorsa, satın alma bir [Ücretli aboneliği](https://azure.microsoft.com/pricing/details/information-protection/) Azure Rights Management Koruması hizmetine sahip olmaz içerir Dolaşım özelliğinin işlevselliği üzerindeki etkilerini ve hiçbir yapılandırma değişikliği, BT yöneticiniz tarafından yapılması gerekir.

## <a name="known-issues"></a>Bilinen sorunlar
Lütfen belgelere bakın [sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md) bilinen sorunların bir listesi için bölüm. 

## <a name="related-topics"></a>İlgili konular
* [Kurumsal durum gezici genel bakış](active-directory-windows-enterprise-state-roaming-overview.md)
* [Azure Active Directory'de gezici Kurumsal durumunu etkinleştir](active-directory-windows-enterprise-state-roaming-enable.md)
* [Grup İlkesi ve ayarları eşitleme için MDM ayarları](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Windows 10 Dolaşım ayarları başvurusu](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
* [Sorun giderme](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
