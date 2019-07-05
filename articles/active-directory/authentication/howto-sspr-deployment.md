---
title: Dağıtım planı - Azure Active Directory Self Servis parola sıfırlama
description: Stratejisi başarılı uygulama Azure AD Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b566bfc3f6c49f6cb9fe31f166356f6ae351e38
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440931"
---
# <a name="deploy-azure-ad-self-service-password-reset"></a>Dağıtma Azure AD Self Servis parola sıfırlama

Self Servis parola sıfırlama (SSPR), çalışanlar BT personeli bağlanmaya gerek kalmadan parolalarını sıfırlamak sağlayan bir Azure Active Directory özelliğidir. Çalışanlar, kaydolması gerekir veya Self Servis parola sıfırlama hizmetini kullanmadan önce kayıtlı olması. Kayıt sırasında çalışan bir veya daha fazla kimlik doğrulama yöntemleri ve kuruluşun etkin seçer.

SSPR hızlı bir şekilde Engellemesi alın ve nerede veya günün saatini ne olursa olsun çalışmaya devam etmek çalışanların sağlar. Kullanıcıların kendilerini engellemesini izin vererek, kuruluşunuzda parola güvenlikle ilgili en yaygın sorunlar için yüksek destek maliyetlerini ve üretken olmayan saat azaltabilir.

Hızlı bir şekilde başka bir uygulama veya hizmet kuruluşunuzdaki yanı sıra SSPR dağıtarak kaydolun kullanıcıların yardımcı olur. Bu işlem, oturum açma işlemleri büyük miktarda oluşturur ve kayıt artıracak.

SSPR dağıtmadan önce kuruluşların kaç parola ilgili Yardım Masası çağrılarının sorun zaman ve her çağrı, ortalama maliyeti üzerinden sıfırlama belirlemek isteyebilirsiniz. SSPR kuruluşunuza getiriyor değeri göstermek için bu verileri dağıtım sonrası kullanabilirler.  

## <a name="how-sspr-works"></a>SSPR nasıl çalışır?

1. Bir kullanıcı, parola sıfırlama çalıştığında, önceden kaydedilmiş kimlik doğrulama yöntemi veya kimliklerini kanıtlamak için yöntemleri doğrulamanız gerekir.
1. Ardından kullanıcının yeni bir parola girer.
   1. Yalnızca bulut kullanıcıları için yeni parola Azure Active Directory'de depolanır. Daha fazla bilgi için bkz [nasıl sspr'nin](concept-sspr-howitworks.md#how-does-the-password-reset-portal-work).
   1. Karma kullanıcılar için parola, Azure AD Connect hizmeti aracılığıyla şirket içi Active Directory'ye geri yazılır. Daha fazla bilgi için bkz [parola geri yazma nedir](concept-sspr-writeback.md#how-password-writeback-works).

## <a name="licensing-considerations"></a>Lisans hakkında önemli noktalar

Azure Active Directory lisansı kullanıcı başına anlamı her kullanıcının kullanabileceklerini özellikler için uygun bir lisansı olması gerekir.

- Yalnızca bulutta yer alan kullanıcılar için Self Servis parola sıfırlamayı ile Azure AD temel veya üstü kullanılabilir.
- Self Servis parola gerektiren karma ortamlar için Azure AD Premium P1 veya üzeri ile şirket içi geri yazma sıfırlama.

Lisanslama hakkında daha fazla bilgi bulunabilir [Azure Active Directory fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory/)

## <a name="enable-combined-registration-for-sspr-and-mfa"></a>Birleşik kayıt SSPR ve mfa'yı etkinleştir

Microsoft, kuruluşların SSPR ve çok faktörlü kimlik doğrulaması için birleşik kayıt deneyimi etkinleştirmenizi önerir. Bu birleşik kayıt deneyimi etkinleştirdiğinizde kullanıcılar her iki özellikleri etkinleştirmek için yalnızca bir kez kayıt bilgileri seçmeniz gerekir.

![Birleştirilmiş güvenlik bilgileri kayıt](./media/howto-sspr-deployment/combined-security-info.png)

Birleşik kayıt deneyimi kuruluşlar kullanılacak SSPR ve Azure multi-Factor Authentication'ı etkinleştirmek için gerekli değildir. Birleşik kayıt deneyimi daha iyi bir kullanıcı deneyimi, geleneksel tek tek bileşenlere kıyasla kuruluşların sağlar. Makalesinde birleşik kayıt ve nasıl etkinleştirileceği hakkında daha fazla bilgi bulunabilir [birleştirilmiş güvenlik bilgileri kayıt (Önizleme)](concept-registration-mfa-sspr-combined.md)

## <a name="plan-the-configuration"></a>Yapılandırmasını planlama

Aşağıdaki ayarlar, önerilen değerleri ile birlikte SSPR'ı etkinleştirmek için gereklidir.

| Alan | Ayar | Değer |
| --- | --- | --- |
| **SSPR özellikleri** | Self Servis parola sıfırlama etkinleştirildi | **Seçili** için pilot grup / **tüm** üretim için |
| **Kimlik doğrulama yöntemleri** | Kaydolma için gereken kimlik doğrulama yöntemleri | Her zaman 1 sıfırlama için gerekenden daha fazla |
|   | Sıfırlamak için gereken kimlik doğrulama yöntemleri | Bir veya iki |
| **Kayıt** | Kullanıcılardan oturum açarken kaydolmalarını iste | Evet |
|   | Kullanıcıların kendi kimlik doğrulama bilgilerini yeniden doğrulamaları istemeden önce geçen gün sayısı | 90 – 180 gün |
| **Bildirimleri** | Parola sıfırlamayı kullanıcılara bildir | Evet |
|   | Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildir | Evet |
| **Özelleştirme** | Yardım Masası bağlantısını Özelleştir | Evet |
|   | Özel bir Yardım Masası e-posta veya URL | Destek sitesi veya e-posta adresi |
| **Şirket içi tümleştirme** | Parolalar şirket içi geri yazma AD | Evet |
|   | Kullanıcıların hesap kilidini açma parolasını sıfırlama olmadan izin ver | Evet |

### <a name="sspr-properties-recommendations"></a>SSPR özellikleri önerileri

Self Servis parola sıfırlama etkinleştirirken, pilot aşamasında kullanılacak bir güvenlik grubu seçin.

Hizmet daha geniş kapsamda başlatmak planlama yaparken, kuruluşunuzdaki herkes için SSPR zorlamak için tüm seçeneğini kullanmanızı öneririz. Tüm, uygun Azure AD güvenlik grubunu veya AD grubu Azure AD'ye eşitlenmiş olarak ayarlanamıyor

### <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

En az bir sıfırlamak için gereken sayıdan fazla kaydetmek için gereken kimlik doğrulama yöntemleri ayarlayın. Sıfırlamak gerektiğinde birden fazla izin vererek kullanıcıların esnekliği sağlar.

Ayarlama **sıfırlamak için gereken yöntemlerin sayısı** kuruluşunuz için uygun bir seviyeye. İki güvenlik duruşunuzu artırabilirsiniz ancak bir en az uyuşmazlıkları gerektirir.

Bkz: [kimlik doğrulama yöntemleri nelerdir](concept-authentication-methods.md) hangi kimlik doğrulama yöntemleri SSPR, önceden tanımlı güvenlik soruları ve özelleştirilmiş güvenlik sorularını oluşturma için kullanılabilen daha ayrıntılı bilgi için.

### <a name="registration-settings"></a>Kayıt ayarları

Ayarlama **oturum açarken kaydolmalarını iste** için **Evet**. Bu ayar tüm kullanıcılar korunmasını sağlamak kullanıcıların oturum açarken kaydetmeye zorlanır anlamına gelir.

Ayarlama **kullanıcıların kendi kimlik doğrulama bilgilerini yeniden doğrulamaları istemeden önce geçen gün sayısını** için arasında **90** ve **180** gün, kuruluşunuz bir iş yoksa gereksinimi kısa bir zaman dilimi için.

### <a name="notifications-settings"></a>Bildirim ayarları

Hem yapılandırma **parola sıfırlamayı kullanıcılara bildir** ve **diğer yöneticiler parolalarını sıfırladığında tüm Yöneticilere bildirilsin** için **Evet**. Seçme **Evet** hem de kullanıcıların kendi parolalarını sıfırlama ve tüm yöneticileri yönetici zaman uyumlu bir parola değişikliklerini haberdar olduğunuzu sağlayarak güvenliği artırır. Kullanıcı veya yönetici böyle bir bildirim alırsınız ve bunlar değişiklik başlattınız değil, hemen olası bir güvenlik ihlali rapor edebilirsiniz.

### <a name="customization"></a>Özelleştirme

Özelleştirme için kritik **Yardım Masası e-posta veya URL** kullanıcıları sorunlarla hızlı bir şekilde Yardım Al emin olmak için. Sık kullanılan Yardım Masası e-posta adresi veya web sayfası, kullanıcılarınızın aşina olduğu için bu seçeneği ayarlayın.

### <a name="on-premises-integration"></a>Şirket içi tümleştirme

Karma bir ortamınız varsa, emin **parolalarını şirket içi geri yazma AD** ayarlanır **Evet**. Ayrıca, daha fazla esneklik sağlar gibi Evet'e parolayı sıfırlamadan hesabının kilidini açmak için izin ver kullanıcıların ayarlayın.

### <a name="changingresetting-passwords-of-administrators"></a>Yöneticilerin parolalarını değiştirme/sıfırlama

Yönetici hesapları, yükseltilmiş izinlerle özel hesaplarıdır. Bunların güvenliğini sağlamak için yöneticilerin parolaları değiştirmek için aşağıdaki kısıtlamalar uygulanır:

- Şirket içi Kurumsal yöneticileri veya etki alanı yöneticileri SSPR aracılığıyla kullanıcının parolasını sıfırlayamazsınız. Bunlar, yalnızca kendi şirket içi ortamda parolasını değiştirebilirsiniz. Bu nedenle, Azure AD'ye AD yönetici hesapları değil eşitleme şirket öneririz.
- Yönetici gizli sorular ve yanıtlar parola sıfırlama için bir yöntem olarak kullanamazsınız.

### <a name="environments-with-multiple-identity-management-systems"></a>Birden çok kimlik yönetimi sistemlerinde ortamlarla

Yönetim sistemleri ortamında Oracle AM gibi şirket içi kimlik yöneticileri gibi birden çok kimliği varsa, SiteMinder, veya diğer sistemler ve parolaları Active Directory ile yazılmış bir aracı gibi kullanarak diğer sistemlere eşitlenmesi gerekebilir Parola değiştirme bildirim hizmetini (PCNS) ile Microsoft Identity Manager (MIM). Daha karmaşık bu senaryo hakkında bilgi bulmak için bkz [bir etki alanı denetleyicisinde MIM parola değiştirme bildirimi Hizmeti'ni dağıtma](https://docs.microsoft.com/microsoft-identity-manager/deploying-mim-password-change-notification-service-on-domain-controller).

## <a name="plan-deployment-and-support-for-sspr"></a>Dağıtım ve SSPR için destek planı

### <a name="engage-the-right-stakeholders"></a>Doğru proje katılımcıları etkileşim kurun

Teknoloji projeleri başarısız olursa, bunlar genellikle etkisi, sonuçları ve sorumlulukları eşleşmeyen beklentiler nedeniyle bunu yapar. Bu sorunları önlemek için doğru proje katılımcıları ilgi çekici ve paydaşlara ve proje giriş ve Sorumluluk belgeleme tarafından anlaşılan projenin proje Katılımcısı rollerinde iyi olduğundan emin olun.

### <a name="communications-plan"></a>İletişim planı

İletişim, herhangi bir yeni hizmeti başarısı için önemlidir. Proaktif bir şekilde hizmetin nasıl kullanılacağını ve herhangi bir şey beklendiği gibi çalışmazsa Yardım almak için yapabileceklerini Kullanıcılarınızla iletişim kurar. Gözden geçirme [Self Servis parola sıfırlama Microsoft İndirme merkezinde ürün malzemeleri](https://www.microsoft.com/download/details.aspx?id=56768) son kullanıcı iletişim stratejinizi planlayın ilişkin fikirler için.

### <a name="testing-plan"></a>Test planı

Dağıtımınızın beklendiği gibi çalıştığından emin olmak için uygulama doğrulamak için kullanacağınız test çalışmalarını bir dizi planlamanız gerekir. Aşağıdaki tablo, kuruluşunuzun, ilkelere dayalı sonuçları beklenen belge için kullanabileceğiniz bazı yararlı test senaryoları içerir.

| Kurum başarı hikayesi | Beklenen sonuç |
| --- | --- |
| SSPR portalı kurumsal ağ içinden erişilebilir | Kuruluşunuz tarafından belirlenen |
| SSPR portalı kurumsal ağ dışından erişilebilir | Kuruluşunuz tarafından belirlenen |
| Kullanıcı parola sıfırlama için etkinleştirilmediğinde tarayıcısından kullanıcı parolası sıfırlama | Kullanıcı parola sıfırlama işlem akışında erişmesi mümkün değil |
| Kullanıcı parola sıfırlama için kayıtlı değil, tarayıcı kullanıcı parolası sıfırlama | Kullanıcı parola sıfırlama işlem akışında erişmesi mümkün değil |
| Parola sıfırlama kaydı ne zaman içinde kullanıcı işaretlerini zorlanır | Kullanıcıdan güvenlik bilgilerini kaydetme istenir. |
| Parola sıfırlama kaydı ne zaman içinde kullanıcı işaretlerini tamamlandı | Güvenlik bilgileri kaydetmek için kullanıcıya sorulmaz |
| Kullanıcı bir lisans olmadığında SSPR portalı erişilebilir | Erişilebilir |
| Kullanıcı kaydolduktan sonra Windows 10 AADJ veya H + AADJ cihaz kilit ekranında kullanıcı parolası sıfırlama | Kullanıcı parolasını sıfırlayabilirsiniz. |
| SSPR kaydı ve kullanım verileri neredeyse gerçek zamanlı olarak yöneticiler tarafından kullanılabilir | Denetim günlükleri kullanılabilir |

### <a name="support-plan"></a>Destek planı

SSPR kullanıcı sorunları genellikle oluşturmaz ancak destek personeli kaynaklanabilecek sorunlarla başa çıkmak için hazır olması önemlidir.

Yönetici değiştirebilir veya Azure AD Portalı aracılığıyla son kullanıcılar için parola sıfırlama sırasında bir Self Servis destek işlem sorunu çözmenize yardımcı olması daha iyidir.

Bu belgenin işlem kılavuzu bölümünde, destek ve olası nedenlerin listesini oluşturmak ve çözüm Kılavuzu oluşturun.

### <a name="auditing-and-reporting"></a>Denetim ve Raporlama

Dağıtımdan sonra birçok kuruluşun öğrenmek istediğiniz nasıl ya da (SSPR) Self Servis parola sıfırlama, gerçekten kullanılıyor. Azure Active Directory (Azure AD) sağladığı raporlama özelliği önceden oluşturulmuş raporları kullanarak soruları yanıtlamanıza yardımcı olur.

Kayıt ve parola sıfırlama denetim günlüklerini 30 gün boyunca kullanılabilir. Şirket içinde güvenlik denetimi daha uzun bekletme süresi gerekiyorsa, bu nedenle, günlükleri dışarı aktarılmalı ve bir SIEM aracına gibi tüketilen sağlamanız [Azure Gözcü](../../sentinel/connect-azure-active-directory.md), Splunk veya ArcSight.

Bir tablodaki bir aşağıdaki gibi yedekleme zamanlaması, sistem ve sorumlu taraflar belgeleyin. Denetim ve raporlama yedeklemeler ayrı gerekmeyebilir, ancak bir sorundan kurtulmak ayrı bir yedekleme olması gerekir.

|   | İndirme sıklığı | Hedef sistem | Sorumlu kişi |
| --- | --- | --- | --- |
| Denetim yedekleme |   |   |   |
| Raporlama yedekleme |   |   |   |
| Olağanüstü durum kurtarma yedeklemesi |   |   |   |

## <a name="implementation"></a>Uygulama

Uygulama, üç aşamada gerçekleşir:

- Kullanıcıları ve lisansları yapılandırın
- Azure AD SSPR kayıt ve Self Servis için yapılandırma
- Azure AD Connect parola geri yazma için yapılandırma

### <a name="communicate-the-change"></a>Değişikliği iletişim

Planlama aşamasında geliştirilen iletişim planı uygulaması başlar.

### <a name="ensure-groups-are-created-and-populated"></a>Gruplar oluşturulur ve doldurulur emin olun.

Planlama parola kimlik doğrulama yöntemleri bölümüne bakın ve gruplara pilot veya üretim uygulaması için kullanılabilir ve tüm uygun kullanıcıları gruplara eklenen emin olun.

### <a name="apply-licenses"></a>Uygulama lisansları

Uygulamak için seçeceğiz grupları Azure AD olmalıdır premium lisansı atanmış. Doğrudan gruba lisans atayabilirsiniz veya PowerShell veya grup tabanlı lisanslama ile var olan lisans ilkeleri gibi kullanabilirsiniz.

Kullanıcılara, gruplara lisans atama hakkında bilgi makalesinde bulunabilir [Azure Active Directory'de Grup üyeliği kullanıcılara lisans atama](../users-groups-roles/licensing-groups-assign.md).

### <a name="configure-sspr"></a>SSPR yapılandırma

#### <a name="enable-groups-for-sspr"></a>Gruplar için SSPR etkinleştirme

1. Erişim Azure portalı ile bir yönetici hesabı.
1. Tüm hizmetler seçin ve filtre kutusuna Azure Active Directory ve Azure Active Directory'ı seçin.
1. Active Directory dikey penceresinde, parola sıfırlama seçin.
1. Özellikler bölmesinde seçili seçin. Tüm kullanıcıların Tümünü Seç etkin istiyorsanız.
1. Varsayılan parola sıfırlama İlkesi dikey penceresi, ilk grup adını yazın, seçmek ve ardından ekranın alt kısmındaki Seç'e tıklayın ve ekranın en üstünde Kaydet'seçeneğini belirleyin.
1. Bu işlem, her grup için yineleyin.

#### <a name="configure-the-authentication-methods"></a>Kimlik doğrulama yöntemlerini yapılandırın

Bu belgenin planlama parola kimlik doğrulama yöntemleri bölümünden planlama başvuru.

1. Kayıt seçin, altında register when signing in, Evet'i seçin ve ardından süresi dolmadan önce gün sayısını ayarlayın zorunlu tutun ve Kaydet'ı seçin.
1. Bildirim, seçin ve planınızı yapılandırmak ve Kaydet'ı seçin.
1. Özelleştirme, seçin ve planınızı yapılandırma ve kaydetme'ı seçin.
1. Şirket içi tümleştirme seçin ve planınızı yapılandırma ve kaydetme'ı seçin.

### <a name="enable-sspr-in-windows"></a>SSPR Windows içinde etkinleştir

1803 veya üzeri sürümü çalıştıran ya da Azure AD'ye katılan Windows 10 cihazları veya hibrit Azure AD'ye katılmış Windows oturum açma ekranında parolalarını sıfırlayabilir. Bilgi ve bu özelliği yapılandırma adımları makalesinde bulunan [sıfırlama oturum açma ekranından Azure AD parola](tutorial-sspr-windows.md)

### <a name="configure-password-writeback"></a>Parola geri yazmayı yapılandırın

Makalesinde bulunabilir kuruluşunuz için parola geri yazmayı yapılandırma adımları [nasıl yapılır: Parola geri yazmayı yapılandırın](howto-sspr-writeback.md).

## <a name="manage-sspr"></a>SSPR'ı yönetme

Self Servis parola sıfırlama ile ilişkili özellikleri yönetmek için gerekli rolleri.

| İş rolü/kişi | Azure AD rolüne (gerekirse) |
| :---: | :---: |
| Düzey 1 Yardım Masası | Parola Yöneticisi |
| Düzey 2 Yardım Masası | Kullanıcı Yöneticisi |
| SSPR yönetici | Genel yönetici |

### <a name="support-scenarios"></a>Destek senaryoları

Destek ekibi başarınızı etkinleştirmek için kullanıcılarınızdan gelen aldığınız sorular temel bir SSS oluşturabilirsiniz. Aşağıdaki tabloda, destek senaryoları içerir.

| Senaryolar | Açıklama |
| --- | --- |
| Kullanıcının kayıtlı kimlik doğrulama yöntemleri kullanılabilir yok | Bir kullanıcının parolasını sıfırlamak çalışıyor, ancak herhangi bir kimlik doğrulama yöntemi yok kullanılabilir kayıtlı (örnek: evde, cep telefonlarına sola ve e-postalarına erişemez) |
| Kullanıcı bir metin alma değil veya kendi office veya cep telefonu çağrısı | Bir kullanıcı, metin veya çağrı aracılığıyla kendi kimliğini doğrulamak çalışıyor ancak bir metin/çağrı almıyor. |
| Kullanıcı parola sıfırlama portalı erişemiyor | Bir kullanıcı parolalarını sıfırlamak istiyor, ancak parola sıfırlama için etkinleştirilmedi ve parolaları güncelleştirmek için sayfayı etkinleştirilemez. |
| Kullanıcı yeni bir parola ayarlanamıyor | Bir kullanıcı, parola sıfırlama işlem akışında sırasında doğrulama tamamlanır ancak yeni bir parola ayarlanamaz. |
| Kullanıcı parola sıfırlama bağlantısı, bir Windows 10 cihazında görmez | Windows 10 kilit ekranından parolasını sıfırlamak bir kullanıcı çalışıyor ancak cihaz ya da Azure AD'ye katılmış değil ya da Intune cihaz ilkesi etkin değil |

Ek sorun giderme amacıyla aşağıdaki gibi bilgileri eklemek isteyebilirsiniz.

- Hangi grupların SSPR için etkinleştirilir.
- Hangi kimlik doğrulama yöntemleri yapılandırılır.
- Ya da şirket ağının ilgili erişim ilkeleri.
- Sık karşılaşılan senaryolara yönelik sorun giderme adımları.

Ayrıca çevrimiçi Belgelerimizi en yaygın SSPR senaryolar için genel sorun giderme adımlarını anlamak için Self Servis parola sıfırlamayı giderme başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD parola koruması uygulamayı düşünün](concept-password-ban-bad.md)

- [Azure AD akıllı kilitleme uygulamayı düşünün](howto-password-smart-lockout.md)
