---
title: Azure AD Connect eşitlemesi ile parola karması eşitleme sorunlarını giderme | Microsoft Docs
description: Bu makalede, parola karması eşitleme sorunlarını giderme hakkında bilgi sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6feed11fcfc597658f3ec148b5dd18bb7e3f8f83
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58793331"
---
# <a name="troubleshoot-password-hash-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitlemesi ile parola karması eşitleme sorunlarını giderme

Bu konu, parola karması eşitleme ile ilgili sorunları gidermek adımlar sağlar. Parolaları eşitlemiyor beklendiği gibi kullanıcıların bir alt kümesine veya tüm kullanıcılar için olabilir.

İçin Azure Active Directory (Azure AD) Connect dağıtım 1.1.614.0 sürümüyle ya da sonra parola karması eşitleme sorunlarını gidermek için Sihirbazı'nda sorun giderme görevini kullanın:

* Burada hiçbir parola eşitlenmedi bir sorun varsa, başvurmak [hiçbir parola eşitlenmedi: sorun giderme görevini kullanarak sorun giderme](#no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task) bölümü.

* Ayrı ayrı nesneleri ile ilgili bir sorun varsa, başvurmak [bir nesne parolaları eşitlemiyor: sorun giderme görevini kullanarak sorun giderme](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task) bölümü.

Sürüm 1.1.524.0 ile dağıtmak için ya da daha sonra parola karması eşitleme sorunlarını gidermek için kullanabileceğiniz bir tanı cmdlet'i vardır:

* Burada hiçbir parola eşitlenmedi bir sorun varsa, başvurmak [hiçbir parola eşitlenmedi: sorun giderme tanılama cmdlet'ini kullanarak](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) bölümü.

* Ayrı ayrı nesneleri ile ilgili bir sorun varsa, başvurmak [bir nesne parolaları eşitlemiyor: sorun giderme tanılama cmdlet'ini kullanarak](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) bölümü.

Azure AD Connect dağıtımının daha eski sürümleri için:

* Burada hiçbir parola eşitlenmedi bir sorun varsa, başvurmak [hiçbir parola eşitlenmedi: sorun giderme adımları el ile](#no-passwords-are-synchronized-manual-troubleshooting-steps) bölümü.

* Ayrı ayrı nesneleri ile ilgili bir sorun varsa, başvurmak [bir nesne parolaları eşitlemiyor: sorun giderme adımları el ile](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) bölümü.



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task"></a>Hiçbir parola eşitlenmedi: sorun giderme görevini kullanarak sorun giderme

Hiçbir parola eşitlenmedi neden kullanıma anlamak için sorun giderme görevini kullanabilirsiniz.

> [!NOTE]
> Sorun giderme görevini yalnızca Azure AD Connect sürümünüzü 1.1.614.0 için veya sonraki kullanılabilir.

### <a name="run-the-troubleshooting-task"></a>Sorun giderme görevini Çalıştır

Burada hiçbir parola eşitlenmedi sorunlarını gidermek için:

1. Azure AD Connect sunucunuzda ile yeni bir Windows PowerShell oturumu açın **yönetici olarak çalıştır** seçeneği.

2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.

3. Azure AD Connect Sihirbazı'nı başlatın.

4. Gidin **ek görevler** sayfasında **sorun giderme**, tıklatıp **sonraki**.

5. Sorun giderme sayfasında tıklayın **başlatma** PowerShell'de sorun giderme menü başlatmak için.

6. Ana menüde **parola karması eşitleme sorunlarını giderme**.

7. Alt menüde **parola karması eşitleme hiç çalışmıyor**.

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme görevini sonuçlarını anlama

Sorun giderme görevini aşağıdaki denetimleri gerçekleştirir:

* Azure AD kiracınız için parola karması eşitleme özelliği etkin olduğunu doğrular.

* Azure AD Connect sunucusu hazırlama modunda olmadığını doğrular.

* (Mevcut bir Active Directory ormanına karşılık gelir) her mevcut şirket içi Active Directory Bağlayıcısı için:

   * Parola Karması eşitleme özelliği etkin olduğunu doğrular.
   
   * Aramalar için parola eşitleme sinyal olayları Windows uygulama olay günlüklerinde karma.

   * Her Active Directory etki alanı için şirket içi Active Directory bağlayıcısını altında:

      * Etki alanı Azure AD Connect sunucusundan erişilebilir olduğunu doğrular.

      * Şirket içi Active Directory Bağlayıcısı tarafından kullanılan Active Directory etki alanı Hizmetleri (AD DS) hesaplarına doğru adı, parola ve parola karması eşitleme için gerekli izinlere sahip olduğunu doğrular.

Aşağıdaki diyagram, bir tek etki alanı, şirket içi Active Directory topolojisi için cmdlet'inin sonuçlarını gösterir:

![Parola Karması eşitleme için tanılama çıkışı](./media/tshoot-connect-password-hash-synchronization/phsglobalgeneral.png)

Bu bölümün geri kalanında görev ve ilgili sorunları tarafından döndürülen belirli sonuçları açıklar.

#### <a name="password-hash-synchronization-feature-isnt-enabled"></a>Parola Karması eşitleme özelliği etkin değil

Azure AD Connect Sihirbazı'nı kullanarak parola karması eşitleme etkinleştirmediyseniz, aşağıdaki hata döndürülür:

![Parola Karması eşitleme etkin değil](./media/tshoot-connect-password-hash-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a>Azure AD Connect sunucusu hazırlama modunda olan

Azure AD Connect sunucusu hazırlama modundayken, parola karması eşitlemeyi geçici olarak devre dışı bırakıldı ve şu hatayı döndürdü:

![Azure AD Connect sunucusu hazırlama modunda olan](./media/tshoot-connect-password-hash-synchronization/phsglobalstaging.png)

#### <a name="no-password-hash-synchronization-heartbeat-events"></a>Parola Karması eşitleme sinyal olayları

Kendi parola karması eşitleme kanalının her şirket içi Active Directory bağlayıcısını sahiptir. Eşitlenecek parola değişiklikleri yok ve parola karması eşitleme kanalının kurulur, bir sinyal olay (EventID 654) her 30 dakikada altındaki Windows uygulama olay günlüğü oluşturulur. Her şirket içi Active Directory Bağlayıcısı için cmdlet karşılık gelen sinyal olayları için son üç saat içinde arar. Sinyal olay bulunursa, şu hata döndürülür:

![Olay hiç parola karması eşitleme Kalp beat](./media/tshoot-connect-password-hash-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a>AD DS hesap doğru izinlere sahip değil

Parola karmalarını eşitleyecek şekilde şirket içi Active Directory Bağlayıcısı tarafından kullanılan AD DS hesap uygun izinlere sahip değilse, aşağıdaki hata döndürülür:

![Yanlış kimlik bilgileri](./media/tshoot-connect-password-hash-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a>Kullanıcı adı veya parola yanlış AD DS hesabı

Parola karmalarını eşitleyecek şekilde şirket içi Active Directory Bağlayıcısı tarafından kullanılan AD DS hesabı, bir hatalı kullanıcı adı veya parola sahipse, aşağıdaki hata döndürülür:

![Yanlış kimlik bilgileri](./media/tshoot-connect-password-hash-synchronization/phsglobalaccountincorrectcredential.png)



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task"></a>Bir nesne parolaları eşitlemiyor: sorun giderme görevini kullanarak sorun giderme

Neden bir nesne parolaları eşitlemiyor belirlemek için sorun giderme görevini kullanabilirsiniz.

> [!NOTE]
> Sorun giderme görevini yalnızca Azure AD Connect sürümünüzü 1.1.614.0 için veya sonraki kullanılabilir.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet'ini çalıştırın

Belirli bir kullanıcı nesnesi için sorunları gidermek için:

1. Azure AD Connect sunucunuzda ile yeni bir Windows PowerShell oturumu açın **yönetici olarak çalıştır** seçeneği.

2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.

3. Azure AD Connect Sihirbazı'nı başlatın.

4. Gidin **ek görevler** sayfasında **sorun giderme**, tıklatıp **sonraki**.

5. Sorun giderme sayfasında tıklayın **başlatma** PowerShell'de sorun giderme menü başlatmak için.

6. Ana menüde **parola karması eşitleme sorunlarını giderme**.

7. Alt menüde **parola için belirli bir kullanıcı hesabı eşitlenmez**.

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme görevini sonuçlarını anlama

Sorun giderme görevini aşağıdaki denetimleri gerçekleştirir:

* Active Directory Bağlayıcısı alanına, meta veri deposu ve Azure Active Directory nesne durumunu inceler AD bağlayıcı alanında.

* Etkin ve Active Directory nesneye uygulanan parola karması eşitleme olan eşitleme kuralları olduğunu doğrular.

* Almak ve son nesne için parola eşitleme girişimi sonuçları görüntülemek çalışır.

Aşağıdaki diyagram, tek bir nesne için parola karması eşitleme sorunlarını giderirken cmdlet'inin sonuçlarını gösterir:

![Parola Karması eşitleme - tek bir nesne için tanılama çıkışı](./media/tshoot-connect-password-hash-synchronization/phssingleobjectgeneral.png)

Bu bölümün geri kalanında ilgili sorunları ve cmdlet'i tarafından döndürülen belirli sonuçları açıklar.

#### <a name="the-active-directory-object-isnt-exported-to-azure-ad"></a>Active Directory nesnesi Azure AD'ye aktarılan değil

Azure AD kiracısında karşılık gelen hiçbir nesne olduğundan bu şirket içi Active Directory hesabı için parola karması eşitlemeyi başarısız olur. Şu hatayı döndürdü:

![Azure AD nesnesi eksik](./media/tshoot-connect-password-hash-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a>Kullanıcının bir geçici parola

Şu anda Azure AD Connect geçici parolaları Azure AD ile eşitleme desteklemez. Bir parola geçici olarak kabul edilir, **sonraki oturum açışında parolasını değiştirme** seçeneği, şirket içi Active Directory kullanıcı ayarlanır. Şu hatayı döndürdü:

![Geçici parola verilmez](./media/tshoot-connect-password-hash-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-to-synchronize-password-arent-available"></a>Parola Eşitleme için son girişimi sonuçları kullanılabilir değil

Varsayılan olarak, Azure AD Connect parola karması eşitleme denemesi yedi gün boyunca sonuçlarını depolar. Aşağıdaki uyarı seçili bir Active Directory nesne için kullanılabilir sonuç varsa, döndürülür:

![Tek nesne - hiçbir parola eşitleme geçmişi için tanılama çıkışı](./media/tshoot-connect-password-hash-synchronization/phssingleobjectnohistory.png)



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Hiçbir parola eşitlenmedi: Tanılama cmdlet'ini kullanarak sorun giderme

Kullanabileceğiniz `Invoke-ADSyncDiagnostics` cmdlet'ini hiçbir parola eşitlenmedi neden kullanıma şekil.

> [!NOTE]
> `Invoke-ADSyncDiagnostics` Cmdlet'i, yalnızca Azure AD Connect sürüm 1.1.524.0 kullanılabilir veya üzeri.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet'ini çalıştırın

Burada hiçbir parola eşitlenmedi sorunlarını gidermek için:

1. Azure AD Connect sunucunuzda ile yeni bir Windows PowerShell oturumu açın **yönetici olarak çalıştır** seçeneği.

2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.

3. `Import-Module ADSyncDiagnostics` öğesini çalıştırın.

4. `Invoke-ADSyncDiagnostics -PasswordSync` öğesini çalıştırın.



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Bir nesne parolaları eşitlemiyor: Tanılama cmdlet'ini kullanarak sorun giderme

Kullanabileceğiniz `Invoke-ADSyncDiagnostics` cmdlet'ini neden bir nesne parolaları eşitlemiyor belirleyin.

> [!NOTE]
> `Invoke-ADSyncDiagnostics` Cmdlet'i, yalnızca Azure AD Connect sürüm 1.1.524.0 kullanılabilir veya üzeri.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet'ini çalıştırın

Burada, bir kullanıcı için hiçbir parola eşitlenmedi sorunlarını gidermek için:

1. Azure AD Connect sunucunuzda ile yeni bir Windows PowerShell oturumu açın **yönetici olarak çalıştır** seçeneği.

2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.

3. `Import-Module ADSyncDiagnostics` öğesini çalıştırın.

4. Aşağıdaki cmdlet'i çalıştırın:

   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```

   Örneğin:

   ```powershell
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```



## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a>Hiçbir parola eşitlenmedi: el ile sorun giderme adımları

Neden hiçbir parola eşitlenmedi belirlemek için aşağıdaki adımları izleyin:

1. Connect sunucu [hazırlama modunda](how-to-connect-sync-staging-server.md)? Bir sunucu hazırlama modunda parolaları eşitlemez.

2. Betiği Çalıştır [parola eşitleme ayarlarının durumunu alma](#get-the-status-of-password-sync-settings) bölümü. Parola Eşitleme yapılandırmasına genel bakış sağlar.  

    ![PowerShell komut dosyası çıktısını parola eşitleme ayarları](./media/tshoot-connect-password-hash-synchronization/psverifyconfig.png)  

3. Azure AD'de özelliği etkin değilse veya eşitleme kanal durumu etkin değilse, Connect Yükleme Sihirbazı'nı çalıştırın. Seçin **eşitleme seçeneklerini özelleştirme**, parola eşitleme seçimini temizleyin. Bu değişiklik özelliğini geçici olarak devre dışı bırakır. Ardından Sihirbazı yeniden çalıştırın ve parola eşitleme yeniden etkinleştirin. Yeniden yapılandırma doğru olduğunu doğrulamak için betiği çalıştırın.

4. Hataları için olay günlüğüne bakın. Bir sorunu gösterir aşağıdaki olaylar arayın:
    * Kaynak: "Dizin eşitleme" kimliği: 0, 611, 652, bu olaylar görüyorsanız 655 bağlantı sorunu vardır. Olay günlüğü iletisi bir sorun olduğu orman ile ilgili bilgi içerir. Daha fazla bilgi için [bağlantısı sorunu](#connectivity problem).

5. Hiçbir sinyal görürseniz veya başka bir şey yaradıysa [tam eşitlemesi tüm parolaların](#trigger-a-full-sync-of-all-passwords). Betik yalnızca bir kez çalıştırın.

6. Parolalar bölümüne eşitlenmeyen sorun giderme bir nesne bakın.

### <a name="connectivity-problems"></a>Bağlantı sorunları

Azure AD ile bağlantısı var mı?

Tüm etki alanlarında parola karmalarının okumak için gerekli izinlere sahip hesabın? Hızlı ayarları kullanarak bağlan yüklü değilse, izinler zaten doğru olması gerekir. 

Özel yükleme kullandıysanız, aşağıdakileri yaparak izinleri elle ayarlayın:
    
1. Active Directory Bağlayıcısı tarafından kullanılan hesabın öğrenmek için başlangıç **Eşitleme Hizmeti Yöneticisi**. 
 
2. Git **Bağlayıcılar**ve sonra şirket içi Active Directory ormanı sorun gidermek için arama yapın. 
 
3. Bağlayıcıyı seçin ve ardından **özellikleri**. 
 
4. Git **Active Directory ormanına Bağlan**.  
    
    ![Active Directory Bağlayıcısı tarafından kullanılan hesap](./media/tshoot-connect-password-hash-synchronization/connectoraccount.png)  
    Kullanıcı adı ve etki alanı hesabının bulunduğu unutmayın.
    
5. Başlangıç **Active Directory Kullanıcıları ve Bilgisayarları**ve ardından daha önce bulduğunuz hesabı ormanınızdaki tüm etki alanı kökünde aşağıdaki izinlere sahip olduğunu doğrulayın:
    * Dizin Değişikliklerini Çoğaltma
    * Çoğaltma Directory yapılan tüm değişiklikler

6. Etki alanı denetleyicileri, Azure AD Connect tarafından erişilebilir? Connect sunucusu, tüm etki alanı denetleyicilerine bağlanamıyorsanız, yapılandırma **yalnızca tercih edilen etki alanı denetleyicisini kullan**.  
    
    ![Active Directory Bağlayıcısı tarafından kullanılan etki alanı denetleyicisi](./media/tshoot-connect-password-hash-synchronization/preferreddc.png)  
    
7. Geri Git **Eşitleme Hizmeti Yöneticisi** ve **yapılandırma dizini bölümü**. 
 
8. Etki alanınızı seçin **dizin bölümlerini Seç**seçin **yalnızca tercih edilen etki alanı denetleyicileri kullanmak** onay kutusunu işaretleyin ve ardından **yapılandırma**. 

9. Parola Eşitleme için Connect kullanması gereken etki alanları listesinde girin. Aynı liste, içeri ve dışarı aktarma da için kullanılır. Tüm etki alanları için bu adımları uygulayın.

10. Betik, hiçbir sinyal olduğunu gösteriyorsa, komut dosyasını çalıştırmak [tam eşitlemesi tüm parolaların](#trigger-a-full-sync-of-all-passwords).

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a>Bir nesne parolaları eşitlemiyor: el ile sorun giderme adımları

Ayrıca, bir nesnenin durumlarını gözden geçirerek kolayca parola karması eşitleme sorunlarını giderebilirsiniz.

1. İçinde **Active Directory Kullanıcıları ve Bilgisayarları**, kullanıcı için arama yapın ve çalıştığını doğrulayın **kullanıcının sonraki oturum açışında parolasını değiştirmesi** onay kutusu işaretli değilse.  

    ![Active Directory üretken parolaları](./media/tshoot-connect-password-hash-synchronization/adprodpassword.png)  

    Onay kutusu seçili değilse, kullanıcının oturum açmasını ve parolayı değiştirmek isteyin. Geçici parolaları Azure AD'ye eşitlenmez.

2. Parolayı Active Directory'de doğru görünüyorsa, eşitleme altyapısı kullanıcı izleyin. Kullanıcının şirket içi Active Directory'den Azure AD'ye takip ederek, nesne üzerinde bir açıklayıcı hata olup olmadığını görebilirsiniz.

    a. Başlangıç [Eşitleme Hizmeti Yöneticisi](how-to-connect-sync-service-manager-ui.md).

    b. Tıklayın **Bağlayıcılar**.

    c. Seçin **Active Directory Bağlayıcısı** kullanıcının bulunduğu.

    d. Seçin **bağlayıcı alanı arama**.

    e. İçinde **kapsam** kutusunda **DN'si ya da bağlantı**ve ardından sorun gidermeye kullanıcı tam etki alanı adı girin.

    ![Kullanıcı bağlayıcı alanında DN ile arayın](./media/tshoot-connect-password-hash-synchronization/searchcs.png)  

    f. Aradığınız ve ardından kullanıcıyı bulmaya **özellikleri** tüm öznitelikleri görmek için. Kullanıcı Arama sonucunda değilse, doğrulama, [filtreleme kuralları](how-to-connect-sync-configure-filtering.md) ve siz çalıştırdığınızdan emin olun [Uygula ve değişiklikleri doğrulayın](how-to-connect-sync-configure-filtering.md#apply-and-verify-changes) kullanıcının Bağlan görünür.

    g. Geçen hafta için nesne parola eşitleme ayrıntılarını görmek için tıklayın **günlük**.  

    ![Nesne günlük ayrıntıları](./media/tshoot-connect-password-hash-synchronization/csobjectlog.png)  

    Azure AD Connect, nesne günlük boşsa, Active Directory'den parola karması okunamıyor olmuştur. Sorun gidermeyi bağlantı hataları ile devam edin. Daha başka bir değer görürseniz **başarı**, tabloya başvuran [parola eşitleme günlüğünü](#password-sync-log).

    h. Seçin **kökenini** sekmesini tıklatıp, en az bir eşitleme kuralı olduğundan emin olun **PasswordSync** sütun **True**. Varsayılan yapılandırmasında eşitleme kuralının adıdır. **içinde ad - kullanıcı AccountEnabled**.  

    ![Yakınlaştırabilir bir kullanıcı hakkında](./media/tshoot-connect-password-hash-synchronization/cspasswordsync.png)  

    i. Tıklayın **meta veri deposu nesne özellikleri** kullanıcı özniteliklerinin bir listesini görüntüleyin.  

    ![Meta veri deposu bilgileri](./media/tshoot-connect-password-hash-synchronization/mvpasswordsync.png)  

    Olduğunu doğrulayın hiçbir **cloudFiltered** özniteliği. Etki alanı öznitelikleri (domainFQDN ve domainNetBios) beklenen değerleri sahip olduğunuzdan emin olun.

    j. Tıklayın **Bağlayıcılar** sekmesi. Hem şirket içi Active Directory hem de Azure AD bağlayıcıları gördüğünüzden emin olun.

    ![Meta veri deposu bilgileri](./media/tshoot-connect-password-hash-synchronization/mvconnectors.png)  

    k. Azure AD temsil tıklayın satırı seçin **özellikleri**ve ardından **kökenini** sekmesi. Bağlayıcı alanı nesnesi bir giden kuralı olmalıdır **PasswordSync** sütun kümesine **True**. Varsayılan yapılandırmasında eşitleme kuralının adıdır. **Out AAD için - kullanıcı katılın**.  

    ![Bağlayıcı alanı nesne özellikleri iletişim kutusu](./media/tshoot-connect-password-hash-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Parola Eşitleme günlüğü

Durum sütununda, aşağıdaki değerlere sahip olabilir:

| Durum | Açıklama |
| --- | --- |
| Başarılı |Parola başarıyla eşitlendi. |
| FilteredByTarget |Parola ayarlanmış **kullanıcının sonraki oturum açışında parolasını değiştirmesi**. Parola eşitlenmedi. |
| NoTargetConnection |Meta veri deposu veya Azure AD bağlayıcı alanı nesne. |
| SourceConnectorNotPresent |Nesnesi şirket içi Active Directory Bağlayıcı alanında bulunamadı. |
| TargetNotExportedToDirectory |Azure AD bağlayıcı alanında nesne henüz verilemez. |
| MigratedCheckDetailsForMoreInfo |Günlük girişi 1.0.9125.0 derlemeden önce oluşturuldu ve eski durumuna gösterilir. |
| Hata |Hizmet bilinmeyen bir hata döndürdü. |
| Bilinmeyen |Parola karmalarının bir toplu iş işlenirken bir hata oluştu.  |
| MissingAttribute |Azure AD Domain Services için gereken belirli öznitelikleri (örneğin, Kerberos karması) kullanılabilir değil. |
| RetryRequestedByTarget |Azure AD Domain Services için gereken belirli öznitelikleri (örneğin, Kerberos karması) daha önce kullanılabilir değildi. Kullanıcının parola karmasını yeniden eşitlemek için bir deneme yapılır. |

## <a name="scripts-to-help-troubleshooting"></a>Sorun giderme Yardımı için komut dosyaları

### <a name="get-the-status-of-password-sync-settings"></a>Parola Eşitleme ayarlarının durumunu Al

```powershell
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

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Tüm parolalar tam eşitlemesi

> [!NOTE]
> Bu betik yalnızca bir kez çalıştırın. Birden çok kez çalışmasına ihtiyacınız varsa başka bir sorundur. Sorunu gidermek için Microsoft desteğine başvurun.

Aşağıdaki betiği kullanarak tam bir eşitleme tüm parolaların tetikleyebilirsiniz:

```powershell
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

* [Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama](how-to-connect-password-hash-synchronization.md)
* [Azure AD Connect eşitleme: Eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)