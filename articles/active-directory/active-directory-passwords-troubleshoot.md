---
title: 'Sorun giderme: Azure AD SSPR''yi | Microsoft Docs'
description: "Sorun giderme Azure AD Self Servis parola sıfırlama"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 2ee92359056abec660ac45acbeb72016554e89bf
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="how-to-troubleshoot-self-service-password-reset"></a>Self Servis parola sıfırlama ile ilgili sorunları giderme

Self Servis parola sıfırlama ile ilgili sorunları yaşıyorsanız izleyin öğeleri hızlı bir şekilde çalışmaya işlerinizi size yardımcı olabilir.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-may-see"></a>Bir kullanıcı görebilirsiniz Self Servis parola sıfırlama hatalarında sorun giderme

| Hata | Ayrıntılar | Teknik Ayrıntılar |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Özür dileriz <br> Yöneticinize parola sıfırlama, kuruluşunuz için devre dışı bıraktığından şu anda parolanızı sıfırlayamazsınız. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve bu özelliği etkinleştirmek için isteyin. Daha fazla bilgi edinmek için okuma [Yardım, unuttum Azure AD parolamı](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: Parola sıfırlama yöneticiniz tarafından etkinleştirilmemiş olduğundan algıladık. Lütfen yöneticinize başvurun ve kuruluşunuz için parola sıfırlama etkinleştirme isteyin. |
| WritebackNotEnabled = 10 |Özür dileriz <br> Yöneticiniz, kuruluşunuz için gerekli bir hizmeti etkin değil çünkü şu anda parolanızı sıfırlayamazsınız. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve kuruluşunuzun yapılandırmasını denetleyin isteyin. Bu gerekli hizmet hakkında daha fazla bilgi için okuma [parola geri yazma yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-writeback#configure-password-writeback). | SSPR_0010: Parola geri yazma etkinleştirilmemiş olduğundan algıladık. Lütfen yöneticinize başvurun ve parola geri yazma özelliğini etkinleştirmek için isteyin. |
| SsprNotEnabledInUserPolicy = 11 | Özür dileriz  <br> Yöneticiniz, parola sıfırlama, kuruluşunuz için yapılandırılmamış olduğundan şu anda parolanızı sıfırlayamazsınız. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve parola sıfırlama yapılandırma isteyin. Parola hakkında daha fazla bilgi için sıfırlama yapılandırma makaleyi okuyun [hızlı başlangıç: Azure AD Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Kuruluşunuz bir parola sıfırlama ilkesi tanımlı değil. Lütfen yöneticinize başvurun ve bir parola sıfırlama ilkesini tanımlamak için isteyin. |
| UserNotLicensed = 12 | Özür dileriz <br> Gerekli olan lisansları kuruluşunuzdan eksik olduğundan şu anda parolanızı sıfırlayamazsınız. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve lisans atamasını denetlemek için isteyin. Makaleyi okuyun lisanslama hakkında daha fazla bilgi edinmek için [lisans gereksinimleri için Azure AD Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: Kuruluşunuzun parola sıfırlama işlemini gerçekleştirmek için gerekli gerekli lisansları sahip değil. Lütfen yöneticinize başvurun ve lisans anlaşmalarını gözden isteyin. |
| UserNotMemberOfScopedAccessGroup = 13 | Özür dileriz <br> Yöneticiniz hesabınızı parola sıfırlama kullanacak biçimde yapılandırılmamış olduğundan şu anda parolanızı sıfırlayamazsınız. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve hesabınız parola sıfırlama için yapılandırma isteyin. Parola sıfırlama makaleyi okuyun için hesap yapılandırması hakkında daha fazla bilgi için [parola sıfırlama kullanıcılar için kullanıma alma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012:, Parola sıfırlama için etkinleştirilmiş bir grup üyesi değildir. Yönetici ve grubuna eklenmesi isteği başvurun. |
| UserNotProperlyConfigured = 14 | Özür dileriz <br> Gerekli bilgileri hesabınızdan eksik olduğundan şu anda parolanızı sıfırlayamazsınız. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticisine başvurun ve parolanızı sizin için sıfırlamasını isteyin. Hesabınıza yeniden erişiminiz sonra öğrenebilirsiniz nasıl makalesindeki adımları izleyerek gerekli bilgileri kayıt [Self Servis parola sıfırlama için kaydetme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-reset-register). | SSPR_0014: Ek güvenlik bilgileri, parolanızı sıfırlamak için gereklidir. Devam etmek için yöneticinize başvurun ve parolanızı sıfırlamak için isteyin. Hesabınıza erişim elde ettikten sonra ek güvenlik bilgisi https://aka.ms/ssprsetup adresindeki kaydedebilirsiniz. Yöneticiniz ek güvenlik bilgileri hesabınıza içindeki adımları izleyerek ekleyebilirsiniz [kümesi ve parola sıfırlama için okuma kimlik doğrulama verileri](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired 29 = | Özür dileriz <br> Kuruluşunuzun parola sıfırlama yapılandırması ile ilgili bir sorun nedeniyle biz şu anda parolanızı sıfırlayamazsınız. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve araştırmak için isteyin. Olası hakkında daha fazla bilgi için sorunu makaleyi okuyun [parola geri yazma sorun giderme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: Şirket içi yapılandırmanızda bir hata nedeniyle, parolanızı sıfırlamak alamıyoruz. Lütfen yöneticinize başvurun ve araştırmak için isteyin. |
| OnPremisesConnectivityError = 30 | Özür dileriz <br> Kuruluşunuz için bağlantı sorunu nedeniyle biz şu anda parolanızı sıfırlayamazsınız. Şu anda yapılacak eylemi yok, ancak daha sonra yeniden deneyin, sorun giderilebilir. Sorun devam ederse, lütfen yöneticinize başvurun ve araştırmak için isteyin. Okuma bağlantısı sorunları hakkında daha fazla bilgi edinmek için [sorun giderme parola geri yazma bağlantı](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Biz şirket içi ortamınız ile zayıf bağlantı nedeniyle, parolanızı sıfırlayamazsınız. Lütfen yöneticinize başvurun ve araştırmak için isteyin.|


## <a name="troubleshoot-password-reset-configuration-in-the-azure-portal"></a>Parola sıfırlama yapılandırma Azure portalında sorun giderme

| **Hata** | Çözüm |
| --- | --- |
| Görmezsiniz **parola sıfırlama** Azure portalında Azure AD altında bölümü | Bir Azure AD Premium veya işlemi gerçekleştiren yönetici atanmış temel lisans yoksa bu durum oluşabilir. <br> Bu makale kullanarak söz konusu yönetici hesabı için bir lisans atayarak çözülebilir [atayabilir, doğrulayın ve lisans sorunları gidermek](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Belirli yapılandırma seçeneği görmüyorum | Çok sayıda öğesi UI gerektiği kadar gizlenir. Görmek istediğiniz tüm seçenekleri etkinleştirmeyi deneyin. |
| Görmüyorum **şirket içi tümleştirme** sekmesi | Azure AD Connect indirilir ve parola geri yazma yapılandırılmışsa bu seçenek yalnızca görünür hale gelir. Bu konu hakkında daha fazla bilgi için bkz: [hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](./connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Parola sıfırlama raporlama sorunlarını giderme

| **Hata** | Çözüm |
| --- | --- |
| Görünür tüm parola yönetimi etkinlik türleri görmüyorum **Self Servis parola yönetimi** denetim olay kategorisi | Bir Azure AD Premium veya işlemi gerçekleştiren yönetici atanmış temel lisans yoksa bu durum oluşabilir. <br> Bu makale kullanarak söz konusu yönetici hesabı için bir lisans atayarak çözülebilir [atamak, doğrulayın ve lisans sorunları gidermek] |
| Birden çok kez kullanıcı kayıtlarını Göster | Bir kullanıcı kaydolduğunda, şu anda, şu anda ayrı bir olay olarak kayıtlı verileri tek tek her parçasının oturumunuzu. <br> Bu verileri toplamak istiyorsanız, raporu yükleyin ve daha fazla esneklik sağlamak için bir Özet Tablo excel gibi veri açın.

## <a name="troubleshoot-password-reset-registration-portal"></a>Parola sıfırlama kayıt portalı sorun giderme

| **Hata** | Çözüm |
| --- | --- |
| Dizin, parola sıfırlama için etkinleştirilmedi **yöneticiniz size bu özelliği kullanmak etkinleştirilmemiş** | Anahtar **Self Servis parola sıfırlama etkin** bayrağını **seçili** veya **tüm** tıklatıp **Kaydet** |
| Bir Azure AD Premium veya Basic lisansı atanmış kullanıcı yok **yöneticiniz size bu özelliği kullanmak etkinleştirilmemiş** | Bir Azure AD Premium veya işlemi gerçekleştiren yönetici atanmış temel lisans yoksa bu durum oluşabilir. <br> Bu makale kullanarak söz konusu yönetici hesabı için bir lisans atayarak çözülebilir [atayabilir, doğrulayın ve lisans sorunları gidermek](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| İstek işlenirken hata oluştu | Bu pek çok sorundan kaynaklanabilir, ancak genellikle bu hata tarafından bir hizmet kesintisi veya yapılandırma sorunu nedeniyle oluşur. Bu hatayı görürseniz ve işinizin etkileyen ek Yardım için Microsoft Destek'e başvurun. |

## <a name="troubleshoot-password-reset-portal"></a>Parola sıfırlama portalı sorun giderme

| **Hata** | Çözüm |
| --- | --- |
| Dizin, parola sıfırlama için etkinleştirilmedi. | Anahtar **Self Servis parola sıfırlama etkin** bayrağını **seçili** veya **tüm** tıklatıp **Kaydet** |
| Bir Azure AD Premium veya Basic lisansı atanmış kullanıcı yok | Bir Azure AD Premium veya işlemi gerçekleştiren yönetici atanmış temel lisans yoksa bu durum oluşabilir. <br> Bu makale kullanarak söz konusu yönetici hesabı için bir lisans atayarak çözülebilir [atayabilir, doğrulayın ve lisans sorunları gidermek](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) |
| Dizin, parola sıfırlama için etkin, ancak kullanıcı eksik veya yanlış biçimlendirilmiş kimlik doğrulama bilgilerine sahip değil | Bu kullanıcı kişi verilerini dizindeki devam etmeden önce dosyayı üzerinde düzgün bir şekilde oluşturulduğundan olun. Bu konu hakkında daha fazla bilgi için bkz: [Azure AD Self Servis parola sıfırlama tarafından kullanılan verileri](active-directory-passwords-data.md). |
| Dizin, parola sıfırlama için etkin, ancak ilke iki doğrulama adımları gerektirecek şekilde ayarlandığında, bir kullanıcı kişi verilerini bir parçasını dosyada yalnızca sahiptir. | Bu kullanıcının en az iki düzgün yapılandırılmış iletişim yöntemlerini sahip olduğundan emin olun (örnek: cep telefonu **ve** ofis telefonu) devam etmeden önce. |
| Dizin, parola sıfırlama için etkin ve kullanıcı düzgün şekilde yapılandırılmış, ancak kullanıcı kurulmasını alamıyor | Bu geçici hizmet hatası veya değil biz düzgün algılayamadı yanlış kişi verilerini sonucu olabilir. <br> <br> Varsa kullanıcının 10 saniye, yeniden deneyin bekler ve "yöneticinize başvurun" bağlantısı görüntülenir. Try tıklatarak yeniden çağrı yeniden deneme sayısı, "yöneticinize başvurun" tıklatarak ancak parola bu kullanıcı hesabı için gerçekleştirilecek sıfırlama isteyen yöneticiler için bir form e-posta gönderir. |
| Kullanıcı parola sıfırlama SMS veya telefon görüşmesi hiçbir zaman alır | Bu dizinde yanlış biçimlendirilmiş telefon numarası sonucu olabilir. Telefon numarası biçiminde olduğundan emin olun "+ bilgi xxxyyyzzzzXeeee". <br> <br> (Çağrı dağıtılmasından önce bunlar kaldırılır) dizininde bir tane belirtin olsa bile, parola sıfırlama uzantıları, desteklemez. Bir uzantısı olmadan bir numara kullanmayı deneyin veya uzantı PBX'iniz telefon numarasını içine tümleştirme. |
| Kullanıcı parola sıfırlama e-posta hiçbir zaman alır. | Bu sorunu en yaygın nedeni ileti istenmeyen posta Filtresi tarafından reddedilir olmasıdır. İstenmeyen posta, e-posta için Önemsiz veya silinen öğeler klasörünü denetleyin. <br> Ayrıca doğru e-posta iletisi için denetimi emin olun. |
| Bir parola sıfırlama İlkesi ayarlamış olmanız, ancak bir yönetici hesabı parola sıfırlama kullandığında, bu ilke uygulanmaz | Microsoft tarafından yönetilir ve denetim ilkesi yüksek düzeyde güvenlik sağlamak için yönetici parolasını sıfırlayamaz. |
| Kullanıcı parola sıfırlama bir gün içinde çok fazla kez çalışırken engelledi | Biz Otomatik azaltma mekanizmasını blok kullanıcılar için çok fazla kez bir kısa süre içinde parolalarını sıfırlamaya çalışırken uygulayın. Bu meydana zaman: <br> 1. Kullanıcı, bir telefon numarası bir saat içinde 5 kez doğrulamayı dener. <br> 2. Kullanıcı güvenlik soruları kapısı 5 kez bir saat içinde kullanmayı dener. <br> 3. Kullanıcı 5 kez bir saat içinde aynı kullanıcı hesabı için parola sıfırlama girişiminde bulunur. <br> Bu sorunu gidermek için 24 saat son girişimi ve kullanıcı ardından parolalarını sıfırlama kuramaz sonra beklenecek kullanıcı isteyin. |
| Kullanıcı telefon numarasını doğrulanırken bir hata görür | Girdiğiniz telefon numarası dosya telefon numarasının eşleşmiyor Bu hata oluşur. Kullanıcı parola sıfırlama için telefon tabanlı bir yöntemini kullanmaya çalışılırken zaman alan ve ülke kodu da dahil olmak üzere tam telefon numarasını girdiğinden emin olun. |
| İstek işlenirken hata oluştu | Bu pek çok sorundan kaynaklanabilir, ancak genellikle bu hata tarafından bir hizmet kesintisi veya yapılandırma sorunu nedeniyle oluşur. Bu hatayı görürseniz ve işinizin etkileyen ek Yardım için Microsoft Destek'e başvurun. |

## <a name="troubleshoot-password-writeback"></a>Parola geri yazma sorunlarını giderme

| **Hata** | Çözüm |
| --- | --- |
| Parola sıfırlama hizmeti şirket içi Azure AD Connect makinenin uygulama olay günlüğünde hata 6800 ile başlamaz. <br> <br> Federe ekleme ya da parola karması eşitlenen sonra kullanıcıların parolalarını sıfırlayamazsınız. | Parola geri yazma etkinleştirildiğinde, eşitleme altyapısı için bulut ekleme hizmeti konuşarak (hazırlama) yapılandırmasını gerçekleştirmek için geri yazma kitaplığı çağırır. Hazırlama sırasında veya parola geri yazma özelliğini Azure AD Connect makinenizin olay günlüğündeki olay günlüğündeki hatalara sonuçlanır için WCF uç noktası başlatma sırasında karşılaşılan hataları. <br> <br> Geri yazma yapılandırıldıysa, ADSync hizmeti yeniden başlatma sırasında WCF uç noktası başlatıldıktan. Ancak, uç nokta başlatma başarısız olursa, şu olay 6800 oturum ve eşitleme hizmeti başlangıç izin. Bu olay varlığını parola uç noktası başlatılmadı yukarı geri yazma özelliğini anlamına gelir. Bileşen neden uç nokta başlatılamadı gösterir ait tarafından olay günlüğü ayrıntıları olay günlüğü girişleri ile birlikte bu olay (6800) oluşturur. Bu olay günlüğü hatalarını gözden geçirin ve parola geri yazma hala çalışmıyorsa, Azure AD Connect'i yeniden başlatmayı deneyin. Sorun devam ederse, devre dışı bırakın ve yeniden parola geri yazma özelliğini etkinleştirmek deneyin.
| Kullanıcı parola sıfırlama veya parola geri yazma etkinleştirilmiş bir hesap kilidini açma girişiminde bulunduğunda, işlem başarısız olur. <br> <br> Ayrıca, Azure AD Connect içeren olay günlüğü içindeki bir olay görürsünüz: "eşitleme altyapısı hata hr döndürülen 800700CE, = ileti filename = veya uzantısı çok uzun" kilit açma işlemi tamamlandıktan sonra. | Active Directory hesabı için Azure AD Connect bulmak ve en çok 127 karakter içermesi için parolayı sıfırlayın. Daha sonra eşitleme hizmeti Başlat menüsünden açmak. Bağlayıcılar gidin ve Active Directory bağlayıcısını bulun. Seçin ve Özellikler'i tıklatın. Sayfa kimlik gidin ve yeni bir parola girin. Sayfayı kapatmak için Tamam'ı seçin. |
| Azure AD Connect yükleme işleminin son adımında, parola geri yazma yapılandırılamadı belirten bir hata görürsünüz. <br> <br> Azure AD Connect uygulama olay günlüğü "Kimlik doğrulama belirteci alma hatası" hata 32009 metinle içerir. | Bu hata aşağıdaki iki durumlarda oluşur:<br> <br> a. Azure AD Connect yükleme işleminin başında belirtilen genel yönetici hesabı için hatalı bir parola belirttiniz.<br> b. Azure AD Connect yükleme işleminin başında belirtilen genel yönetici hesabı için bir Federasyon kullanıcısı kullanma girişiminde bulundunuz.<br> <br> Bu hatayı düzeltmek için birleştirilmiş bir hesap için genel yönetici kullanarak Azure AD Connect yükleme işleminin başında belirtilen ve belirtilen parolanın doğru olduğunu olmadığından emin olun. |
| Azure AD Connect makine olay günlüğü hatası ait tarafından oluşturulan 32002 içerir. <br> <br> Hata okur: "hata ServiceBus için bağlanmak, belirteç sağlayıcı güvenlik belirteci... sağlayamadı" | Şirket içi ortamınız bulutta service bus uç noktası bağlanabiliyor değil. Bu hata genellikle belirli bir bağlantı noktası veya web adresi giden bir bağlantı engelleyen bir güvenlik duvarı kuralı kaynaklanır. Bkz: [bağlantı Önkoşullar](./connect/active-directory-aadconnect-prerequisites.md) daha fazla bilgi için. Bu kurallar güncelleştirdikten sonra Azure AD Connect makineyi yeniden başlatmak ve parola geri yazma yeniden çalışmaya başlamanız gerekir. |
| Bazı zaman, Federasyon veya parola karması için çalışma eşitlenen sonra kullanıcıların parolalarını sıfırlayamazsınız. | Bazı nadir durumlarda, Azure AD Connect'i yeniden başlatıldığında yeniden başlatmak parola geri yazma hizmet başarısız olabilir. Bu durumlarda, ilk olarak, parola geri yazma etkinleştirilmiş-tesis içi görünüp görünmediğini denetleyin Bu yapılabilir Azure AD Connect Sihirbazı veya powershell (yukarıdaki bkz HowTos bölümünde) kullanarak. Özelliğin etkinleştirilmesi için görünürse etkinleştirme veya özelliği yeniden kullanıcı Arabirimi veya PowerShell yoluyla devre dışı bırakmayı deneyin. Bu işe yaramazsa, tamamen kaldırıp Azure AD Connect'i yeniden yüklemeyi deneyin. |
| Federe veya hizmet ile ilgili bir sorun oluştu belirten parola gönderdikten sonra bir hata parolalarını sıfırlama girişimi parola karması eşitlenen kullanıcılar bakın. <br ><br> Buna ek olarak, parola sıfırlama işlemleri sırasında yönetim Aracısı ile ilgili bir hata oluştu, şirket içi olay günlüklerinde erişim reddedildi görebilirsiniz. | Olay Günlüğü'nde bu hatalar görürseniz (Sihirbazı'nda yapılandırma sırasında belirtilen) AD MA hesabı parola geri yazma için gerekli izinlere sahip olduğunu doğrulayın. <br> <br> **Bu izin verildikten sonra sdprop arka plan görevi DC'de aracılığıyla aşağı trickle izni 1 saat kadar sürebilir.** <br> <br> Çalışmak amacıyla parola sıfırlama için parolasını sıfırlama kullanıcı nesnesinin güvenlik tanımlayıcı damgalı izniniz gerekiyor. Kullanıcı nesnesindeki bu izni görüntülenir kadar parola sıfırlama erişim reddedildi hatasıyla başarısız olmaya devam eder. |
| Federe veya hizmet ile ilgili bir sorun oluştu belirten parola gönderdikten sonra bir hata parolalarını sıfırlama girişimi parola karması eşitlenen kullanıcılar bakın. <br> <br> Buna ek olarak, parola sıfırlama işlemleri sırasında bir hata, olay günlüklerinde "Nesne bulunamadı" hata belirten Azure AD Connect hizmetinden görebilirsiniz. | Bu hata genellikle eşitleme altyapısı bulamıyor kullanıcı nesnesi AAD bağlayıcı alanı veya bağlantılı MV veya AD bağlayıcı alanı nesne olduğunu gösterir. <br> <br> Bu sorunu gidermek için kullanıcının gerçekten şirket içinden Azure AD Connect geçerli örneği aracılığıyla aad'ye eşitlendiğinden emin olun ve bağlayıcı alanları ve MV nesneleri durumunu inceleyin. AD CS Nesne "Microsoft.InfromADUserAccountEnabled.xxx" kuralı aracılığıyla MV nesne bağlayıcıya olduğunu doğrulayın.|
| Federe veya hizmet ile ilgili bir sorun oluştu belirten parola gönderdikten sonra bir hata parolalarını sıfırlama girişimi parola karması eşitlenen kullanıcılar bakın. <br> <br> Buna ek olarak, parola sıfırlama işlemleri sırasında bir hata, olay günlüklerinde "birden çok eşleşme bulunamadı" hatası gösteren Azure AD Connect hizmetinden görebilirsiniz. | Bu, eşitleme altyapısı MV nesnesi "Microsoft.InfromADUserAccountEnabled.xxx" aracılığıyla bağlı birden fazla AD CS nesneleri olduğunu algıladı gösterir. Bu, kullanıcı birden fazla ormanda etkinleştirilmiş bir hesabı olduğu anlamına gelir. **Bu senaryo, parola geri yazma için desteklenmiyor.** |
| Parola işlemleri, bir yapılandırma hatasıyla başarısız oluyor. Uygulama olay günlüğü içerir <br> <br> Azure AD Connect hata 6329 metinle: 0x8023061f (Parola Eşitleme etkin değil. Bu yönetim Aracısı işlemi başarısız oldu.) | Bu, Azure AD Connect yapılandırmasının yeni bir AD ormanına eklemek için (veya kaldırın ve mevcut bir ormana readd) sonra değiştirilirse parola geri yazma özelliği zaten etkin oluşur. Parola işlemleri gibi kullanıcılar için yeni orman başarısız eklendi. Sorunu düzeltmek için devre dışı bırakın ve orman yapılandırma değişiklikleri tamamladıktan sonra parola geri yazma özelliğini yeniden etkinleştirin. |

## <a name="password-writeback-event-log-error-codes"></a>Parola geri yazma olay günlüğü hata kodları

En iyi uygulama parola geri yazma ile ilgili sorunları giderme Azure AD Connect makinenizi o uygulama olay günlüğünü incelemek üzere olduğunda. Bu olay günlüğü olayı ilgilenilen iki kaynaklardan parola geri yazma için içerir. Ait kaynak işlemlerinin ve parola geri yazma işlemi için ilgili sorunlar açıklanmaktadır. ADSync kaynak işlemlerinin ve parolaları AD ortamınızda ayarlamakla ilgili sorunları açıklar.

### <a name="source-of-event-is-adsync"></a>Olay ADSync kaynağıdır

| Kod | Ad/iletisi | Açıklama |
| --- | --- | --- |
| 6329 | BAIL: MMS(4924) 0x80230619 – "bir kısıtlama parola belirtilen geçerli bir değiştirilen engeller." | Bu olay, parola geri yazma hizmet parola geçerlilik süresi, geçmiş, karmaşıklık veya etki alanının filtreleme gereksinimlerini karşılamıyor, yerel dizin parola ayarlama girişiminde bulunduğunda oluşur. <br> <br> En az parola geçerlilik süresi varsa ve bu zaman aralığı içinde parola kısa süre önce değiştirilmiştir, etki alanınızda belirtilen yaş ulaşana kadar parolayı tekrar değiştiremezsiniz değildir. Test amacıyla, minimum yaş 0 olarak ayarlanması gerekir. <br> <br> Parola geçmişi gereksinimlerini etkin olması durumunda, son N kez kullanılmayan bir parola seçmeniz gerekir burada N parola geçmişi ayarı. En son N zamanlarını kullanılan bir parola seçin, ardından, bir hata bu durumda görürsünüz. Test amacıyla, geçmiş 0 olarak ayarlanması gerekir. <br> <br> Parola karmaşıklık gereksinimleri varsa, kullanıcı değiştirme veya bir parola sıfırlama girişiminde bulunduğunda, bunların tümünün uygulanır. <br> <br> Parola filtreleri etkinleştirilmiş varsa ve bir kullanıcı bir parola seçtikten sonra sıfırlama filtreleme ölçütlerini karşılamıyor veya değiştirme işlemi başarısız olur. |
| 6329 | MMS(3040): admaexport.cpp(2837): sunucu LDAP Parola İlkesi denetimi içermiyor. | LDAP_SERVER_POLICY_HINTS_OID denetimi (1.2.840.113556.1.4.2066) etki alanı denetleyicilerinde etkin değilse bu sorun oluşur. Parola geri yazma özelliğini kullanmak için denetimi etkinleştirmeniz gerekir. Bunu yapmak için etki alanı denetleyicileri (en son SP ile) Windows Server 2008 veya üzeri olması gerekir. (R2 öncesi) 2008'de, DC'lerin olan sonra düzeltmeyi uygulamanız gerekir [KB2386717](http://support.microsoft.com/kb/2386717). |
| HR 8023042 | Eşitleme altyapısı döndürülen bir hata hr = 80230402, ileti aynı bağlantı ile yinelenen girdiler olduğundan başarısız oldu nesneyi alma girişimi = | Bu olay için aynı kullanıcı kimliğini birden çok etki alanında etkin olduğunda oluşur. Örneğin, hesap/kaynak ormanına eşitleniyor ve aynı kullanıcı kimliği varsa ve her etkin varsa, bu hata oluşabilir. <br> <br> Bu hata, benzersiz olmayan bağlayıcı öznitelik (örneğin, diğer ad veya UPN) kullanıyorsanız ve iki kullanıcı o aynı bağlantı özniteliği paylaşan da oluşabilir. <br> <br> Bu sorunu çözmek için etki alanı içinde yinelenen kullanıcılardan yoksa ve her kullanıcı için bir benzersiz bağlantı özniteliğini kullanarak emin olun. |

### <a name="source-of-event-is-passwordresetservice"></a>Olay kaynağına ait değil

| Kod | Ad/iletisi | Açıklama |
| --- | --- | --- |
| 31001 | PasswordResetStart | Bu olay, şirket içi hizmet isteği buluttan kaynaklanan bir federe veya parola karma eşitlenmiş kullanıcı için bir parola sıfırlama algılanan gösterir. Bu olay ilk olay her parola geri yazma işlemi sıfırlanır. |
| 31002 | PasswordResetSuccess | Bu olay, bir kullanıcı bir parola sıfırlama işlemi sırasında yeni bir parola seçili, biz bu parola Kurumsal parola gereksinimlerini karşıladığından ve parola geri yerel AD ortama yazıldı belirlenen gösterir. |
| 31003 | PasswordResetFail | Bu olay, bir kullanıcı bir parola seçili ve parola başarıyla şirket içi ortama geldi, ancak biz yerel AD ortamında parola ayarlamanız çalışırken bir hata oluştu gösterir. Bu durum çeşitli nedenlerle oluşabilir: <br> <br> Kullanıcının parolasını karşılamıyor yaşı, geçmiş, karmaşıklık, veya etki alanı için gereksinimleri filtre. Bu sorunu çözmek için tamamen yeni bir parola deneyin. <br> <br> MA hizmet hesabının söz konusu kullanıcı hesabı üzerinde yeni bir parola ayarlamak için uygun izinlere sahip değil. <br> <br> Kullanıcının parolasını ayarlama işlemleri izin vermez, etki alanı veya enterprise admins gibi bir korumalı grup içindeki hesabıdır. |
| 31004 | OnboardingEventStart | Bu olay parola geri yazma özelliğini Azure AD Connect ile etkinleştirirseniz gerçekleşir ve biz kuruluşunuzun parola geri yazma web hizmetine ekleme başladı gösterir. |
| 31005 | OnboardingEventSuccess | Bu olay, onboarding işlemi başarılı oldu ve parola geri yazma özelliği kullanıma hazır olduğunu gösterir. |
| 31006 | ChangePasswordStart | Bu olay, şirket içi hizmeti buluttan kaynaklanan bir federe veya parola karma eşitlenmiş kullanıcı için bir parola değiştirme isteği algıladı gösterir. Bu olay her parola değişikliği geri yazma işleminin ilk etkinliğidir. |
| 31007 | ChangePasswordSuccess | Bu olay, bir kullanıcı yeni bir parola parola değiştirme işlemi sırasında seçilen, biz bu parola Kurumsal parola gereksinimlerini karşıladığından ve parola geri yerel AD ortama yazıldı belirlenen gösterir. |
| 31008 | ChangePasswordFail | Bu olay, bir kullanıcı bir parola seçili ve parola başarıyla şirket içi ortama geldi, ancak biz yerel AD ortamında parola ayarlamanız çalışırken bir hata oluştu gösterir. Bu durum çeşitli nedenlerle oluşabilir: <br> <br> Kullanıcının parolasını karşılamıyor yaşı, geçmiş, karmaşıklık, veya etki alanı için gereksinimleri filtre. Bu sorunu çözmek için tamamen yeni bir parola deneyin. <br> <br> MA hizmet hesabının söz konusu kullanıcı hesabı üzerinde yeni bir parola ayarlamak için uygun izinlere sahip değil. <br> <br> Kullanıcının parolasını ayarlama işlemleri izin vermez, etki alanı veya enterprise admins gibi bir korumalı grup içindeki hesabıdır. |
| 31009 | ResetUserPasswordByAdminStart | Şirket içi hizmet isteği bir Federasyon için bir parola sıfırlama veya bir kullanıcı adına yöneticisinden kaynaklanan kullanıcı parola karmasını eşitlenen algıladı. Her yönetici tarafından başlatılan parola sıfırlama geri yazma işleminin ilk olay olaydır. |
| 31010 | ResetUserPasswordByAdminSuccess | Yönetici, bir yönetici tarafından başlatılan parola sıfırlama işlemi sırasında yeni bir parola seçili, biz bu parola Kurumsal parola gereksinimlerini karşıladığından ve parola geri yerel AD ortama yazıldı belirledi. |
| 31011 | ResetUserPasswordByAdminFail | Yönetici bir kullanıcı adına bir parola seçili ve parola başarıyla şirket içi ortama geldi, ancak biz yerel AD ortamında parola ayarlamanız çalışırken bir hata oluştu. Bu durum çeşitli nedenlerle oluşabilir: <br> <br> Kullanıcının parolasını karşılamıyor yaşı, geçmiş, karmaşıklık, veya etki alanı için gereksinimleri filtre. Bu sorunu çözmek için tamamen yeni bir parola deneyin. <br> <br> MA hizmet hesabının söz konusu kullanıcı hesabı üzerinde yeni bir parola ayarlamak için uygun izinlere sahip değil. <br> <br> Kullanıcının parolasını ayarlama işlemleri izin vermez, etki alanı veya enterprise admins gibi bir korumalı grup içindeki hesabıdır. |
| 31012 | OffboardingEventStart | Bu olay, Azure AD Connect ile parola geri yazma özelliğini devre dışı bırakırsanız oluşur ve biz kuruluşunuzun parola geri yazma web hizmeti için çıkarma başlatıldı gösterir. |
| 31013| OffboardingEventSuccess| Bu olay çıkarma işlemi başarılı oldu ve parola geri yazma yeteneği başarıyla devre dışı olduğunu gösterir. |
| 31014| OffboardingEventFail| Bu olay çıkarma işlemi başarılı değil gösterir. Bu yapılandırması sırasında belirtilen Bulut veya şirket içi yönetici hesabı üzerindeki izinleri hatası nedeniyle olabilir veya parola geri yazma özelliğini devre dışı bırakılırken bir Federasyon bulut genel yönetici kullanmaya çalışıyorsunuz. Bu sorunu gidermek için yönetim denetleme izinleri ve herhangi bir kullanmıyorsanız federe hesabı parola geri yazma özelliği yapılandırılırken.|
| 31015| WriteBackServiceStarted| Bu olay, parola geri yazma hizmeti başarıyla başlatıldı ve bulutta parola yönetimi isteklerini kabul etmeye hazır olduğunu gösterir.|
| 31016| WriteBackServiceStopped| Bu olay, parola geri yazma Hizmeti durdu ve bulut gelen tüm parola yönetimi isteklerin başarılı olmaz gösterir.|
| 31017| AuthTokenSuccess| Bu olay, biz çıkarma veya ekleme işlemini başlatmak için Azure AD Connect Kurulum sırasında belirtilen genel yönetici için bir yetkilendirme belirteci başarıyla alındı gösterir.|
| 31018| KeyPairCreationSuccess| Bu olay, başarılı bir şekilde şirket içi ortamınız için gönderilecek bulutta parolaları şifrelemek için kullanılan şifreleme anahtarını oluşturduğumuz gösterir.|
| 32000| Başvuruları| Bu olay, bir parola yönetimi işlemi sırasında bilinmeyen bir hata gösterir. Daha fazla ayrıntı için olay özel durum metni bakın. Sorun yaşıyorsanız, devre dışı bırakma ve yeniden etkinleştirme parola geri yazma deneyin. Bu yardımcı olmazsa, olay günlüğünüzü belirtilen izleme kimliği ile birlikte bir kopyasını dahil destek mühendisinize Insider.|
| 32001| ServiceError| Bu olay oluştu bulut parola bağlanırken bir hata hizmetini sıfırlamak gösterir ve genellikle şirket içi servis parola sıfırlama web hizmetine bağlanamadı oluşur.|
| 32002| ServiceBusError| Bu olay, kiracının hizmet veri yolu örneğine bağlanırken bir hata oluştu gösterir. Giden bağlantılar, şirket içi ortamınızda engelleme nedeni bu olabilir. TCP 443 üzerinden ve https://ssprsbprodncu-sb.accesscontrol.windows.net/ bağlantılara izin ver ve yeniden deneyin emin olmak için güvenlik duvarınızı denetleyin. Hala sorun yaşıyorsanız, devre dışı bırakma ve yeniden etkinleştirme parola geri yazma deneyin.|
| 32003| InPutValidationError| Bu olay, bizim web hizmeti API'sine geçirilen giriş geçersiz olduğunu gösterir. İşlemi yeniden deneyin.|
| 32004| DecryptionError| Bu olay buluttan gelen parola çözme bir hata olduğunu gösterir. Bu, bulut hizmeti ve şirket içi ortamınız arasında bir şifre çözme anahtarı uyuşmazlığı nedeniyle olabilir. Bu sorunu çözmek için devre dışı bırakın ve parola geri yazma, şirket içi ortamınızda yeniden etkinleştirin.|
| 32005| ConfigurationError| Onboarding işlemi sırasında bir yapılandırma dosyası, şirket içi ortamınızda Kiracı özgü bilgileri kaydedin. Bu olay, bu dosya kaydedilirken bir hata oluştu veya hizmeti var. başlatıldığında bir hata dosyası okunurken edildi gösterir. Bu sorunu gidermek için devre dışı bırakma ve yeniden etkinleştirme bu yapılandırma dosyası yeniden zorlamak için geri yazma parola deneyin.|
| 32007| OnBoardingConfigUpdateError| Onboarding işlemi sırasında hizmet bulut verileri şirket içi parola sıfırlama gönderin. Bu veriler, sonra eşitleme hizmeti için bu bilgiyi güvenli bir şekilde diskte saklamak için gönderilmeden önce bir bellek içi dosyasına yazılır. Bu olay, yazma ya da bu verileri bellekte güncelleştirme ile ilgili bir sorun gösterir. Bu sorunu gidermek için devre dışı bırakma ve yeniden etkinleştirme Bu yapılandırmanın bir yeniden yazma zorlamak için geri yazma parola deneyin.|
| 32008| ValidationError| Bu olay, parola sıfırlama web hizmetinden geçersiz bir yanıt aldık gösterir. Bu sorunu gidermek için devre dışı bırakma ve yeniden etkinleştirme parola geri yazma deneyin.|
| 32009| AuthTokenError| Bu olay, biz bir yetkilendirme Azure AD Connect Kurulum sırasında belirtilen genel yönetici hesabı için belirteç alınamadı olduğunu gösterir. Hatalı kullanıcı adı tarafından bu hataya neden olabilir veya parola genel yönetici hesabı için belirtilen veya genel yönetici hesabı belirtilmediğinden Federasyon. Bu sorunu gidermek için doğru kullanıcı adını ve parolayı yapılandırmayla yeniden çalıştırın ve yönetilen bir (yalnızca Bulut veya parola eşitlenmiş) hesap yönetici olduğundan emin olun.|
| 32010| CryptoError| Bu olay, parola oluşturulurken bir hata oluştu gösterir şifreleme anahtarı veya Bulut hizmetinden ulaşan parola çözme. Bu hata muhtemelen ortamınız ile ilgili bir sorunu gösterir. Daha fazla bilgi edinmek ve bu sorunu çözmek için olay günlüğünün ayrıntılarını inceleyin. Ayrıca deneyebilirsiniz devre dışı bırakma ve bu sorunu gidermek için parola geri yazma hizmetini yeniden etkinleştirme.|
| 32011| OnBoardingServiceError| Bu olay, şirket içi hizmet düzgün ekleme işlemini başlatmak için parola sıfırlama web hizmetiyle iletişim kuramadı olduğunu gösterir. Bu bir güvenlik duvarı kuralı veya kiracınız için bir kimlik doğrulama belirteci alınırken bir sorun nedeniyle olabilir. Bu sorunu gidermek için emin olun, giden bağlantılar TCP 443 ve TCP 9350-9354 veya https://ssprsbprodncu-sb.accesscontrol.windows.net/ için üzerinden engellemediğinden ve kullanmakta olduğunuz için yerleşik AAD yönetici hesabı birleşik.|
| 32013| OffBoardingError| Bu olay, şirket içi hizmet düzgün çıkarma işlemini başlatmak için parola sıfırlama web hizmetiyle iletişim kuramadı olduğunu gösterir. Bu bir güvenlik duvarı kuralı veya kiracınız için bir yetki belirteci alınırken bir sorun nedeniyle olabilir. Bu sorunu gidermek için giden bağlantılar 443 üzerinden veya https://ssprsbprodncu-sb.accesscontrol.windows.net/ engellemediğinden ve AAD yönetici hesabı için offboard kullanıyorsanız birleşik emin olun.|
| 32014| ServiceBusWarning| Bu olay, biz, kiracının hizmet veri yolu örneğine bağlanmak için yeniden deneme gerekiyordu gösterir. Normal koşullar altında bu değil ilgili bir sorun olabilir, ancak çoğu zaman, bu olay görürseniz, özellikle bir gecikme süresi yüksek veya düşük bant genişlikli bağlantı olması durumunda, hizmet veri yolu, ağ bağlantısı denetleniyor göz önünde bulundurun.|
| 32015| ReportServiceHealthError| Parola geri yazma Hizmetinizin durumunu izlemek için her 5 dakikada bir web hizmeti sinyal veri bizim parola sıfırlama gönderin. Bu olay, bu sistem durumu bilgileri bulut web hizmetine gönderirken bir hata olduğunu gösterir. Bu sistem durumu bilgileri OII veya PII verilerini dahil etmez ve böylece hizmet durumu bilgileri bulutta sağladığımız zamanıyla ilgili bir sinyal ve temel hizmeti istatistikleri.|
| 33001| ADUnKnownError| Bu olay, AD tarafından döndürülen bilinmeyen bir hata olduğunu gösterir, bu hata hakkında daha fazla bilgi için ADSync kaynağından olayları için Azure AD Connect sunucusunun olay günlüğünü denetleyin.|
| 33002| ADUserNotFoundError| Bu olay, sıfırlamak veya parola değiştirme çalışıyor kullanıcının şirket içi dizininde bulunamadı gösterir. Bu kullanıcı silinen şirket içi kaldığında ancak bulutta oluşabilir veya Eşitleme ile ilgili bir sorun varsa. Eşitleme günlüklerinizi denetleyin ve daha fazla bilgi için ayrıntıları son birkaç eşitleme çalıştırın.|
| 33003| ADMutliMatchError| Parola sıfırlama veya buluttan değişiklik isteğinin kaynaklandığı, Azure AD Connect Kurulum işlemi sırasında belirtilen bulut bağlantı bir kullanıcı, şirket içi ortamınızda bu isteği yeniden bağlamak nasıl belirlemek için kullanırız. Bu olay, iki kullanıcı aynı bulut bağlantı özniteliği ile şirket içi dizininizdeki bulduk olduğunu gösterir. Eşitleme günlüklerinizi denetleyin ve daha fazla bilgi için ayrıntıları son birkaç eşitleme çalıştırın.|
| 33004| ADPermissionsError| Bu olay, yönetim aracısı hizmet hesabı yeni bir parola ayarlamak için söz konusu hesabı uygun izinleri yok gösterir. Kullanıcının ormanındaki MA hesabı ormandaki tüm nesneler üzerinde sıfırlama ve değiştirme parola izinleri olduğundan emin olun. Hakkında daha fazla bilgi için bunun için adım 4'e bakın: uygun Active Directory izinlerini ayarlayabilirsiniz.|
| 33005| ADUserAccountDisabled| Bu olay, biz sıfırlama veya şirket içinde devre dışı bırakılmış bir hesap için bir parola değiştirme girişiminde bulunuldu gösterir. Hesabı etkinleştirin ve işlemi yeniden deneyin.|
| 33006| ADUserAccountLockedOut| Olay biz sıfırlama veya şirket içinde kilitli bir hesap için bir parola değiştirme girişiminde bulunuldu gösterir. Bir kullanıcı bir değişiklik denediniz veya parola işlemi çok fazla kez kısa bir süre içinde sıfırlama kilitlemeleri oluşabilir. Hesabın kilidini açıp işlemi yeniden deneyin.|
| 33007| ADUserIncorrectPassword| Bu olay, bir parola gerçekleştirme değiştirdiğinizde işlemi kullanıcı yanlış bir geçerli parola belirtilen gösterir. Doğru geçerli parolayı belirtin ve yeniden deneyin.|
| 33008| ADPasswordPolicyError| Bu olay, parola geri yazma hizmet parola geçerlilik süresi, geçmiş, karmaşıklık veya etki alanının filtreleme gereksinimlerini karşılamıyor, yerel dizin parola ayarlama girişiminde bulunduğunda oluşur. <br> <br> En az parola geçerlilik süresi varsa ve bu zaman aralığı içinde parola kısa süre önce değiştirilmiştir, etki alanınızda belirtilen yaş ulaşana kadar parolayı tekrar değiştiremezsiniz değildir. Test amacıyla, minimum yaş 0 olarak ayarlanması gerekir. <br> <br> Parola geçmişi gereksinimlerini etkin olması durumunda, son N kez kullanılmayan bir parola seçmeniz gerekir burada N parola geçmişi ayarı. En son N zamanlarını kullanılan bir parola seçin, ardından, bir hata bu durumda görürsünüz. Test amacıyla, geçmiş 0 olarak ayarlanması gerekir. <br> <br> Parola karmaşıklık gereksinimleri varsa, kullanıcı değiştirme veya bir parola sıfırlama girişiminde bulunduğunda, bunların tümünün uygulanır. <br> <br> Parola filtreleri etkinleştirilmiş varsa ve bir kullanıcı bir parola seçtikten sonra sıfırlama filtreleme ölçütlerini karşılamıyor veya değiştirme işlemi başarısız olur.|
| 33009| ADConfigurationError| Bu olay, bir parola geri şirket içi dizininize bir yapılandırma sorunu nedeniyle Active Directory ile bir sorun yazma vardı gösterir. ADSync hizmeti hangi hata hakkında daha fazla bilgi için gelen iletiler için Azure AD Connect makinenin uygulama olay günlüğünü denetleyin.|

## <a name="troubleshoot-password-writeback-connectivity"></a>Parola geri yazma bağlantı sorunlarını giderme

Azure AD Connect hizmet kesintilerine parola geri yazma bileşeni ile karşılaşıyorsanız, bu sorunu gidermek için bazı hızlı adımlar şunlardır:

* [Ağ bağlantısını doğrulayın](#confirm-network-connectivity)
* [Yeniden başlatma Azure AD Connect eşitleme hizmeti](#restart-the-azure-ad-connect-sync-service)
* [Devre dışı bırakın ve parola geri yazma özelliğini yeniden etkinleştirin](#disable-and-re-enable-the-password-writeback-feature)
* [En son Azure AD Connect sürümü yüklemek](#install-the-latest-azure-ad-connect-release)
* [Parola Geri Yazma Sorunlarını Giderme](#troubleshoot-password-writeback)

Genel olarak, sırayla hizmetinizi en hızlı bir şekilde kurtarmak için yukarıdaki adımları yürütün öneririz.

### <a name="confirm-network-connectivity"></a>Ağ bağlantısını doğrulayın

En yaygın hatası, güvenlik duvarının noktasıdır ve veya proxy bağlantı noktalarını ve boşta kalma zaman aşımı yanlış yapılandırılmış. Makalede bağlantı önkoşulları gözden geçirin [Azure AD Connect için Önkoşullar](./connect/active-directory-aadconnect-prerequisites.md) daha fazla bilgi için.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Yeniden başlatma Azure AD Connect eşitleme hizmeti

Azure AD Connect eşitleme hizmeti yeniden bağlantı sorunları veya hizmeti ile geçici diğer sorunları çözmeye yardımcı olabilir.

1. Yönetici olarak **Başlat** çalıştıran sunucuda **Azure AD Connect**.
2. Tür **"Services.msc olanağını"** basın ve arama kutusuna **Enter**.
3. Ara **Microsoft Azure AD eşitleme** girişi.
4. Hizmeti girişinde sağ tıklayın, **yeniden**ve işlemin tamamlanmasını bekleyin.

   ![Azure AD eşitleme hizmetini yeniden başlatın][Service Restart]

Bu adımları, bulut hizmeti ile yeniden bağlantı ve yaşıyor olabilir kesintiler çözümleyin. Eşitleme hizmeti yeniden başlatmak sorunu çözmezse, devre dışı bırakın ve bir sonraki adım parola geri yazma özelliğini yeniden etkinleştirmek denemenizi öneririz.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Devre dışı bırakın ve parola geri yazma özelliğini yeniden etkinleştirin

Devre dışı bırakma ve parola geri yazma özelliğini yeniden etkinleştirme bağlantı sorunları çözmeye yardımcı olabilir.

1. Bir yönetici olarak açın **Azure AD Connect Yapılandırma Sihirbazı'nı**.
2. Üzerinde **Azure ad Connect** iletişim kutusunda, girin, **Azure AD genel yönetici kimlik bilgileri**
3. Üzerinde **AD DS'ye Bağlan** iletişim kutusunda, girin, **AD etki alanı Hizmetleri yönetici kimlik bilgileri**.
4. Üzerinde **kullanıcılarınızı benzersiz olarak tanımlama** iletişim kutusunda, tıklatın **sonraki** düğmesi.
5. Üzerinde **isteğe bağlı özellikler** iletişim kutusunun işaretini kaldırın **parola geri yazma** onay kutusu.
6. Tıklatın **sonraki** için elde edene kadar değiştirmeden herhangi bir şey kalan iletişim sayfaları aracılığıyla **yapılandırma için hazır** sayfası.
7. Yapılandır sayfası gösterir emin **devre dışı olarak parola geri yazma seçeneği** ve yeşil ardından **yapılandırma** değişikliklerinizi uygulamak için düğmesini.
8. Üzerinde **tamamlandı** iletişim kutusunda, seçimini **Şimdi Eşitle** seçeneğini ve ardından **son** sihirbazı kapatın.
9. Yeniden **Azure AD Connect Yapılandırma Sihirbazı'nı**.
10. **2-8 arasındaki adımları yineleyin**, olmanız dışında **parola geri yazma seçeneği işaretleyin** üzerinde **isteğe bağlı özellikler** ekran hizmetini yeniden etkinleştirin.

Bu adımları bağlantınızı bizim bulut hizmeti ile yeniden oluşturmak ve yaşıyor olabilir kesintiler çözümleyin.

Devre dışı bırakma ve parola geri yazma özelliğini yeniden etkinleştirme sorununuzu çözmezse, sonraki adım olarak Azure AD Connect'i yeniden yüklemeyi deneyin öneririz.

### <a name="install-the-latest-azure-ad-connect-release"></a>En son Azure AD Connect sürümü yüklemek

Azure AD Connect'i yeniden yapılandırma ve bulut hizmetlerimizi ve yerel AD ortamınızı arasında bağlantı sorunları çözebilirsiniz.

Öneririz, yalnızca yukarıda açıklanan ilk iki adım denemeden sonra bu adımı gerçekleştirin.

> [!WARNING]
> Giden kutusu eşitleme kurallarının özelleştirdiyseniz **yükseltmeye devam etmeden önce bu yedeklemek ve tamamladıktan sonra bunları el ile yeniden**.

1. Azure AD Connect'ten en son sürümünü indirme [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
2. Azure AD Connect zaten yüklü olduğundan, Azure AD Connect yüklemenizi en son sürüme güncelleştirmek için yerinde yükseltme gerçekleştirmek için gerekir.
3. İndirilen paketi yürütmek ve izleyin ekrandaki yönergeleri Azure AD Connect makinenizi güncelleştirin.

Bu adımları bağlantınızı bizim bulut hizmeti ile yeniden oluşturun ve yaşıyor olabilir kesintiler çözümleyin.

Azure AD Connect sunucusu en son sürümünü yükleme sorunu çözmezse, en son sürüm yükledikten sonra son adım olarak devre dışı bırakma ve yeniden etkinleştirme parola geri yazma deneyin öneririz.

## <a name="verify-whether-azure-ad-connect-has-the-required-permission-for-password-writeback"></a>Azure AD Connect parola geri yazma için gereken izne sahip olup olmadığını doğrulayın
Azure AD Connect gerektirir AD **parola sıfırlama** parola geri yazma gerçekleştirme izni. Azure AD Connect için izni olup olmadığını öğrenmek için bir şirket içi AD kullanıcı hesabı, Windows etkin izni özelliğini kullanabilirsiniz:

1. Azure AD Connect sunucusu için oturum açın ve Başlat **Eşitleme Hizmeti Yöneticisi'ni** (Başlangıç → eşitleme hizmeti).
2. Altında **Bağlayıcılar** sekmesinde, şirket içi seçin **AD Bağlayıcısı** tıklatıp **özellikleri**.  
![Etkin izni - 2. adım](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
3. Açılan iletişim kutusunda seçin **Active Directory ormanına Bağlan** sekmesinde ve aşağı Not **kullanıcı adı** özelliği. Dizin eşitleme gerçekleştirmek için Azure AD Connect tarafından kullanılan AD DS hesap budur. Parola geri yazma gerçekleştirmek Azure AD Connect için AD DS hesabı parola sıfırlama izninizin olması gerekir.  
![Etkin izni - 3. adım](./media/active-directory-passwords-troubleshoot/checkpermission02.png) 
4. Bir şirket içi etki alanı denetleyicisine oturum açın ve Başlat **Active Directory Kullanıcıları ve Bilgisayarları** uygulama.
5. Tıklatın **Görünüm** ve emin olun **Gelişmiş Özellikler** seçeneği etkinleşir.  
![Etkin izni - 5. adım](./media/active-directory-passwords-troubleshoot/checkpermission03.png) 
6. Doğrulamak istediğiniz AD kullanıcı hesabı arayın. Sağ tıklatın ve hesap **özellikleri**.  
![Etkin izni - 6. adım](./media/active-directory-passwords-troubleshoot/checkpermission04.png) 
7. Açılan iletişim kutusunda, Git **güvenlik** sekmesinde **Gelişmiş**.  
![Etkin izni - 7. adım](./media/active-directory-passwords-troubleshoot/checkpermission05.png) 
8. Gelişmiş güvenlik ayarları açılan iletişim kutusunda, Git **etkili erişim** sekmesi.
9. Tıklayın **bir kullanıcı seçin** ve Azure AD Connect tarafından kullanılan AD DS hesabı seçin (3. adıma bakın). Ardından **görüntülemek etkili erişim**.  
![Etkin izni - 9. adım](./media/active-directory-passwords-troubleshoot/checkpermission06.png) 
10. Aşağı kaydırın ve Ara **parola sıfırlama**. Giriş işaretli değilse, AD DS hesabı seçilen AD kullanıcı hesabının parolasını sıfırlama izni olduğunu anlamına gelir.  
![Etkin izni - 10. adım](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Azure AD forumları

Azure AD hakkında genel bir sorunuz varsa ve Self Servis parola sıfırlama, isteyin Topluluk Yardım için üzerinde [Azure AD forumları](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Topluluk üyeleri mühendisleri, ürün yöneticileri, MVP ve arkadaşa BT uzmanları içerir.

## <a name="contact-microsoft-support"></a>Microsoft Destek'e başvurun

Bir sorunun yanıtını bulamazsanız bizim destek ekiplerini her zaman daha fazla Yardım kullanılabilir.

Düzgün yardımcı olmak için kadar ayrıntı mümkün olduğunca büyük dahil olmak üzere açarken sağlamanız istenir:

* **Genel hata açıklaması** -hata nedir? Bildirildi davranışı neydi? Nasıl biz hata üretebileceği? Lütfen mümkün olduğu kadar ayrıntı sağlar.
* **Sayfa** -hangi sayfa olan, hata olduğunda fark? Lütfen yükleyebilir, URL ve bir ekran görüntüsü içerir.
* **Kodu destekleyecek** -hata kullanıcı gördüğünüz zaman oluşturulan destek kodu neydi?
    * Bunu bulmak için hatayı yeniden oluşturmaya sonra ekranın altındaki destek kodu bağlantısına tıklayın ve sonuçları GUID destek mühendisine göndermek.
    ![Ekranın altındaki destek kod Bul][Support Code]
    * Sayfanın altındaki desteği olmadan kullanıyorsanız, F12 ve SID ve arama CID tuşuna basın ve bu iki sonuçlar destek mühendisine gönderin.
* **Tarih, saat ve saat dilimi** -Lütfen kesin bir tarih ve saat dahil **saat dilimi ile** , bir hata oluştu.
* **Kullanıcı Kimliği** -hata gördüğünüz kullanıcının edildi? (user@contoso.com)
    * Bu, bir Federasyon kullanıcısı mı?
    * Bu, bir parola karması eşitlenmiş kullanıcı mi?
    * Bu bulut tek kullanıcı mi?
* **Lisans** -kullanıcı bir Azure AD Premium veya Azure AD temel lisansı atanmış sahip mi?
* **Uygulama olay günlüğü** - parola geri yazma özelliğini kullanıyorsanız ve bir hatadır, şirket içi altyapıda, destek ile iletişim kurarken, uygulama olay günlüğü'nden Azure AD Connect sunucusu daraltılmış bir kopyasını içerir.



[Service Restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Azure AD eşitleme hizmetini yeniden başlatın"
[Support Code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Destek kod penceresinin alt sağ tarafta bulunur"

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantılar, Azure AD kullanarak parola sıfırlama ile ilgili ek bilgiler sağlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisans ile ilgili sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
