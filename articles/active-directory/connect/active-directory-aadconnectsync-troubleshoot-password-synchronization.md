---
title: "Parola Eşitleme ile Azure AD Connect eşitleme sorunlarını giderme | Microsoft Docs"
description: "Bu makalede parola eşitleme sorunlarını giderme hakkında bilgi sağlar."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: d3bb2883257896c72cc616ea7476f3d25ee6aa4b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a>Parola Eşitleme ile Azure AD Connect eşitleme sorunlarını giderme
Bu konu, parola eşitleme ile ilgili sorunları gidermek adımlar sağlar. Parolalar beklendiği gibi eşitlemiyor, bir kullanıcı alt kümesini veya tüm kullanıcılar için olabilir.

İçin Azure Active Directory (Azure AD) Bağlan dağıtım 1.1.614.0 sürümüyle ya da sonra sorun giderme Görev parola eşitleme sorunlarını gidermek için Sihirbazı'nda kullanın:

* Burada hiçbir parolalar eşitlenir bir sorun varsa, başvurmak [hiçbir parolalar eşitlenir: sorun giderme görevini kullanarak sorun giderme](#no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task) bölümü.

* Ayrı ayrı nesneleri ile ilgili bir sorun varsa, başvurmak [bir nesne değil parolaları eşitleme: sorun giderme görevini kullanarak sorun giderme](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task) bölümü.

Dağıtım 1.1.524.0 sürümüyle ya da daha sonra parola eşitleme sorunlarını gidermek için kullanabileceğiniz bir tanı cmdlet vardır:

* Burada hiçbir parolalar eşitlenir bir sorun varsa, başvurmak [hiçbir parolalar eşitlenir: Tanılama cmdlet'ini kullanarak sorun giderme](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) bölümü.

* Ayrı ayrı nesneleri ile ilgili bir sorun varsa, başvurmak [bir nesne değil parolaları eşitleme: Tanılama cmdlet'ini kullanarak sorun giderme](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) bölümü.

Azure AD Connect dağıtımı daha eski sürümleri için:

* Burada hiçbir parolalar eşitlenir bir sorun varsa, başvurmak [hiçbir parolalar eşitlenir: sorun giderme adımları el ile](#no-passwords-are-synchronized-manual-troubleshooting-steps) bölümü.

* Ayrı ayrı nesneleri ile ilgili bir sorun varsa, başvurmak [bir nesne değil parolaları eşitleme: sorun giderme adımları el ile](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) bölümü.



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task"></a>Hiçbir parolalar eşitlenir: sorun giderme görevini kullanarak sorun giderme
Hiçbir parolalar neden eşitlenir bulmak için sorun giderme görevini kullanabilirsiniz.

> [!NOTE]
> Sorun giderme yalnızca Azure AD Connect sürüm 1.1.614.0 kullanılabilir veya sonraki bir görevdir.

### <a name="run-the-troubleshooting-task"></a>Sorun giderme görevi çalıştırma
Burada hiçbir parolalar eşitlenir sorunları gidermek için:

1. Azure AD Connect sunucunuzla üzerinde yeni bir Windows PowerShell oturumu açın **yönetici olarak çalıştır** seçeneği.

2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.

3. Azure AD Connect Sihirbazı'nı başlatın.

4. Gidin **ek görevleri** sayfasında, **sorun giderme**, tıklatıp **sonraki**.

5. Sorun giderme sayfasında, tıklatın **başlatma** sorun giderme menü PowerShell'de başlatmak için.

6. Ana menüde seçin **parola eşitleme sorunlarını giderme**.

7. Alt menüde seçin **parola eşitleme hiç çalışmıyor**.

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme Görev sonuçlarını anlama
Sorun giderme görev aşağıdaki denetimleri gerçekleştirir:

* Parola Eşitleme Özelliği Azure AD kiracınız için etkin olduğunu doğrular.

* Azure AD Connect sunucusu hazırlama modunda olduğunu doğrular.

* (Var olan bir Active Directory ormanına karşılık gelir) her mevcut şirket içi Active Directory Bağlayıcısı için:

   * Parola eşitleme özelliği etkin olduğunu doğrular.
   
   * Parola eşitleme sinyali olayları Windows uygulama olay günlüklerinde arar.

   * Her Active Directory etki alanı için şirket içi Active Directory Bağlayıcısı altında:

      * Etki alanı Azure AD Connect sunucudan erişilebilir olduğunu doğrular.

      * Doğru kullanıcı adı, parola ve parola eşitlemesi için gerekli izinlere sahip olduğundan'şirket içi Active Directory Bağlayıcısı tarafından kullanılan Active Directory etki alanı Hizmetleri (AD DS) hesaplarına doğrular.

Aşağıdaki diyagram, bir tek etki alanı, şirket içi Active Directory topolojisi cmdlet'inin sonuçlarını gösterir:

![Parola Eşitleme için tanılama çıktıları](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

Bu bölümde rest ilgili sorunları ve görev tarafından döndürülen belirli sonuçlarını açıklar.

#### <a name="password-synchronization-feature-isnt-enabled"></a>Parola eşitleme özelliği etkin değil
Parola Eşitleme Azure AD Connect sihirbazını kullanarak etkinleştirmediyseniz şu hatayı döndürdü:

![Parola Eşitleme etkin değil](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a>Azure AD Connect'i hazırlama modunda sunucusudur
Azure AD Connect'i hazırlama modunda sunucusuysa, parola eşitleme geçici olarak devre dışıdır ve şu hatayı döndürdü:

![Azure AD Connect'i hazırlama modunda sunucusudur](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a>Parola eşitleme sinyali olayı yok
Her bir şirket içi Active Directory Bağlayıcısı kendi parola eşitleme kanalının sahiptir. Parola Eşitleme kanalının kurulur ve eşitlenecek parola değişiklikleri yok, bir sinyal olayı (EventID 654) her 30 dakikada altındaki Windows uygulama olay günlüğü oluşturulur. Her şirket içi Active Directory Bağlayıcısı için cmdlet son üç saat içinde karşılık gelen sinyal olaylarını arar. Sinyal Olayı bulunursa, şu hatayı döndürdü:

![Olay yok parola eşitleme Kalp yapı](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a>AD DS hesap doğru izinlere sahip değil
Şirket içi Active Directory Bağlayıcısı tarafından parola karmaları eşitlemek için kullanılan AD DS hesap uygun izinlere sahip değilse, şu hatayı döndürdü:

![Yanlış kimlik bilgileri](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a>Kullanıcı adı veya parola yanlış AD DS hesap
Parola karmaları eşitlemek için şirket içi Active Directory Bağlayıcısı tarafından kullanılan AD DS hesabı hatalı kullanıcı adı veya parola varsa, şu hatayı döndürdü:

![Yanlış kimlik bilgileri](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task"></a>Bir nesne değil parolaları eşitleme: sorun giderme görevini kullanarak sorun giderme

Sorun giderme Görev neden bir nesne parolaları eşitlemiyor belirlemek için kullanabilirsiniz.

> [!NOTE]
> Sorun giderme yalnızca Azure AD Connect sürüm 1.1.614.0 kullanılabilir veya sonraki bir görevdir.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet'ini çalıştırın
Belirli bir kullanıcı nesnesi için sorunları gidermek için:

1. Azure AD Connect sunucunuzla üzerinde yeni bir Windows PowerShell oturumu açın **yönetici olarak çalıştır** seçeneği.

2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.

3. Azure AD Connect Sihirbazı'nı başlatın.

4. Gidin **ek görevleri** sayfasında, **sorun giderme**, tıklatıp **sonraki**.

5. Sorun giderme sayfasında, tıklatın **başlatma** sorun giderme menü PowerShell'de başlatmak için.

6. Ana menüde seçin **parola eşitleme sorunlarını giderme**.

7. Alt menüde seçin **parola belirli bir kullanıcı hesabı için eşitlenmemiş**.

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme Görev sonuçlarını anlama
Sorun giderme görev aşağıdaki denetimleri gerçekleştirir:

* Active Directory Bağlayıcısı alanı, meta veri deposu ve Azure Active Directory nesne durumunu inceler AD bağlayıcı alanı.

* Eşitleme kuralları etkinleştirilmiş ve Active Directory nesneye uygulanan parola eşitlemeye sahip olduğunu doğrular.

* Almak ve son nesne için eşitleme girişimi sonuçlarını görüntülemek çalışır.

Aşağıdaki diyagram, tek bir nesne için parola eşitleme sorunlarını giderirken cmdlet'inin sonuçlarını gösterir:

![Parola Eşitleme - tek nesne için tanılama çıktıları](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

Bu bölümde rest ilgili sorunları ve cmdlet tarafından döndürülen belirli sonuçları açıklar.

#### <a name="the-active-directory-object-isnt-exported-to-azure-ad"></a>Active Directory nesnesi Azure AD'ye aktarılan değil
Azure AD kiracısında karşılık gelen hiçbir nesne olduğundan bu şirket içi Active Directory hesabı için parola eşitleme başarısız olur. Şu hatayı döndürdü:

![Azure AD nesnesi eksik.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a>Kullanıcının geçici bir parolaya sahip
Şu anda Azure AD Connect geçici parolaları Azure AD ile eşitleme desteklemez. Bir parola geçici olarak değerlendirilir, **sonraki oturumda parola Değiştir** seçeneği, şirket içi Active Directory kullanıcı ayarlanır. Şu hatayı döndürdü:

![Geçici parolayı verilmez](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-to-synchronize-password-arent-available"></a>Son eşitleme girişimi sonuçlarını kullanılamaz
Varsayılan olarak, Azure AD Connect yedi gün için parola eşitleme denemelerinin sonuçlarını depolar. Seçili Active Directory nesne için kullanılabilen hiç sonuç yoksa şu uyarıyı döndürdü:

![Tanılama çıktıları tek nesnesi - parola eşitleme geçmişi yok](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Hiçbir parolalar eşitlenir: Tanılama cmdlet'ini kullanarak sorun giderme
Kullanabileceğiniz `Invoke-ADSyncDiagnostics` hiçbir parolalar neden eşitlenir bulmak için cmdlet.

> [!NOTE]
> `Invoke-ADSyncDiagnostics` Cmdlet'ini veya üstünü yalnızca Azure AD Connect sürüm 1.1.524.0 için kullanılabilir.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet'ini çalıştırın
Burada hiçbir parolalar eşitlenir sorunları gidermek için:

1. Azure AD Connect sunucunuzla üzerinde yeni bir Windows PowerShell oturumu açın **yönetici olarak çalıştır** seçeneği.

2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.

3. `Import-Module ADSyncDiagnostics` öğesini çalıştırın.

4. `Invoke-ADSyncDiagnostics -PasswordSync` öğesini çalıştırın.



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Bir nesne değil parolaları eşitleme: Tanılama cmdlet'ini kullanarak sorun giderme
Kullanabileceğiniz `Invoke-ADSyncDiagnostics` neden bir nesne parolaları eşitlemiyor belirlemek için cmdlet.

> [!NOTE]
> `Invoke-ADSyncDiagnostics` Cmdlet'ini veya üstünü yalnızca Azure AD Connect sürüm 1.1.524.0 için kullanılabilir.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet'ini çalıştırın
Bir kullanıcı için hiçbir parolalar burada eşitlenir sorunları gidermek için:

1. Azure AD Connect sunucunuzla üzerinde yeni bir Windows PowerShell oturumu açın **yönetici olarak çalıştır** seçeneği.

2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.

3. `Import-Module ADSyncDiagnostics` öğesini çalıştırın.

4. Aşağıdaki cmdlet'i çalıştırın:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   Örneğin:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```



## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a>Hiçbir parolalar eşitlenir: sorun giderme adımları el ile
Hiçbir parolalar neden eşitlenir belirlemek için aşağıdaki adımları izleyin:

1. Connect sunucu, [hazırlama modu](active-directory-aadconnectsync-operations.md#staging-mode)? Bir sunucu hazırlama modunda parolaları eşitlemez.

2. Komut dosyasını Çalıştır [parola eşitleme ayarlarının durumunu Al](#get-the-status-of-password-sync-settings) bölümü. Parola Eşitleme yapılandırmasına genel bakış sağlar.  

    ![Parola Eşitleme ayarlarını PowerShell komut dosyası çıktısı](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. Azure AD'de özelliği etkin değilse veya eşitleme kanal durumu etkin değilse, Connect Yükleme Sihirbazı'nı çalıştırın. Seçin **eşitleme seçeneklerini özelleştirme**ve Parola Eşitleme'seçimini kaldırın. Bu değişiklik özelliğini geçici olarak devre dışı bırakır. Ardından Sihirbazı yeniden çalıştırın ve parola eşitleme yeniden etkinleştirin. Yeniden yapılandırma doğru olduğunu doğrulamak için komut dosyasını çalıştırın.

4. Hatalar için olay günlüğüne bakın. Bir sorunu gösterir aşağıdaki olayları arayın:
    * Kaynak: "Dizin eşitleme" kimliği: 0, 611, 652, bu olayları görürseniz 655 bağlantı sorun oluştu. Olay günlüğü iletisi bir sorun olduğu orman ile ilgili bilgi içerir. Daha fazla bilgi için bkz: [bağlantı sorunu](#connectivity problem).

5. Sinyal alınmadı bakın veya başka bir şey çalışılan gerekiyorsa, çalıştırmak [tüm parolaların tam eşitlemesi](#trigger-a-full-sync-of-all-passwords). Komut dosyası yalnızca bir kez çalıştırın.

6. Bkz: [parolaları eşitleme değil bir nesne sorun giderme](#one-object-is-not-synchronizing-passwords) bölümü.

### <a name="connectivity-problems"></a>Bağlantı sorunları

Azure AD ile bağlantı var mı?

Hesap ve parola karma değerleri tüm etki alanlarında okumak için gerekli izinlere sahip mi? Hızlı ayarları kullanarak bağlan yüklediyseniz izinleri zaten doğru olması gerekir. 

Özel yükleme kullandıysanız, aşağıdakileri yaparak izinlerini el ile ayarlayın:
    
1. Active Directory Bağlayıcısı tarafından kullanılan hesabın bulmak için başlangıç **Eşitleme Hizmeti Yöneticisi'ni**. 
 
2. Git **Bağlayıcılar**ve sorun gidermeye şirket içi Active Directory ormanı için arama yapın. 
 
3. Bağlayıcıyı seçin ve ardından **özellikleri**. 
 
4. Git **Active Directory ormanına Bağlan**.  
    
    ![Active Directory Bağlayıcısı tarafından kullanılan hesap](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    Kullanıcı adı ve etki alanı hesabının bulunduğu unutmayın.
    
5. Başlat **Active Directory Kullanıcıları ve Bilgisayarları**ve ardından daha önce bulunan hesabının, ormandaki tüm etki alanları kökündeki izleyin izinlere sahip olduğunu doğrulayın:
    * Dizin Değişikliklerini Çoğaltma
    * Tüm çoğaltma dizini değiştirir

6. Etki alanı denetleyicileri, Azure AD Connect tarafından erişilebilir? Connect sunucusu tüm etki alanı denetleyicilerine bağlanamıyorsanız, yapılandırma **yalnızca tercih edilen etki alanı denetleyicisini kullanmasını**.  
    
    ![Active Directory Bağlayıcısı tarafından kullanılan etki alanı denetleyicisi](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. Geri dönerek **Eşitleme Hizmeti Yöneticisi'ni** ve **yapılandırma dizini bölümü**. 
 
8. Etki alanınızda seçin **dizin bölümlerini Seç**seçin **yalnızca tercih edilen etki alanı denetleyicilerinin kullandığı** onay kutusunu işaretleyin ve ardından **yapılandırma**. 

9. Listede, bağlantı için parola eşitlemesi kullanması gereken etki alanı denetleyicileri girin. Aynı liste içeri ve dışarı aktarma de için kullanılır. Tüm etki alanları için bu adımları uygulayın.

10. Komut dosyası hiçbir sinyal olduğunu gösteriyorsa, komut dosyasını Çalıştır [tüm parolaların tam eşitlemesi](#trigger-a-full-sync-of-all-passwords).

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a>Bir nesne değil parolaları eşitleme: sorun giderme adımları el ile
Bir nesnenin durumunu gözden geçirerek kolayca parola eşitleme sorunlarını giderebilirsiniz.

1. İçinde **Active Directory Kullanıcıları ve Bilgisayarları**aramak için kullanıcı ve ardından doğrulayın **kullanıcı bir sonraki oturumda parola değiştirmeli** onay kutusu işaretli.  

    ![Active Directory üretken parolaları](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    Onay kutusu seçiliyse, kullanıcı oturum açma ve parola değiştirme isteyin. Geçici parolalar Azure AD ile eşitlenmemiş.

2. Eşitleme altyapısı kullanıcı parolası Active Directory'de doğru görünüyorsa, izleyin. Azure AD ile şirket içi Active Directory'den kullanıcı izleyerek, nesne üzerinde açıklayıcı bir hata olup olmadığını görebilirsiniz.

    a. Başlat [Eşitleme Hizmeti Yöneticisi'ni](active-directory-aadconnectsync-service-manager-ui.md).

    b. Tıklatın **Bağlayıcılar**.

    c. Seçin **Active Directory Bağlayıcısı** kullanıcının bulunduğu.

    d. Seçin **bağlayıcı alanı arama**.

    e. İçinde **kapsam** kutusunda **DN ya da bağlantı**ve sorun gidermeye kullanıcı tam etki alanı adı girin.

    ![Bağlayıcı alanı DN'si kullanıcı ara](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    f. Aradığınız ve ardından kullanıcı bulun **özellikleri** tüm öznitelikleri görmek için. Kullanıcı Arama sonucunda değilse doğrulayın, [filtreleme kuralları](active-directory-aadconnectsync-configure-filtering.md) ve, çalıştırdığınızdan emin olun [Uygula ve değişiklikleri doğrulamak](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) kullanıcı Bağlan görünmesi için.

    g. Geçen hafta nesne parola eşitleme ayrıntılarını görmek için tıklatın **günlük**.  

    ![Nesne günlük ayrıntıları](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    Nesne günlük boşsa, Azure AD Connect parola karması Active Directory'den okuyamıyor olmuştur. Sorunlarını giderme devam [bağlantı hataları](#connectivity-errors). Başka bir değerden görürseniz **başarı**, tabloya başvuruda [Parola Eşitleme günlüğü](#password-sync-log).

    h. Seçin **lineage** sekmesinde ve bu en az bir eşitleme kuralı emin olun **PasswordSync** sütunu **doğru**. Varsayılan yapılandırmada, eşitleme kuralının adıdır **içinde AD'den - kullanıcı AccountEnabled**.  

    ![Bir kullanıcı hakkındaki bilgileri çizgileri](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    ı. Tıklatın **meta veri deposu nesne özellikleri** kullanıcı özniteliklerinin bir listesini görüntülemek için.  

    ![Meta veri bilgileri](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    Olduğunu doğrulayın hiçbir **cloudFiltered** özniteliği mevcut. Etki alanı öznitelikler (domainFQDN ve domainNetBios) beklenen değerler olduğundan emin olun.

    j. Tıklatın **Bağlayıcılar** sekmesi. Şirket içi Active Directory ve Azure AD Bağlayıcılarına gördüğünüzden emin olun.

    ![Meta veri bilgileri](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    k. Azure AD'ı temsil tıklatın satırı seçin **özellikleri**ve ardından **çizgileri** sekmesi. Bağlayıcı alanı nesne bir giden kuralı olmalıdır **PasswordSync** sütun kümesine **doğru**. Varsayılan yapılandırmada, eşitleme kuralının adıdır **Out AAD'ye - kullanıcı katılma**.  

    ![Bağlayıcı alanı nesne özellikleri iletişim kutusu](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Parola Eşitleme günlüğü
Durum sütununda aşağıdaki değerlere sahip olabilir:

| Durum | Açıklama |
| --- | --- |
| Başarılı |Parola başarıyla eşitlendi. |
| FilteredByTarget |Parola ayarlanmış **kullanıcı bir sonraki oturumda parola değiştirmeli**. Parola eşitlenmemiş. |
| NoTargetConnection |Nesne yok meta veri veya Azure AD Bağlayıcısı alanı. |
| SourceConnectorNotPresent |Nesnesi şirket içi Active Directory Bağlayıcısı alanında bulunamadı. |
| TargetNotExportedToDirectory |Azure AD bağlayıcı alanı nesne henüz verilemez. |
| MigratedCheckDetailsForMoreInfo |Günlük girişi 1.0.9125.0 Yapı önce oluşturuldu ve eski durumuna gösterilir. |
| Hata |Hizmet bilinmeyen bir hata döndürdü. |
| Bilinmeyen |Parola karmaları toplu işlemeye çalışırken bir hata oluştu.  |
| MissingAttribute |Azure AD etki alanı Hizmetleri tarafından gereken belirli öznitelikleri (örneğin, Kerberos karması) kullanılabilir değil. |
| RetryRequestedByTarget |Azure AD etki alanı Hizmetleri tarafından gereken belirli öznitelikleri (örneğin, Kerberos karması) daha önce kullanılamıyordu. Kullanıcının parola karmasını eşitlemek için bir girişimde. |

## <a name="scripts-to-help-troubleshooting"></a>Sorun giderme Yardımı için komut dosyaları

### <a name="get-the-status-of-password-sync-settings"></a>Parola Eşitleme ayarlarının durumunu Al
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Tüm parolaların tam eşitlemesi
> [!NOTE]
> Bu komut dosyası yalnızca bir kez çalıştırın. Birden çok kez çalıştırmak gerekirse, başka bir sorundur. Sorunu gidermek için Microsoft Destek'e başvurun.

Aşağıdaki komut dosyası kullanarak bir tam eşitleme tüm parolaların tetikleyebilirsiniz:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Sonraki adımlar
* [Parola Eşitleme ile Azure AD Connect eşitleme uygulama](active-directory-aadconnectsync-implement-password-synchronization.md)
* [Azure AD Connect eşitleme: eşitleme seçeneklerini özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
