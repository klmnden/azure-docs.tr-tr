---
title: 'Azure AD Connect eşitleme: filtrelemeyi yapılandırma | Microsoft Docs'
description: Azure AD Connect eşitleme filtrelemenin nasıl yapılandırılacağı açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 880facf6-1192-40e9-8181-544c0759d506
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 0b4b306d1224b5521774b05a110c862b58450eb3
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect Eşitleme: Filtrelemeyi yapılandırma
Filtreleme kullanarak, hangi nesnelerin Azure Active Directory (Azure AD) görüneceğini şirket içi dizininizden denetleyebilirsiniz. Varsayılan yapılandırma, yapılandırılmış ormandaki tüm etki alanlarındaki tüm nesneleri alır. Genel olarak, bu önerilen yapılandırmadır. E-posta gönderin ve herkesin çağırmak için Exchange Online ve Skype Kurumsal, gibi Office 365 iş yükleri kullanarak kullanıcıların tam genel adres listesinden yararlanır. Varsayılan yapılandırma ile kullanıcılar bir şirket içi Exchange veya Lync uygulamasıyla olurdu aynı deneyimi gerekir.

Bazı durumlarda ancak, gerekli varsayılan yapılandırması bazı değişiklikler yapın. İşte bazı örnekler:

* Kullanmayı planladığınız [birden çok Azure AD directory topolojisi](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-tenant). Belirli bir hangi nesnelerin eşitleneceğini denetlemek için bir filtre uygulamak gereken sonra Azure AD dizini.
* Azure veya Office 365 için bir pilot çalıştırın ve Azure AD içinde yalnızca bir kullanıcı alt kümesini istiyor. Küçük pilot genel adres işlevselliğini göstermek için liste sağlamak önemli değildir.
* Çok sayıda hizmet hesapları ve Azure AD içinde istemediğiniz nonpersonal diğer hesapları var.
* Uyumluluk nedenleriyle, tüm kullanıcı hesapları şirket içi silmeyin. Yalnızca bunları devre dışı. Ancak Azure AD'de yalnızca bulunması için etkin hesaplar.

Bu makalede, farklı filtreleme yöntemlerinin nasıl yapılandırılacağı ele alınmaktadır.

> [!IMPORTANT]
> Microsoft, değiştirme veya Azure AD Connect eşitleme resmi olarak belgelenen eylemleri dışındaki işletim desteklemiyor. Bu eylemlerden herhangi birini bir Azure AD Connect eşitleme tutarsız veya desteklenmeyen durumda neden olabilir. Sonuç olarak, Microsoft teknik destek böyle dağıtımlarda sağlayamaz.

## <a name="basics-and-important-notes"></a>Temel kavramları ve önemli notlar
Azure AD Connect eşitleme herhangi bir zamanda filtreleme etkinleştirebilirsiniz. Dizin eşitleme, varsayılan yapılandırmayla başlatma ve filtrelemeyi yapılandırma, filtrelenmiş nesneler artık Azure AD ile eşitlenir. Bu değişikliği nedeniyle, Azure AD'de daha önce eşitlendi, ancak ardından filtre tüm nesnelerin Azure AD'de silinir.

Filtreleme için değişiklik yapmaya başlamadan önce emin olun, [zamanlanmış görevi devre dışı](#disable-scheduled-task) şekilde doğru olması için henüz doğrulamadınız değişiklikleri yanlışlıkla vermeyin.

Filtreleme birçok nesne aynı anda kaldırabilirsiniz çünkü herhangi bir değişiklik için Azure AD dışarı aktarma başlamadan önce yeni filtrelerinizi doğru olduğundan emin olmak istersiniz. Yapılandırma adımlarını tamamladıktan sonra uymanızı öneririz [doğrulama adımları](#apply-and-verify-changes) dışarı aktarma ve Azure AD ile değişiklik yapmak için önce.

Yanlışlıkla özellik birçok nesne silme korumak için "[yanlışlıkla silmeleri engelleme](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)" varsayılan olarak açıktır. (Varsayılan değer 500) filtreleme nedeniyle birçok nesne silerseniz, Azure AD üzerinden geçerek siler izin vermek için bu makaledeki adımları gerekir.

Kasım 2015 önce bir yapı kullanıyorsanız ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), bir filtre yapılandırma değişiklik ve yapılandırmayı tamamladıktan sonra tam eşitleme tüm parolaların tetiklemek gereken sonra parola karma eşitlemesi, kullanın. Parola tam eşitlemesi adımları için bkz: [tüm parolaların tam eşitlemesi](active-directory-aadconnectsync-troubleshoot-password-hash-synchronization.md#trigger-a-full-sync-of-all-passwords). Yapı 1.0.9125 üzerinde değilseniz veya sonraki sürümlerde, sonra normal **tam eşitleme** eylemi de hesaplar parolalarını eşitlenmiş olması gerektiği ve bu ek adım artık gerekli olup olmadığını.

Varsa **kullanıcı** yanlışlıkla silinen nesneleri Azure AD'de bir filtre hatası nedeniyle, kullanıcı nesnelerini filtreleme yapılandırmalarınızın kaldırarak Azure AD'de yeniden oluşturabilirsiniz. Ardından, dizinleri yeniden eşitleyebilirsiniz. Bu eylem, kullanıcıların Azure AD'de Geri Dönüşüm Kutusu'ndan geri yükler. Ancak, diğer nesne türleri geri alamazsınız. Örneğin, bir güvenlik grubu yanlışlıkla silerseniz ve ACL için kullanılan bir kaynak, Grup ve onun ACL'ler kurtarılamıyor.

Azure AD Connect, yalnızca bir kez kapsamda olarak kabul nesneleri siler. Başka bir eşitleme altyapısı tarafından oluşturulan nesneleri Azure AD'de vardır ve bu nesnelerin kapsamında değil, filtreleme ekleyerek bunları kaldırmaz. Örneğin, tüm dizin tam bir kopyasını Azure AD'de oluşturulan bir DirSync sunucunuz ile başlayın ve baştan etkin filtreleme ile paralel yeni bir Azure AD Connect eşitleme sunucusu yüklerseniz, Azure AD Connect ek nesneleri kaldırmaz DirSync tarafından oluşturulur.

Yüklediğinizde veya Azure AD Connect daha yeni bir sürüme yükselttiğinizde filtreleme yapılandırması korunur. Yapılandırma yanlışlıkla bir yükseltmeden sonra yeni bir sürüme ilk eşitleme döngüsü çalıştırmadan önce değiştiğini değildi doğrulamak için her zaman en iyi bir uygulamadır.

Birden fazla ormanınız varsa, (tümü için aynı yapılandırmayı istediğiniz varsayılarak) her orman için bu konuda açıklanan filtreleme yapılandırmalar uygulamanız gerekir.

### <a name="disable-the-scheduled-task"></a>Zamanlanmış görev devre dışı bırak
30 dakikada bir eşitleme döngüsü tetikleyen yerleşik Zamanlayıcısı'nı devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Bir PowerShell komut istemi gidin.
2. Çalıştırma `Set-ADSyncScheduler -SyncCycleEnabled $False` Zamanlayıcısı'nı devre dışı bırakmak için.
3. Bu makalede belgelenen değişiklikleri yapın.
4. Çalıştırma `Set-ADSyncScheduler -SyncCycleEnabled $True` Zamanlayıcı'yı yeniden etkinleştirmek için.

**Bir Azure AD Connect yapı 1.1.105.0 önce kullanıyorsanız**  
Üç saatte bir eşitleme döngüsü tetikler zamanlanmış görevi devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Başlat **Görev Zamanlayıcı** gelen **Başlat** menüsü.
2. Doğrudan altında **Görev Zamanlayıcı Kitaplığı**, adlı görev bulunamadı **Azure AD Sync Scheduler**, sağ tıklatın ve seçin **devre dışı**.  
   ![Görev Zamanlayıcısı](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Artık yapılandırma değişikliklerini yapın ve el ile eşitleme altyapısı çalıştırabilirsiniz **Eşitleme Hizmeti Yöneticisi'ni** konsol.

Tüm filtre değişiklikleri tamamladıktan sonra geri dönmeniz unutmayın ve **etkinleştirmek** görev yeniden.

## <a name="filtering-options"></a>Filtreleme seçenekleri
Dizin eşitleme aracı için aşağıdaki filtreleme yapılandırma türleri uygulayabilirsiniz:

* [**Grup tabanlı**](#group-based-filtering): tek bir grup göre filtreleme yalnızca yapılandırılabilir ilk yüklemesinde Yükleme Sihirbazı'nı kullanarak.
* [**Etki alanı tabanlı**](#domain-based-filtering): Bu seçeneği kullanarak, hangi etki alanlarının Azure AD'ye eşitleyebilirsiniz seçebilirsiniz. Ayrıca, ekleyebilir ve Azure AD Connect eşitleme yükledikten sonra şirket içi altyapınızda değişiklikler yaptığınızda etki alanları eşitleme altyapısı yapılandırmasından kaldırın.
* [**Kuruluş birimi (OU) – temel**](#organizational-unitbased-filtering): Bu seçeneği kullanarak, OU'ları Azure AD'ye eşitleyebilirsiniz seçebilirsiniz. Bu seçenek, seçili OU içindeki tüm nesne türleri için kullanılır.
* [**Öznitelik tabanlı**](#attribute-based-filtering): Bu seçeneği kullanarak nesneleri nesneleri öznitelik değerlerine göre filtreleyebilirsiniz. Ayrıca, farklı nesne türleri için farklı filtreler de olabilir.

Aynı anda birden çok filtre seçeneklerini kullanabilirsiniz. Örneğin, yalnızca bir OU'da nesneleri içerecek şekilde OU tabanlı filtreleme kullanabilirsiniz. Aynı anda özniteliği tabanlı filtreleme nesneleri daha fazla filtrelemek için kullanabilirsiniz. Birden çok filtre yöntemi kullandığınızda, filtreleri filtreler arasında mantıksal "ve" kullanın.

## <a name="domain-based-filtering"></a>Etki alanı tabanlı filtreleme
Bu bölümde, etki alanı filtre yapılandırma adımlarını sağlar. Eklenen veya Azure AD Connect yüklendikten sonra ormanda etki alanları kaldırıldı, ayrıca filtre yapılandırmanızı güncelleştirmeniz gerekir.

Etki alanı tabanlı filtreleme değiştirmek için tercih edilen Yükleme Sihirbazı'nı çalıştıran ve değiştirme yoludur [etki alanı ve OU filtreleme](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Yükleme Sihirbazı'nı bu konuda belgelenmiştir tüm görevleri otomatik hale getirir.

Herhangi bir nedenle Yükleme Sihirbazı'nı çalıştırmak yapamıyorsanız yalnızca aşağıdaki adımları izlemelisiniz.

Etki alanı tabanlı filtreleme yapılandırması aşağıdaki adımlardan oluşur:

1. [Etki alanlarını seçin](#select-domains-to-be-synchronized) eşitlemeye dahil etmek istediğiniz.
2. Her eklenebilir ve etki alanı kaldırılabilir için Ayarla [çalıştırma profili](#update-run-profiles).
3. [Uygulama ve değişiklikleri doğrulamak](#apply-and-verify-changes).

### <a name="select-the-domains-to-be-synchronized"></a>Eşitlenecek etki alanlarını seçin
Etki alanı filtre ayarlamak için aşağıdaki adımları uygulayın:

1. Oturum üyesi olan bir hesabı kullanarak Azure AD Connect eşitleme çalıştıran sunucuya **ADSyncAdmins** güvenlik grubu.
2. Başlat **eşitleme hizmeti** gelen **Başlat** menüsü.
3. Seçin **Bağlayıcılar**hem de **Bağlayıcılar** listesinde, bağlayıcı türü olan seçin **Active Directory etki alanı Hizmetleri**. İçinde **Eylemler**seçin **özellikleri**.  
   ![Bağlayıcı özellikleri](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Tıklatın **dizin bölümlerini Yapılandır**.
5. İçinde **dizin bölümlerini Seç** listesinden seçin ve gerektiğinde etki alanları seçimini kaldırın. Eşitlemek istediğiniz bölümleri'nin seçili olduğunu doğrulayın.  
   ![Bölümler](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
   Şirket içi Active Directory altyapınızı değişti ve ekledikten veya ormandan etki alanları kaldırılır, ardından **yenileme** güncelleştirilmiş listesini almak için düğmesi. Yenilediğinizde, kimlik bilgileri sorulur. Herhangi bir kimlik bilgisi için Windows Server Active Directory ile okuma erişimi sağlar. İletişim kutusunda önceden doldurulmaz kullanıcı olması gerekmez.  
   ![Yenileme gerekiyor](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. İşiniz bittiğinde kapatmanız **özellikleri** tıklatarak iletişim **Tamam**. Ormandan etki alanları kaldırdıysanız, bir etki alanı kaldırıldı ve bu yapılandırma temizlendi açılır ileti söyler.
7. Ayarlamaya devam [çalıştırma profili](#update-run-profiles).

### <a name="update-the-run-profiles"></a>Çalıştırma profillerini güncelleştir
Etki alanı filtre güncelleştirdik, ayrıca çalıştırma profillerini güncelleştirmeniz gerekir.

1. İçinde **Bağlayıcılar** listesinde, önceki adımda değiştirdiğiniz Connector'ın seçili olduğundan emin olun. İçinde **Eylemler**seçin **çalıştırma profillerini Yapılandır**.  
   ![Çalıştırma profilleri 1 Bağlayıcısı](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  
2. Bul ve Profiller belirleyin:
    * Tam içeri aktarma
    * Tam eşitleme
    * Delta içeri aktarma
    * Delta eşitleme
    * Dışarı Aktarma
3. Her bir profil için ayarlamak **eklenen** ve **kaldırılan** etki alanları.
    1. Her beş profilleri için her biri için aşağıdaki adımları uygulayın **eklenen** etki alanı:
        1. Çalıştırma profili seçin ve tıklatın **yeni adım**.
        2. Üzerinde **yapılandırma adımı** sayfasında **türü** adım türü yapılandırmakta profili aynı ada sahip açılan menüsünde seçin. Ardından **İleri**'ye tıklayın.  
        ![Çalıştırma profilleri 2 Bağlayıcısı](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
        3. Üzerinde **Bağlayıcı yapılandırması** sayfasında **bölüm** açılır menüsünde, etki alanı filtreniz eklediğiniz etki alanı adı seçin.  
        ![Çalıştırma profilleri 3 Bağlayıcısı](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
        4. Kapatmak için **çalıştırma profilini Yapılandır** iletişim kutusunda, tıklatın **son**.
    2. Her beş profilleri için her biri için aşağıdaki adımları uygulayın **kaldırılan** etki alanı:
        1. Çalıştırma profilini seçin.
        2. Varsa **değeri** , **bölüm** özniteliği bir GUID, çalışma adımı seçin ve'ı tıklatın **silmek adım**.  
        ![Çalıştırma profilleri 4 Bağlayıcısı](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  
    3. Değişikliğiniz doğrulayın. Eşitlemek istediğiniz her bir etki alanı, her çalıştırma profili bir adım olarak listelenmesi gerekir.
4. Kapatmak için **çalıştırma profillerini Yapılandır** iletişim kutusunda, tıklatın **Tamam**.
5.  Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam alma** ve **Delta eşitleme**. Bölüm okuma devam [Uygula ve değişiklikleri doğrulamak](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Kuruluş birimi tabanlı filtreleme
OU tabanlı filtreleme değiştirmek için tercih edilen Yükleme Sihirbazı'nı çalıştıran ve değiştirme yoludur [etki alanı ve OU filtreleme](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Yükleme Sihirbazı'nı bu konuda belgelenmiştir tüm görevleri otomatik hale getirir.

Herhangi bir nedenle Yükleme Sihirbazı'nı çalıştırmak yapamıyorsanız yalnızca aşağıdaki adımları izlemelisiniz.

Kuruluş birimi – göre filtrelemeyi yapılandırmak için aşağıdaki adımları uygulayın:

1. Oturum üyesi olan bir hesabı kullanarak Azure AD Connect eşitleme çalıştıran sunucuya **ADSyncAdmins** güvenlik grubu.
2. Başlat **eşitleme hizmeti** gelen **Başlat** menüsü.
3. Seçin **Bağlayıcılar**hem de **Bağlayıcılar** listesinde, bağlayıcı türü olan seçin **Active Directory etki alanı Hizmetleri**. İçinde **Eylemler**seçin **özellikleri**.  
   ![Bağlayıcı özellikleri](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Tıklatın **dizin bölümlerini Yapılandır**, yapılandırmak ve ardından istediğiniz etki alanını seçin **kapsayıcıları**.
5. İstendiğinde, herhangi bir kimlik bilgisi, şirket içi Active Directory ile okuma erişimi sağlar. İletişim kutusunda önceden doldurulmaz kullanıcı olması gerekmez.
6. İçinde **kapsayıcıları Seç** iletişim kutusunda, bulut dizini ile eşitleyebilir ve ardından istemediğiniz OU'ları temizleyin **Tamam**.  
   ![Kapsayıcıları Seç iletişim kutusunda OU'lar](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
   * **Bilgisayarlar** kapsayıcı, Azure AD ile başarıyla eşitlenmek üzere Windows 10 bilgisayarlarınız için seçilmelidir. Etki alanına katılmış bilgisayarlarınızı diğer OU'larda bulunuyorsa, bunlar seçildiğinden emin olun.
   * Güvenler içeren birden çok ormanınız varsa, **ForeignSecurityPrincipals** kapsayıcısı seçili olmalıdır. Bu kapsayıcı, ormanlar arası güvenlik grubu üyeliğinin çözümlenebilmesine olanak tanır.
   * **RegisteredDevices** cihaz geri yazma özelliği etkinleştirilmişse, OU seçilmelidir. Başka bir geri yazma özelliğini kullanırsanız, grup geri yazma gibi bu konumları seçildiğinden emin olun.
   * Kullanıcılar, iNetOrgPersons, gruplar, kişiler ve bilgisayarlar bulunduğu OU seçin. Aşağıdaki resimde, bu OU'lar ManagedObjects OU'da yer alır.
   * Grup tabanlı filtreleme kullanırsanız, Grup bulunduğu OU'ya dahil edilmelidir.
   * Filtreleme yapılandırması tamamlandıktan sonra eklenen yeni OU'lar eşitlenen veya eşitlenmemiş olmadığını yapılandırabilirsiniz unutmayın. Ayrıntılar için sonraki bölüme bakın.
7. İşiniz bittiğinde kapatmanız **özellikleri** tıklatarak iletişim **Tamam**.
8. Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam alma** ve **Delta eşitleme**. Bölüm okuma devam [Uygula ve değişiklikleri doğrulamak](#apply-and-verify-changes).

### <a name="synchronize-new-ous"></a>Yeni OU'lar Eşitle
Varsayılan olarak filtreleme yapılandırıldıktan sonra oluşturulan yeni OU'lar eşitlenir. Bu durum, seçili bir onay kutusu tarafından belirtilir. Ayrıca, bazı alt kuruluş birimlerini işaretini kaldırabilirsiniz. Bu davranışı elde etmek üzere mavi onay işaretiyle (varsayılan durumuna) beyaz oluncaya kadar kutusuna tıklayın. Ardından eşitlemek istemediğiniz tüm alt-OU'lar seçimini kaldırın.

Tüm alt OU'lar eşitlenir, kutunun mavi bir onay işareti ile beyaz olur.  
![Seçili tüm kutuları ile OU](./media/active-directory-aadconnectsync-configure-filtering/ousyncnewall.png)

Bazı alt kuruluş birimlerini seçilmemiş olsa, kutu gri beyaz onay işaretine sahip olur.  
![Bazı alt-seçili OU'ları ile OU](./media/active-directory-aadconnectsync-configure-filtering/ousyncnew.png)

Bu yapılandırma ile ManagedObjects altında oluşturulan yeni bir OU eşitlenir.

Azure AD Connect Yükleme Sihirbazı'nı her zaman bu yapılandırma oluşturur.

### <a name="dont-synchronize-new-ous"></a>Yeni OU'lar eşitleme
Filtreleme yapılandırması tamamlandıktan sonra yeni OU'lar eşitlemek için eşitleme altyapısı yapılandırabilirsiniz. Bu durum kullanıcı arabiriminde hiçbir onay işaretiyle düz gri görünen kutusu tarafından belirtilir. Bu davranışı elde etmek üzere hiçbir onay işaretiyle beyaz oluncaya kadar kutusuna tıklayın. Ardından, eşitlemek istediğiniz alt OU seçin.

![Seçili kök ile OU](./media/active-directory-aadconnectsync-configure-filtering/oudonotsyncnew.png)

Bu yapılandırma ile ManagedObjects altında oluşturulan yeni bir OU eşitlenmemiş.

## <a name="attribute-based-filtering"></a>Öznitelik tabanlı filtreleme
Kasım 2015 kullandığınızdan emin olun ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) veya daha sonra bu adımları çalışmaya oluşturabilirsiniz.

Öznitelik tabanlı filtreleme, filtre nesnelere en esnek yoludur. Gücünü kullanabilirsiniz [bildirim temelli hazırlama](active-directory-aadconnectsync-understanding-declarative-provisioning.md) bir nesne için Azure AD zaman eşitlenmiş, neredeyse her açıdan denetlemek için.

Uygulayabileceğiniz [gelen](#inbound-filtering) için meta veri, Active Directory'den filtreleme ve [giden](#outbound-filtering) için Azure AD meta veri filtreleme. Kolay korumak olduğu için gelen filtreleme uygulamanızı öneririz. Yalnızca değerlendirme gerçekleştirilebildiği önce birden çok orman nesneleri katılmak için gerekli olan, giden filtreleme kullanmanız gerekir.

### <a name="inbound-filtering"></a>Filtreleme gelen
Gelen filtreleme, varsayılan yapılandırma, Azure AD ile giderek nesneleri için bir değer eşitlenecek ayarlanmadı meta veri deposu özniteliği cloudFiltered sahip olduğu gerekir kullanır. Bu özniteliğin değeri ayarlanmışsa **doğru**, nesne eşitlenmemiş sonra. Kümesine döndürmemelidir **yanlış**, tasarım. Bir değer katkıda yeteneği olan diğer kurallar emin olmak için bu öznitelik yalnızca bekleniyor değerlere sahip **True** veya **NULL** (yok).

Gelen filtreleme ın gücünü kullanın **kapsam** eşitleme veya değil eşitlemek için hangi nesnelerin belirlemek için. Bu, kendi kuruluşunuzun gereksinimlerine uyacak şekilde ayarlamalar burada olur. Kapsam modülü bir **grup** ve **yan tümcesi** bir eşitleme kuralının kapsamda olduğunda belirlemek için. Bir veya daha çok yan tümceleri içeren bir grubu. Birden çok yan tümcesi birden çok gruplar arasındaki mantıksal "veya" arasındaki mantıksal "ve" yoktur.

Bize bir örneğe bakın:  
![Kapsam](./media/active-directory-aadconnectsync-configure-filtering/scope.png)  
Bu olarak okunmalıdır **(departman = BT) veya (departman = satış ve c = US)**.

Aşağıdaki örnekleri ve adımları, kullanıcı nesnesi bir örnek olarak kullanabilirsiniz, ancak bu tüm nesne türleri için kullanabilirsiniz.

Aşağıdaki örneklerde, öncelik değeri 50 ile başlar. Bu, kullanılmayan herhangi bir sayı olabilir, ancak 100'den daha az olmalıdır.

#### <a name="negative-filtering-do-not-sync-these"></a>Negatif filtreleme: "Bu eşitleme değil"
Aşağıdaki örnekte, filtrelemek (eşitleme değil) tüm kullanıcılar nerede **extensionAttribute15** değerine sahip **NoSync**.

1. Oturum üyesi olan bir hesabı kullanarak Azure AD Connect eşitleme çalıştıran sunucuya **ADSyncAdmins** güvenlik grubu.
2. Başlat **eşitleme kuralları Düzenleyicisi** gelen **Başlat** menüsü.
3. Emin olun **gelen** seçilir ve tıklatın **Yeni Kural Ekle**.
4. Kuralına gibi açıklayıcı bir ad verin "*içinde AD'den – kullanıcı DoNotSyncFilter*". Doğru orman seçin, **kullanıcı** olarak **CS Nesne türü**seçip **kişi** olarak **MV nesne türü**. İçinde **bağlantı türü**seçin **katılma**. İçinde **öncelik**, şu anda başka bir eşitleme kuralı (örneğin, 50) tarafından kullanılmayan bir değer yazın ve ardından **sonraki**.  
   ![Gelen 1 açıklaması](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. İçinde **Scoping filtre**, tıklatın **Grup Ekle**, tıklatıp **yan tümce Ekle**. İçinde **özniteliği**seçin **ExtensionAttribute15**. Olduğundan emin olun **işleci** ayarlanır **eşit**ve değer yazın **NoSync** içinde **değeri** kutusu. **İleri**’ye tıklayın.  
   ![2 kapsam gelen](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Bırakın **katılma** boş kuralları ve ardından **sonraki**.
7. Tıklatın **ekleme dönüşümünü**seçin **FlowType** olarak **sabit**seçip **cloudFiltered** olarak **hedef Öznitelik**. İçinde **kaynak** metin kutusunda, **doğru**. Tıklatın **Ekle** kuralını kaydetmek için.  
   ![3 dönüştürme gelen](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam eşitleme**. Bölüm okuma devam [Uygula ve değişiklikleri doğrulamak](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Pozitif filtreleme: "bunlar yalnızca eşitleme"
Ayrıca konferans odaları gibi eşitlenmesi belirgin olmayan nesneler dikkate almanız gerekir çünkü pozitif filtreleme ifade daha zor olabilir. Out-of-box kuralında varsayılan filtre geçersiz kılma olacak **içinde AD'den - kullanıcı katılma**. Özel filtre oluşturduğunuzda, kritik sistem nesneleri, yineleme çakışma nesneleri, özel posta kutuları ve hizmet hesapları için Azure AD Connect değil eklediğinizden emin olun.

Pozitif filtreleme seçeneği iki eşitleme kuralı gerektirir. Eşitlenecek nesneleri doğru kapsamla bir kural (veya birkaç) gerekir. Ayrıca, eşitlenmesi gereken bir nesne olarak henüz tanımlanmış henüz tüm nesneleri filtreleyen ikinci bir catch tüm eşitleme kuralı gerekir.

Aşağıdaki örnekte, yalnızca kullanıcı nesneleri departmanı özniteliğinin değeri olduğu eşitleme **satış**.

1. Oturum üyesi olan bir hesabı kullanarak Azure AD Connect eşitleme çalıştıran sunucuya **ADSyncAdmins** güvenlik grubu.
2. Başlat **eşitleme kuralları Düzenleyicisi** gelen **Başlat** menüsü.
3. Emin olun **gelen** seçilir ve tıklatın **Yeni Kural Ekle**.
4. Kuralına gibi açıklayıcı bir ad verin "*AD'den – kullanıcı satış senkronize*". Doğru orman seçin, **kullanıcı** olarak **CS Nesne türü**seçip **kişi** olarak **MV nesne türü**. İçinde **bağlantı türü**seçin **katılma**. İçinde **öncelik**, şu anda başka bir eşitleme kuralı (örneğin 51) tarafından kullanılmayan bir değer yazın ve ardından **sonraki**.  
   ![4 açıklama gelen](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. İçinde **Scoping filtre**, tıklatın **Grup Ekle**, tıklatıp **yan tümce Ekle**. İçinde **özniteliği**seçin **departmanı**. İşleci olarak ayarlandığından emin olun **eşit**ve değer yazın **satış** içinde **değeri** kutusu. **İleri**’ye tıklayın.  
   ![5 kapsam gelen](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Bırakın **katılma** boş kuralları ve ardından **sonraki**.
7. Tıklatın **ekleme dönüşümünü**seçin **sabit** olarak **FlowType**seçip **cloudFiltered** olarak **hedef Öznitelik**. İçinde **kaynak** kutusuna **False**. Tıklatın **Ekle** kuralını kaydetmek için.  
   ![6 dönüştürme gelen](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
   Bu açıkça ayarladığınız cloudFiltered için özel bir durumdur **False**.
8. Biz şimdi catch tüm eşitleme kuralı oluşturmanız gerekir. Kuralına gibi açıklayıcı bir ad verin "*içinde AD'den – kullanıcı Catch-tüm filtre*". Doğru orman seçin, **kullanıcı** olarak **CS Nesne türü**seçip **kişi** olarak **MV nesne türü**. İçinde **bağlantı türü**seçin **katılma**. İçinde **öncelik**, şu anda başka bir eşitleme kuralı (örneğin 99) tarafından kullanılmayan bir değer yazın. Yüksek (daha düşük önceliğe) önceki eşitleme kuralı bir öncelik değeri seçtiniz. Ancak ek departmanlar Eşitlemeyi Başlat istediğinizde daha fazla filtreleme eşitleme kurallarını daha sonra ekleyeceği bazı yer de bırakmış olabilirsiniz. **İleri**’ye tıklayın.  
   ![7 açıklama gelen](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Bırakın **Scoping filtre** boş öğesini tıklatıp **sonraki**. Boş bir filtre kuralı tüm nesnelere uygulanacak gösterir.
10. Bırakın **katılma** boş kuralları ve ardından **sonraki**.
11. Tıklatın **ekleme dönüşümünü**seçin **sabit** olarak **FlowType**seçip **cloudFiltered** olarak **hedef Öznitelik**. İçinde **kaynak** kutusuna **doğru**. Tıklatın **Ekle** kuralını kaydetmek için.  
    ![3 dönüştürme gelen](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam eşitleme**. Bölüm okuma devam [Uygula ve değişiklikleri doğrulamak](#apply-and-verify-changes).

Gerekiyorsa, burada daha çok nesne eşitlemeye dahil ilk türü daha fazla kural oluşturabilirsiniz.

### <a name="outbound-filtering"></a>Giden filtreleme
Bazı durumlarda, yalnızca meta veri deposunda nesneleri katıldıktan sonra filtreleme yapmak gereklidir. Örneğin, kaynak ormandaki posta özniteliğinin ve bir nesne olarak eşitleneceğini belirlemek için hesap ormandan userPrincipalName özniteliği bakmak gerekli olabilir. Bu durumlarda, filtreleme giden kuralı oluşturun.

Bu örnekte, böylece yalnızca kullanıcılar, posta ve userPrincipalName biten sahip filtreleme değiştirmek @contoso.com eşitlenir:

1. Oturum üyesi olan bir hesabı kullanarak Azure AD Connect eşitleme çalıştıran sunucuya **ADSyncAdmins** güvenlik grubu.
2. Başlat **eşitleme kuralları Düzenleyicisi** gelen **Başlat** menüsü.
3. Altında **kuralları türü**, tıklatın **giden**.
4. Kullandığınız Bağlan sürümüne bağlı olarak, ya da adlı kural bulunamadı **Out AAD'ye – kullanıcı katılma** veya **çıkışı AAD'ye - kullanıcı katılma SOAInAD**, tıklatıp **Düzenle**.
5. Açılır pencerede yanıt **Evet** kuralın bir kopyası oluşturulamıyor.
6. Üzerinde **açıklama** sayfasında, değişiklik **öncelik** 50 gibi kullanılmayan bir değere.
7. Tıklatın **Scoping filtre** sol gezinti ve ardından **Ekle yan tümcesi**. İçinde **özniteliği**seçin **posta**. İçinde **işleci**seçin **ENDSWITH**. İçinde **değeri**, türü **@contoso.com**ve ardından **Ekle yan tümcesi**. İçinde **özniteliği**seçin **userPrincipalName**. İçinde **işleci**seçin **ENDSWITH**. İçinde **değeri**, türü **@contoso.com**.
8. **Kaydet**’e tıklayın.
9. Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam eşitleme**. Bölüm okuma devam [Uygula ve değişiklikleri doğrulamak](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Uygulama ve değişiklikleri doğrulayın
Yapılandırma değişikliklerini yaptıktan sonra zaten sistemde bulunan nesnelere uygulamanız gerekir. Eşitleme Altyapısı şu anda olmayan nesneler işlenmesi gerektiğini de olabilir (ve yeniden içeriğini doğrulamak için kaynak sistemi okumak eşitleme altyapısı gerekiyor).

Kullanarak yapılandırmayı değiştirdiyseniz **etki alanı** veya **kuruluş birimi** filtreleme yapmanıza gerek sonra bir **tam alma**, ardından **Delta Eşitleme**.

Kullanarak yapılandırmayı değiştirdiyseniz **özniteliği** filtreleme yapmanıza gerek sonra bir **tam eşitleme**.

Aşağıdaki adımları uygulayın:

1. Başlat **eşitleme hizmeti** gelen **Başlat** menüsü.
2. Seçin **Bağlayıcılar**. İçinde **Bağlayıcılar** listesinde, bir yapılandırma değişikliği daha önce yaptığınız burada Bağlayıcısı'nı seçin. İçinde **Eylemler**seçin **çalıştırmak**.  
   ![Bağlayıcı çalıştırın](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. İçinde **çalıştırma profili**, önceki bölümde belirtildiği işlemi seçin. İki eylem çalıştırmanız gerekiyorsa, ilk tamamlandıktan sonra ikinci çalıştırın. ( **Durumu** sütunu **boşta** Seçili bağlayıcı için.)

Eşitlemeden sonra tüm değişiklikleri verilecek hazırlanır. Azure AD'de gerçekte bir değişiklik yapmadan önce bu değişiklikleri doğru olduğundan emin olun istiyor.

1. Bir komut istemi açın ve gidin `%Program Files%\Microsoft Azure AD Sync\bin`.
2. `csexport "Name of Connector" %temp%\export.xml /f:x` öğesini çalıştırın.  
   Eşitleme Hizmeti Bağlayıcısı adıdır. "Contoso.com için – AAD" benzer bir ada sahip Azure AD için.
3. `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` öğesini çalıştırın.
4. Artık Microsoft Excel'de incelenebilir export.csv adlı % temp % içinde bir dosyanız var. Bu dosya dışarı aktarılacak olan tüm değişiklikleri içerir.
5. Veriler veya yapılandırma gerekli değişiklikleri yapın ve bu adımları tekrar (içeri aktarma, eşitleme ve doğrula) çalıştırın, dışarı aktarılacak olan değişiklikleri beklediğiniz kadar.

Memnun kaldığınızda, değişiklikler için Azure AD verin.

1. Seçin **Bağlayıcılar**. İçinde **Bağlayıcılar** listesinde, Azure AD Bağlayıcısı'nı seçin. İçinde **Eylemler**seçin **çalıştırmak**.
2. İçinde **çalıştırma profili**seçin **verme**.
3. Daha sonra yapılandırma değişikliklerinizi birçok nesne silerseniz, sayı (varsayılan değer 500) yapılandırılmış eşikten daha fazla olduğunda hata dışarı bakın. Bu hatayı görmek sonra geçici olarak devre dışı bırakmanız gerekir "[yanlışlıkla silmeleri engelleme](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)" özelliği.

Şimdi Zamanlayıcı'yı yeniden etkinleştirmek için zaman yapılır.

1. Başlat **Görev Zamanlayıcı** gelen **Başlat** menüsü.
2. Doğrudan altında **Görev Zamanlayıcı Kitaplığı**, adlı görev bulunamadı **Azure AD Sync Scheduler**, sağ tıklatın ve seçin **etkinleştirmek**.

## <a name="group-based-filtering"></a>Grup tabanlı filtreleme
Grup tabanlı filtreleme kullanarak Azure AD Connect yükleme ilk kez yapılandırabilirsiniz [özel yükleme](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups). Eşitlenecek nesneleri, yalnızca küçük bir kümesini istediğiniz, pilot dağıtımlar için tasarlanmıştır. Grup tabanlı filtreleme devre dışı bıraktığınızda, yeniden etkinleştirilemez. Bunun *desteklenmiyor* grup tabanlı filtreleme özel bir yapılandırmada kullanılacak. Yalnızca bu özellik, Yükleme Sihirbazı'nı kullanarak yapılandırmak için desteklenir. Pilot uygulamanızı tamamladıktan sonra diğer filtreleme seçenekleri birini bu konudaki kullanın. OU dayalı birlikte grup tabanlı ile filtreleme kullanırken, Grup ve üyelerini bulunduğu OU(s) dahil edilmelidir.

Birden fazla AD ormanına eşitlerken her AD Bağlayıcısı için farklı bir grubu belirterek grup tabanlı filtreleme yapılandırabilirsiniz. İsterseniz, bir kullanıcı bir AD içinde eşitlemek için orman ve aynı kullanıcı olan bir ya da daha fazla karşılık gelen diğer AD ormanlarda nesneleri, kullanıcı nesnesi ve tüm ilgili nesneleri kapsam grup tabanlı içinde'nin filtreleme emin olmalısınız. Örnekler için:

* Karşılık gelen bir FSP (yabancı güvenlik sorumlusu) nesnesini başka bir ormanda olan bir ormandaki bir kullanıcı sahip. Her iki nesneleri Grup tabanlı içinde kapsam filtreleme gerekir. Aksi takdirde, kullanıcının Azure AD ile eşitlenmeyecek.

* Karşılık gelen bir kaynak hesabı (örneğin, bağlı posta kutusu) başka bir ormanda olan bir ormandaki bir kullanıcı sahip. Ayrıca, kaynak hesabı kullanıcıyla bağlamak için Azure AD Connect yapılandırdınız. Her iki nesneleri Grup tabanlı içinde kapsam filtreleme gerekir. Aksi takdirde, kullanıcının Azure AD ile eşitlenmeyecek.

* Karşılık gelen bir posta olan bir ormandaki bir kullanıcı sahip başka bir ormanda başvurun. Ayrıca, kullanıcının posta kişiyle bağlamak için Azure AD Connect yapılandırdınız. Her iki nesneleri Grup tabanlı içinde kapsam filtreleme gerekir. Aksi takdirde, kullanıcının Azure AD ile eşitlenmeyecek.


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.
- Daha fazla bilgi edinmek [şirket içi kimliklerinizi Azure AD ile tümleştirme](active-directory-aadconnect.md).
