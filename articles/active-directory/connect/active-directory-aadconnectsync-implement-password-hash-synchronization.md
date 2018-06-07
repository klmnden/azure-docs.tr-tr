---
title: Parola karma eşitlemesi ile Azure AD Connect eşitleme uygulama | Microsoft Docs
description: Parola karma eşitlemesini nasıl çalıştığını ve nasıl ayarlandığı hakkında bilgi sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: abe439cc91a003137c116f57c0cc8bbb61430114
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34593461"
---
# <a name="implement-password-hash-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitlemesi ile parola karma eşitlemesi uygulama
Bu makale, şirket içi Active Directory örneğinden bir bulut tabanlı Azure Active Directory (Azure AD) örneği, kullanıcı parolalarını eşitlemek için gereken bilgileri sağlar.

## <a name="what-is-password-hash-synchronization"></a>Parola karma eşitlemesi nedir
Olasılık unutulmuş parola nedeniyle işinizi alma engellenen unutmayın için ihtiyacınız olan farklı parola sayısını ilişkilidir. Unutmayın, yüksek olasılık bir unuttunuz yapmanız parolaların. Sorular ve parola sıfırlama ve diğer parola ile ilgili sorunlar hakkında çağrıları çoğu Yardım Masası kaynakları talep.

Parola karma eşitlemesini, bulut tabanlı bir Azure için şirket içi Active Directory örneğinden kullanıcı parolalarını eşitlemek için kullanılan bir özelliğidir AD örneği.
Office 365, Microsoft Intune, CRM Online ve Azure Active Directory etki alanı Hizmetleri (Azure AD DS) gibi Azure AD hizmetlerinde oturum açmak için bu özelliği kullanın. Hizmete şirket içi Active Directory örneğinizle oturum açmak için kullandığınız aynı parola kullanarak oturum açın.

![Azure AD Connect nedir?](./media/active-directory-aadconnectsync-implement-password-hash-synchronization/arch1.png)

Parola sayısını azaltarak yalnızca birine korumak için kullanıcılarınızın gerekir. Parola karma eşitlemesi size yardımcı olur:

* Kullanıcılarınızın verimliliğini artırır.
* Yardım Masası maliyetlerini azaltır.  

Ayrıca, kullanmaya karar verirseniz [Active Directory Federasyon Hizmetleri (AD FS) ile Federasyon](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), AD FS altyapınızı başarısız olursa, isteğe bağlı olarak parola karması eşitlemesi yedek olarak ayarlayabilirsiniz.

Parola karma eşitlemesini, Azure AD Connect eşitleme tarafından uygulanan dizin eşitleme özelliğini uzantısıdır. Parola karma eşitlemesi ortamınızda kullanmak için aktarmanız gerekir:

* Azure AD Connect'i yükleme.  
* Şirket içi Active Directory örneğinizi ve Azure Active Directory örneğinizi arasında dizin eşitlemesi yapılandırın.
* Parola karma eşitlemesini etkinleştirin.

Daha ayrıntılı bilgi için bkz. [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).

> [!NOTE]
> Azure Active Directory etki alanı FIPS ve parola karma eşitlemesi için yapılandırılmış hizmetleri hakkında daha fazla ayrıntı için bu makalenin sonraki bölümlerinde "parola karma eşitlemesi ve FIPS" konusuna bakın.
>
>

## <a name="how-password-hash-synchronization-works"></a>Parola karma eşitleme nasıl çalışır
Active Directory etki alanı hizmeti parolaları gerçek kullanıcı parolası bir karma değer gösterimini biçiminde depolar. Bir karma değer tek yönlü matematiksel işlevi sonucudur ( *karma algoritması*). Bir parola düz metin sürümü için tek yönlü işlevin sonucu dönmek için bir yöntem yoktur. Şirket içi ağınızda oturum açmak için parola karması kullanamazsınız.

Parolanızı eşitlemek için şirket içi Active Directory örneğinden parola karmasını Azure AD Connect eşitleme ayıklar. Azure Active Directory kimlik doğrulama hizmeti eşitlenmeden önce ek güvenlik işleme parola karması uygulanır. Parolalar, kullanıcı başına temelinde ve kronolojik sıraya göre eşitlenir.

Parola karma eşitlemesi işleminin gerçek veri akışı DisplayName veya e-posta adresleri gibi kullanıcı verilerinin eşitlemesine ilişkin benzer. Ancak, parolalar diğer öznitelikleri için standart dizin eşitleme pencere daha sık eşitlenir. Parola karma eşitlemesi işlemi 2 dakikada bir çalışır. Bu işlemin sıklığını değiştiremezsiniz. Parola Eşitleme sırasında mevcut bulut parolayı üzerine yazar.

İlk kez parola karma Eşitlemesi özelliği etkinleştirdiğinizde, parolaların tüm kapsam kullanıcıların bir başlangıç eşitlemesi gerçekleştirir. Bir alt kümesini eşitlemek istediğiniz kullanıcı parolalarını açıkça tanımlayamazsınız.

Bir şirket içi parola değiştirdiğinizde, güncelleştirilmiş parolayı, genellikle birkaç dakika içinde eşitlenir.
Parola karma Eşitlemesi özelliği başarısız eşitleme girişimleri otomatik olarak yeniden dener. Bir hata, bir parola eşitleme girişimi sırasında bir hata oluşursa, Olay Görüntüleyicisi'nde günlüğe kaydedilir.

Parola Eşitleme, şu anda açık olan kullanıcının herhangi bir etkisi yoktur.
Geçerli bulut hizmeti oturumunuz bir bulut hizmetinde oturum sırasında oluşan bir eşitlenmiş parola değişikliği hemen etkilenmez. Ancak, bulut hizmeti yeniden kimlik doğrulaması gerektirdiğinde, yeni parolanızı sağlamanız gerekir.

Bir kullanıcının şirket kimlik bilgilerini, olup bunlar şirket ağlarına oturum açtınız bağımsız olarak Azure AD kimlik doğrulaması için ikinci kez girmesi gerekir. Kullanıcı oturum açmada (KMSI) onay kutusundaki bana imzalı tut seçerse bu deseni, ancak en aza indirgenebilir. Bu seçim, kısa bir süre için kimlik doğrulaması atlayan bir oturum tanımlama bilgisi ayarlar. KMSI davranışı etkin veya Azure AD Yöneticisi tarafından devre dışı.

> [!NOTE]
> Parola Eşitleme, yalnızca Active Directory nesne türü kullanıcı için desteklenir. İNetOrgPerson nesne türü için desteklenmiyor.

### <a name="detailed-description-of-how-password-hash-synchronization-works"></a>Parola karma eşitlemesini nasıl çalıştığını ayrıntılı açıklama
Aşağıdaki ayrıntılı parola karması eşitlemesi Active Directory ve Azure AD arasında nasıl çalıştığını açıklar.

![Ayrıntılı parola akışı](./media/active-directory-aadconnectsync-implement-password-hash-synchronization/arch3.png)


1. Her iki dakikada AD Connect sunucusu parola karma eşitlemesi aracısında standart aracılığıyla bir DC gelen depolanan parola karmalarını (unicodePwd özniteliği) istekleri [MS-DRSR](https://msdn.microsoft.com/library/cc228086.aspx) verileri eşitlemek için kullanılan çoğaltma Protokolü DC'lerin arasında. Hizmet hesabı, parola karmaları almak için dizin değişiklikleri Çoğalt ve (yüklemesinde varsayılan olarak) çoğaltmak dizin değişiklikleri tüm AD izinler olmalıdır.
2. Göndermeden önce etki alanı denetleyicisi MD4 parola karması bir anahtarı kullanarak şifreler bir [MD5](http://www.rfc-editor.org/rfc/rfc1321.txt) karma RPC oturum anahtarı ve bir veri dizesi. Bunu daha sonra sonuç parola karma eşitlemesi aracıya RPC üzerinden gönderir. DC ayrıca salt eşitleme Aracısı aracı Zarf şifresini mümkün olması için DC çoğaltma protokolü kullanarak geçirir.
3.  Parola karma eşitlemesi aracı şifrelenmiş Zarf sahip olduktan sonra onu kullanır [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) ve özgün MD4 biçiminde dön alınan verilerin şifresini çözmek için bir anahtar oluşturmak için veri dizesi. Herhangi bir noktada parola karma eşitlemesi Aracısı düz metin parolası erişimi yok. Parola karma eşitlemesi aracısının MD5 kesinlikle DC ile çoğaltma Protokolü uyumluluk için kullanılır ve yalnızca şirket içi etki alanı denetleyicisi ve parola karma eşitlemesi aracısı arasında kullanılır.
4.  Parola karma eşitlemesi aracı ilk Bu dize dönüştürme bir bayt 32 onaltılık dize karma geri UTF-16 kodlamalı ikili dönüştürerek 16 bayt ikili parola karması 64 bayt genişletir.
5.  Parola karma eşitlemesi Aracısı ekler bir kullanıcı salt, özgün karma daha iyi korumak için 64-bayt ikili için 10 bayt uzunlukta bir veri dizesi oluşan başına.
6.  Parola karma eşitlemesi Aracısı sonra MD4 karma birleştirir artı kullanıcı salt başına ve içine girdi [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) işlevi. 1000 yinelemelerini [HMAC SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) anahtarlı karma algoritma kullanılır. 
7.  Parola karma eşitlemesi Aracısı elde edilen 32 baytlık karma alır, her ikisi de art arda ekler kullanıcı salt ve SHA256 sayısı ona yineleme (tarafından kullanım için Azure AD), ardından iletir Azure ad, Azure AD Connect dizeden SSL üzerinden.</br> 
8.  Bir kullanıcı Azure AD ile oturum açmasını sağlamaya çalışır ve parolasını girer, parola aynı MD4 + salt + PBKDF2 + HMAC SHA256 işlemi çalıştırılır. Sonuçta elde edilen karma Azure AD'de depolanan karma eşleşirse, kullanıcı doğru parolayı geçtiğini ve doğrulanır. 

>[!Note] 
>Özgün MD4 karma Azure AD'ye aktarılan değil. Bunun yerine, özgün MD4 karma SHA256 karma iletilir. Sonuç olarak, Azure AD'de depolanan karması alınıyorsa, bir şirket içi pass--hash saldırısında kullanılamaz.

### <a name="how-password-hash-synchronization-works-with-azure-active-directory-domain-services"></a>Parola karma eşitlemesi Azure Active Directory etki alanı Hizmetleri ile nasıl çalışır?
Şirket içi parolalarınızı eşitlemek için parola karma Eşitlemesi özelliği de kullanabilirsiniz [Azure Active Directory etki alanı Hizmetleri](../../active-directory-domain-services/active-directory-ds-overview.md). Bu senaryoda, Azure Active Directory etki alanı Hizmetleri örneği, kullanıcılarınızın bulutta şirket içi Active Directory örneğinizi kullanılabilir tüm yöntemleri ile kimliğini doğrular. Bu senaryo deneyimi, bir şirket içi ortamda Active Directory Geçiş Aracı'nı (ADMT) kullanmaya benzer.

### <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler
Parola eşitleme yaparken parolanızı düz metin sürümü Azure ad parola karma Eşitlemesi özelliği veya ilişkili hizmetlerden herhangi biri gösterilmez.

Kullanıcı kimlik doğrulaması Azure ad yerine kuruluşun kendi Active Directory örneğiyle gerçekleşir. Kuruluşunuzun parola verileri herhangi bir şirket içi bırakarak formunda, SHA256 parola verileri Azure AD'de--depolanan düşünmek endişeniz varsa özgün MD4 karma--karmasını Active Directory'de depolanan daha önemli ölçüde daha güvenlidir. Ayrıca, bu SHA256 karma şifresi çözülemiyor olduğundan, bu olamaz kuruluşunuzun Active Directory ortamına geri getirildi ve bir geçerli kullanıcının parolası pass--hash saldırısı olarak sunulan.





### <a name="password-policy-considerations"></a>Parola ilke konuları
Parola karma eşitlemesini etkinleştirerek etkilenen parola ilkeleri iki tür vardır:

* Parola karmaşıklık İlkesi
* Parola süre sonu ilkesi

#### <a name="password-complexity-policy"></a>Parola karmaşıklık İlkesi  
Parola karma eşitlemesi etkinleştirildiğinde, şirket içi Active Directory örneğinizi parola karmaşıklık ilkelerinde eşitlenen kullanıcılar için bulutta karmaşıklık ilkeleri geçersiz kılar. Azure AD hizmetlerine erişmek için şirket içi Active Directory örneğinden tüm geçerli parolaların kullanabilirsiniz.

> [!NOTE]
> Doğrudan bulutta oluşturulan kullanıcılar için parola hala parola ilkeleri tabi bulutta tanımlanır.

#### <a name="password-expiration-policy"></a>Parola süre sonu ilkesi  
Bir kullanıcı parola karması eşitlemesi kapsamında ise, bulut hesap parolası kümesine *süresi dolmayacak*.

Bulut hizmetlerinizi, şirket içi ortamınızda süresi eşitlenmiş bir parola kullanarak oturum açmaya devam edebilirsiniz. Bulut parolanızı, parola şirket içi ortamda bir sonraki değiştirdiğinizde güncelleştirilir.

#### <a name="account-expiration"></a>Hesabın süre sonu
Kuruluşunuzun kullanıcı hesabı yönetimi bir parçası olarak accountExpires özniteliği kullanıyorsa, bu öznitelik Azure AD ile eşitlenmemiş unutmayın. Sonuç olarak, süresi dolmuş bir Active Directory hesabı parola karma eşitlemesi için yapılandırılmış bir ortamda Azure AD'de etkin olmaya devam eder. Hesap süresi dolmuşsa, bir iş akışı eylemi kullanıcının Azure AD hesabının devre dışı bırakan bir PowerShell Betiği tetiklemesi gereken öneririz. Hesap açıldığında, buna karşılık, Azure AD örneğinde açık olmalıdır.

### <a name="overwrite-synchronized-passwords"></a>Eşitlenmiş parolalar üzerine yaz
Bir yönetici el ile Windows PowerShell kullanarak parolanızı sıfırlayabilirsiniz.

Bu durumda, yeni parola eşitlenmiş parolanızı geçersiz kılar ve bulutta tanımlanan tüm parola ilkelerini yeni parola uygulanır.

Şirket içi parolanızı değiştirirseniz yeniden, yeni parola buluta eşitlenir ve el ile güncelleştirilmiş parolayı geçersiz kılar.

Parola Eşitleme, açık olan kullanıcının Azure üzerinde hiçbir etkisi olmaz. Geçerli bulut hizmeti oturumunuz bir bulut hizmetine oturum açtınız ancak oluşan bir eşitlenmiş parola değişikliği hemen etkilenmez. KMSI bu fark süresini uzatır. Bulut hizmeti yeniden kimlik doğrulaması gerektirdiğinde, yeni parolanızı sağlamanız gerekir.

### <a name="additional-advantages"></a>Ek avantajlar

- Genellikle, parola karması eşitlemesi bir Federasyon Hizmeti uygulamak daha basittir. Herhangi bir ek sunucu gerektirmez ve kullanıcıların kimliklerini doğrulamak için bir yüksek oranda kullanılabilir Federasyon Hizmeti bağımlılığı ortadan kaldırır. 
- Parola karma eşitlemesi yanı sıra Federasyon de etkinleştirilebilir. Federasyon hizmetinize bir kesinti oluşursa bir geri dönüş olarak kullanılabilir.












## <a name="enable-password-hash-synchronization"></a>Parola karma eşitlemesini etkinleştir
Yüklediğinizde, Azure AD Connect kullanarak **hızlı ayarlar** seçeneği, parola karması eşitlemesi otomatik olarak etkinleştirilir. Daha fazla ayrıntı için bkz: [hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](active-directory-aadconnect-get-started-express.md).

Azure AD Connect yüklediğinizde özel ayarları kullanırsanız, parola karması eşitlemesi kullanıcı oturum açma sayfasında kullanılabilir. Daha fazla ayrıntı için bkz: [Azure AD Connect özel yüklemesi](active-directory-aadconnect-get-started-custom.md).

![Parola karma eşitlemesini etkinleştirme](./media/active-directory-aadconnect-get-started-custom/usersignin2.png)

### <a name="password-hash-synchronization-and-fips"></a>Parola karma eşitlemesi ve FIPS
Sunucunuz Federal Bilgi İşleme Standardı (FIPS göre) kilitli, MD5 devre dışı bırakılır.

**Parola karma eşitlemesi için MD5 etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. %ProgramFiles%\Azure AD Sync\Bin gidin.
2. Miiserver.exe.config açın.
3. Dosyanın sonunda yapılandırma/çalışma zamanı düğümüne gidin.
4. Aşağıdaki düğüm ekleyin: `<enforceFIPSPolicy enabled="false"/>`
5. Yaptığınız değişiklikleri kaydedin.

Başvuru için bu kod parçacığında ne gibi görünmelidir şöyledir:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Güvenlik ve FIPS hakkında daha fazla bilgi için bkz: [AAD parola eşitleme, şifreleme ve FIPS Uyumluluk](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-hash-synchronization"></a>Parola karma eşitleme sorunlarını giderme
Parola karma eşitlemesi ile ilgili sorununuz olup [parola karması eşitlemesi sorun giderme](active-directory-aadconnectsync-troubleshoot-password-hash-synchronization.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect eşitleme: eşitleme seçeneklerini özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
