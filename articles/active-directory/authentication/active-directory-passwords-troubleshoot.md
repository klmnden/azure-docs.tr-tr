---
title: Self Servis parola sıfırlama sorun giderme - Azure Active Directory
description: Sorun giderme Azure AD Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.custom: seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5adb857e6032e46c31a86685913277ec3eb571be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60416251"
---
# <a name="troubleshoot-self-service-password-reset"></a>Self Servis parola sıfırlama sorunlarını giderme

Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) ile ilgili bir sorun yaşıyorsunuz? Aşağıdaki bilgiler, yeniden çalışma işlemleri başlatmak için yardımcı olabilir.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-might-see"></a>Bir kullanıcının görebileceği Self Servis parola sıfırlama hatalarını giderme

| Hata | Ayrıntılar | Teknik ayrıntılar |
| --- | --- | --- |
| TenantSSPRFlagDisabled 9 = | Yöneticinizin parola sıfırlamayı kuruluşunuz için devre dışı bıraktığından şu anda parolanızı sıfırlayamazsınız. özür dileriz. Bu durumu çözmek için gerçekleştirebileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurarak bu özelliği etkinleştirmesini isteyin. Daha fazla bilgi için bkz. [Yardım, Azure AD parolamı unuttum](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: Parola sıfırlama, yönetici tarafından etkinleştirilmedi algıladık. Lütfen yöneticinize başvurarak parola sıfırlamayı kuruluşunuz için etkinleştirmesini isteyin. |
| WritebackNotEnabled 10 = |Yöneticiniz kuruluşunuz için gerekli bir hizmeti etkinleştirilmemiş olduğundan şu anda parolanızı sıfırlayamazsınız. özür dileriz. Bu durumu çözmek için gerçekleştirebileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurarak kuruluşunuzun yapılandırmasını denetlemesini isteyin. Bu gerekli hizmet hakkında daha fazla bilgi için bkz: [parola geri yazmayı yapılandırmayla](howto-sspr-writeback.md). | SSPR_0010: Bu parola geri yazma etkinleştirilmediğinden algıladık. Lütfen yöneticinize başvurarak parola geri yazmayı etkinleştirmesini isteyin. |
| SsprNotEnabledInUserPolicy 11 = | Yöneticiniz, parola sıfırlama için kuruluşunuzun yapılandırmadığından şu anda parolanızı sıfırlayamazsınız. özür dileriz. Bu durumu çözmek için gerçekleştirebileceğiniz başka bir eylem yoktur. Yöneticinize başvurarak parola sıfırlamayı yapılandırmasını isteyin. Yapılandırmayı Sıfırla parola hakkında daha fazla bilgi edinmek için [hızlı başlangıç: Azure AD Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Kuruluşunuz bir parola sıfırlama ilkesi tanımlı değil. Lütfen yöneticinize başvurarak bir parola sıfırlama İlkesi tanımlamasını isteyin. |
| UserNotLicensed = 12 | Gerekli lisansları, kuruluşunuzdaki eksik olduğundan şu anda parolanızı sıfırlayamazsınız özür dileriz. Bu durumu çözmek için gerçekleştirebileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurarak, lisans atamasını denetlemek için isteyin. Lisanslama hakkında daha fazla bilgi için bkz: [lisans gereksinimleri için Azure AD Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: Kuruluşunuzda gerekli lisanslar parola sıfırlama işlemini gerçekleştirmesine gerek yok. Lütfen yöneticinize başvurarak lisans atamalarını gözden geçirmesini isteyin. |
| UserNotMemberOfScopedAccessGroup = 13 | Yöneticiniz hesabınızı parola sıfırlama kullanacak şekilde yapılandırmadığından şu anda parolanızı sıfırlayamazsınız. özür dileriz. Bu durumu çözmek için gerçekleştirebileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve bunları hesabınızı parola sıfırlama için yapılandırmasını isteyin. Parola sıfırlama için hesap yapılandırması hakkında daha fazla bilgi için bkz. [kullanıcılar için parola sıfırlamayı kullanıma alma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0013: Parola sıfırlama için etkinleştirilmiş bir grubun üyesi değildir. Gruba eklemesini isteyin ve yönetici ile iletişime geçin. |
| UserNotProperlyConfigured 14 = | Gerekli bilgileri hesabınızdan eksik olduğundan şu anda parolanızı sıfırlayamazsınız. özür dileriz. Bu durumu çözmek için gerçekleştirebileceğiniz başka bir eylem yoktur. Lütfen yöneticisine başvurun ve parolanız sizin için sıfırlamasını isteyin. Hesabınıza yeniden erişiminiz sonra gerekli bilgileri kaydetmek gerekir. Bilgileri kaydetmek için adımları izleyin. [Self Servis parola sıfırlama için kaydolmasını](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-reset-register) makalesi. | SSPR_0014: Ek güvenlik bilgileri, parolanızı sıfırlamak için gereklidir. Devam etmek için yöneticinize başvurun ve parolanızı sıfırlamasını isteyin. Hesabınıza erişimi oluşturduktan sonra ek güvenlik bilgileri kaydedebilirsiniz https://aka.ms/ssprsetup. Yöneticiniz ek güvenlik bilgileri hesabınıza içindeki adımları izleyerek ekleyebilirsiniz [ayarlama ve parola sıfırlama için okunan kimlik doğrulama verilerini](howto-sspr-authenticationdata.md). |
| OnPremisesAdminActionRequired 29 = | Biz parolanızı şu anda kuruluşunuzun parola sıfırlama yapılandırmasında bir sorun nedeniyle sıfırlayamazsınız özür dileriz. Bu durumu çözmek için gerçekleştirebileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurarak durumu araştırmasını isteyin. Olası sorun hakkında daha fazla bilgi için bkz: [parola geri yazma sorunlarını gidermek](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: Şirket içi yapılandırmanızdaki bir hata nedeniyle, parolanızı sıfırlayamıyoruz belirleyemiyoruz. Lütfen yöneticinize başvurarak durumu araştırmasını isteyin. |
| OnPremisesConnectivityError = 30 | Biz parolanızı şu anda kuruluşunuzun bağlantı sorunları nedeniyle sıfırlayamazsınız özür dileriz. Şu anda herhangi bir eylem yoktur, ancak daha sonra yeniden denediğinizde sorun çözülmüş olabilir. Sorun devam ederse lütfen yöneticinize başvurun ve durumu araştırmasını isteyin. Bağlantı sorunları hakkında daha fazla bilgi için bkz: [parola geri yazma bağlantısı sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Biz şirket içi ortamınızla kurulan bağlantı kötü olduğundan, parolanızı sıfırlayamazsınız. Yöneticinize başvurarak durumu araştırmasını isteyin.|

## <a name="troubleshoot-the-password-reset-configuration-in-the-azure-portal"></a>Azure portalında parola sıfırlama yapılandırma sorunlarını giderme

| Hata | Çözüm |
| --- | --- |
| Göremiyorum **parola sıfırlama** bölümü altında Azure portalında Azure AD. | Bir Azure AD Premium veya temel lisansı işlemi gerçekleştiren yönetici atanmış yoksa bu durum oluşabilir. <br> <br> Söz konusu yönetici hesabı için bir lisans atayın. Adımları izleyerek [atamak, doğrulamak ve lisans sorunları çözmeniz](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses) makalesi.|
| Belirli yapılandırma seçeneğini görmüyorum. | Gerekene kadar kullanıcı arabiriminin birçok öğe gizlenir. Görmek istediğiniz tüm seçenekleri etkinleştirmeyi deneyin. |
| Göremiyorum **şirket içi tümleştirme** sekmesi. | Azure AD Connect'i yüklediniz ve parola geri yazma yapılandırdıysanız bu seçeneği yalnızca görünür hale gelir. Daha fazla bilgi için [hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](../hybrid/how-to-connect-install-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Parola sıfırlama raporlama sorunlarını giderme

| Hata | Çözüm |
| --- | --- |
| Tüm parola yönetimi etkinlik türleri göremiyorum **Self Servis parola yönetimi** denetim olay kategorisi. | Bir Azure AD Premium veya temel lisansı işlemi gerçekleştiren yönetici atanmış yoksa bu durum oluşabilir. <br> <br> Söz konusu yönetici hesabı için bir lisans atayarak bu sorunu çözebilirsiniz. Bağlantısındaki [atamak, doğrulamak ve lisans sorunları çözmeniz](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses) makalesi. |
| Kullanıcı kayıtları, birden çok kez gösterir. | Şu anda, bir kullanıcı kayıt olurkenki, biz ayrı ayrı bir olay olarak kayıtlı veri parçalarını oturum. <br> <br> Bu veri toplama ve nasıl görüntüleyebileceğiniz daha fazla esneklik sağlamak, raporu indirin ve verileri Excel'de Özet Tablo olarak açın.

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Parola sıfırlama kayıt portalı sorunlarını giderme

| Hata | Çözüm |
| --- | --- |
| Dizin, parola sıfırlama için etkinleştirilmedi. **Yöneticiniz bu özelliği kullanmak için etkinleştirmedi.** | Anahtar **Self Servis parola sıfırlama etkin** bayrak **seçili** veya **tüm** seçip **Kaydet**. |
| Bir Azure AD Premium veya temel lisansı atanmış kullanıcı yok. **Yöneticiniz bu özelliği kullanmak için etkinleştirmedi.** | Bir Azure AD Premium veya temel lisansı işlemi gerçekleştiren yönetici atanmış yoksa bu durum oluşabilir. <br> <br> Söz konusu yönetici hesabı için bir lisans atayarak bu sorunu çözebilirsiniz. Bağlantısındaki [atamak, doğrulamak ve lisans sorunları çözmeniz](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses) makalesi.|
| İstek işlenirken bir hata yoktur. | Tarafından birçok sorunlar buna neden olabilir, ancak genellikle bu hata bir hizmet kesintisi veya bir yapılandırma sorunu neden olur. Bu hatayı görürseniz ve sorunların, işletmenizi etkilemeden Ek Yardım için Microsoft desteğine başvurun. |

## <a name="troubleshoot-the-password-reset-portal"></a>Parola sıfırlama portalındaki sorunları

| Hata | Çözüm |
| --- | --- |
| Dizin, parola sıfırlama için etkinleştirilmedi. | Anahtar **Self Servis parola sıfırlama etkin** bayrak **seçili** veya **tüm** seçip **Kaydet**. |
| Bir Azure AD Premium veya temel lisansı atanmış kullanıcı yok. | Bir Azure AD Premium veya temel lisansı işlemi gerçekleştiren yönetici atanmış yoksa bu durum oluşabilir. <br> <br> Söz konusu yönetici hesabı için bir lisans atamanız durumunda, bu sorunu çözebilirsiniz. Bağlantısındaki [atamak, doğrulamak ve lisans sorunları çözmeniz](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses) makalesi. |
| Dizin, parola sıfırlama için etkinleştirilmiş, ancak kullanıcının kimlik bilgileri eksik veya hatalı biçimlendirilmiş. | Devam etmeden önce kullanıcı dizinde dosya çubuğunda kişi verilerini düzgün bir şekilde oluşturulduğundan olun. Daha fazla bilgi için [veri kullanılan Azure AD Self Servis parola sıfırlama](howto-sspr-authenticationdata.md). |
| Dizin, parola sıfırlama için etkinleştirilmiş, ancak İlkesi iki doğrulama yöntemi gerektirecek şekilde ayarlandığında kullanıcının dosya kişi verilerini tek bir parçası sahip. | Devam etmeden önce kullanıcı düzgün bir şekilde yapılandırılmış en az iki iletişim yöntemlerini olduğundan emin olun. Örnek bir cep telefonu numarası hem yaşıyor *ve* bir ofis telefon numarası. |
| Dizin, parola sıfırlama için etkinleştirilmiş ve kullanıcı düzgün şekilde yapılandırıldı, ancak kullanıcı temas kuramadı. | Bu geçici hizmet hatası nedeniyle veya doğru olamaz doğru kişi verilerini ise algılar. <br> <br> Kullanıcı 10 saniye bekler, "deneyin" ve "yöneticinize başvurun" bağlantılar görüntülenir. Kullanıcı "deneyin" seçerse, çağrı yeniden denenir. Kullanıcı "yöneticinize başvurun" seçerse bu kullanıcı hesabı için gerçekleştirilmesi için bir parola sıfırlama isteğinde yöneticileri form e-posta gönderir. |
| Kullanıcı, parola sıfırlama SMS veya telefon görüşmesi hiçbir zaman alır. | Bu dizindeki bir hatalı telefon numarası sonucu olabilir. Telefon numarası biçiminde olduğundan emin olun "+ kutulardaki xxxyyyzzzzXeeee". <br> <br> Bir dizindeki belirtmiş olsanız da, parola sıfırlama uzantıları desteklemez. Çağrı dağıtılmasından önce uzantılar kaldırılır. Uzantı olmadan bir sayı kullanın veya özel dal exchange (PBX) telefon numarasını uzantısı tümleştirin. |
| Kullanıcı parola sıfırlama e-posta hiçbir zaman alır. | Bu sorun için en yaygın nedeni, ileti istenmeyen posta Filtresi tarafından reddedildi ' dir. İstenmeyen posta, e-posta klasörünüzü veya Silinmiş Öğeler klasörü denetleyin. <br> <br> Ayrıca, ileti için doğru e-posta hesabı denetimi emin olun. |
| Bir parola sıfırlama İlkesi ayarlamak, ancak bir yönetici hesabı parola sıfırlama kullandığında, o ilke uygulanmaz. | Microsoft yönetir ve denetimleri ilkesi yüksek düzeyde güvenlik sağlamak için yönetici parolasını sıfırlayamaz. |
| Kullanıcı parola sıfırlama bir gün içinde çok fazla kez denemelerini engellenir. | Biz otomatik bir azaltma mekanizması blok kullanıcılara parolalarını sıfırlama birden çok kez bir kısa süre içinde denemelerini uygulayın. Azaltma gerçekleşir olduğunda: <br><ul><li>Kullanıcı, bir telefon numarası bir saat içinde beş kez doğrulamayı dener.</li><li>Kullanıcı, güvenlik soruları andan itibaren bir saat içinde beş kez kullanmayı dener.</li><li>Kullanıcı, bir saat içinde beş kez aynı kullanıcı hesabı için parola sıfırlama dener.</li></ul>Bu sorunu gidermek için son denemeden sonra 24 saat bekleyin kullanıcıya bildirin. Kullanıcı daha sonra kendi parolalarını sıfırlayabilir. |
| Kullanıcı telefon numarasını doğrulanırken bir hata görür. | Girdiğiniz telefon numarası dosya çubuğunda telefon numarası eşleşmiyor Bu hata oluşur. Kullanıcı parola sıfırlama için telefon tabanlı bir yöntemini kullanmayı denediğinizde alan ve ülke kodu da dahil olmak üzere tam telefon numarasını girdiğinden emin olun. |
| İstek işlenirken bir hata yoktur. | Tarafından birçok sorunlar buna neden olabilir, ancak genellikle bu hata bir hizmet kesintisi veya bir yapılandırma sorunu neden olur. Bu hatayı görürseniz ve sorunların, işletmenizi etkilemeden Ek Yardım için Microsoft desteğine başvurun. |
| Şirket içi İlkesi ihlali | Parola, şirket içi Active Directory parola ilkesini karşılamıyor. |
| Parola belirsiz İlkesi uyumlu değil | Kullanılan parola görünür [parola listesine Yasaklanmış](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad#how-are-passwords-evaluated) ve kullanılamaz. |

## <a name="troubleshoot-password-writeback"></a>Parola geri yazma sorunlarını giderme

| Hata | Çözüm |
| --- | --- |
| Şirket içi parola sıfırlama hizmeti başlamıyor. Hata 6800 makinenin Azure AD Connect uygulama olay günlüğünde görüntülenir. <br> <br> Federasyon, ekledikten sonra geçişli kimlik doğrulaması ve parola karması eşitlenmiş kullanıcılar parolalarını sıfırlayamazsınız. | Parola geri yazma etkinleştirildiğinde, eşitleme altyapısı için bulut ekleme hizmeti iletişim kurarak (ekleme) yapılandırmasını gerçekleştirmek için geri yazma kitaplığı çağırır. Ekleme sırasında veya Azure AD Connect makinenizdeki olay günlüğündeki hatalar, Windows Communication Foundation (WCF) uç nokta için parola geri yazma sonuçları başlatılırken karşılaşılan hataları. <br> <br> Geri yazma yapılandırıldıysa, Azure AD eşitleme (ADSync) hizmetinin yeniden başlatma sırasında WCF uç nokta başlatılır. Ancak, uç nokta başlatma başarısız olursa, günlüğe olay 6800 ve başlatma eşitleme hizmeti sağlar. Bu olay varlığını parola geri yazma uç nokta başlatılmadığını anlamına gelir. Neden uç noktasını başlatılamıyor. Bu olay günlüğü girişleri PasswordResetService bileşeni tarafından oluşturmak birlikte 6800, olay için olay günlüğü ayrıntıları belirtin. Bu olay günlüğü hatalarını gözden geçirin ve Azure AD Connect parola geri yazma hala çalışmıyorsa yeniden başlatmayı deneyin. Sorun devam ederse, parola geri yazma'ı yeniden etkinleştirin ve devre dışı bırakmak deneyin.
| Kullanıcı parola sıfırlama veya parola geri yazma etkinleştirilmiş bir hesap kilidini açma girişiminde bulunduğunda, işlem başarısız olur. <br> <br> Ayrıca, bir olay içeren Azure AD Connect olay günlüğüne bakın: "Eşitleme altyapısı döndürülen hata hr = 800700CE, ileti filename = veya uzantı çok uzun" kilit açma işlemi tamamlandıktan sonra. | Azure AD Connect için Active Directory hesabını bulun ve en fazla 127 karakter içeren parolayı sıfırlayın. Açılacağını **eşitleme hizmeti** gelen **Başlat** menüsü. Gözat **Bağlayıcılar** ve bulma **Active Directory Bağlayıcısı**. Seçin ve ardından **özellikleri**. Gözat **kimlik bilgilerini** sayfasında ve yeni parolayı girin. Seçin **Tamam** sayfasını kapatın. |
| Azure AD Connect yükleme işleminin son adımında, parola geri yazma yapılandırılamıyor belirten bir hata görürsünüz. <br> <br> Azure AD Connect uygulama olay günlüğü "Kimlik doğrulama belirteci alınırken hata." hatası 32009 metinle içerir. | Bu hata, aşağıdaki iki durumda gerçekleşir: <br><ul><li>Azure AD Connect yükleme işleminin başında belirtilen genel yönetici hesabı için hatalı bir parola belirttiniz.</li><li>Azure AD Connect yükleme işleminin başında belirtilen genel yönetici hesabı için bir Federasyon kullanıcısı kullanma girişiminde bulundunuz.</li></ul> Bu sorunu gidermek için yükleme işleminin başında belirtilen genel yönetici için birleştirilmiş bir hesap kullanmadığınızı emin olun. Ayrıca, belirtilen parolanın doğru olduğundan emin olun. |
| Azure AD Connect makine olay günlüğü hata 32002 PasswordResetService çalıştırılarak oluşturulan içerir. <br> <br> Hata görünür: "Servicebus'a bağlanılırken hata oluştu. Belirteç sağlayıcısı bir güvenlik belirteci sağlayamadı." | Şirket içi ortamınızı bulutta Azure Service Bus uç noktasına bağlanmanız mümkün değildir. Bu hata genellikle belirli bir bağlantı noktası veya web adresi için bir giden bağlantı engelleyen bir güvenlik duvarı kuralı kaynaklanır. Bkz: [bağlantı önkoşulları](../hybrid/how-to-connect-install-prerequisites.md) daha fazla bilgi için. Bu kurallar güncelleştirildikten sonra Azure AD Connect makineyi yeniden başlatın ve parola geri yazma yeniden çalışmaya başlayabilirsiniz. |
| Federasyon, çalıştıktan sonra bir süre, geçişli kimlik doğrulaması ve parola karması eşitlenmiş kullanıcılar parolalarını sıfırlayamazsınız. | Bazı nadir durumlarda, Azure AD Connect'i yeniden başlatıldığında yeniden başlatmak parola geri yazma hizmetinin başarısız olabilir. Bu gibi durumlarda, ilk olarak, parola geri yazma etkinleştirilmiş şirket içi olarak görünüp görünmediğini denetleyin. Azure AD Connect Sihirbazı veya PowerShell kullanarak denetleyebilirsiniz (önceki HowTos bölümüne bakın). Özelliği etkin olarak görünüyorsa etkinleştirme veya özelliği yeniden kullanıcı Arabirimi veya PowerShell aracılığıyla devre dışı bırakmayı deneyin. Bu işe yaramazsa, tam bir kaldırma deneyin ve Azure AD Connect yeniden yükleyin. |
| Federasyon, geçişli kimlik doğrulaması ya da kendi parolalarını sıfırlama girişimi parola karması eşitlenmiş kullanıcılar parolalarını göndermeye çalışırken bir hata görürsünüz. Hata, bir hizmet sorunu olduğunu gösterir. <br ><br> Bu sorunu yanı sıra, parola sıfırlama işlemleri sırasında yönetim Aracısı, şirket içi olay günlüklerinde erişim reddedildi hata görebilirsiniz. | Olay Günlüğü'nde bu hatalar görürseniz, yapılandırma sırasında sihirbazında belirtilen Active Directory Yönetim Aracısı'nı (ADMA) hesabı parola geri yazma için gerekli izinlere sahip olduğunu doğrulayın. <br> <br> Bu izni verilen sonra aşağı aracılığıyla trickle izni için bir saate kadar sürebilir `sdprop` arka plan görevini etki alanı denetleyicisi (DC). <br> <br> İş için parola sıfırlama için izin parolasını sıfırlama kullanıcı nesnesi üzerinde güvenlik tanımlayıcısı damgalı gerekir. Bu iznin kullanıcı nesnesindeki gösterilir kadar parola sıfırlama erişim reddedildi iletisi ile başarısız olmaya devam eder. |
| Kullanıcılar parolalarını gönderdikten sonra Federasyon, geçişli kimlik doğrulaması ya da kendi parolalarını sıfırlama girişimi parola karması eşitlenmiş kullanıcılar bir hata görürsünüz. Hata, bir hizmet sorunu olduğunu gösterir. <br> <br> Bu sorunu yanı sıra, parola sıfırlama işlemleri sırasında olay günlüklerinde "Nesne bulunamadı" hata gösteren Azure AD Connect hizmetinden bir hata görebilirsiniz. | Eşitleme altyapısı, Azure AD bağlayıcı alanında veya bağlı meta veri deposu (MV) veya Azure AD bağlayıcı alanı nesnesi kullanıcı nesnesi bulunamadı. Bu hata genellikle gösterir. <br> <br> Bu sorunu gidermek için kullanıcının gerçekten şirket içinden Azure AD Connect geçerli örneği aracılığıyla Azure AD'ye eşitlendiğinden emin olun ve bağlayıcı alanları ve MV nesnelerinin durumunu denetleyin. Active Directory Sertifika Hizmetleri (AD CS) nesne "Microsoft.InfromADUserAccountEnabled.xxx" kural aracılığıyla MV nesneye bağlı olduğunu doğrulayın.|
| Kullanıcılar parolalarını gönderdikten sonra Federasyon, geçişli kimlik doğrulaması ya da kendi parolalarını sıfırlama girişimi parola karması eşitlenmiş kullanıcılar bir hata görürsünüz. Hata, bir hizmet sorunu olduğunu gösterir. <br> <br> Bu sorunu yanı sıra, parola sıfırlama işlemleri sırasında olay günlüklerinde "birden çok eşleşme bulunamadı" hata olduğunu gösteren Azure AD Connect hizmetinden bir hata görebilirsiniz. | Bu, eşitleme altyapısı MV nesnesi birden fazla AD CS nesnesi "Microsoft.InfromADUserAccountEnabled.xxx" ile bağlandığı algılandığını gösterir. Bu, kullanıcının birden fazla ormanda etkinleştirilmiş bir hesabı olduğu anlamına gelir. Bu senaryo için parola geri yazma desteklenmez. |
| Parola işlemleri, bir yapılandırma hatasıyla başarısız oluyor. Uygulama olay günlüğü "0x8023061f (işlem parola eşitleme bu yönetim aracı üzerinde etkin olmadığından başarısız oldu)" metni ile Azure AD Connect hata 6329 içerir. | Parola geri yazma özelliği, Azure AD Connect yapılandırmasının yeni bir Active Directory ormanına eklemek için (veya kaldırın ve mevcut bir ormana rolleriniz) sonra değiştirilirse zaten etkinleştirildi, bu hata oluşur. Bu kullanıcılar için parola işlemleri, yakın zamanda ormanları başarısız eklendi. Sorunu gidermek için devre dışı bırakın ve orman yapılandırma değişiklikleri tamamladıktan sonra parola geri yazma özelliğini yeniden etkinleştirin. |

## <a name="password-writeback-event-log-error-codes"></a>Parola geri yazma olay günlüğü hata kodları

Parola geri yazma sorunlarını gidermenize, en iyi uygulama, Azure AD Connect makinenizdeki uygulama olay günlüğünü inceleyin sağlamaktır. Bu olay günlüğü, parola geri yazma için ilgilenilen iki farklı kaynaktan gelen olayları içerir. PasswordResetService kaynağı, operations ve parola geri yazma işlemi için ilgili sorunları açıklar. Ad eşitleme kaynağı işlemleri ve parolalar Active Directory ortamınızı ayarlamak ilgili sorunları açıklar.

### <a name="if-the-source-of-the-event-is-adsync"></a>Olay kaynağını ADSync ise

| Kod | Adı veya ileti | Açıklama |
| --- | --- | --- |
| 6329 | BAIL: MMS(4924) 0X80230619: "Bir kısıtlama parola belirtilen geçerli bir değişmesini önler." | Bu olay, parola geri yazma hizmetini parola geçerlilik süresi, geçmiş, karmaşıklığı ya da etki alanının filtreleme gereksinimleri karşılayacak değil, yerel dizin bir parola ayarlama girişiminde bulunduğunda oluşur. <br> <br> En az parola geçerlilik süresi olan ve bu zaman penceresi içinde parola kısa süre önce değiştirildi, etki alanınızda belirtilen yaş ulaşana kadar parola tekrar değiştiremezsiniz değilseniz. Test amacıyla en düşük yaş 0 olarak ayarlanması gerekir. <br> <br> Parola geçmişini gereksinimlerini etkin olması durumunda son kullanılmadığında bir parola seçmeniz gerekir *N* süreleri ve nerede *N* parola geçmişi ayardır. Son kullanılmış bir parolayı seçeneğini belirlerseniz *N* zaman sonra bu durumda bir hata görürsünüz. Test amacıyla, parola geçmişi 0 olarak ayarlanması gerekir. <br> <br> Parola karmaşıklık gereksinimleri varsa, kullanıcı parola sıfırlama veya değiştirme girişiminde bulunduğunda, bunların tümünün uygulanır. <br> <br> Parola filtreleri etkinleştirilmiş olması ve bir kullanıcı bir parola seçerse, sıfırlama filtreleme ölçütleri karşılamıyor veya değiştirme işlemi başarısız olur. |
| 6329 | MMS(3040): admaexport.cpp(2837): Sunucunun, LDAP parola ilkesi denetimini içermiyor. | Etki alanı denetleyicilerine LDAP_SERVER_POLICY_HINTS_OID denetimi (1.2.840.113556.1.4.2066) etkinleştirilmediyse, bu sorun oluşur. Parola geri yazma özelliğini kullanmak için denetimi etkinleştirmeniz gerekir. Bunu yapmak için DC'ler, Windows Server 2008R2 veya üstü olmalıdır. |
| HR 8023042 | Eşitleme altyapısı hata hr döndürülen = 80230402, ileti = aynı bağlantı yinelenen girişler olduğundan başarısız oldu. bir nesne alma denemesi. | Birden çok etki alanlarında aynı kullanıcı Kimliğini etkin olduğunda bu hata oluşur. Hesap ve kaynak orman eşitleniyor ve her ormanda aynı kullanıcı Kimliğini mevcut ve etkin olması, örnek verilebilir. <br> <br> Bir diğer adı veya UPN ve iki kullanıcı o aynı bağlantı özniteliği paylaşan gibi bir benzersiz olmayan bir bağlantı özniteliği kullanıyorsanız bu hata ayrıca oluşabilir. <br> <br> Bu sorunu çözmek için etki alanlarında bulunan herhangi bir yinelenen kullanıcı yok ve her kullanıcı için benzersiz bir bağlantı özniteliği kullanın emin olun. |

### <a name="if-the-source-of-the-event-is-passwordresetservice"></a>Olay kaynağını PasswordResetService ise

| Kod | Adı veya ileti | Açıklama |
| --- | --- | --- |
| 31001 | PasswordResetStart | Bu olay, şirket içi hizmeti isteği bir Federasyon, geçişli kimlik doğrulaması veya buluttan kaynaklanan parola karması eşitlenmiş kullanıcı için bir parola sıfırlama algılandığını gösterir. Bu olay, her parola sıfırlama geri yazma işleminde ilk olaydır. |
| 31002 | PasswordResetSuccess | Bu olay, bir kullanıcı bir parola sıfırlama işlemi sırasında yeni bir parola seçili gösterir. Bu parola Kurumsal parola gereksinimleri karşıladığından emin ediyoruz göre belirlenir. Parola başarıyla yerel Active Directory ortamına yeniden yazıldı. |
| 31003 | PasswordResetFail | Bu olay, bir kullanıcı seçili bir parola ve parola başarıyla şirket içi ortama ulaştığını gösterir. Ancak biz, yerel Active Directory ortamında parola ayarlamaya çalışırken, bir hata oluştu. Bu hata, çeşitli nedenlerle oluşabilir: <br><ul><li>Kullanıcının parolasını karşılamıyor yaş, geçmiş, karmaşıklık, veya gereksinimleri etki alanı için filtreleyin. Bu sorunu çözmek için yeni bir parola oluşturun.</li><li>ADMA hizmet hesabı, söz konusu kullanıcı hesabında yeni bir parola ayarlamak için uygun izinlere sahip değil.</li><li>Kullanıcının parola ayar işlemlerini izin vermiyor, etki alanı veya Kurumsal Yönetici grubu gibi korumalı bir gruptaki hesabıdır.</li></ul>|
| 31004 | OnboardingEventStart | Bu olay, Azure AD Connect ile parola geri yazmayı etkinleştirirseniz ve kuruluşunuz için parola geri yazma web hizmeti ekleme Başladık oluşur. |
| 31005 | OnboardingEventSuccess | Bu olay, onboarding işleminin başarılı olduğunu ve parola geri yazma özelliğini kullanıma hazır olduğunu gösterir. |
| 31006 | ChangePasswordStart | Bu olay, şirket içi hizmet birleştirilmiş, geçişli kimlik doğrulaması veya buluttan kaynaklanan parola karması eşitlenmiş kullanıcı için bir parola değişikliği isteğini algılandığını gösterir. Bu olay, her değişiklik parola geri yazma işlemi olarak ilk olaydır. |
| 31007 | ChangePasswordSuccess | Bu olay, bir kullanıcı yeni bir parola, parola değiştirme işlemi sırasında seçili, artık parola Kurumsal parola gereksinimlerini karşıladığından ve parola başarıyla geri yerel Active Directory ortamına yazıldığını kullanmayacağımıza karar gösterir. |
| 31008 | ChangePasswordFail | Bir kullanıcı bir parola seçili ve parola başarıyla şirket içi ortamı geldi, ancak biz, yerel Active Directory ortamında parola ayarlamaya çalışırken, bir hata oluştu. Bu olay gösterir. Bu hata, çeşitli nedenlerle oluşabilir: <br><ul><li>Kullanıcının parolasını karşılamıyor yaş, geçmiş, karmaşıklık, veya gereksinimleri etki alanı için filtreleyin. Bu sorunu çözmek için yeni bir parola oluşturun.</li><li>ADMA hizmet hesabı, söz konusu kullanıcı hesabında yeni bir parola ayarlamak için uygun izinlere sahip değil.</li><li>Kullanıcının parola ayar işlemlerini izin vermiyor, etki alanı veya enterprise admins gibi korumalı bir gruptaki hesabıdır.</li></ul> |
| 31009 | ResetUserPasswordByAdminStart | Şirket içi hizmeti, istek bir Federasyon, geçişli kimlik doğrulaması veya kullanıcı adına Yöneticisi'nden kaynaklanan parola karması eşitlenmiş kullanıcı için bir parola sıfırlama algıladı. Bu olay bir yönetici tarafından başlatılan her parola sıfırlama geri yazma işlemi ilk olaydır. |
| 31010 | ResetUserPasswordByAdminSuccess | Yönetici, yönetici tarafından başlatılan parola sıfırlama işlemi sırasında yeni bir parola seçili. Bu parola Kurumsal parola gereksinimleri karşıladığından emin ediyoruz göre belirlenir. Parola başarıyla yerel Active Directory ortamına yeniden yazıldı. |
| 31011 | ResetUserPasswordByAdminFail | Yönetici bir kullanıcı adına parola seçili. Parola başarıyla şirket içi ortamı geldi. Ancak biz, yerel Active Directory ortamında parola ayarlamaya çalışırken, bir hata oluştu. Bu hata, çeşitli nedenlerle oluşabilir: <br><ul><li>Kullanıcının parolasını karşılamıyor yaş, geçmiş, karmaşıklık, veya gereksinimleri etki alanı için filtreleyin. Bu sorunu çözmek için yeni bir parola deneyin.</li><li>ADMA hizmet hesabı, söz konusu kullanıcı hesabında yeni bir parola ayarlamak için uygun izinlere sahip değil.</li><li>Kullanıcının parola ayar işlemlerini izin vermiyor, etki alanı veya enterprise admins gibi korumalı bir gruptaki hesabıdır.</li></ul>  |
| 31012 | OffboardingEventStart | Bu olay, Azure AD Connect ile parola geri yazmayı devre dışı bırakırsanız gerçekleşir ve biz kuruluşunuz parola geri yazma web hizmeti için çıkarma başlatıldı gösterir. |
| 31013| OffboardingEventSuccess| Bu olay, çıkarma işleminin başarılı olduğunu ve parola geri yazma özelliği başarıyla devre dışı olduğunu gösterir. |
| 31014| OffboardingEventFail| Bu olay, çıkarma işlemi başarılı olmadı gösterir. Bu, yapılandırma sırasında belirtilen bulutta veya şirket içi yönetici hesabı üzerinde izinleri hatası nedeniyle olabilir. Parola geri yazmayı devre dışı bırakılırken birleştirilmiş bulut genel Yöneticisi kullanmaya çalıştığınız, hata da meydana gelebilir. Bu sorunu gidermek için yönetim izinlerinizi denetleyin ve birleştirilmiş bir hesap parola geri yazma yeteneği yapılandırılırken kullanmakta olduğunuz değil emin olun.|
| 31015| WriteBackServiceStarted| Bu olay, parola geri yazma hizmetini başarıyla başlatıldığını gösterir. Buluttan parola yönetim isteklerini kabul etmek üzere hazırdır.|
| 31016| WriteBackServiceStopped| Bu olay, parola geri yazma hizmetinin durdurulduğunu gösterir. Buluttan tüm parola yönetim isteği başarısız olur.|
| 31017| AuthTokenSuccess| Bu olay, biz başarıyla için genel yönetici çıkarma veya ekleme işlemini başlatmak için Azure AD Connect Kurulum sırasında belirtilen bir yetkilendirme belirteci alındı gösterir.|
| 31018| KeyPairCreationSuccess| Bu olay başarıyla şifreleme anahtarını oluşturduğumuz gösterir. Bu anahtar, şirket içi ortamınıza gönderilecek buluttan parolaları şifrelemek için kullanılır.|
| 32000| Başvuruları| Bu olay, bir parola yönetimi işlemi sırasında bilinmeyen bir hata gösterir. Daha fazla ayrıntı için olay istisna metninde bakın. Sorun yaşıyorsanız, devre dışı bırakma ve parola geri yazma'ı yeniden etkinleştirmeyi deneyin. Bu yardımcı olmazsa, olay günlüğünü izleme ile birlikte bir kopyasını dahil destek mühendisinize için belirtilen Insider kimliği.|
| 32001| ServiceError| Bu olay oluştu sıfırlama hizmetinin bulut parolasını bağlanırken bir hata gösterir. Şirket içi hizmet parola sıfırlama web hizmetine bağlanamadı bu hata genellikle oluşur.|
| 32002| ServiceBusError| Kiracınızın Service Bus örneğine bağlanırken bir hata oluştu. Bu olay gösterir. Şirket içi ortamınızda giden bağlantıları engelleyen bu durum ortaya çıkabilir. Bağlantılar ve için TCP 443 üzerinden izin sağlamak için güvenlik duvarınızı denetleyin https://ssprsbprodncu-sb.accesscontrol.windows.net/ve sonra yeniden deneyin. Hala sorun yaşıyorsanız, devre dışı bırakma ve parola geri yazma'ı yeniden etkinleştirmeyi deneyin.|
| 32003| InPutValidationError| Bu olay, web hizmeti API'sine geçirilen giriş geçersiz olduğunu gösterir. İşlemi yeniden deneyin.|
| 32004| DecryptionError| Bu olay, buluttan gelen parola şifresini bir hata olduğunu gösterir. Bu, şirket içi ortamınız ve bulut hizmeti arasında bir şifre çözme anahtarı uyumsuzluğu nedeniyle olabilir. Bu sorunu çözmek için devre dışı bırakın ve sonra şirket içi ortamınızda parola geri yazma özelliğini yeniden etkinleştirin.|
| 32005| ConfigurationError| Ekleme sırasında bir yapılandırma dosyası, şirket içi ortamınızda kiracıya özgü bilgileri kaydedin. Bu olay, bu dosya kaydedilirken bir hata oluştu veya hizmeti başlatıldığında, dosya okunurken bir hata oluştu gösterir. Bu sorunu gidermek için devre dışı bırakıp yeniden yapılandırma dosyasının zorlamak için parola geri yazma'ı yeniden etkinleştirmeyi deneyin.|
| 32007| OnBoardingConfigUpdateError| Ekleme sırasında veriler buluttan şirket içi parola sıfırlama hizmeti göndereceğiz. Güvenli bir şekilde diskte depolanacak eşitleme hizmetine gönderilmeden önce verileri daha sonra bir bellek içi dosyasına yazılır. Bu olay, yazma veya bu verileri bellek içinde güncelleştirilirken bir sorun olduğunu gösterir. Bu sorunu gidermek için devre dışı bırakılması ve sonra bu yapılandırma dosyasını yeniden yazmak zorlamak için parola geri yazma'ı yeniden etkinleştirmeyi deneyin.|
| 32008| ValidationError| Bu olay, parola sıfırlama web hizmetinden geçersiz bir yanıt aldık gösterir. Bu sorunu gidermek için devre dışı bırakma ve parola geri yazma'ı yeniden etkinleştirmeyi deneyin.|
| 32009| AuthTokenError| Bu olay, biz bir yetkilendirme Azure AD Connect Kurulum sırasında belirtilen genel yönetici hesabı için belirteç alınamadı olduğunu gösterir. Bu hata bir hatalı kullanıcı adı veya genel yönetici hesabı için belirtilen parola neden olabilir. Belirtilen genel yönetici hesabı birleştiriliyorsa bu hatayı da meydana gelebilir. Bu sorunu gidermek için doğru kullanıcı adını ve parolayı yapılandırmayla yeniden çalıştırın ve yöneticinin yönetilen (yalnızca bulutta yer alan veya parola eşitlenen) hesabı olduğundan emin olun.|
| 32010| CryptoError| Bu olay, şifreleme anahtarını oluşturma veya Bulut hizmetinden ulaşan bir parola dosyanın şifresi çözülürken bir hata gösterir. Bu hata büyük olasılıkla, ortamınıza bir sorun olduğunu gösterir. Bu sorunun nasıl giderileceği hakkında daha fazla bilgi için olay günlüğüne ayrıntılarına bakın. Devre dışı bırakılması ve sonra da parola geri yazma hizmetini yeniden etkinleştirerek de deneyebilirsiniz.|
| 32011| OnBoardingServiceError| Bu olay, şirket içi hizmeti düzgün şekilde ekleme işlemini başlatmak için parola sıfırlama web hizmetiyle iletişim kurulamadı gösterir. Kiracınız için bir kimlik doğrulama belirteci alınırken bir sorun olup olmadığını veya bir güvenlik duvarı kuralı sonucu olarak bu durum ortaya çıkabilir. Bu sorunu gidermek için giden bağlantılar üzerinden TCP 443 ve TCP 9350-9354 veya için engellemeyen emin olun https://ssprsbprodncu-sb.accesscontrol.windows.net/. Ayrıca, yerleşik olmayan Federasyon için kullanmakta olduğunuz Azure AD yönetici hesabı emin olun.|
| 32013| OffBoardingError| Bu olay, şirket içi hizmeti düzgün şekilde çıkarma işlemini başlatmak için parola sıfırlama web hizmetiyle iletişim kurulamadı gösterir. Kiracınız için bir yetkilendirme belirteci alınırken bir sorun olup olmadığını veya bir güvenlik duvarı kuralı sonucu olarak bu durum ortaya çıkabilir. Bu sorunu gidermek için çok veya 443 üzerinden giden bağlantılara engellemeyen emin olun https://ssprsbprodncu-sb.accesscontrol.windows.net/, ve çıkarma için kullanmakta olduğunuz Azure Active Directory yönetici hesabı Federasyon değil.|
| 32014| ServiceBusWarning| Bu olay, kiracınızın Service Bus örneğine bağlanmayı yeniden denemek vardı gösterir. Normal koşullar altında bu değil bir sorun olabilir, ancak çoğu zaman, bu olay görürseniz, özellikle yüksek Gecikmeli veya düşük bant genişlikli bağlantı ise Service Bus, ağ bağlantısı denetlemeyi düşünün.|
| 32015| ReportServiceHealthError| Parola geri yazma hizmetinizi durumunu izlemek için sinyal veri parola sıfırlama web hizmetimiz beş dakikada göndereceğiz. Bu olay, bu sistem durumu bilgileri bulut web hizmetine gönderilirken bir hata olduğunu gösterir. Bu sistem durumu bilgileri bir nesne olarak tanımlanabilir bilgileri (OII) veya kişisel bilgileri (PII) verileri içermez ve böylece bulut hizmet durumu bilgileri sağlıyoruz zamanıyla ilgili bir sinyal ve temel hizmet istatistikleri.|
| 33001| ADUnKnownError| Bu olay, Active Directory tarafından döndürülen bilinmeyen bir hata olduğunu gösterir. Daha fazla bilgi için ADSync kaynağından olaylar için Azure AD Connect sunucusunun olay günlüğünü denetleyin.|
| 33002| ADUserNotFoundError| Bu olay, sıfırlama veya parolayı değiştirmek için çalışan kullanıcı şirket içi dizinde bulunamadı gösterir. Bu hata, silinen şirket içi kullanıcı olduğunda ancak bulutta oluşabilir. Bu hata, eşitleme ile ilgili bir sorun varsa da oluşabilir. Eşitleme günlüklerinizi ve daha fazla bilgi için son birkaç eşitleme çalıştırma ayrıntılarını kontrol edin.|
| 33003| ADMutliMatchError| Parola sıfırlama ya da değişiklik isteği buluttan kaynaklanan bulut bağlantı Azure AD Connect Kurulum işlemi sırasında belirtilen bir kullanıcı, şirket içi ortamınızda bu isteği yeniden bağlamak nasıl belirlemek için kullanırız. Bu olay, aynı bulut bağlantısı özniteliği ile şirket içi dizininizdeki iki kullanıcı bulundu olduğunu gösterir. Eşitleme günlüklerinizi ve daha fazla bilgi için son birkaç eşitleme çalıştırma ayrıntılarını kontrol edin.|
| 33004| ADPermissionsError| Bu olay, Active Directory Yönetim Aracısı'nı (ADMA) hizmet hesabı yeni bir parola ayarlamak için söz konusu hesapta uygun izinleri yok gösterir. ADMA hesabını kullanıcının ormanındaki sıfırladı ve ormandaki tüm nesneler parola izinleri değiştirme emin olun. 4. adım izinlerin nasıl ayarlanacağı hakkında daha fazla bilgi için bkz: Uygun Active Directory izinlerini ayarlayabilirsiniz.|
| 33005| ADUserAccountDisabled| Bu olay, biz sıfırlama veya şirket devre dışı olan bir hesap için bir parola değiştirme girişiminde bulunuldu gösterir. Hesabı etkinleştirin ve işlemi yeniden deneyin.|
| 33006| ADUserAccountLockedOut| Bu olay, biz sıfırlama veya şirket içi kilitli olan bir hesap için bir parola değiştirme girişiminde bulunuldu gösterir. Bir kullanıcı bir değişiklik denediniz veya parola işlemi çok fazla kez kısa bir süre içinde sıfırlama kilitlenmelerden ortaya çıkabilir. Hesap kilidinizi açın ve işlemi yeniden deneyin.|
| 33007| ADUserIncorrectPassword| Bu olay, bir parola gerçekleştirme değiştirdiğinizde işlemi kullanıcı yanlış bir geçerli parola belirtilen gösterir. Doğru geçerli parolayı belirtin ve yeniden deneyin.|
| 33008| ADPasswordPolicyError| Bu olay, parola geri yazma hizmetini parola geçerlilik süresi, geçmiş, karmaşıklığı ya da etki alanının filtreleme gereksinimleri karşılayacak değil, yerel dizin bir parola ayarlama girişiminde bulunduğunda oluşur. <br> <br> En az parola geçerlilik süresi olan ve bu zaman penceresi içinde parola kısa süre önce değiştirildi, etki alanınızda belirtilen yaş ulaşana kadar parola tekrar değiştiremezsiniz değilseniz. Test amacıyla en düşük yaş 0 olarak ayarlanması gerekir. <br> <br> Parola geçmişini gereksinimlerini etkin olması durumunda son kullanılmadığında bir parola seçmeniz gerekir *N* süreleri ve nerede *N* parola geçmişi ayardır. Son kullanılmış bir parolayı seçeneğini belirlerseniz *N* zaman sonra bu durumda bir hata görürsünüz. Test amacıyla, parola geçmişi 0 olarak ayarlanması gerekir. <br> <br> Parola karmaşıklık gereksinimleri varsa, kullanıcı parola sıfırlama veya değiştirme girişiminde bulunduğunda, bunların tümünün uygulanır. <br> <br> Parola filtreleri etkinleştirilmiş olması ve bir kullanıcı bir parola seçerse, sıfırlama filtreleme ölçütleri karşılamıyor veya değiştirme işlemi başarısız olur.|
| 33009| ADConfigurationError| Bir parola, Active Directory ile bir yapılandırma sorunu nedeniyle şirket içi dizininize geri yazılırken bir sorun oluştu. Bu olay gösterir. ADSync hizmeti üzerinde hata oluştu. daha fazla bilgi için gelen iletiler için Azure AD Connect makinenin uygulama olay günlüğünü denetleyin.|

## <a name="troubleshoot-password-writeback-connectivity"></a>Parola geri yazma bağlantısı sorunlarını giderme

Azure AD Connect parola geri yazma bileşeni ile hizmet kesintilerini karşılaşıyorsanız, bu sorunu çözmek için gerçekleştirebileceğiniz bazı hızlı adımlar şunlardır:

* [Ağ bağlantısını doğrulayın](#confirm-network-connectivity)
* [Azure AD Connect eşitleme hizmetini yeniden başlatın](#restart-the-azure-ad-connect-sync-service)
* [Parola geri yazma özelliğini yeniden etkinleştirin ve devre dışı bırak](#disable-and-re-enable-the-password-writeback-feature)
* [Azure AD Connect'in en son sürümünü yükleyin](#install-the-latest-azure-ad-connect-release)
* [Parola geri yazma sorunlarını giderme](#troubleshoot-password-writeback)

Genel olarak, hizmetinizi en hızlı bir şekilde kurtarmak için yukarıda bahsedilen sırasına göre aşağıdaki adımları yürütün öneririz.

### <a name="confirm-network-connectivity"></a>Ağ bağlantısını doğrulayın

En yaygın hata noktasını, güvenlik duvarının ve veya proxy bağlantı noktası ve boşta kalma zaman aşımı yanlış yapılandırılmış. 

Azure AD Connect sürümü 1.1.443.0 ve üstü, giden HTTPS aşağıdakilere erişim:

* \*.passwordreset.microsoftonline.com
* \*. servicebus.windows.net

Daha fazla ayrıntı için güncelleştirilmiş listesini başvuru [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) sonraki Pazartesi yürürlüğe koymak ve her Çarşamba günü güncelleştirildi.

Daha fazla bilgi için bağlantı önkoşulları gözden geçirin [Azure AD Connect önkoşulları](../hybrid/how-to-connect-install-prerequisites.md) makalesi.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Azure AD Connect eşitleme hizmetini yeniden başlatın

Bağlantı sorunları veya hizmeti ile geçici diğer sorunları gidermek için Azure AD Connect eşitleme hizmeti yeniden başlatın:

1. Bir yönetici olarak seçin **Başlat** Azure AD Connect'i çalıştıran sunucu üzerinde.
1. ENTER **services.msc** arama alanını seçip **Enter**.
1. Aranacak **Microsoft Azure AD eşitleme** girişi.
1. Servis girişine sağ tıklayın, **yeniden**ve işlemin tamamlanmasını bekleyin.

   ![GUI kullanarak Azure AD eşitleme hizmetini yeniden başlatın][Service restart]

Bu adımlar, bulut hizmeti, bağlantıyı yeniden kurun ve herhangi bir kesinti yaşıyor olabilir çözümleyin. ADSync hizmeti yeniden başlatmayı sorununuzu çözmezse, devre dışı bırakın ve ardından parola geri yazma özelliğini yeniden etkinleştirmek aşağıdakileri öneririz.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Parola geri yazma özelliğini yeniden etkinleştirin ve devre dışı bırak

Bağlantı sorunlarını gidermek için devre dışı bırakın ve sonra da parola geri yazma özelliğini yeniden etkinleştirin:

   1. Yönetici olarak, Azure AD Connect yapılandırma sihirbazını açın.
   1. İçinde **Azure ad Connect**, Azure AD genel yönetici kimlik bilgilerinizi girin.
   1. İçinde **AD DS'ye Bağlan**, AD etki alanı Hizmetleri yönetici kimlik bilgilerinizi girin.
   1. İçinde **kullanıcılarınız eşsiz şekilde tanımlanıyor**seçin **sonraki** düğmesi.
   1. İçinde **isteğe bağlı özellikler**temizleyin **parola geri yazma** onay kutusu.
   1. Seçin **sonraki** gelene kadar değiştirmeden herhangi bir şey kalan iletişim kutusu sayfaları aracılığıyla **yapılandırma için hazır** sayfası.
   1. Emin **sayfasını yapılandırmak hazır** gösterir **parola geri yazma** olarak seçeneğini **devre dışı** seçip yeşil **yapılandırma** Değişikliklerinizi kaydetmek için düğme.
   1. İçinde **tamamlandı**temizleyin **Şimdi Eşitle** seçeneğini belirtin ve ardından **son** sihirbazı kapatın.
   1. Azure AD Connect yapılandırma sihirbazını açın.
   1. 2-8 dışında seçtiğinizden emin olun, arasındaki adımları yineleyin **parola geri yazma** seçeneğini **isteğe bağlı özellikler** hizmeti yeniden etkinleştirmek için sayfa.

Bu adımlar, müşterilerimize bulut hizmeti, bağlantıyı yeniden kurun ve herhangi bir kesinti yaşıyor olabilir çözümleyin.

Devre dışı bırakıp sonra yeniden parola geri yazma özelliğini etkinleştirerek sorununuzu çözmezse, Azure AD Connect'i yeniden yüklemenizi öneririz.

### <a name="install-the-latest-azure-ad-connect-release"></a>Azure AD Connect'in en son sürümünü yükleyin

Azure AD Connect'i yeniden yüklemek, yapılandırması ve bulut hizmetlerimizi ve yerel Active Directory ortamınız arasında bağlantı sorunları çözebilirsiniz.

Yalnızca, daha önce açıklanan ilk iki adımını denemeden sonra bu adımı gerçekleştirmenizi öneririz.

> [!WARNING]
> Kullanıma hazır eşitleme kurallarını özelleştirdiyseniz *yükseltme işlemine devam etmeden önce yedeklemek ve işiniz bittiğinde sonra sonra el ile bunları yeniden dağıtın.*

1. Azure AD Connect'ten en son sürümünü indirin [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=615771).
1. Azure AD Connect'in yüklü olduğundan, Azure AD Connect yüklemenizin en son sürüme güncelleştirmek için bir yerinde yükseltme gerçekleştirmek için gerekir.
1. İndirilen paketi yürütün ve izleyin ekrandaki yönergeleri, Azure AD Connect makinenizi.

Önceki adımları, müşterilerimize bulut hizmeti, bağlantıyı yeniden kurun ve herhangi bir kesinti yaşıyor olabilir çözümlemek.

Azure AD Connect sunucusu en son sürümünü yükleme sorunu çözmezse, devre dışı bırakma ve en son sürümünü yükledikten sonra parola geri yazma son adım olarak yeniden etkinleştirmeyi deneyin öneririz.

## <a name="verify-that-azure-ad-connect-has-the-required-permission-for-password-writeback"></a>Azure AD Connect parola geri yazma için gerekli izne sahip olduğundan emin olun

Azure AD Connect'in, Active Directory gerektirir **parolayı Sıfırla** parola geri yazma gerçekleştirme izni. Azure AD Connect belirli şirket içi Active Directory kullanıcı hesabı için gerekli izne sahip olup olmadığını öğrenmek için Windows etkili izne özelliğini kullanabilirsiniz:

1. Azure AD Connect sunucusu için oturum açın ve başlangıç **Eşitleme Hizmeti Yöneticisi** seçerek **Başlat** > **eşitleme hizmeti**.
1. Altında **Bağlayıcılar** sekmesinde, şirket içi seçin **Active Directory Domain Services** bağlayıcı tıklayın ve ardından **özellikleri**.  
   ![Eşitleme Hizmeti Yöneticisi özelliklerini düzenlemek nasıl gösteriliyor](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
  
1. Açılır pencerede seçin **Active Directory ormanına Bağlan** ve Not **kullanıcı adı** özelliği. Bu özellik, dizin eşitlemeyi gerçekleştirmek için Azure AD Connect tarafından kullanılan AD DS hesabı olur. Parola geri yazma gerçekleştirmek Azure AD Connect için AD DS hesap parolasını izni sıfırlamanız gerekir.  

   ![Eşitleme hizmeti Active Directory kullanıcı hesabı bulma](./media/active-directory-passwords-troubleshoot/checkpermission02.png) 
  
1. Bir şirket içi etki alanı denetleyicisine oturum açın ve başlangıç **Active Directory Kullanıcıları ve Bilgisayarları** uygulama.
1. Seçin **görünümü** emin **Gelişmiş Özellikler** seçeneği etkinleştirilir.  

   ![Active Directory Kullanıcıları ve bilgisayarları Gelişmiş özellikleri göster](./media/active-directory-passwords-troubleshoot/checkpermission03.png) 
  
1. Doğrulamak istediğiniz Active Directory kullanıcı hesabı için bakın. Hesap adına sağ tıklayıp **özellikleri**.  
1. Açılır pencerede Git **güvenlik** sekmenize **Gelişmiş**.  
1. İçinde **yönetici için Gelişmiş güvenlik ayarları** açılır pencere, Git **etkili erişim** sekmesi.
1. Seçin **bir kullanıcı seçin**, Azure AD tarafından kullanılan AD DS hesabı seçin (bkz. 3. adım) bağlanın ve ardından **etkili erişimi görüntüle**.

   ![Eşitleme hesabı gösteren etkili erişim sekmesi](./media/active-directory-passwords-troubleshoot/checkpermission06.png) 
  
1. Ekranı aşağı kaydırın ve Ara **parolayı Sıfırla**. Giriş bir onay işareti varsa, AD DS hesabı seçili Active Directory kullanıcı hesabının parolasını sıfırlama izni vardır.  

   ![Eşitleme hesabı parola sıfırlama izni olduğunu doğrulama](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Azure AD forumları

Azure AD hakkında genel bir sorum varsa ve Self Servis parola sıfırlama sorabileceğiniz topluluktan Yardım almak için şirket [Azure AD forumlarında](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Mühendisler, ürün yöneticileri, MVP'ler ve sizin gibi BT uzmanları, topluluk üyeleri içerir.

## <a name="contact-microsoft-support"></a>Microsoft desteğine başvurun

Destek ekiplerimiz her zaman bir sorunun yanıtını bulamazsanız, daha fazla yardımcı olmak kullanılabilir.

Düzgün bir şekilde yardımcı olması için bir servis talebi açılırken mümkün olduğu kadar ayrıntı verin, isteyin. Bu Ayrıntılar arasında:

* **Genel hata açıklamasını**: Hata nedir? Bildirildi davranışı neydi? Nasıl şu hatayı yeniden oluşturabilirsiniz? Mümkün olduğu kadar ayrıntı verin.
* **Sayfa**: Hangi sayfa were, hatanın ne zaman fark Şunları kullanıyorsanız URL ve sayfasının ekran görüntüsü içerir.
* **Destek kodu**: Kullanıcı hatası gördünüz zaman oluşturan destek kodunu neydi?
   * Bu kodu bulmak için hatayı yeniden oluşturmaya ve ardından **destek kod** bağlantı ekranın alt kısmında ve sonuçları GUID destek mühendisine göndermek.

   ![Ekranın alt kısmındaki destek kod bulma][Support code]

  * En altta bir destek kodu olmadan bir sayfa üzerinde kullanıyorsanız, F12 ve SID ve CID ara seçin ve bu iki sonucu destek mühendisine göndermek.
* **Tarih, saat ve saat dilimi**: Kesin tarih ve saat içeren *saat dilimi ile* , bir hata oluştu.
* **Kullanıcı Kimliği**: Kimin hata gördüğünüz kullanıcı neydi? Bir örnek *kullanıcı\@contoso.com*.
   * Bu, bir Federasyon kullanıcısı mı?
   * Bu, geçişli kimlik doğrulaması kullanıcı mı?
   * Bu, bir parola karması eşitlenmiş kullanıcı mı?
   * Bu, bir yalnızca bulut kullanıcı mı?
* **Lisanslama**: Kullanıcıya atanmış bir Azure AD Premium veya Azure AD temel lisansı var mı?
* **Uygulama olay günlüğüne**: Parola geri yazma özelliğini kullanıyorsanız ve şirket içi altyapınızı hatası ise, uygulama olay günlüğü Azure AD Connect sunucusundan sıkıştırılmış bir kopyasını içerir.

[Service restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Azure AD eşitleme hizmetini yeniden başlatın"
[Support code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Destek kodu pencerenin alt sağ tarafta bulunur"

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler Azure AD aracılığıyla parola sıfırlama hakkında daha fazla bilgi sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
