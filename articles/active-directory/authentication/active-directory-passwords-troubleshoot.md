---
title: Sorun giderme - Azure Active Directory Self Servis parola sıfırlama
description: Sorun giderme Azure AD Self Servis parola sıfırlama
services: active-directory
keywords: ''
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
ms.custom: it-pro;seohack1
ms.openlocfilehash: 53afdfd3286a224a824b58287fb835c3b98ede3d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="troubleshoot-self-service-password-reset"></a>Self Servis parola sıfırlama sorun giderme

Azure Active Directory (Azure AD) Self Servis parola sıfırlama (SSPR) ile ilgili bir sorun yaşıyorsunuz? Aşağıdaki bilgiler yeniden çalışma işlerinizi size yardımcı olabilir.

## <a name="troubleshoot-self-service-password-reset-errors-that-a-user-might-see"></a>Bir kullanıcı görebilirsiniz Self Servis parola sıfırlama hatalarında sorun giderme

| Hata | Ayrıntılar | Teknik Ayrıntılar |
| --- | --- | --- |
| TenantSSPRFlagDisabled = 9 | Yöneticinize parola sıfırlama, kuruluşunuz için devre dışı bıraktığından şu anda parolanızı sıfırlayamazsınız özür dileriz. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve bu özelliği etkinleştirmek için isteyin. Daha fazla bilgi için bkz: [Yardım, unuttum Azure AD parolamı](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password#common-problems-and-their-solutions). | SSPR_0009: Bu parola sıfırlama yöneticiniz tarafından etkin değil algıladık. Lütfen yöneticinize başvurun ve parola, kuruluşunuz için sıfırlamayı etkinleştirmek isteyin. |
| WritebackNotEnabled = 10 |Yöneticiniz, kuruluşunuz için gerekli bir hizmeti etkin değil çünkü şu anda parolanızı sıfırlayamazsınız özür dileriz. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve kuruluşunuzun yapılandırmasını denetleyin isteyin. Bu gerekli hizmet hakkında daha fazla bilgi için bkz: [parola geri yazma yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-writeback#configure-password-writeback). | SSPR_0010: Bu parola geri yazma etkinleştirilmemiş algıladık. Lütfen yöneticinize başvurun ve parola geri yazma özelliğini etkinleştirmek için isteyin. |
| SsprNotEnabledInUserPolicy = 11 | Yöneticiniz, parola sıfırlama, kuruluşunuz için yapılandırılmamış olduğundan şu anda parolanızı sıfırlayamazsınız özür dileriz. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Yöneticinize başvurun ve parola sıfırlama yapılandırma isteyin. Yapılandırma sıfırlama parolası hakkında daha fazla bilgi edinmek için [hızlı başlangıç: Azure AD Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started). | SSPR_0011: Kuruluşunuz bir parola sıfırlama ilkesi tanımlı değil. Lütfen yöneticinize başvurun ve bir parola sıfırlama ilkesini tanımlamak için isteyin. |
| UserNotLicensed = 12 | Gerekli olan lisansları kuruluşunuzdan eksik olduğundan şu anda parolanızı sıfırlayamazsınız özür dileriz. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve lisans atamasını denetlemek için isteyin. Lisanslama hakkında daha fazla bilgi için bkz: [lisans gereksinimleri için Azure AD Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-licensing). | SSPR_0012: Kuruluşunuzun parola sıfırlama işlemini gerçekleştirmek için gerekli gerekli lisansları sahip değil. Lütfen yöneticinize başvurun ve lisans anlaşmalarını gözden isteyin. |
| UserNotMemberOfScopedAccessGroup = 13 | Yöneticiniz hesabınızı parola sıfırlama kullanacak biçimde yapılandırılmamış olduğundan şu anda parolanızı sıfırlayamazsınız özür dileriz. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve hesabınız parola sıfırlama için yapılandırma isteyin. Parola sıfırlama için hesap yapılandırması hakkında daha fazla bilgi için bkz: [parola sıfırlama kullanıcılar için kullanıma alma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-best-practices). | SSPR_0012:, Parola sıfırlama için etkinleştirilmiş bir grup üyesi değildir. Yönetici ve grubuna eklenmesi isteği başvurun. |
| UserNotProperlyConfigured = 14 | Gerekli bilgileri hesabınızdan eksik olduğundan şu anda parolanızı sıfırlayamazsınız özür dileriz. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticisine başvurun ve parolanızı sizin için sıfırlamasını isteyin. Hesabınıza yeniden erişiminiz sonra gerekli bilgileri kaydetmeniz gerekir. Bilgileri kaydetmek için adımları [Self Servis parola sıfırlama için kaydetme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-reset-register) makalesi. | SSPR_0014: Ek güvenlik bilgileri, parolanızı sıfırlamak için gereklidir. Devam etmek için yöneticinize başvurun ve parolanızı sıfırlamak için isteyin. Hesabınıza erişimi sonra ek güvenlik bilgisi kaydedebilirsiniz https://aka.ms/ssprsetup. Yöneticiniz ek güvenlik bilgileri hesabınıza içindeki adımları izleyerek ekleyebilirsiniz [kümesi ve parola sıfırlama için okuma kimlik doğrulama verileri](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-data#set-and-read-authentication-data-using-powershell). |
| OnPremisesAdminActionRequired 29 = | Biz parolanızı şu anda kuruluşunuzun parola sıfırlama yapılandırması ile ilgili bir sorun nedeniyle sıfırlayamazsınız özür dileriz. Bu durumu çözümlemek için uygulayabileceğiniz başka bir eylem yoktur. Lütfen yöneticinize başvurun ve araştırmak için isteyin. Olası sorun hakkında daha fazla bilgi için bkz: [parola geri yazma sorun giderme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback). | SSPR_0029: Şirket içi yapılandırmanızda bir hata nedeniyle, parolanızı sıfırlamak alamıyoruz. Lütfen yöneticinize başvurun ve araştırmak için isteyin. |
| OnPremisesConnectivityError = 30 | Biz parolanızı şu anda kuruluşunuz için bağlantı sorunu nedeniyle sıfırlayamazsınız özür dileriz. Şu anda yapılacak eylemi yok, ancak daha sonra yeniden deneyin, sorun çözümlenmiş olabilir. Sorun devam ederse, lütfen yöneticinize başvurun ve araştırmak için isteyin. Bağlantı sorunları hakkında daha fazla bilgi için bkz: [parola geri yazma bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#troubleshoot-password-writeback-connectivity). | SSPR_0030: Biz şirket içi ortamınız ile zayıf bağlantı nedeniyle, parolanızı sıfırlayamazsınız. Yöneticinize başvurun ve araştırmak için isteyin.|


## <a name="troubleshoot-the-password-reset-configuration-in-the-azure-portal"></a>Azure portalında parola sıfırlama yapılandırma sorunlarını giderme

| Hata | Çözüm |
| --- | --- |
| Görmüyorum **parola sıfırlama** bölümü Azure portalında Azure AD altında. | Bir Azure AD Premium veya işlemi gerçekleştiren yönetici atanmış temel lisans yoksa bu durum oluşabilir. <br> <br> Söz konusu yönetici hesabı için bir lisans atayın. İçindeki adımları takip edebilirsiniz [atayabilir, doğrulayın ve lisans sorunları gidermek](../active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) makalesi.|
| Belirli yapılandırma seçeneği görmüyorum. | Gerekene kadar UI çok sayıda öğe gizlenir. Görmek istediğiniz tüm seçenekleri etkinleştirmeyi deneyin. |
| Görmüyorum **şirket içi tümleştirme** sekmesi. | Azure AD Connect'i yüklediniz ve parola geri yazma yapılandırdıysanız bu seçenek yalnızca görünür hale gelir. Daha fazla bilgi için bkz: [hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](./../connect/active-directory-aadconnect-get-started-express.md). |

## <a name="troubleshoot-password-reset-reporting"></a>Parola sıfırlama raporlama sorunlarını giderme

| Hata | Çözüm |
| --- | --- |
| Tüm parola yönetimi etkinliği türlerinde görmüyorum **Self Servis parola yönetimi** denetim olay kategorisi. | Bir Azure AD Premium veya işlemi gerçekleştiren yönetici atanmış temel lisans yoksa bu durum oluşabilir. <br> <br> Söz konusu yönetici hesabı için bir lisans atayarak bu sorunu çözebilirsiniz. Adımları [atayabilir, doğrulayın ve lisans sorunları gidermek](../active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) makalesi. |
| Kullanıcı kayıtları birden çok kez gösterir. | Bir kullanıcı kaydolduğunda, şu anda, her tek tek parça ayrı bir olay olarak kayıtlı veri oturumunuzu. <br> <br> Bu verileri toplamak ve nasıl görüntüleyebileceğiniz daha fazla esneklik sağlamak istiyorsanız raporu yükleyin ve veri Excel'deki Özet Tablo olarak açın.

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Parola sıfırlama kayıt portalı sorun giderme

| Hata | Çözüm |
| --- | --- |
| Dizin, parola sıfırlama için etkinleştirilmedi. **Yöneticiniz, bu özelliği kullanmak etkinleştirmedi.** | Anahtar **Self Servis parola sıfırlama etkin** bayrağını **seçili** veya **tüm** ve ardından **kaydetmek**. |
| Bir Azure AD Premium veya Basic lisansı atanmış kullanıcı yok. **Yöneticiniz, bu özelliği kullanmak etkinleştirmedi.** | Bir Azure AD Premium veya işlemi gerçekleştiren yönetici atanmış temel lisans yoksa bu durum oluşabilir. <br> <br> Söz konusu yönetici hesabı için bir lisans atayarak bu sorunu çözebilirsiniz. Adımları [atayabilir, doğrulayın ve lisans sorunları gidermek](../active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) makalesi.|
| İstek işlenirken bir hata var. | Bu pek çok sorundan kaynaklanabilir, ancak bir hizmet kesintisi veya yapılandırma sorunu tarafından bu hata genellikle kaynaklanır. Bu hatayı görürseniz ve işinizin etkiler Ek Yardım için Microsoft Destek'e başvurun. |

## <a name="troubleshoot-the-password-reset-portal"></a>Parola sıfırlama portalı sorun giderme

| Hata | Çözüm |
| --- | --- |
| Dizin, parola sıfırlama için etkinleştirilmedi. | Anahtar **Self Servis parola sıfırlama etkin** bayrağını **seçili** veya **tüm** ve ardından **kaydetmek**. |
| Bir Azure AD Premium veya Basic lisansı atanmış kullanıcı yok. | Bir Azure AD Premium veya işlemi gerçekleştiren yönetici atanmış temel lisans yoksa bu durum oluşabilir. <br> <br> Söz konusu yönetici hesabı için bir lisans atadığınızda, bu sorunu çözebilirsiniz. Adımları [atayabilir, doğrulayın ve lisans sorunları gidermek](../active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses) makalesi. |
| Dizin, parola sıfırlama için etkin, ancak kullanıcı kimlik doğrulama bilgileri eksik veya hatalı sahiptir. | Devam etmeden önce o kullanıcı kişi verilerini dizinde dosya üzerinde düzgün bir şekilde oluşturulduğundan olun. Daha fazla bilgi için bkz: [Azure AD Self Servis parola tarafından kullanılan verileri sıfırlama](howto-sspr-authenticationdata.md). |
| Dizin, parola sıfırlama için etkin, ancak ilke iki doğrulama yöntemi gerektirecek şekilde ayarlandığında kullanıcı kişi verilerini yalnızca bir parçasının dosyada sahiptir. | Devam etmeden önce kullanıcının en az iki düzgün yapılandırılmış iletişim yöntemlerini olduğundan emin olun. Bir örnek hem bir cep telefonu numarası yaşıyor *ve* ofis telefon numarası. |
| Dizin, parola sıfırlama için etkin ve kullanıcı düzgün şekilde yapılandırılmış, ancak kullanıcı kurulmasını alamıyor. | Bu geçici olarak hizmet hatanın sonucu olabilir veya düzgün olamaz yanlış kişi verilerini ise tespit. <br> <br> Kullanıcı 10 saniye bekler, "yeniden deneyin." ve "yöneticinize başvurun" bağlantılar görüntülenir. Kullanıcı "yeniden deneyin" seçerse, çağrı yeniden dener. Kullanıcı "yöneticinize başvurun" seçerse, yöneticilerinin parola bu kullanıcı hesabı için gerçekleştirilecek sıfırlama isteyen form e-posta gönderir. |
| Kullanıcı, parola sıfırlama SMS veya telefon görüşmesi hiçbir zaman alır. | Bu dizinde bir hatalı telefon numarası sonucu olabilir. Telefon numarası biçiminde olduğundan emin olun "+ bilgi xxxyyyzzzzXeeee". <br> <br> Bir dizindeki belirttiğiniz olsa bile parola sıfırlama uzantıları desteklemez. Çağrı dağıtılmasından önce uzantıları kaldırılır. Uzantı, özel aktarmalı (PBX) telefon numarasını şekilde entegre veya uzantı içermeyen bir sayı kullanın. |
| Kullanıcı hiçbir zaman parola sıfırlama e-posta alır. | Bu sorun için en yaygın nedeni ileti istenmeyen posta Filtresi tarafından reddedilir oluşturur. İstenmeyen posta, e-posta için Önemsiz veya silinen öğeler klasörünü denetleyin. <br> <br> Ayrıca iletisi için doğru e-posta hesabı denetimi emin olun. |
| Bir parola sıfırlama İlkesi ayarlamış olmanız, ancak bir yönetici hesabı parola sıfırlama kullandığında, bu ilkenin geçerli değil. | Microsoft tarafından yönetilir ve denetim ilkesi yüksek düzeyde güvenlik sağlamak için yönetici parolasını sıfırlayamaz. |
| Kullanıcı parola sıfırlama bir gün içinde çok fazla kez çalışırken engellenir. | Biz Otomatik azaltma mekanizmasını blok kullanıcılar için çok fazla kez bir kısa süre içinde parolalarını sıfırlamaya çalışırken uygulayın. Azaltma oluşur zaman: <br><ul><li>Kullanıcı, bir telefon numarası bir saat içinde 5 kez doğrulamayı dener.</li><li>Kullanıcı güvenlik soruları kapısı 5 kez bir saat içinde kullanmayı dener.</li><li>Kullanıcı 5 kez bir saat içinde aynı kullanıcı hesabı için parola sıfırlama girişiminde bulunur.</li></ul>Bu sorunu gidermek için son denemeden sonra 24 saat bekleyin kullanıcıya isteyin. Kullanıcı ardından parolalarını sıfırlayabilir. |
| Kullanıcı telefon numarasını doğrulanırken bir hata görür. | Girdiğiniz telefon numarası dosya telefon numarasının eşleşmiyor Bu hata oluşur. Kullanıcı parola sıfırlama için telefon tabanlı bir yöntemini kullanmaya çalıştığınızda alan ve ülke kodu da dahil olmak üzere tam telefon numarasını girdiğinden emin olun. |
| İstek işlenirken bir hata var. | Bu pek çok sorundan kaynaklanabilir, ancak bir hizmet kesintisi veya yapılandırma sorunu tarafından bu hata genellikle kaynaklanır. Bu hatayı görürseniz ve işinizin etkiler Ek Yardım için Microsoft Destek'e başvurun. |

## <a name="troubleshoot-password-writeback"></a>Parola geri yazma sorunlarını giderme

| Hata | Çözüm |
| --- | --- |
| Parola sıfırlama hizmeti şirket içi başlatılmaz. Hata 6800 Azure AD Connect makinenin uygulama olay günlüğünde görünür. <br> <br> Onboarding sonra Federasyon veya parola karma eşitlenen kullanıcılar parolalarını sıfırlayamazsınız. | Parola geri yazma etkinleştirildiğinde, eşitleme altyapısı için bulut ekleme hizmeti iletişim kurarak (hazırlama) yapılandırmasını gerçekleştirmek için geri yazma kitaplığı çağırır. Hazırlama sırasında veya Windows Communication Foundation (WCF) uç noktası parola geri yazma sonuçları için Azure AD Connect makinenizi olay günlüğünde hataları başlatılması sırasında karşılaşılan hataları. <br> <br> Geri yazma yapılandırıldıysa, Azure AD eşitleme (ADSync) hizmetinin yeniden başlatma sırasında WCF uç noktası başlar. Ancak, uç nokta başlatma başarısız olursa, biz olay 6800 oturum ve başlatılması eşitleme hizmeti sağlar. Bu olay varlığını parola geri yazma endpoint başlatılmadığını anlamına gelir. Uç nokta ayarlamayı neden başlatılamıyor olay günlüğü girişleri ait bileşen tarafından oluşturmak yanı sıra bu olay 6800, olay günlüğü ayrıntılarını belirtin. Bu olay günlüğü hatalarını gözden geçirin ve parola geri yazma hala çalışmıyorsa, Azure AD Connect'i yeniden başlatmayı deneyin. Sorun devam ederse, devre dışı bırakın ve parola geri yazma yeniden etkinleştirmek deneyin.
| Kullanıcı parola sıfırlama veya parola geri yazma etkinleştirilmiş bir hesap kilidini açma girişiminde bulunduğunda, işlem başarısız olur. <br> <br> İçeren Azure AD Connect olay günlüğüne bir olay ek olarak, bkz: "eşitleme altyapısı döndürülen bir hata hr = 800700CE, ileti filename = veya uzantısı çok uzun" kilit açma işlemi tamamlandıktan sonra. | Active Directory hesabı için Azure AD Connect bulun ve en çok 127 karakter içeren parola sıfırlama. Ardından açın **eşitleme hizmeti** gelen **Başlat** menüsü. Gözat **Bağlayıcılar** ve Bul **Active Directory Bağlayıcısı**. Seçin ve ardından **özellikleri**. Gözat **kimlik bilgileri** sayfasında ve yeni bir parola girin. Seçin **Tamam** sayfasını kapatın. |
| Azure AD Connect yükleme işleminin son adımında, bu parola geri yazma yapılandırılamıyor belirten bir hata görürsünüz. <br> <br> "Kimlik doğrulama belirteci alma hatası." hatası 32009 metin ile Azure AD Connect uygulama olay günlüğü içerir | Bu hata aşağıdaki iki durumlarda oluşur: <br><ul><li>Azure AD Connect yükleme işleminin başında belirtilen genel yönetici hesabı için hatalı bir parola belirttiniz.</li><li>Azure AD Connect yükleme işleminin başında belirtilen genel yönetici hesabı için bir Federasyon kullanıcısı kullanma girişiminde bulundunuz.</li></ul> Bu sorunu gidermek için yükleme işleminin başında belirtilen genel yönetici için birleştirilmiş bir hesap kullanmadığınızı emin olun. Ayrıca belirtilen parolanın doğru olduğundan emin olun. |
| Azure AD Connect makine olay günlüğü ait çalıştırarak atılır 32002 hata içeriyor. <br> <br> Hata okur: "ServiceBus bağlanma hatası. Belirteç sağlayıcı güvenlik belirteci sağlayamadı." | Şirket içi ortamınız bulutta Azure Service Bus uç noktası bağlanabiliyor değil. Bu hata genellikle belirli bir bağlantı noktası veya web adresi giden bir bağlantı engelleyen bir güvenlik duvarı kuralı kaynaklanır. Bkz: [bağlantı Önkoşullar](./../connect/active-directory-aadconnect-prerequisites.md) daha fazla bilgi için. Bu kurallar güncelleştirildikten sonra Azure AD Connect makineyi yeniden başlatmak ve parola geri yazma yeniden çalışmaya başlamanız gerekir. |
| Belirli bir süre için çalıştıktan sonra Federasyon veya parola karma eşitlenen kullanıcılar parolalarını sıfırlayamazsınız. | Bazı nadir durumlarda, Azure AD Connect'i yeniden başlatıldığında yeniden başlatmak parola geri yazma hizmeti başarısız olabilir. Bu durumlarda, ilk olarak, parola geri yazma etkinleştirilmiş şirket içi görünüp görünmediğini denetleyin. Azure AD Connect Sihirbazı veya PowerShell kullanarak denetleyebilirsiniz (önceki HowTos bölümüne bakın). Özelliğin etkinleştirilmesi için görünürse etkinleştirme veya özelliği yeniden kullanıcı Arabirimi veya PowerShell yoluyla devre dışı bırakmayı deneyin. Bu işe yaramazsa, tam bir kaldırma deneyin ve Azure AD Connect yeniden yükleyin. |
| Parolalarını sıfırlama girişimi federe veya parola karma eşitlenen kullanıcılar parolalarını göndermeye çalışırken bir hata bakın. Hata, bir hizmet sorunu olduğunu gösterir. <br ><br> Bu sorunu ek olarak, parola sıfırlama işlemleri sırasında yönetim Aracısı, şirket içi olay günlüklerinde erişim reddedildi hata görebilirsiniz. | Olay Günlüğü'nde bu hatalar görürseniz, sihirbazda yapılandırma sırasında belirttiğiniz Active Directory Yönetim Aracısı'nı (ADMA) hesabı parola geri yazma için gerekli izinlere sahip olduğunu doğrulayın. <br> <br> Bu izin verilen sonra onu bir saat aracılığıyla aşağı trickle izni için alabileceğine dikkat edin. `sdprop` arka plan görevi etki alanı denetleyicisinde (DC). <br> <br> Çalışmak amacıyla parola sıfırlama için parolasını sıfırlama kullanıcı nesnesinin güvenlik tanımlayıcı damgalı izniniz gerekiyor. Kullanıcı nesnesindeki bu izni görüntülenir kadar parola sıfırlama erişim reddedildi iletisi ile başarısız olmaya devam eder. |
| Federe veya kullanıcıların parolalarını sıfırlama girişimi parola karması eşitlenen kullanıcılar parolalarını gönderdikten sonra hataya bakın. Hata, bir hizmet sorunu olduğunu gösterir. <br> <br> Bu sorunu ek olarak, parola sıfırlama işlemleri sırasında olay günlüklerinde "Nesne bulunamadı" hata belirten Azure AD Connect hizmetinden bir hata görebilirsiniz. | Bu hata genellikle eşitleme altyapısı Azure AD Bağlayıcısı alanı veya bağlı meta veri deposu (MV) veya Azure AD bağlayıcı alanı nesne kullanıcı nesnesi bulamıyor olduğunu gösterir. <br> <br> Bu sorunu gidermek için kullanıcının gerçekten şirket içinden Azure AD Connect geçerli örneği aracılığıyla Azure ad eşitlendiğinden emin olun ve bağlayıcı alanları ve MV nesneleri durumunu inceleyin. Active Directory Sertifika Hizmetleri (AD CS) nesne "Microsoft.InfromADUserAccountEnabled.xxx" kuralı aracılığıyla MV nesneye bağlı olduğunu onaylayın.|
| Federe veya parolalarını gönderdikten sonra parolalarını sıfırlama girişimi parola karması eşitlenen kullanıcılar hataya bakın. Hata, bir hizmet sorunu olduğunu gösterir. <br> <br> Bu sorunu ek olarak, parola sıfırlama işlemleri sırasında olay günlüklerinde "birden çok eşleşme bulunamadı" hatası olduğunu gösteren Azure AD Connect hizmetinden bir hata görebilirsiniz. | Bu, eşitleme altyapısı MV nesne birden fazla AD CS nesnesi "Microsoft.InfromADUserAccountEnabled.xxx" üzerinden bağlandığı algıladığını belirtir. Bu, kullanıcı birden fazla ormanda etkinleştirilmiş bir hesabı olduğu anlamına gelir. Bu senaryo parola geri yazma için desteklenmediğini unutmayın. |
| Parola işlemleri, bir yapılandırma hatasıyla başarısız oluyor. Uygulama olay günlüğü "0x8023061f (Parola Eşitleme etkin değil. Bu yönetim Aracısı olduğundan işlem başarısız oldu)" metni ile Azure AD Connect hata 6329 içerir. | Bu hata, sonra yeni bir Active Directory ormanına eklemek için (veya kaldırın ve mevcut bir ormana yeniden eklemek için) Azure AD Connect yapılandırması değiştirdiyseniz, parola geri yazma özelliği zaten etkin oluşur. Bu kullanıcılar için parola işlemleri yakın zamanda ormanlar başarısız eklendi. Sorunu düzeltmek için devre dışı bırakın ve orman yapılandırma değişiklikleri tamamladıktan sonra parola geri yazma özelliğini yeniden etkinleştirin. |

## <a name="password-writeback-event-log-error-codes"></a>Parola geri yazma olay günlüğü hata kodları

Parola geri yazma sorunları giderirken en iyi Azure AD Connect makinenizi uygulama olay günlüğünü incelemek için uygulamadır. Bu olay günlüğü parola geri yazma için ilgilenilen iki kaynaklardan gelen olayları içerir. Ait kaynak işlemlerinin ve parola geri yazma işlemi için ilgili sorunlar açıklanmaktadır. ADSync kaynak işlemlerinin ve parolaları Active Directory ortamınızı ayarlamakla ilgili sorunları açıklar.

### <a name="if-the-source-of-the-event-is-adsync"></a>Olay kaynağı ADSync ise

| Kod | Adı veya ileti | Açıklama |
| --- | --- | --- |
| 6329 | BAIL: MMS(4924) 0x80230619: "bir kısıtlama parola belirtilen geçerli bir değiştirilen engeller." | Bu olay, parola geri yazma hizmeti parola geçerlilik süresi, geçmiş, karmaşıklık veya etki alanının filtreleme gereksinimlerini karşılamıyor, yerel dizin parola ayarlama girişiminde bulunduğunda oluşur. <br> <br> En az parola geçerlilik süresi varsa ve bu zaman aralığı içinde parola kısa süre önce değiştirilmiştir, etki alanınızda belirtilen yaş ulaşana kadar parolayı tekrar değiştiremezsiniz değilsiniz. Test amacıyla, minimum yaş 0 olarak ayarlanması gerekir. <br> <br> Parola geçmişi gereksinimlerini etkin olması durumunda son kullanılmadı bir parola seçmelisiniz *N* zaman, nerede *N* parola geçmişi ayardır. Son kullanılmış bir parolayı seçeneğini belirlerseniz *N* zaman sonra bu durumda bir hata görürsünüz. Test amacıyla, parola geçmişi 0 olarak ayarlanması gerekir. <br> <br> Parola karmaşıklık gereksinimleri varsa, kullanıcı değiştirme veya bir parola sıfırlama girişiminde bulunduğunda, bunların tümünün uygulanır. <br> <br> Parola filtreleri etkinleştirilmiş varsa ve bir kullanıcı bir parola seçtikten sonra sıfırlama filtreleme ölçütlerini karşılamıyor veya değiştirme işlemi başarısız olur. |
| 6329 | MMS(3040): admaexport.cpp(2837): sunucu LDAP Parola İlkesi denetimi içermiyor. | LDAP_SERVER_POLICY_HINTS_OID denetimi (1.2.840.113556.1.4.2066) DC'leri üzerinde etkin değil, bu sorun oluşur. Parola geri yazma özelliğini kullanmak için denetimi etkinleştirmeniz gerekir. Bunu yapmak için DC'ler (en son SP ile) Windows Server 2008 veya üzeri olması gerekir. (R2 öncesi) 2008'de, DC'lerin olan sonra düzeltmeyi uygulamanız gerekir [KB2386717](http://support.microsoft.com/kb/2386717). |
| HR 8023042 | Eşitleme altyapısı döndürülen bir hata hr = 80230402, ileti aynı bağlantı ile yinelenen girdiler olduğundan başarısız oldu nesneyi alma girişimi =. | Birden çok etki alanında aynı kullanıcı kimliği etkin olduğunda bu hata oluşur. Hesap ve kaynak ormanlar eşitleniyor ve her ormanda aynı kullanıcı kimliği mevcut ve etkin olması örneğidir. <br> <br> Bir diğer ad veya UPN ve iki kullanıcı o aynı bağlantı özniteliği paylaşan gibi benzersiz olmayan bağlayıcı öznitelik kullanırsanız, bu hata ayrıca oluşabilir. <br> <br> Bu sorunu gidermek için etki alanı içinde yinelenen kullanıcılardan yoktur ve her kullanıcı için bir benzersiz bağlantı özniteliği kullanması emin olun. |

### <a name="if-the-source-of-the-event-is-passwordresetservice"></a>Olay kaynağı ait ise

| Kod | Adı veya ileti | Açıklama |
| --- | --- | --- |
| 31001 | PasswordResetStart | Bu olay, şirket içi hizmet isteği buluttan kaynaklanan bir federe veya parola karma eşitlenmiş kullanıcı için bir parola sıfırlama algılanan gösterir. Bu olay her parola sıfırlama geri yazma işleminin ilk etkinliğidir. |
| 31002 | PasswordResetSuccess | Bu olay, bir kullanıcı bir parola sıfırlama işlemi sırasında yeni bir parola seçilen gösterir. Bu parola Kurumsal parola gereksinimlerini karşıladığını belirledi. Parola geri yerel Active Directory ortamına yazıldı. |
| 31003 | PasswordResetFail | Bu olay, seçilen bir kullanıcı bir parola ve parola başarıyla şirket içi ortamına gelen gösterir. Ancak biz yerel Active Directory ortamında parola ayarlamaya çalışırken bir hata oluştu. Bu hata çeşitli nedenlerden kaynaklanabilir: <br><ul><li>Kullanıcının parolasını karşılamıyor yaşı, geçmiş, karmaşıklık, veya etki alanı için gereksinimleri filtre. Bu sorunu çözmek için yeni bir parola oluşturun.</li><li>ADMA hizmet hesabı söz konusu kullanıcı hesabı üzerinde yeni bir parola ayarlamak için uygun izinlere sahip değil.</li><li>Kullanıcının parolasını ayarlama işlemleri izin vermez, etki alanı veya kuruluş Yönetici grubu gibi bir korumalı grup içindeki hesabıdır.</li></ul>|
| 31004 | OnboardingEventStart | Bu olay, Azure AD Connect ile parola geri yazma özelliğini etkinleştirir ve ekleme, kuruluşunuzun parola geri yazma web hizmetine başlattığınız oluşur. |
| 31005 | OnboardingEventSuccess | Bu olay ekleme işleminin başarılı olduğunu ve parola geri yazma özelliği kullanıma hazır olduğunu gösterir. |
| 31006 | ChangePasswordStart | Bu olay, şirket içi hizmeti buluttan kaynaklanan bir federe veya parola karma eşitlenmiş kullanıcı için bir parola değiştirme isteği algıladı gösterir. Bu olay her parola değişikliği geri yazma işleminin ilk etkinliğidir. |
| 31007 | ChangePasswordSuccess | Bu olay, bir kullanıcı yeni bir parola parola değiştirme işlemi sırasında seçilen, biz parola Kurumsal parola gereksinimlerini karşıladığından ve parola başarıyla geri yerel Active Directory ortamına yazıldığını belirlenen gösterir. |
| 31008 | ChangePasswordFail | Bu olay, bir kullanıcı bir parola seçilen ve parola başarıyla şirket içi ortama geldi, ancak biz yerel Active Directory ortamında parola ayarlamaya çalışırken bir hata oluştu gösterir. Bu hata çeşitli nedenlerden kaynaklanabilir: <br><ul><li>Kullanıcının parolasını karşılamıyor yaşı, geçmiş, karmaşıklık, veya etki alanı için gereksinimleri filtre. Bu sorunu çözmek için yeni bir parola oluşturun.</li><li>ADMA hizmet hesabı söz konusu kullanıcı hesabı üzerinde yeni bir parola ayarlamak için uygun izinlere sahip değil.</li><li>Kullanıcının parolasını ayarlama işlemleri izin vermez, etki alanı veya enterprise admins gibi bir korumalı grup içindeki hesabıdır.</li></ul> |
| 31009 | ResetUserPasswordByAdminStart | Şirket içi hizmet isteği Yöneticisi'nden bir kullanıcı adına kaynaklanan bir federe veya parola karma eşitlenmiş kullanıcı için bir parola sıfırlama algıladı. Bu olay bir yönetici tarafından başlatılan her parola sıfırlama geri yazma işleminin, ilk etkinliğidir. |
| 31010 | ResetUserPasswordByAdminSuccess | Yönetici, bir yönetici tarafından başlatılan parola sıfırlama işlemi sırasında yeni bir parola seçili. Bu parola Kurumsal parola gereksinimlerini karşıladığını belirledi. Parola geri yerel Active Directory ortamına yazıldı. |
| 31011 | ResetUserPasswordByAdminFail | Yönetici bir kullanıcı adına bir parola seçili. Parola başarıyla şirket içi ortama gelmedi. Ancak biz yerel Active Directory ortamında parola ayarlamaya çalışırken bir hata oluştu. Bu hata çeşitli nedenlerden kaynaklanabilir: <br><ul><li>Kullanıcının parolasını karşılamıyor yaşı, geçmiş, karmaşıklık, veya etki alanı için gereksinimleri filtre. Bu sorunu çözmek için yeni bir parola deneyin.</li><li>ADMA hizmet hesabı söz konusu kullanıcı hesabı üzerinde yeni bir parola ayarlamak için uygun izinlere sahip değil.</li><li>Kullanıcının parolasını ayarlama işlemleri izin vermez, etki alanı veya enterprise admins gibi bir korumalı grup içindeki hesabıdır.</li></ul>  |
| 31012 | OffboardingEventStart | Bu olay, Azure AD Connect ile parola geri yazma özelliğini devre dışı bırakırsanız oluşur ve biz kuruluşunuzun parola geri yazma web hizmeti için çıkarma başlatıldı gösterir. |
| 31013| OffboardingEventSuccess| Bu olay çıkarma işleminin başarılı olduğunu ve parola geri yazma yeteneği başarıyla devre dışı olduğunu gösterir. |
| 31014| OffboardingEventFail| Bu olay çıkarma işlemi başarılı olmadı gösterir. Bu, yapılandırma sırasında belirtilen bulutta veya şirket içi yönetici hesabı üzerindeki izinleri hatası nedeniyle olabilir. Hata ayrıca parola geri yazma özelliğini devre dışı bırakılırken bir Federasyon bulut genel yönetici kullanılacak denediğiniz oluşabilir. Bu sorunu gidermek için yönetim izinlerinizi denetleyin ve birleştirilmiş bir hesap parola geri yazma özelliği yapılandırırken kullandığınız değil emin olun.|
| 31015| WriteBackServiceStarted| Bu olay, parola geri yazma hizmeti başarıyla başlatıldı gösterir. Bulutta parola yönetimi isteklerini kabul etmeye hazırdır.|
| 31016| WriteBackServiceStopped| Bu olay, parola geri yazma Hizmeti durdu gösterir. Bulut gelen tüm parola yönetimi isteklerin başarılı olmaz.|
| 31017| AuthTokenSuccess| Bu olay, biz çıkarma veya ekleme işlemini başlatmak için Azure AD Connect Kurulum sırasında belirtilen genel yönetici için bir yetkilendirme belirteci başarıyla alındı gösterir.|
| 31018| KeyPairCreationSuccess| Bu olay başarıyla parola şifreleme anahtarını oluşturduğumuz gösterir. Bu anahtar, şirket içi ortamınız için gönderilecek bulutta parolaları şifrelemek için kullanılır.|
| 32000| Başvuruları| Bu olay, bir parola yönetimi işlemi sırasında bilinmeyen bir hata oluştu gösterir. Daha fazla ayrıntı için olay özel durum metni bakın. Sorun yaşıyorsanız, devre dışı bırakma ve parola geri yazma yeniden etkinleştirmeyi deneyin. Bu yardımcı olmazsa, olay günlüğünüzü izleme ile birlikte bir kopyasını dahil destek mühendisinize için belirtilen Insider kimliği.|
| 32001| ServiceError| Bu olay oluştu bulut parola bağlanırken bir hata hizmetini sıfırlamak gösterir. Bu hata genellikle şirket içi servis parola sıfırlama web hizmetine bağlanamadı oluşur.|
| 32002| ServiceBusError| Bu olay, kiracının Service Bus örneğine bağlanırken bir hata oluştu gösterir. Şirket içi ortamınızda giden bağlantıları engelleme bu durum meydana gelebilir. TCP 443 üzerinden ve çok bağlantılara izin emin olmak için güvenlik duvarınızı denetleyin https://ssprsbprodncu-sb.accesscontrol.windows.net/ve ardından yeniden deneyin. Hala sorun yaşıyorsanız, devre dışı bırakma ve parola geri yazma yeniden etkinleştirmeyi deneyin.|
| 32003| InPutValidationError| Bu olay, bizim web hizmeti API'sine geçirilen giriş geçersiz olduğunu gösterir. İşlemi yeniden deneyin.|
| 32004| DecryptionError| Bu olay buluttan gelen parola çözme bir hata olduğunu gösterir. Bu, bulut hizmeti ve şirket içi ortamınız arasında bir şifre çözme anahtarı uyuşmazlığı nedeniyle olabilir. Bu sorunu gidermek için devre dışı bırakmak ve şirket içi ortamınızda parola geri yazma'ı yeniden etkinleştirin.|
| 32005| ConfigurationError| Onboarding işlemi sırasında bir yapılandırma dosyası, şirket içi ortamınızda Kiracı özgü bilgileri kaydedin. Bu olay, bu dosya kaydedilirken bir hata oluştu veya hizmeti başlatıldığında, olduğunu dosyası okunurken bir hata gösterir. Bu sorunu gidermek için devre dışı bırakma ve yeniden yapılandırma dosyasının zorlamak için parola geri yazma yeniden etkinleştirmeyi deneyin.|
| 32007| OnBoardingConfigUpdateError| Hazırlama sırasında size veri buluttan şirket içi parola sıfırlama hizmet göndereceğiz. Disk üzerinde güvenli şekilde depolanması için eşitleme hizmetine gönderilmeden önce bu verileri daha sonra bir bellek içi dosyasına yazılır. Bu olay, yazma ya da bu verileri bellekte güncelleştirme ile ilgili bir sorun olduğunu gösterir. Bu sorunu gidermek için devre dışı bırakma ve bu yapılandırma dosyası yeniden zorlamak için parola geri yazma yeniden etkinleştirmeyi deneyin.|
| 32008| ValidationError| Bu olay, parola sıfırlama web hizmetinden geçersiz bir yanıt aldık gösterir. Bu sorunu gidermek için devre dışı bırakma ve parola geri yazma yeniden etkinleştirmeyi deneyin.|
| 32009| AuthTokenError| Bu olay, bir yetkilendirme Azure AD Connect Kurulum sırasında belirtilen genel yönetici hesabı için belirteç alamadık olduğunu gösterir. Bu hata bir hatalı kullanıcı adı veya genel yönetici hesabı için belirtilen parola neden olabilir. Bu hata ayrıca belirtilen genel yönetici hesabı federe oluşabilir. Bu sorunu gidermek için doğru kullanıcı adını ve parolayı yapılandırmayla yeniden çalıştırın ve yönetici bir yönetilen (yalnızca Bulut veya parola eşitlenmiş) hesabı olduğundan emin olun.|
| 32010| CryptoError| Bu olay, bir sorun oluştu parola şifreleme anahtarını oluşturulurken veya Bulut hizmetinden ulaşan parola çözme gösterir. Bu hata muhtemelen ortamınızın bir sorun olduğunu gösterir. Bu sorunun nasıl çözüleceği hakkında daha fazla bilgi için olay günlüğü ayrıntıları bakın. Ayrıca, devre dışı bırakma ve parola geri yazma hizmeti yeniden etkinleştirerek deneyebilirsiniz.|
| 32011| OnBoardingServiceError| Bu olay, şirket içi hizmet düzgün ekleme işlemini başlatmak için parola sıfırlama web hizmetiyle iletişim kurulamadı gösterir. Kiracınız için bir kimlik doğrulama belirteci alırken bir sorun olup olmadığını ya da bir güvenlik duvarı kuralı sonucunda gerçekleşebilir. Bu sorunu gidermek için giden bağlantılar için veya TCP 443 ve TCP 9350-9354 üzerinden engellemediğini emin olun https://ssprsbprodncu-sb.accesscontrol.windows.net/. Ayrıca, yerleşik değil Federasyon için kullanmakta olduğunuz Azure AD yönetim hesabı emin olun.|
| 32013| OffBoardingError| Bu olay, şirket içi hizmet düzgün çıkarma işlemini başlatmak için parola sıfırlama web hizmetiyle iletişim kurulamadı gösterir. Bu durum ortaya çıkabilir sonucu olarak bir güvenlik duvarı kuralı ya da bir yetkilendirme kiracınız için belirteç alınırken bir sorun olup olmadığını. Bu sorunu gidermek için giden bağlantılar 443 üzerinden veya çok engellemediğini emin olun https://ssprsbprodncu-sb.accesscontrol.windows.net/, ve offboard için kullanmakta olduğunuz Azure Active Directory yönetici hesabı federe değil.|
| 32014| ServiceBusWarning| Bu olay, biz, kiracının Service Bus örneğine bağlanmak için yeniden deneme gerekiyordu gösterir. Normal koşullar altında bu değil ilgili bir sorun olabilir, ancak çoğu zaman, bu olay görürseniz, özellikle bir Yüksek gecikmeli veya düşük bant genişlikli bağlantı ise Service Bus, ağ bağlantısı denetleniyor göz önünde bulundurun.|
| 32015| ReportServiceHealthError| Parola geri yazma Hizmetinizin durumunu izlemek için size sinyal veri parola sıfırlama web hizmetimizi beş dakikada göndereceğiz. Bu olay, bu sistem durumu bilgileri bulut web hizmetine gönderirken bir hata olduğunu gösterir. Bu sistem durumu bilgileri, bir nesne olarak tanımlanabilir bilgileri (OII) veya kişisel bilgileri (PII) verileri içermez ve hizmet durumu bilgileri bulutta sağladığımız tamamen sinyal ve temel hizmeti istatistikleri, böylelikle.|
| 33001| ADUnKnownError| Bu olay, Active Directory tarafından döndürülen bilinmeyen bir hata olduğunu gösterir. Daha fazla bilgi için ADSync kaynağından olayları için Azure AD Connect sunucusunun olay günlüğünü denetleyin.|
| 33002| ADUserNotFoundError| Bu olay, sıfırlamak veya parola değiştirme çalışıyor kullanıcının şirket içi dizininde bulunamadı gösterir. Bu hata, kullanıcı silinen şirket içi kaldığında ancak bulutta oluşabilir. Bu hata, eşitleme ile ilgili bir sorun varsa da oluşabilir. Eşitleme günlüklerinizi ve daha fazla bilgi için son birkaç çalıştırmak eşitleme ayrıntılarını kontrol edin.|
| 33003| ADMutliMatchError| Parola sıfırlama veya buluttan değişiklik isteğinin kaynaklandığı, Azure AD Connect Kurulum işlemi sırasında belirtilen bulut bağlantı bir kullanıcı, şirket içi ortamınızda bu isteği yeniden bağlamak nasıl belirlemek için kullanırız. Bu olay, iki kullanıcı aynı bulut bağlantı özniteliği ile şirket içi dizininizdeki bulduk olduğunu gösterir. Eşitleme günlüklerinizi ve daha fazla bilgi için son birkaç çalıştırmak eşitleme ayrıntılarını kontrol edin.|
| 33004| ADPermissionsError| Bu olay, Active Directory Yönetim Aracısı'nı (ADMA) hizmet hesabı yeni bir parola ayarlamak için söz konusu hesabı uygun izinleri yok gösterir. Kullanıcının ormanında ADMA hesabın sıfırladı ve ormandaki tüm nesnelerin parola izinlerini değiştirme emin olun. Adım 4 izinleri ayarlama hakkında daha fazla bilgi için bkz: uygun Active Directory izinlerini ayarlayabilirsiniz.|
| 33005| ADUserAccountDisabled| Bu olay, biz sıfırlama veya içi devre dışı olan bir hesap için bir parola değiştirme girişiminde bulunuldu gösterir. Hesabı etkinleştirin ve işlemi yeniden deneyin.|
| 33006| ADUserAccountLockedOut| Bu olay, biz sıfırlama veya şirket içi kilitli olan bir hesap için bir parola değiştirme girişiminde bulunuldu gösterir. Bir kullanıcı bir değişiklik denediniz veya parola işlemi çok fazla kez kısa bir süre içinde sıfırlama kilitlemeleri oluşabilir. Hesabın kilidini açıp işlemi yeniden deneyin.|
| 33007| ADUserIncorrectPassword| Bu olay, bir parola gerçekleştirme değiştirdiğinizde işlemi kullanıcı yanlış bir geçerli parola belirtilen gösterir. Doğru geçerli parolayı belirtin ve yeniden deneyin.|
| 33008| ADPasswordPolicyError| Bu olay, parola geri yazma hizmeti parola geçerlilik süresi, geçmiş, karmaşıklık veya etki alanının filtreleme gereksinimlerini karşılamıyor, yerel dizin parola ayarlama girişiminde bulunduğunda oluşur. <br> <br> En az parola geçerlilik süresi varsa ve bu zaman aralığı içinde parola kısa süre önce değiştirilmiştir, etki alanınızda belirtilen yaş ulaşana kadar parolayı tekrar değiştiremezsiniz değilsiniz. Test amacıyla, minimum yaş 0 olarak ayarlanması gerekir. <br> <br> Parola geçmişi gereksinimlerini etkin olması durumunda son kullanılmadı bir parola seçmelisiniz *N* zaman, nerede *N* parola geçmişi ayardır. Son kullanılmış bir parolayı seçeneğini belirlerseniz *N* zaman sonra bu durumda bir hata görürsünüz. Test amacıyla, parola geçmişi 0 olarak ayarlanması gerekir. <br> <br> Parola karmaşıklık gereksinimleri varsa, kullanıcı değiştirme veya bir parola sıfırlama girişiminde bulunduğunda, bunların tümünün uygulanır. <br> <br> Parola filtreleri etkinleştirilmiş varsa ve bir kullanıcı bir parola seçtikten sonra sıfırlama filtreleme ölçütlerini karşılamıyor veya değiştirme işlemi başarısız olur.|
| 33009| ADConfigurationError| Bu olay, bir parola, Active Directory ile bir yapılandırma sorunu nedeniyle şirket içi dizininize geri sorun yazma vardı gösterir. ADSync hizmeti hata oluştuğu daha fazla bilgi için gelen iletiler için Azure AD Connect makinenin uygulama olay günlüğünü denetleyin.|

## <a name="troubleshoot-password-writeback-connectivity"></a>Parola geri yazma bağlantı sorunlarını giderme

Azure AD Connect hizmet kesintilerine parola geri yazma bileşeni ile karşılaşıyorsanız, bu sorunu çözümlemek için uygulayabileceğiniz bazı hızlı adımlar şunlardır:

* [Ağ bağlantısını doğrulayın](#confirm-network-connectivity)
* [Azure AD Connect eşitleme hizmetini yeniden başlatın](#restart-the-azure-ad-connect-sync-service)
* [Devre dışı bırakın ve parola geri yazma özelliğini yeniden etkinleştirin](#disable-and-re-enable-the-password-writeback-feature)
* [En son Azure AD Connect sürümü yüklemek](#install-the-latest-azure-ad-connect-release)
* [Parola geri yazma sorunlarını giderme](#troubleshoot-password-writeback)

Genel olarak, hizmetiniz en hızlı bir şekilde kurtarmak için aşağıdaki adımları daha önce ele alınan sırayla yürütmek öneririz.

### <a name="confirm-network-connectivity"></a>Ağ bağlantısını doğrulayın

En yaygın hatası, güvenlik duvarının noktasıdır ve veya proxy bağlantı noktalarını ve boşta kalma zaman aşımı yanlış yapılandırılmış. 

Azure AD Connect'i sürüm 1.1.443.0 ve yukarıdaki, giden HTTPS aşağıdaki erişim:

   - passwordreset.microsoftonline.com
   - servicebus.Windows.NET

Daha fazla ayrıntı elde etmek için güncelleştirilmiş listesini başvuru [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) her Çarşamba güncelleştirildi ve aşağıdaki yürürlüğe koymak Pazartesi.

Daha fazla bilgi için bağlantı önkoşulları gözden [Azure AD Connect için Önkoşullar](./../connect/active-directory-aadconnect-prerequisites.md) makalesi.



### <a name="restart-the-azure-ad-connect-sync-service"></a>Azure AD Connect eşitleme hizmetini yeniden başlatın

Bağlantı sorunları veya hizmeti ile geçici diğer sorunları çözmek için Azure AD Connect eşitleme hizmetini yeniden başlatın:

   1. Bir yönetici olarak seçin **Başlat** Azure AD Connect çalıştıran sunucuda.
   2. ENTER **services.msc** arama alanı seçip **Enter**.
   3. Ara **Microsoft Azure AD eşitleme** girişi.
   4. Hizmet girişi sağ tıklatın, seçin **yeniden**ve işlemin tamamlanmasını bekleyin.

   ![Azure AD eşitleme hizmetini yeniden başlatın][Service restart]

Bu adımları, bulut hizmeti ile yeniden bağlantı ve yaşıyor olabilir kesintiler çözümleyin. ADSync hizmeti yeniden başlatılıyor sorununuzu çözmezse, devre dışı bırakın ve parola geri yazma özelliği yeniden etkinleştirmek denemenizi öneririz.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Devre dışı bırakın ve parola geri yazma özelliğini yeniden etkinleştirin

Bağlantı sorunları gidermek için devre dışı bırakın ve parola geri yazma özelliğini yeniden etkinleştirin:

   1. Yönetici olarak, Azure AD Connect yapılandırma sihirbazını açın.
   2. İçinde **Azure ad Connect**, Azure AD genel yönetici kimlik bilgilerinizi girin.
   3. İçinde **AD DS'ye Bağlan**, AD etki alanı Hizmetleri yönetici kimlik bilgilerinizi girin.
   4. İçinde **kullanıcılarınızı benzersiz olarak tanımlama**seçin **sonraki** düğmesi.
   5. İçinde **isteğe bağlı özellikler**, Temizle **parola geri yazma** onay kutusu.
   6. Seçin **sonraki** için elde edene kadar değiştirmeden herhangi bir şey kalan iletişim sayfaları aracılığıyla **yapılandırma için hazır** sayfası.
   7. Emin **sayfasını yapılandırmak hazır** gösterir **parola geri yazma** olarak seçeneği **devre dışı** seçip yeşil **yapılandırma** Değişikliklerinizi kaydetmek için düğmesi.
   8. İçinde **tamamlandı**, Temizle **Şimdi Eşitle** seçeneğini ve ardından **son** sihirbazı kapatın.
   9. Azure AD Connect Yapılandırma Sihirbazı'nı yeniden açın.
   10. 2-seçtiğiniz olun dışında 8 arasındaki adımları yineleyin **parola geri yazma** seçeneği **isteğe bağlı özellikler** hizmeti yeniden etkinleştirmek için sayfa.

Bu adımları bağlantınızı bizim bulut hizmeti ile yeniden oluşturun ve yaşıyor olabilir kesintiler çözümleyin.

Devre dışı bırakma ve parola geri yazma özelliği yeniden etkinleştirerek sorununuzu çözmezse, Azure AD Connect'i yeniden yüklemenizi öneririz.

### <a name="install-the-latest-azure-ad-connect-release"></a>En son Azure AD Connect sürümü yüklemek

Azure AD Connect'i yeniden yapılandırma ve bulut hizmetlerimizi ve yerel Active Directory ortamınızı arasında bağlantı sorunları çözebilirsiniz.

Bu adım yalnızca, daha önce açıklanan ilk iki adım denemeden sonra gerçekleştirmenizi öneririz.

> [!WARNING]
> Giden kutusu eşitleme kuralları özelleştirdiyseniz *yükseltmeye devam etmeden önce yedeklemek ve tamamlanmış sonra daha sonra el ile bunları yeniden dağıtın.*

   1. Azure AD Connect'ten en son sürümünü indirme [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).
   2. Azure AD Connect zaten yüklü olduğundan, Azure AD Connect yüklemenizi en son sürüme güncelleştirmek için yerinde yükseltme gerçekleştirmek için gerekir.
   3. İndirilen paketi yürütmek ve izleyin ekrandaki yönergeleri Azure AD Connect makinenizi güncelleştirin.

Önceki adımları bağlantınızı bizim bulut hizmeti ile yeniden oluşturun ve yaşıyor olabilir kesintiler çözmek gerekir.

Azure AD Connect sunucusu en son sürümünü yükleme sorununuzu çözmezse, devre dışı bırakma ve en son sürüm yükledikten sonra parola geri yazma son adım olarak yeniden etkinleştirerek denemenizi öneririz.

## <a name="verify-that-azure-ad-connect-has-the-required-permission-for-password-writeback"></a>Azure AD Connect parola geri yazma için gereken izne sahip olduğunu doğrulayın

Azure AD Connect gerektirir Active Directory **parola sıfırlama** parola geri yazma gerçekleştirme izni. Azure AD Connect verilen şirket içi Active Directory kullanıcı hesabı için gereken izne sahip olup olmadığını öğrenmek için Windows etkin izni özelliğini kullanabilirsiniz:

   1. Azure AD Connect sunucuya oturum açın ve Başlat **Eşitleme Hizmeti Yöneticisi'ni** seçerek **Başlat** > **eşitleme hizmeti**.
   2. Altında **Bağlayıcılar** sekmesinde, şirket içi seçin **Active Directory etki alanı Hizmetleri** bağlayıcı ve ardından **özellikleri**.  

   ![Etkin izni - 2. adım](./media/active-directory-passwords-troubleshoot/checkpermission01.png)  
  
   3. Açılan pencerede seçin **Active Directory ormanına Bağlan** ve Not **kullanıcı adı** özelliği. Bu özellik, dizin eşitleme gerçekleştirmek için Azure AD Connect tarafından kullanılan AD DS hesabıdır. Parola geri yazma gerçekleştirmek Azure AD Connect için AD DS hesap parolası izni sıfırlamanız gerekir.  
   
   ![Etkin izni - 3. adım](./media/active-directory-passwords-troubleshoot/checkpermission02.png) 
  
   4. Bir şirket içi etki alanı denetleyicisine oturum açın ve Başlat **Active Directory Kullanıcıları ve Bilgisayarları** uygulama.
   5. Seçin **Görünüm** ve emin olun **Gelişmiş Özellikler** seçeneği etkinleşir.  
   
   ![Etkin izni - 5. adım](./media/active-directory-passwords-troubleshoot/checkpermission03.png) 
  
   6. Doğrulamak istediğiniz Active Directory kullanıcı hesabı için bakın. Hesap adını sağ tıklatıp **özellikleri**.  
   
   ![Etkin izni - 6. adım](./media/active-directory-passwords-troubleshoot/checkpermission04.png) 

   7. Açılan pencerede Git **güvenlik** sekmesinde ve seçin **Gelişmiş**.  
   
   ![Etkin izni - 7. adım](./media/active-directory-passwords-troubleshoot/checkpermission05.png) 
   
   8. İçinde **yönetici için Gelişmiş güvenlik ayarları** açılır pencere, Git **etkili erişim** sekmesi.
   9. Seçin **bir kullanıcı seçin**, Azure AD tarafından kullanılan AD DS hesabı seçin (3. adıma bakın) bağlanmak ve ardından **görüntülemek etkili erişim**.  
   
   ![Etkin izni - 9. adım](./media/active-directory-passwords-troubleshoot/checkpermission06.png) 
  
   10. Aşağı kaydırın ve Ara **parola sıfırlama**. Giriş bir onay işareti varsa, AD DS hesabı seçilen Active Directory kullanıcı hesabının parolasını sıfırlama izni vardır.  
   
   ![Etkin izni - 10. adım](./media/active-directory-passwords-troubleshoot/checkpermission07.png)  

## <a name="azure-ad-forums"></a>Azure AD forumları

Azure AD hakkında genel bir sorunuz varsa ve Self Servis parola sıfırlama, isteyin Topluluk Yardım için üzerinde [Azure AD forumları](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Topluluk üyeleri mühendisleri, ürün yöneticileri, MVP ve diğer BT uzmanlarının içerir.

## <a name="contact-microsoft-support"></a>Microsoft Destek'e başvurun

Bir sorunun yanıtını bulamazsanız, bizim destek ekiplerini her zaman daha fazla Yardım kullanılabilir.

Düzgün yardımcı olması için bir kasayı açarken mümkün olduğu kadar ayrıntı verin, isteyin. Bu ayrıntıları içerir:

* **Genel hata açıklaması**: hata nedir? Bildirildi davranışı neydi? Nasıl biz hata üretebileceği? Mümkün olduğu kadar ayrıntı sağlar.
* **Sayfa**: hangi sayfa olan, hata olduğunda fark? URL için varsa ve bir sayfasının ekran görüntüsü içerir.
* **Kodu destekleyecek**: hata kullanıcı gördüğünüz oluşturulan destek kodunu neydi?
    * Bu kod bulmak için hatayı yeniden oluşturmaya ve ardından **destek kodu** bağlantı ekranın alt kısmında ve sonuçları GUID destek mühendisine göndermek.

    ![Ekranın altındaki destek kod Bul][Support code]

    * Alt desteği olmadan bir sayfada değilseniz, F12 ve SID ve arama CID seçin ve bu iki sonuçlar destek mühendisine gönderin.
* **Tarih, saat ve saat dilimi**: kesin bir tarih ve saat dahil *saat dilimi ile* , bir hata oluştu.
* **Kullanıcı Kimliği**: hata gördüğünüz kullanıcının edildi? Örnek *user@contoso.com*.
    * Bu, bir Federasyon kullanıcısı mı?
    * Bu, bir parola karması eşitlenmiş kullanıcı mi?
    * Bu bir salt bulut kullanıcı mi?
* **Lisans**: kullanıcı bir Azure AD Premium veya Azure AD temel lisansı atanmış sahip mi?
* **Uygulama olay günlüğü**: parola geri yazma özelliğini kullanıyorsanız ve şirket içi altyapınızda hata ise, uygulama olay günlüğü Azure AD Connect sunucusundan daraltılmış bir kopyasını içerir.



[Service restart]: ./media/active-directory-passwords-troubleshoot/servicerestart.png "Azure AD eşitleme hizmetini yeniden başlatın"
[Support code]: ./media/active-directory-passwords-troubleshoot/supportcode.png "Destek kod penceresinin alt sağ tarafta bulunur"

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler parola Azure AD sıfırlama hakkında ek bilgi sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
