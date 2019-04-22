---
title: Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama | Microsoft Docs
description: Parola Karması eşitleme nasıl çalıştığı ve nasıl ayarlandığı hakkında bilgi sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/02/2019
ms.subservice: hybrid
ms.author: billmath
search.appverid:
- MET150
ms.collection: M365-identity-device-management
ms.openlocfilehash: 146fdc3ca2af708a96e6b9a604493eb63c2e6530
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58916385"
---
# <a name="implement-password-hash-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama
Bu makalede, şirket içi Active Directory örneğinden bulut tabanlı bir Azure Active Directory (Azure AD) örneği, kullanıcı parolalarını eşitlemek için gereken bilgileri sağlar.

## <a name="how-password-hash-synchronization-works"></a>Parola Karması eşitleme nasıl çalışır?
Active Directory etki alanı hizmeti parolaları bir karma değer gösterimini gerçek kullanıcı parolasının biçiminde depolar. Bir karma değer, tek yönlü bir matematiksel işlev sonucudur ( *karma algoritması*). Tek yönlü işlevin sonucunu, parolanın düz metin sürümüne döndürmek mümkün değildir. Parola karması kullanarak şirket içi ağınızda oturum açamazsınız.

Parolanızı eşitlemek için şirket içi Active Directory örneğinden, parola karmasını Azure AD Connect eşitleme ayıklar. Azure Active Directory kimlik doğrulama hizmeti eşitlenmeden önce ek güvenlik işleme için parola karması uygulanır. Parolalar, kullanıcı başına temelinde ve kronolojik sırayla eşitlenir.

Parola Karması eşitleme işlemini gerçek veri akışını verilerinin eşitlemesine ilişkin kullanıcı için benzerdir. Ancak, parolaları diğer öznitelikler için standart dizin eşitleme penceresinde daha sık eşitlenir. Parola Karması eşitleme işlemi 2 dakikada bir çalışır. Bu işlemin sıklığını değiştiremezsiniz. Parola Eşitleme sırasında mevcut bulut parolayı üzerine yazar.

İlk kez parola karması eşitleme özelliğini etkinleştirdiğinizde tüm kapsamındaki kullanıcıların parolalarının ilk eşitlemeyi gerçekleştirir. Bir alt kümesini eşitlemek istediğiniz kullanıcı parolalarını açıkça tanımlayamazsınız.

Bir şirket içi parolayı değiştirdiğinizde, güncelleştirilmiş parolayı, genellikle birkaç dakika içinde eşitlenir.
Parola Karması eşitleme özelliği eşitleme başarısız girişimleri otomatik olarak yeniden dener. Bir hata, bir parola eşitleme girişimi sırasında bir hata meydana gelirse, Olay Görüntüleyicisi'nde günlüğe kaydedilir.

Parola Eşitleme, şu anda oturum açan kullanıcının herhangi bir etkisi yoktur.
Geçerli bulut hizmeti oturumunuzu, bir bulut hizmeti için oturum açtığınız sırada gerçekleşen, eşitlenmiş parola değişikliği hemen etkilenmez. Ancak, bulut hizmetine yeniden kimlik doğrulaması gerektirdiğinde, yeni parolanızı vermeniz gerekir.

Bir kullanıcı şirket kimlik bilgilerini, olup, şirket ağlarına oturum açmadıysanız bağımsız olarak Azure AD'ye kimlik doğrulaması için ikinci bir kez girmeniz gerekir. Bu düzen, ancak kullanıcı oturum açma sırasında (KMSI) onay kutusu Oturumumu açık bırak seçerse indirgenebilir. Bu seçim, 180 gün için kimlik doğrulamasını atlar bir oturum tanımlama bilgisini ayarlar. KMSI'yi davranışı, etkin veya Azure AD Yöneticisi tarafından devre dışı. Ayrıca, etkinleştirerek parola istemlerinin azaltabilir [sorunsuz çoklu oturum açma](how-to-connect-sso.md), hangi otomatik olarak açarsa Kurumsal cihazlarından şirket ağınıza bağlı olduklarında kullanıcıların.

> [!NOTE]
> Parola Eşitleme, yalnızca Active Directory nesne türü kullanıcı için desteklenir. InetOrgPerson nesnesi türü için desteklenmiyor.

### <a name="detailed-description-of-how-password-hash-synchronization-works"></a>Parola Karması eşitleme nasıl çalıştığına ilişkin ayrıntılı bir açıklaması
Aşağıdaki bölümde açıklanmaktadır, ayrıntılı, Active Directory ve Azure AD arasında parola karması eşitleme nasıl çalışır.

![Ayrıntılı parola akış](./media/how-to-connect-password-hash-synchronization/arch3b.png)


1. Her iki dakikada bir DC gelen parola karmalarını (unicodePwd özniteliğini) AD Connect sunucusu isteklerde parola karması eşitleme Aracısı depolanır.  Bu istek için standarttır [MS DRSR](https://msdn.microsoft.com/library/cc228086.aspx) DC'leri arasında verileri eşitleyebilmeniz için kullanılan çoğaltma protokolü. Hizmet hesabı, parola karmaları almak için dizin değişikliklerini çoğaltma ve (varsayılan olarak yükleme izni) dizin değişikliklerini tümüne çoğaltma AD izinleri olmalıdır.
2. Göndermeden önce DC MD4 parola karması bir anahtarı kullanarak şifreler bir [MD5](https://www.rfc-editor.org/rfc/rfc1321.txt) karma RPC oturum anahtarı ve bir güvenlik değeri. Bunu ardından sonuç parola karması eşitleme Aracısı ile RPC üzerinden gönderir. Eşitleme aracısına ayrıca geçirir salt aracı Zarf şifresini olacak şekilde DC çoğaltma protokolü kullanarak etki alanı denetleyicisi.
3.  Parola Karması eşitleme Aracısı şifrelenmiş Zarf sahip olduktan sonra bunu kullanan [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) ve özgün MD4 biçiminde dön alınan verilerin şifresini çözmek için bir anahtar oluşturmak için güvenlik değeri. Parola Karması eşitleme Aracısı düz metin parolası hiç erişebilir. Parola Karması eşitleme aracısının MD5 kesinlikle DC ile çoğaltma protokol uyumluluk için kullanılır ve yalnızca şirket içi etki alanı denetleyicisi ve parola karması eşitleme aracısı arasında kullanılır.
4.  Parola Karması eşitleme Aracısı'nı, ilk UTF-16 kodlamalı ikili ardından bu dize dönüştürme 32 bayt onaltılık dize, karma geri dönüştürerek 64 bayt ile 16 bayt ikili parola karması genişletir.
5.  Parola Karması eşitleme Aracısı ekler bir kullanıcı güvenlik değeri, özgün karma daha iyi korumak için 64-bayt ikili için 10 bayt uzunlukta bir güvenlik değeri oluşan başına.
6.  Parola Karması eşitleme Aracısı'nı, ardından MD4 karma birleştirir ve kullanıcı salt başına ve içine girişlerinin [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) işlevi. 1000 yinelemeleri [HMAC SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) anahtarlı karma algoritması kullanılır. 
7.  Parola Karması eşitleme Aracısı elde edilen 32 bayt karma alır, her ikisini birleştirerek kullanıcı salt ve SHA256 sayısı ona yinelemeler (tarafından kullanılacak Azure AD), ardından iletir Azure AD Connect dizeden Azure AD'ye SSL üzerinden.</br> 
8.  Bir kullanıcının Azure AD'de oturum dener ve parolasını girer, parola aynı MD4 + salt + PBKDF2 + HMAC SHA256 işlemi çalıştırılır. Sonuçta elde edilen karma Azure AD'de depolanan karma eşleşirse, kullanıcı doğru parolayı geçtiğini ve doğrulanır. 

>[!Note] 
>Özgün MD4 karma Azure AD'ye aktarılan değil. Bunun yerine, özgün MD4 karma SHA256 karma iletilir. Sonuç olarak, Azure AD'de depolanan karma aldıysanız, bir şirket içi pass--hash saldırısında kullanılamaz.

### <a name="how-password-hash-synchronization-works-with-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services ile parola karma eşitleme nasıl çalışır
Şirket içi parolalarınızı eşitlemek için parola karması eşitleme özelliğini de kullanabilirsiniz [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). Bu senaryoda, şirket içi Active Directory Örneğinizde kullanılabilir tüm yöntemleri ile kullanıcılarınızın buluttaki Azure Active Directory Domain Services örneğini doğrular. Bu senaryonun deneyimi, bir şirket içi ortamda Active Directory Geçiş Aracı (ADMT) kullanmaya benzer.

### <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler
Parola eşitleme yaparken parolanızı düz metin sürümünü Azure AD'ye parola karması eşitleme özelliğini veya tüm ilişkili hizmetlerin gösterilmez.

Kullanıcı kimlik doğrulamasını Azure AD'ye yönelik yerine kuruluşun kendi Active Directory örneğine karşı gerçekleşir. Azure AD'de--depolanan SHA256 parola verileri özgün MD4 karma--karma Active Directory'de depolanan değerinden daha güvenlidir. Ayrıca, bu SHA256 karma şifresi çözülemiyor olduğundan, bu olamaz kuruluşun Active Directory ortamına geri getirildi ve pass--hash saldırısı bir geçerli kullanıcı parolası olarak sunulan.

### <a name="password-policy-considerations"></a>Parola ilke konuları
Parola Karması eşitlemeyi etkinleştirerek, etkilenen parola ilkelerini iki tür vardır:

* Parola karmaşıklığı İlkesi
* Parola süre Dolum İlkesi

#### <a name="password-complexity-policy"></a>Parola karmaşıklığı İlkesi  
Parola Karması eşitleme etkinleştirildiğinde, şirket içi Active Directory Örneğinizde parola karmaşıklık ilkeleri eşitlenmiş kullanıcılar için bulutta karmaşıklığı ilkeleri geçersiz kılar. Azure AD Hizmetleri erişmek için şirket içi Active Directory örneğinden tüm geçerli parolaların kullanabilirsiniz.

> [!NOTE]
> Bulutta tanımlandığı gibi doğrudan bulutta oluşturulan sanal kullanıcıların parolalarını yine de parola ilkelerine sahiptir.

#### <a name="password-expiration-policy"></a>Parola süre Dolum İlkesi  
Bir kullanıcı parola karması eşitleme kapsamında, bulut hesap parolası kümesine *dolmasın*.

Şirket içi ortamınızda süresi dolmuş bir eşitlenmiş parola kullanarak bulut hizmetlerinizde oturum açmak devam edebilirsiniz. Bulut parolanızı, parola şirket içi ortamda bir sonraki değiştirdiğinizde güncelleştirilir.

#### <a name="account-expiration"></a>Hesap süresi
Kuruluşunuzun kullanıcı hesabı Yönetimi işleminin bir parçası olarak accountExpires öznitelik kullanıyorsa, bu öznitelik Azure AD'ye eşitlenmez. Sonuç olarak, süresi dolmuş bir Active Directory hesabı için parola karması eşitlemeyi yapılandırılmış bir ortamda hala Azure AD'de etkin olacaktır. Hesabın süresi, bir iş akışı eylemi, kullanıcının Azure AD hesabı devre dışı bırakan bir PowerShell Betiği tetiklemesi gereken öneririz (kullanın [Set-AzureADUser](https://docs.microsoft.com/powershell/module/azuread/set-azureaduser?view=azureadps-2.0) cmdlet'i). Hesap açık olduğunda, buna karşılık, Azure AD örneği açık olması.

### <a name="overwrite-synchronized-passwords"></a>Eşitlenmiş parolalar üzerine yaz
Bir yönetici, el ile Windows PowerShell kullanarak parolanızı sıfırlayabilirsiniz.

Bu durumda, yeni parola eşitlenmiş parolanızı geçersiz kılar ve bulutta tanımlanan tüm parola ilkelerini yeni parola uygulanır.

Şirket içi parolanızı değiştirirseniz yeniden, yeni parola, buluta eşitlenebilir ve el ile güncelleştirilmiş parolayı geçersiz kılar.

Parola Eşitleme, oturum açmış kullanıcının Azure üzerinde hiçbir etkisi yoktur. Geçerli bulut hizmeti oturumunuzu, bir bulut hizmetine oturumunuz sırasında oluşan eşitlenmiş parola değişikliği hemen etkilenmez. KMSI'yi bu fark süresini uzatır. Bulut hizmetine yeniden kimlik doğrulaması gerektirdiğinde, yeni parolanızı vermeniz gerekir.

### <a name="additional-advantages"></a>Ek avantajlar

- Genellikle, parola karması eşitleme bir Federasyon Hizmeti uygulamak daha kolaydır. Diğer sunucular gerektirmez ve kullanıcıların kimliğini doğrulamak için bir yüksek oranda kullanılabilir bir Federasyon Hizmeti bağımlılığa ortadan kaldırır.
- Parola Karması eşitleme, Federasyon yanı sıra da etkinleştirilebilir. Federasyon hizmetinize bir kesinti oluşursa bir geri dönüş olarak kullanılabilir.

## <a name="enable-password-hash-synchronization"></a>Parola karma eşitlemesini etkinleştirme

>[!IMPORTANT]
>Parola Karması eşitleme için AD FS (veya diğer Federasyon teknolojileri) geçiriyorsanız, yayımlanan ayrıntılı dağıtım kılavuzunu izleyin öneririz [burada](https://aka.ms/adfstophsdpdownload).

Kullanarak Azure AD Connect yüklerken **hızlı ayarlar** seçeneği, parola karması eşitleme otomatik olarak etkinleştirilir. Daha fazla bilgi için [hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](how-to-connect-install-express.md).

Azure AD Connect yükleme sırasında özel ayarları kullanırsanız, parola karması eşitleme, kullanıcı oturum açma sayfasında kullanılabilir. Daha fazla bilgi için [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md).

![Parola Karması eşitlemeyi etkinleştirme](./media/how-to-connect-password-hash-synchronization/usersignin2.png)

### <a name="password-hash-synchronization-and-fips"></a>Parola karma eşitlemesi ve FIPS
Sunucunuz Federal Bilgi İşleme Standardı (FIPS göre) kilitli, MD5 devre dışı bırakıldı.

**MD5 için parola karması eşitlemeyi etkinleştirmek için aşağıdaki adımları gerçekleştirin:**

1. İçin %ProgramFiles%\Azure AD Sync\Bin gidin.
2. Miiserver.exe.config açın.
3. Dosyanın sonunda configuration/çalışma zamanı düğümüne gidin.
4. Aşağıdaki düğüm ekleyin: `<enforceFIPSPolicy enabled="false"/>`
5. Yaptığınız değişiklikleri kaydedin.

Başvuru için bu kod parçacığı, ne gibi görünmelidir gösterilmiştir:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Güvenlik ve FIPS hakkında daha fazla bilgi için bkz. [Azure AD parola karma eşitlemesi, şifreleme ve FIPS uyumluluğunu](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-hash-synchronization"></a>Parola Karması eşitleme sorunlarını giderme
Parola Karması eşitleme ile ilgili sorunlar olup [parola karması eşitleme sorunlarını giderme](tshoot-connect-password-hash-synchronization.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect eşitlemesi: Eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
* [Parola Karması eşitleme için ADFS geçirmek için adım adım dağıtım planı Al](https://aka.ms/authenticationDeploymentPlan)
