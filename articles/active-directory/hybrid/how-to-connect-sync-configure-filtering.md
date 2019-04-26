---
title: 'Azure AD Connect eşitleme: Filtrelemeyi Yapılandırma | Microsoft Docs'
description: Azure AD Connect eşitleme filtrelemeyi yapılandırma açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 880facf6-1192-40e9-8181-544c0759d506
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: eeb2af6283e5c9d8a41e74152a94b85efdae1866
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60243500"
---
# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect eşitleme: Filtrelemeyi yapılandırma
Filtreleme kullanarak, hangi nesnelerin Azure Active Directory (Azure AD) görüneceğini şirket içi dizininizden denetleyebilirsiniz. Varsayılan yapılandırma, yapılandırılmış Ormanlardaki etki alanlarındaki tüm nesneleri alır. Genel olarak, bu önerilen yapılandırmadır. E-posta gönderin ve herkesin çağırmak için eksiksiz bir genel adres listesi gibi Exchange Online ve Skype Kurumsal, Office 365 iş yükleri kullanarak kullanıcıların yararlanır. Varsayılan yapılandırma ile bir şirket içi Exchange veya Lync uygulamasıyla sahip olabileceği aynı deneyimi sahip.

Bazı durumlarda ancak, gerekli bir varsayılan yapılandırmaya bazı değişiklikler yapın. İşte bazı örnekler:

* Kullanmayı planladığınız [birden çok Azure AD directory topolojisini](plan-connect-topologies.md#each-object-only-once-in-an-azure-ad-tenant). Belirli bir hangi nesnelerin eşitleneceğini denetlemek için bir filtre uygulamak gereken sonra Azure AD dizini.
* Azure veya Office 365 için bir pilot çalıştırın ve Azure AD'de yalnızca bir kullanıcı alt istersiniz. Küçük bir pilot çalışmasında tam genel bir adres işlevselliğini göstermek için liste olması önemli değildir.
* Birçok hizmet hesapları ve Azure AD'de istemediğiniz diğer nonpersonal hesapları var.
* Uyumluluk nedeniyle, tüm kullanıcı hesapları şirket içi silmeyin. Yalnızca bunları devre dışı. Ancak Azure AD'de yalnızca etkin hesapları mevcut olması.

Bu makale, farklı filtreleme yöntemlerinden yapılandırma kapsar.

> [!IMPORTANT]
> Microsoft, resmi olarak belgelenen eylemler dışında Azure AD Connect eşitlemesinin değiştirilmesini veya çalıştırılmasını desteklemez. Bu eylemler, tutarsız veya desteklenmeyen Azure AD Connect eşitlemesi durumuyla sonuçlanabilir. Sonuç olarak Microsoft, bu tür dağıtımlar için teknik destek sağlayamaz.

## <a name="basics-and-important-notes"></a>Temel kavramları ve önemli notlar
Azure AD Connect eşitleme filtreleme istediğiniz zaman etkinleştirebilirsiniz. Dizin eşitleme varsayılan yapılandırma ile başlayın ve ardından süzme filtreleniyor nesneleri artık Azure AD ile eşitlenir. Bu değişiklik nedeniyle, daha önce eşitlendi, ancak ardından filtrelenmiş herhangi bir nesne Azure AD'de, Azure AD'de silinir.

Filtreleme için değişiklik yapmadan başlamadan önce emin olun, [zamanlanmış bir görevi devre dışı](#disable-the-scheduled-task) şekilde doğru henüz doğrulamadınız değişiklikleri yanlışlıkla vermeyin.

Filtreleme aynı anda birçok nesne kaldırabilirsiniz olduğundan değişiklikler Azure AD'ye dışarı aktarma işlemine başlamadan önce yeni filtrelerinizi doğru olduğundan emin olmanız gerekir. Yapılandırma adımları tamamladıktan sonra uymanızı öneririz [doğrulama adımları](#apply-and-verify-changes) dışarı aktarma ve Azure AD'ye değişiklik önce.

Çok sayıda nesne özelliği yanlışlıkla silmesi korumak için "[yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md)" varsayılan olarak açıktır. (Varsayılan olarak 500) filtreleme nedeniyle çok sayıda nesne silerseniz, Azure AD'ye üzerinden geçmek üzere siler izin vermek için bu makaledeki adımları izlemeniz gerekir.

Kasım 2015 öncesinde bir yapı kullanıyorsanız ([1.0.9125](reference-connect-version-history.md#1091250)), bir filtre yapılandırmada değişiklik yapmak ve sonra yapılandırmayı tamamladıktan sonra tam eşitleme tüm parolaların tetiklemek gereken parola karması eşitleme kullanın. Parola tam eşitleme tetikleme adımları için bkz: [tam eşitlemesi tüm parolaların](tshoot-connect-password-hash-synchronization.md#trigger-a-full-sync-of-all-passwords). Derlemeyi 1.0.9125 kullanıyorsanız veya daha sonra normal **tam eşitleme** eylem ayrıca hesaplar parolalarını olup eşitlenmesi gerektiğini ve bu ek adım artık gerekli olup olmadığını.

Varsa **kullanıcı** nesneleri filtreleme bir hata nedeniyle Azure AD'de yanlışlıkla silinmiş, kullanıcı nesnelerini filtreleme yapılandırmalarınızın kaldırarak Azure AD'de yeniden oluşturabilirsiniz. Ardından, dizinlerinizi yeniden eşitleyebilirsiniz. Bu eylem, kullanıcıların Azure AD'de geri dönüşüm kutusundan geri yükler. Ancak, diğer nesne türlerini geri alınamaz. Örneğin, bir güvenlik grubu kaza ve ACL için kullanılan bir kaynak grubu ve kendi ACL'leri kurtarılamaz.

Azure AD Connect, yalnızca bir kez kapsam içinde yer dikkate almıştır nesneleri de siler. Azure AD'de başka bir eşitleme altyapısı tarafından oluşturulmuş nesneleri vardır ve bu nesneler kapsamda olmayan, filtreleme ekleyerek bunları kaldırmaz. Örneğin, tüm dizininizin tam bir kopyasını Azure AD'de oluşturulan bir DirSync sunucunuz ile başlamanız ve baştan etkin filtreleme ile paralel yeni bir Azure AD Connect eşitleme sunucusu yükleme, Azure AD Connect ek nesneler kaldırmaz DirSync tarafından oluşturulur.

Yüklediğinizde veya Azure AD Connect'in yeni bir sürüme yükseltme yaptığınızda filtreleme yapılandırması korunur. Yapılandırma yanlışlıkla bir yükseltmeden sonra yeni bir sürüme ilk eşitleme döngüsünü çalıştırmadan önce değiştiğini değildi doğrulamak için her zaman en iyi bir uygulamadır.

Birden fazla ormanınız varsa, (tümü aynı yapılandırmayı istediğiniz varsayılarak) her orman için bu konuda açıklanan filtreleme yapılandırmalar uygulamanız gerekir.

### <a name="disable-the-scheduled-task"></a>Zamanlanmış görev devre dışı bırak
30 dakikada bir eşitleme döngüsü tetikleyen yerleşik Zamanlayıcı'yı devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Bir PowerShell istemi gidin.
2. Çalıştırma `Set-ADSyncScheduler -SyncCycleEnabled $False` Zamanlayıcı devre dışı bırakmak için.
3. Bu makalede belgelenen değişiklikleri yapın.
4. Çalıştırma `Set-ADSyncScheduler -SyncCycleEnabled $True` Zamanlayıcı'yı yeniden etkinleştirmek için.

**Bir Azure AD Connect derlemeden önce 1.1.105.0 kullanıyorsanız**  
Üç saatte bir eşitleme döngüsü tetikleyen bir zamanlanmış görev devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Başlangıç **Görev Zamanlayıcı** gelen **Başlat** menüsü.
2. Doğrudan altında **Görev Zamanlayıcı Kitaplığı**, adlı görev bulunamadı **Azure AD eşitleme Zamanlayıcısı**, sağ tıklatın ve seçin **devre dışı**.  
   ![Görev Zamanlayıcısı](./media/how-to-connect-sync-configure-filtering/taskscheduler.png)  
3. Artık yapılandırma değişiklikleri ve el ile eşitleme altyapısı çalıştırma **Eşitleme Hizmeti Yöneticisi** Konsolu.

Tüm filtre Değişikliklerinizi tamamladıktan sonra geri dönmeniz unutmayın ve **etkinleştirme** yeniden görev.

## <a name="filtering-options"></a>Filtreleme seçenekleri
Dizin eşitleme aracı için aşağıdaki filtreleme yapılandırma türlerine uygulayabilirsiniz:

* [**Grup tabanlı**](#group-based-filtering): Üzerinde tek bir grup tabanlı filtreleme yalnızca ilk yüklemede Yükleme Sihirbazı kullanılarak yapılandırılabilir.
* [**Etki alanı tabanlı**](#domain-based-filtering): Bu seçeneği kullanarak, hangi etki alanlarının Azure AD'ye eşitleyebilirsiniz seçebilirsiniz. Ayrıca, ekleyebilir ve Azure AD Connect Eşitleme'yi yükledikten sonra şirket içi altyapınızı değişiklikler yaptığınızda etki alanlarını eşitleme altyapısı yapılandırmasından kaldırın.
* [**Kuruluş birimi (OU) – temel**](#organizational-unitbased-filtering): Bu seçeneği kullanarak, OU'ları Azure AD'ye eşitleyebilirsiniz seçebilirsiniz. Bu seçenek, seçili OU içindeki tüm nesne türleri için kullanılabilir.
* [**Öznitelik tabanlı**](#attribute-based-filtering): Bu seçeneği kullanarak, nesneleri nesneleri öznitelik değerlerine göre filtreleyebilirsiniz. Ayrıca, farklı nesne türleri için farklı filtreler de sahip olabilir.

Aynı anda birden çok filtre seçeneklerini kullanabilirsiniz. Örneğin, sadece bir OU'da nesneleri eklemek için OU tabanlı filtreleme kullanabilirsiniz. Aynı zamanda, öznitelik tabanlı filtreleme nesneleri filtrelemek için kullanabilirsiniz. Birden çok filtre yöntemi kullandığınızda, filtreleri mantıksal "ve" arasında filtreleri kullanın.

## <a name="domain-based-filtering"></a>Etki alanı tabanlı filtreleme
Bu bölümde, etki alanı filtre yapılandırma adımlarını sağlar. Eklenen ya da Azure AD Connect yüklendikten sonra etki alanı ormanınızda kaldırıldı, aynı zamanda filtreleme yapılandırmasını güncelleştirmeniz gerekir.

Etki alanı tabanlı filtreleme değiştirmek için tercih edilen Yükleme Sihirbazı'nı çalıştıran ve değiştirerek yoludur [etki alanı ve OU filtreleme](how-to-connect-install-custom.md#domain-and-ou-filtering). Yükleme Sihirbazı'nı bu konusunda belgelenen tüm görevleri otomatik hale getirir.

Herhangi bir nedenle Yükleme Sihirbazı'nı çalıştırmak zamanınız yoksa yalnızca aşağıdaki adımları izlemelidir.

Etki alanı tabanlı filtreleme yapılandırması aşağıdaki adımlardan oluşur:

1. Eşitlemeye dahil etmek istediğiniz etki alanlarını seçin.
2. Her eklenen hem Kaldırılan etki için çalıştırma profillerini ayarlayın.
3. [Değişiklikleri uygulama ve doğrulama](#apply-and-verify-changes).

### <a name="select-the-domains-to-be-synchronized"></a>Eşitlenmesi gereken etki alanları seçin
Eşitlenecek etki alanlarını seçmek için iki yolu vardır:
    - Eşitleme Hizmeti'ni kullanma
    - Azure AD Connect Sihirbazı'nı kullanarak.


#### <a name="select-the-domains-to-be-synchronized-using-the-synchronization-service"></a>Eşitleme hizmeti kullanılarak eşitlenmesi için etki alanı seçin
Etki alanı filtre ayarlamak için aşağıdaki adımları uygulayın:

1. Azure AD Connect eşitleme üyesi olan bir hesap kullanarak çalıştıran sunucuya oturum **ADSyncAdmins** güvenlik grubu.
2. Başlangıç **eşitleme hizmeti** gelen **Başlat** menüsü.
3. Seçin **Bağlayıcılar**hem de **Bağlayıcılar** listesinde, bağlayıcı türüyle seçin **Active Directory Domain Services**. İçinde **eylemleri**seçin **özellikleri**.  
   ![Bağlayıcı özellikleri](./media/how-to-connect-sync-configure-filtering/connectorproperties.png)  
4. Tıklayın **dizin bölümlerini Yapılandır**.
5. İçinde **dizin bölümlerini Seç** listesinde seçin ve gerektiğinde etki alanları seçimini kaldırın. Eşitlemek istediğiniz bölümleri'nin seçili olduğunu doğrulayın.  
   ![Bölümler](./media/how-to-connect-sync-configure-filtering/connectorpartitions.png)  
   Şirket içi Active Directory altyapınızı değiştirildi ve ekledikten veya ormandan etki alanları kaldırılır, ardından **Yenile** güncelleştirilmiş bir listesini almak için düğme. Yenilediğinizde, kimlik bilgileri sorulur. Herhangi bir kimlik bilgisi, Windows Server Active Directory ile okuma erişimi sağlar. Bu iletişim kutusundaki önceden doldurulmuş kullanıcı olmak zorunda değildir.  
   ![Yenileme gerekiyor](./media/how-to-connect-sync-configure-filtering/refreshneeded.png)  
6. İşiniz bittiğinde kapatın **özellikleri** tıklayarak iletişim **Tamam**. Ormandan etki alanları kaldırılırsa, bir etki alanı kaldırıldı ve bu yapılandırma Temizlenen bir açılır ileti diyor.
7. Çalıştırma profilleri ayarlamaya devam edin.

#### <a name="select-the-domains-to-be-synchronized-using-the-azure-ad-connect-wizard"></a>Azure AD Connect Sihirbazı'nı kullanarak eşitlenmesi için etki alanı seçin
Etki alanı filtre ayarlamak için aşağıdaki adımları uygulayın:

1.  Azure AD Connect Sihirbazı'nı başlatın
2.  **Yapılandır**'a tıklayın.
3.  Seçin **eşitleme seçeneklerini özelleştirme** tıklatıp **sonraki**.
4.  Azure AD kimlik bilgilerinizi girin
5.  Üzerinde **bağlı dizinleri** ekran tıklatın **sonraki**.
6.  Üzerinde **etki alanı ve OU filtreleme sayfası** tıklayın **Yenile**.  Artık, yeni etki alanları olgu görüntülenir ve silinen etki alanları görünmez.
   ![Bölümler](./media/how-to-connect-sync-configure-filtering/update2.png)  

### <a name="update-the-run-profiles"></a>Çalıştırma profillerini güncelleştirin
Etki alanı filtrenizle güncelleştirdiyseniz, ayrıca çalıştırma profillerini güncelleştirmeniz gerekir.

1. İçinde **Bağlayıcılar** listesinde, önceki adımda değiştirdiğiniz bağlayıcıyı'nın seçili olduğundan emin olun. İçinde **eylemleri**seçin **çalıştırma profillerini Yapılandır**.  
   ![Çalıştırma profilleri 1 Bağlayıcısı](./media/how-to-connect-sync-configure-filtering/connectorrunprofiles1.png)  
2. Bul ve Profiller belirleyin:
    * Tam içeri aktarma
    * Tam eşitleme
    * Delta içeri aktarma
    * Delta eşitleme
    * Dışarı Aktarma
3. Her profil için ayarlamak **eklenen** ve **kaldırıldı** etki alanları.
    1. Her beş profilleri için her biri için aşağıdaki adımları uygulayın **eklenen** etki alanı:
        1. Çalıştırma profilini seçin ve tıklayın **yeni adım**.
        2. Üzerinde **yapılandırma adımı** sayfasında **türü** adım yazın, yapılandırma profili olarak aynı ada sahip öğelerine, seçin. Ardından **İleri**'ye tıklayın.  
        ![Çalıştırma profilleri 2 Bağlayıcısı](./media/how-to-connect-sync-configure-filtering/runprofilesnewstep1.png)  
        3. Üzerinde **Bağlayıcı yapılandırması** sayfasında **bölüm** açılan menüsünde, etki alanı filtreniz eklediğiniz etki alanı adını seçin.  
        ![Çalıştırma profilleri 3 Bağlayıcısı](./media/how-to-connect-sync-configure-filtering/runprofilesnewstep2.png)  
        4. Kapatmak için **çalıştırma profilini Yapılandır** iletişim kutusunda, tıklayın **son**.
    2. Her beş profilleri için her biri için aşağıdaki adımları uygulayın **kaldırıldı** etki alanı:
        1. Çalıştırma profili seçin.
        2. Varsa **değer** , **bölüm** özniteliktir bir GUID, çalışma adımı seçin ve tıklayın **Sil adım**.  
        ![Çalıştırma profilleri 4 Bağlayıcısı](./media/how-to-connect-sync-configure-filtering/runprofilesdeletestep.png)  
    3. Değişikliğinizi doğrulayın. Eşitlemek istediğiniz her bir etki alanı, her çalıştırma profili adımı olarak listelenmiş olmalıdır.
4. Kapatmak için **çalıştırma profillerini Yapılandır** iletişim kutusunda, tıklayın **Tamam**.
5.  Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam içeri aktarma** ve **Delta eşitleme**. Bölüm okumaya devam edin [Uygula ve değişiklikleri doğrulayın](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Kuruluş birimi tabanlı filtreleme
OU tabanlı filtreleme değiştirmek için tercih edilen Yükleme Sihirbazı'nı çalıştıran ve değiştirerek yoludur [etki alanı ve OU filtreleme](how-to-connect-install-custom.md#domain-and-ou-filtering). Yükleme Sihirbazı'nı bu konusunda belgelenen tüm görevleri otomatik hale getirir.

Herhangi bir nedenle Yükleme Sihirbazı'nı çalıştırmak zamanınız yoksa yalnızca aşağıdaki adımları izlemelidir.

Kuruluş birimi tabanlı filtreleme yapılandırmak için aşağıdaki adımları uygulayın:

1. Azure AD Connect eşitleme üyesi olan bir hesap kullanarak çalıştıran sunucuya oturum **ADSyncAdmins** güvenlik grubu.
2. Başlangıç **eşitleme hizmeti** gelen **Başlat** menüsü.
3. Seçin **Bağlayıcılar**hem de **Bağlayıcılar** listesinde, bağlayıcı türüyle seçin **Active Directory Domain Services**. İçinde **eylemleri**seçin **özellikleri**.  
   ![Bağlayıcı özellikleri](./media/how-to-connect-sync-configure-filtering/connectorproperties.png)  
4. Tıklayın **dizin bölümlerini Yapılandır**, yapılandırın ve ardından istediğiniz etki alanını seçin **kapsayıcıları**.
5. İstendiğinde, herhangi bir kimlik bilgisi okuma erişimi ile şirket içi Active Directory'nize sağlar. Bu iletişim kutusundaki önceden doldurulmuş kullanıcı olmak zorunda değildir.
6. İçinde **kapsayıcı Seç** iletişim kutusunda, bulut dizini ile eşitleyin ve ardından istemediğiniz OU'ları Temizle **Tamam**.  
   ![Kapsayıcıları Seç iletişim kutusunda OU'lar](./media/how-to-connect-sync-configure-filtering/ou.png)  
   * **Bilgisayarlar** kapsayıcı, Azure AD'ye başarıyla eşitlenmesi, Windows 10 bilgisayarlar için seçilmelidir. Etki alanına katılmış bilgisayarları diğer OU'larda bulunuyorsa, bunların seçili olduğundan emin olun.
   * Güvenler içeren birden çok ormanınız varsa, **ForeignSecurityPrincipals** kapsayıcısı seçili olmalıdır. Bu kapsayıcı, ormanlar arası güvenlik grubu üyeliğinin çözümlenebilmesine olanak tanır.
   * **RegisteredDevices** cihaz geri yazma özelliğini etkinleştirdiyseniz OU seçili olmalıdır. Başka bir geri yazma özelliğini kullanırsanız grup geri yazma gibi Bu konumlar seçildiğinden emin olun.
   * Kullanıcılar, iNetOrgPersons, gruplar, kişiler ve bilgisayarlar bulunduğu yere OU seçin. Resim bu OU'lar ManagedObjects OU'da yer alır.
   * Grup tabanlı filtreleme kullanırsanız, grubun bulunduğu OU'ya dahil edilmelidir.
   * Filtreleme Yapılandırma tamamlandıktan sonra eklenen yeni OU'lar eşitlenir veya eşitlenmemiş, yapılandırabileceğinize dikkat edin. Ayrıntılar için sonraki bölüme bakın.
7. İşiniz bittiğinde kapatın **özellikleri** tıklayarak iletişim **Tamam**.
8. Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam içeri aktarma** ve **Delta eşitleme**. Bölüm okumaya devam edin [Uygula ve değişiklikleri doğrulayın](#apply-and-verify-changes).

### <a name="synchronize-new-ous"></a>Yeni OU'ları Eşitle
Filtreleme yapılandırıldıktan sonra oluşturulan yeni OU'lar varsayılan olarak eşitlenir. Bu durum, seçili bir onay kutusu tarafından belirtilir. Ayrıca, bazı alt kuruluş birimlerini işaretini kaldırabilirsiniz. Bu davranışı sağlamak için mavi onay işareti (varsayılan durumuna) beyaz olana kadar kutusuna tıklayın. Daha sonra eşitlemek istemediğiniz tüm alt-OU'lar seçimini kaldırın.

Tüm alt OU'lar eşitlenir, kutunun mavi onay işareti ile beyaz olur.  
![Tüm kutuları seçili OU](./media/how-to-connect-sync-configure-filtering/ousyncnewall.png)

Bazı alt kuruluş birimlerini seçilmemiş, kutunun beyaz onay işareti ile gri olur.  
![Seçili bazı alt OU ile OU](./media/how-to-connect-sync-configure-filtering/ousyncnew.png)

Bu yapılandırma ile ManagedObjects altında oluşturulan yeni bir OU eşitlenir.

Azure AD Connect Yükleme Sihirbazı'nı her zaman bu yapılandırmayı oluşturur.

### <a name="dont-synchronize-new-ous"></a>Yeni OU'ların eşitleme
Filtreleme Yapılandırma tamamlandıktan sonra yeni OU'lar eşitlemek için eşitleme altyapısı yapılandırabilirsiniz. Bu durum, kullanıcı arabiriminde görünen onay işareti ile düz gri kutusu tarafından belirtilir. Bu davranışı sağlamak için onay işareti ile beyaz olana kadar kutusuna tıklayın. Ardından, eşitlemek istediğiniz alt OU'ı seçin.

![Seçili kök ile OU](./media/how-to-connect-sync-configure-filtering/oudonotsyncnew.png)

Bu yapılandırma ile ManagedObjects altında oluşturulan yeni bir OU eşitlenmemiş.

## <a name="attribute-based-filtering"></a>Öznitelik tabanlı filtreleme
Kasım 2015 kullandığınızdan emin olun ([1.0.9125](reference-connect-version-history.md#1091250)) veya daha sonra çalışmaya için bu adımları yapı.

> [!IMPORTANT]
>Microsoft'un önerdiği varsayılan kuralları tarafından oluşturulan değiştirmek için **Azure AD Connect**. Kuralı değiştirmek istiyorsanız, kopyalayın ve özgün kuralı devre dışı bırakın. Kopyalanan kuralı tüm değişiklikleri yapın. Bunu yaptığınızda lütfen unutmayın (özgün kuralı devre dışı bırakma), herhangi bir hata düzeltmeleri veya bu kural aracılığıyla etkinleştirdiği özellikler eksik.

Öznitelik tabanlı filtreleme, filtre nesnelere en esnek yolu budur. Gücünü kullanabileceğiniz [bildirim temelli sağlama](concept-azure-ad-connect-sync-declarative-provisioning.md) nesneyi Azure AD'ye eşitlenen zaman, neredeyse her açıdan denetlemek için.

Uygulayabileceğiniz [gelen](#inbound-filtering) meta veri deposu için Active Directory'den filtreleme ve [giden](#outbound-filtering) Azure AD'ye meta veri filtreleme. Kolay tutmak olduğundan, gelen filtreleme uygulamanızı öneririz. Yalnızca birden çok ormandaki nesneleri değerlendirmesi gerçekleşebilmesi için öncelikle katılmak için gerekli giden filtreleme kullanmanız gerekir.

### <a name="inbound-filtering"></a>Gelen filtreleme
Gelen filtreleme, varsayılan yapılandırma, Azure AD'ye giderek nesneleri eşitlenmesi için bir değer ayarlanmadı meta veri deposu özniteliği cloudFiltered olduğu gerekir kullanır. Bu özniteliğin değeri ayarlanırsa **True**, sonra nesne eşitlenmemiş. Kümesine olmamalıdır **False**, tasarıma göre. Diğer kurallara sahip bir değer katkıda bulunma becerisini emin olmak için bu öznitelik olduğundan yalnızca gereken değerlere sahip **True** veya **NULL** (yok).

Filtreleme gelen, gücünü kullanın **kapsam** eşitlemek veya eşitleme için hangi nesnelerin belirlemek için. Kendi kuruluşunuzun gereksinimlerine uyacak şekilde ayarlamalar burada budur. Kapsam modülü bir **grubu** ve **yan tümcesi** ne zaman bir eşitleme kuralı kapsamında olup olmadığını belirlemek için. Bir grubu bir veya daha çok yan tümce içeriyor. Birden çok yan tümceleri ve birden çok grupları arasında mantıksal "veya" arasında mantıksal "ve" yoktur.

Bize bir örneğe bakın:  
![Kapsam](./media/how-to-connect-sync-configure-filtering/scope.png)  
Bu olarak okunmalıdır **(departman = BT) veya (departman satış ve c = us)**.

Aşağıdaki örnekler ve adımlar, örnek olarak kullanıcı nesnesini kullanın, ancak tüm nesne türleri için bunu kullanabilirsiniz.

Aşağıdaki örneklerde, öncelik değeri 50 ile başlar. Bu, kullanılmayan herhangi bir sayı olabilir, ancak 100'den daha az olmalıdır.

#### <a name="negative-filtering-do-not-sync-these"></a>Negatif filtreleme: "Bu eşitliyor musunuz"
Aşağıdaki örnekte, filtrelemek (eşitleme) tüm kullanıcılara nereden **extensionAttribute15** değerine sahip **NoSync**.

1. Azure AD Connect eşitleme üyesi olan bir hesap kullanarak çalıştıran sunucuya oturum **ADSyncAdmins** güvenlik grubu.
2. Başlangıç **eşitleme kuralları Düzenleyicisi** gelen **Başlat** menüsü.
3. Emin **gelen** seçilir ve tıklayın **Yeni Kural Ekle**.
4. Kural gibi açıklayıcı bir ad verin "*içinde ad – kullanıcı DoNotSyncFilter*". Doğru orman seçin, **kullanıcı** olarak **CS Nesne türü**seçip **kişi** olarak **MV nesne türü**. İçinde **bağlantı türü**seçin **katılın**. İçinde **öncelik**başka bir eşitleme kuralı (örneğin, 50) tarafından kullanılmakta olmayan bir değer yazın ve ardından **sonraki**.  
   ![Gelen 1 açıklama](./media/how-to-connect-sync-configure-filtering/inbound1.png)  
5. İçinde **Scoping filtre**, tıklayın **Grup Ekle**, tıklatıp **yan tümce Ekle**. İçinde **özniteliği**seçin **ExtensionAttribute15**. Emin olun **işleci** ayarlanır **eşit**, değeri yazın **NoSync** içinde **değer** kutusu. **İleri**’ye tıklayın.  
   ![Gelen 2 kapsamı](./media/how-to-connect-sync-configure-filtering/inbound2.png)  
6. Bırakın **katılın** boş kurallar ve ardından **sonraki**.
7. Tıklayın **ekleme dönüşümünü**seçin **FlowType** olarak **sabit**seçip **cloudFiltered** olarak **hedef Öznitelik**. İçinde **kaynak** metin kutusunda, **True**. Tıklayın **Ekle** kuralını kaydetmek için.  
   ![Gelen 3 dönüştürme](./media/how-to-connect-sync-configure-filtering/inbound3.png)
8. Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam eşitleme**. Bölüm okumaya devam edin [Uygula ve değişiklikleri doğrulayın](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Pozitif filtreleme: "Bu yalnızca Eşzamanla"
Ayrıca, Konferans salonu gibi eşitlenmesini belirgin olmayan nesneler dikkate alınması gereken olduğundan ifade pozitif filtreleme daha zor olabilir. Kullanıma hazır varsayılan filtresi geçersiz olacak **içinde ad - kullanıcı katılın**. Özel filtrenizle oluşturduğunuzda, kritik sistem nesneleri, çoğaltma çakışma nesneleri, özel posta kutuları ve hizmet hesapları için Azure AD Connect içermeyecek şekilde emin olun.

Pozitif filtreleme seçeneği iki eşitleme kuralı gerektirir. Eşitlenecek nesneleri doğru kapsamı ile bir kural (veya birkaç) gerekir. Ayrıca eşitlenmesi gerektiğini bir nesne olarak henüz tanımlanmış henüz tüm nesneleri filtreler ikinci bir catch tüm eşitleme kuralı gerekir.

Aşağıdaki örnekte, yalnızca bölüm öznitelik değeri olduğu kullanıcı, nesneyi eşitleme **satış**.

1. Azure AD Connect eşitleme üyesi olan bir hesap kullanarak çalıştıran sunucuya oturum **ADSyncAdmins** güvenlik grubu.
2. Başlangıç **eşitleme kuralları Düzenleyicisi** gelen **Başlat** menüsü.
3. Emin **gelen** seçilir ve tıklayın **Yeni Kural Ekle**.
4. Kural gibi açıklayıcı bir ad verin "*AD'den – kullanıcı satış senkronize*". Doğru orman seçin, **kullanıcı** olarak **CS Nesne türü**seçip **kişi** olarak **MV nesne türü**. İçinde **bağlantı türü**seçin **katılın**. İçinde **öncelik**başka bir eşitleme kuralı (örneğin, 51) tarafından kullanılmakta olmayan bir değer yazın ve ardından **sonraki**.  
   ![Gelen 4 açıklaması](./media/how-to-connect-sync-configure-filtering/inbound4.png)  
5. İçinde **Scoping filtre**, tıklayın **Grup Ekle**, tıklatıp **yan tümce Ekle**. İçinde **özniteliği**seçin **departmanı**. İşleç olarak ayarlandığından emin olun **eşit**, değeri yazın **satış** içinde **değer** kutusu. **İleri**’ye tıklayın.  
   ![5 kapsam gelen](./media/how-to-connect-sync-configure-filtering/inbound5.png)  
6. Bırakın **katılın** boş kurallar ve ardından **sonraki**.
7. Tıklayın **ekleme dönüşümünü**seçin **sabit** olarak **FlowType**seçip **cloudFiltered** olarak **hedef Öznitelik**. İçinde **kaynak** kutusuna **False**. Tıklayın **Ekle** kuralını kaydetmek için.  
   ![Gelen 6 dönüştürme](./media/how-to-connect-sync-configure-filtering/inbound6.png)  
   Açıkça ayarladığınız cloudFiltered için özel bir durum budur **False**.
8. Artık genel eşitleme kuralı oluşturmak sahibiz. Kural gibi açıklayıcı bir ad verin "*içinde ad – kullanıcı tümünü filtre*". Doğru orman seçin, **kullanıcı** olarak **CS Nesne türü**seçip **kişi** olarak **MV nesne türü**. İçinde **bağlantı türü**seçin **katılın**. İçinde **öncelik**, başka bir eşitleme kuralı (örneğin 99) tarafından kullanılmakta olmayan bir değer yazın. (Daha düşük daha yüksek öncelikli) önceki eşitleme kuralı bir öncelik değeri seçtiniz. Ancak, ek bölümler eşitlemeye başlamak istediğinizde daha fazla filtre eşitleme kuralları daha sonra ekleyebilmeniz biraz yer ayrıca bırakmış olabilirsiniz. **İleri**’ye tıklayın.  
   ![Gelen 7 açıklaması](./media/how-to-connect-sync-configure-filtering/inbound7.png)  
9. Bırakın **Scoping filtre** boş ve tıklayın **sonraki**. Boş bir filtre kuralı tüm nesnelere uygulanmaya gösterir.
10. Bırakın **katılın** boş kurallar ve ardından **sonraki**.
11. Tıklayın **ekleme dönüşümünü**seçin **sabit** olarak **FlowType**seçip **cloudFiltered** olarak **hedef Öznitelik**. İçinde **kaynak** kutusuna **True**. Tıklayın **Ekle** kuralını kaydetmek için.  
    ![Gelen 3 dönüştürme](./media/how-to-connect-sync-configure-filtering/inbound3.png)  
12. Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam eşitleme**. Bölüm okumaya devam edin [Uygula ve değişiklikleri doğrulayın](#apply-and-verify-changes).

Gerekiyorsa, ilk türü burada daha fazla nesne eşitlemeye dahil daha fazla kural oluşturabilirsiniz.

### <a name="outbound-filtering"></a>Giden filtreleme
Bazı durumlarda, yalnızca nesnelerin meta veri deposunda katıldıktan sonra filtreleme yapmak gereklidir. Örneğin, kaynak ormandaki posta özniteliği ve bir nesne eşitleneceğini belirlemek için hesap ormandan userPrincipalName özniteliği bakmak gerekli olabilir. Bu gibi durumlarda filtreleme giden kuralı oluşturun.

Bu örnekte, böylece yalnızca kullanıcılar kendi e-posta ve userPrincipalName sonu sahip filtreleme değiştirme @contoso.com eşitlenir:

1. Azure AD Connect eşitleme üyesi olan bir hesap kullanarak çalıştıran sunucuya oturum **ADSyncAdmins** güvenlik grubu.
2. Başlangıç **eşitleme kuralları Düzenleyicisi** gelen **Başlat** menüsü.
3. Altında **kuralları türü**, tıklayın **giden**.
4. Kullandığınız Connect sürümüne bağlı olarak, adlı Kural ya da bulmak **Out AAD için – kullanıcı katılın** veya **Out - AAD'ye kullanıcı katılın SOAInAD**, tıklatıp **Düzenle**.
5. Açılır pencerede yanıt **Evet** kuralın bir kopyası oluşturmasını ister.
6. Üzerinde **açıklama** sayfasında, değişiklik **öncelik** 50 gibi kullanılmayan bir değer.
7. Tıklayın **Scoping filtre** sol taraftaki gezinti ve ardından **Ekle yan tümcesi**. İçinde **özniteliği**seçin **posta**. İçinde **işleci**seçin **ENDSWITH**. İçinde **değer**, türü  **\@contoso.com**ve ardından **Ekle yan tümcesi**. İçinde **özniteliği**seçin **userPrincipalName**. İçinde **işleci**seçin **ENDSWITH**. İçinde **değer**, türü  **\@contoso.com**.
8. **Kaydet**’e tıklayın.
9. Yapılandırmayı tamamlamak için çalıştırmanız gerekir bir **tam eşitleme**. Bölüm okumaya devam edin [Uygula ve değişiklikleri doğrulayın](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Değişiklikleri uygulama ve doğrulama
Yapılandırma değişikliklerini yaptıktan sonra zaten sistemde olan nesneleri için uygulamanız gerekir. Şu anda eşitleme altyapısında olmayan nesnelerin işlenmesi gerektiğini de olabilir (ve eşitleme altyapısının yeniden içeriğini doğrulamak için kaynak sistemi okuması gerekir).

Kullanarak yapılandırması değiştiyse **etki alanı** veya **kuruluş birimi** filtreleme sonra yapmanız gereken bir **tam içeri aktarma**çizgidir **değişim Eşitleme**.

Kullanarak yapılandırması değiştiyse **özniteliği** filtreleme sonra yapmanız gereken bir **tam eşitleme**.

Aşağıdaki adımları uygulayın:

1. Başlangıç **eşitleme hizmeti** gelen **Başlat** menüsü.
2. Seçin **Bağlayıcılar**. İçinde **Bağlayıcılar** listesinde, bir yapılandırma değişikliği daha önce burada yaptığınız bağlayıcısını seçin. İçinde **eylemleri**seçin **çalıştırma**.  
   ![Bağlayıcı çalıştırın](./media/how-to-connect-sync-configure-filtering/connectorrun.png)  
3. İçinde **çalıştırma profilleri**, önceki bölümde bahsedilen işlemi seçin. İki eylem çalıştırmanız gerekiyorsa, ilk tamamlandıktan sonra ikinci çalıştırın. ( **Durumu** sütun **boşta** seçili Bağlayıcısı.)

Eşitleme sonrasında tüm değişiklikleri aktarılabilmesi için hazırlanmıştır. Azure AD'de aslında bir değişiklik yapmadan önce bu değişiklikleri doğru olduğundan emin olun istiyorsunuz.

1. Bir komut istemi açın ve gidin `%Program Files%\Microsoft Azure AD Sync\bin`.
2. `csexport "Name of Connector" %temp%\export.xml /f:x` öğesini çalıştırın.  
   Eşitleme Hizmeti Bağlayıcısı adıdır. "Contoso.com – AAD" benzer bir adı varsa Azure AD için.
3. `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` öğesini çalıştırın.
4. Artık Microsoft Excel'de incelenebilir export.csv adlı % temp % içinde bir dosya var. Bu dosya, dışarı aktarılacak olan tüm değişiklikleri içerir.
5. Veriler veya yapılandırma için gerekli değişiklikleri yapın ve şu adımları yeniden (içeri aktarma, eşitleme ve doğrulama) çalıştırın, dışarı aktarılacak olan değişiklikleri beklediğiniz kadar.

Memnun kaldığınızda, değişiklikler Azure AD'ye dışarı aktarın.

1. Seçin **Bağlayıcılar**. İçinde **Bağlayıcılar** listesinde, Azure AD Bağlayıcısı'nı seçin. İçinde **eylemleri**seçin **çalıştırma**.
2. İçinde **çalıştırma profilleri**seçin **dışarı**.
3. Daha sonra yapılandırma değişikliklerinizi birçok nesne silerseniz, bir sayı (varsayılan olarak 500) yapılandırılan eşik değerinden daha fazla olduğunda dışarı aktarma hata bakın. Bu hatayı sonra geçici olarak devre dışı bırakmak gereken "[yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md)" özelliği.

Artık Zamanlayıcı'yı yeniden etkinleştirmek için zamanı geldi.

1. Başlangıç **Görev Zamanlayıcı** gelen **Başlat** menüsü.
2. Doğrudan altında **Görev Zamanlayıcı Kitaplığı**, adlı görev bulunamadı **Azure AD eşitleme Zamanlayıcısı**, sağ tıklatın ve seçin **etkinleştirme**.

## <a name="group-based-filtering"></a>Grup tabanlı filtreleme
Grup tabanlı filtreleme, Azure AD Connect kullanarak yüklemenizi en başta yapılandırabileceğiniz [özel yükleme](how-to-connect-install-custom.md#sync-filtering-based-on-groups). Bu, bir pilot dağıtım için eşitlenecek nesneleri yalnızca az sayıda istediğiniz içindir. Grup tabanlı filtreleme devre dışı bıraktığınızda, yeniden etkinleştirilemez. Sahip *desteklenmiyor* grup tabanlı filtreleme özel bir yapılandırmada kullanılacak. Yalnızca bu özellik, Yükleme Sihirbazı'nı kullanarak yapılandırmak için desteklenir. Pilot uygulamanızı tamamladıktan sonra bir filtre seçeneklerinden birini bu konudaki kullanın. OU tabanlı Grup tabanlı filtrelemeyle birlikte filtreleme kullanırken, Grup ve üyelerini bulunduğu yere OU(s) dahil edilmelidir.

Birden fazla AD ormanına eşitlerken her AD Bağlayıcısı için farklı bir grup belirterek grup tabanlı filtreleme yapılandırabilirsiniz. İsterseniz, bir kullanıcı bir AD'de eşitlemek için orman ve aynı kullanıcı olan bir veya daha fazla karşılık gelen diğer AD ormanlarında nesneleri kullanıcı nesne ve tüm ilgili nesneleri kapsam içinde grup tabanlı'nin filtreleme sağlamalısınız. Örnekler için:

* Karşılık gelen FSP (yabancı güvenlik sorumlusu) nesne başka bir ormanda olan tek bir ormandaki bir kullanıcı vardır. Her iki nesne içinde grup tabanlı kapsam filtreleme gerekir. Aksi takdirde, kullanıcının Azure AD'ye eşitlenmez.

* Başka bir ormanda bir ilgili kaynak hesabı (örneğin, bağlı posta kutusu) sahip tek bir ormandaki bir kullanıcı vardır. Daha fazla kaynak hesabı kullanıcıyla bağlamak için Azure AD Connect yapılandırdınız. Her iki nesne içinde grup tabanlı kapsam filtreleme gerekir. Aksi takdirde, kullanıcının Azure AD'ye eşitlenmez.

* Karşılık gelen bir posta olan bir ormandaki bir kullanıcı sahip başka bir ormanda kişi. Daha fazla kullanıcı posta kişiyle bağlamak için Azure AD Connect yapılandırdınız. Her iki nesne içinde grup tabanlı kapsam filtreleme gerekir. Aksi takdirde, kullanıcının Azure AD'ye eşitlenmez.


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.
- Daha fazla bilgi edinin [şirket içi kimliklerinizi Azure AD ile tümleştirme](whatis-hybrid-identity.md).
