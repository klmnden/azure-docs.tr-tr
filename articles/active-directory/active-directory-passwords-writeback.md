---
title: Azure AD SSPR'yi parola geri yazma ile | Microsoft Docs
description: "Azure AD ve Azure AD kullanarak şirket içi dizine parolaları geri yazma Bağlan"
services: active-directory
keywords: "Active directory parola yönetimi, Azure AD parola yönetimi self servis parola sıfırlama"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9733774570f3148e0092f42c1321b4fac1c80b54
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="password-writeback-overview"></a>Parola geri yazma genel bakış

Parola geri yazma, parolalar, şirket içi Active Directory'ye geri yazma için Azure AD yapılandırmanıza olanak sağlar. Karmaşık şirket içi Self Servis parola sıfırlama çözümünü ayarlamanıza gerek kaldırır ve bulut tabanlı kolay bir yol, kullanıcılarınızın nerede olurlarsa olsunlar şirket içi parolalarını sıfırlamaya sağlar. Parola geri yazma özelliğini bir bileşenidir [Azure Active Directory Connect](./connect/active-directory-aadconnect.md) , etkinleştirilebilir ve Premium geçerli aboneler tarafından kullanılan [Azure Active Directory sürümleri](active-directory-editions.md).

Parola geri yazma aşağıdaki özellikleri sağlar:

* **Sıfır gecikme geri bildirim** -parola geri yazma eşzamanlı bir işlem değil. Kullanıcılarınızın parolalarını İlkesi karşılamayan veya sıfırlama veya herhangi bir nedenle değiştirilen bırakamıyor hemen bildirilir.
* **AD FS veya diğer Federasyon teknolojileri kullanan kullanıcılar parolalarını sıfırlama destekler** -Azure AD kiracınıza eşitlenen Federasyon kullanıcısı hesapları sürece sahip parola geri yazma, bunlar şirket içi yönetebilmek için AD parolalarını buluttan.
* **Kullanarak kullanıcıların parolalarını sıfırlama destekler [parola karması eşitlemesi](./connect/active-directory-aadconnectsync-implement-password-synchronization.md)**  - parola sıfırlama hizmeti algıladığında, eşitlenen kullanıcı hesabı için parola karma eşitlemesi etkinleştirildiğinde, biz içi hem de bu hesabın sıfırlama ve parola aynı anda bulut.
* **Erişim paneli ve Office 365 parolaları değiştirme destekler** - federe olduğunda veya Biz bu parolaları yerel AD ortamınızı geri yazma süresi dolmuş ya da süresi dolmuş olmayan kullanıcıların parolalarını değiştirmek için gelen parola eşitlenen kullanıcılar.
* **Destekleyen bir yönetici bunları Azure portalından sıfırladığınızda parola geri yazma** - bir yönetici bir kullanıcının parolasını sıfırlar her [Azure portal](https://portal.azure.com), kullanıcının Federasyon ya da eşitlenmiş parola, parola ayarlar Yönetici, yerel AD üzerinde de seçer. Bu Office Yönetim Portalı'nda şu anda desteklenmiyor.
* **Şirket içi zorlar AD parola ilkeleri** - bir kullanıcı kendi parolasını sıfırlandıktan sonra biz şirket içi karşıladığından emin olun, dizinine gerçekleştirmeden önce AD ilkesi. Bu geçmiş, karmaşıklık, yaş, parola filtreleri ve yerel AD içinde tanımlanan diğer parola kısıtlamaları içerir.
* **Tüm gelen güvenlik duvarı kurallarını gerektirmeyen** -parola geri yazma, temel alınan bir iletişim kanalı, bu özelliğin çalışması, güvenlik duvarında gelen bağlantı noktalarının açık gerekmez anlamı olarak Azure Service Bus geçişini kullanır.
* **Şirket içi Active Directory'de korunan grupları içinde mevcut kullanıcı hesapları için desteklenmiyor** - korumalı grupları hakkında daha fazla bilgi için bkz: [korumalı hesapları ve grupları Active Directory'de](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Parola geri yazma nasıl çalışır?

Eşitlenen kullanıcı sıfırlama veya buluttaki parolalarını değiştirmek için gelen bir federe ya da parola karması, aşağıdakiler gerçekleşir:

1. Kullanıcının parola türüne sahip görmek için denetleyin.
    * Biz görürseniz parola şirket içinde yönetilir
        * Geri yazma hizmet yukarı olup olmadığını denetleyin ve doğru yazılmışsa, çalışan, biz devam kullanıcı izin verin
        * Geri yazma hizmet yukarı değilse, biz kullanıcı parolalarını şimdi sıfırlanamaz bildirin
2. Ardından, kullanıcı uygun kimlik doğrulama geçitleri geçirir ve sıfırlama parola ekran ulaşır.
3. Kullanıcı yeni bir parola seçer ve bunu doğrular.
4. Gönderme tıklatıldığında, biz düz metin parola geri yazma Kurulum işlemi sırasında oluşturulmuş simetrik anahtarla şifreler.
5. Parola şifreleme sonrasında biz (ayarladığınız biz de için geri yazma Kurulum işlemi sırasında), Kiracı özgü service bus geçişi için bir HTTPS kanal üzerinden gönderilir bir yükü dahil edin. Bu geçiş, yalnızca şirket içi yüklemenizi bilir rastgele oluşturulmuş bir parola tarafından korunuyor.
6. İletinin hizmet veri yolu ulaştığında, parola sıfırlama endpoint otomatik olarak uyanır ve bekleyen sıfırlama isteği olduğunu görür.
7. Hizmeti daha sonra kullanıcı için bulut bağlayıcı özniteliğini kullanarak söz konusu arar. Bu arama başarılı olması:

    * Kullanıcı nesnesi AD Bağlayıcısı alanında bulunması gerekir
    * Kullanıcı nesnesi için karşılık gelen MV nesne bağlanmalıdır
    * Kullanıcı nesnesi karşılık gelen AAD bağlayıcı nesneye bağlı olması gerekir.
    * MV AD Bağlayıcısı nesnesinden bağlantısını eşitleme kuralına sahip olmalıdır `Microsoft.InfromADUserAccountEnabled.xxx` bağlantıyı. <br> <br>
    Çağrı buluttan geldiğinde, eşitleme altyapısı AAD bağlayıcı alanı nesne bağlantı geri MV nesne için aşağıdaki aramak için cloudAnchor özniteliğini kullanır ve ardından bağlantıyı AD nesnesine geri izler. Aynı kullanıcı birden fazla AD nesne (Çoklu orman) olabilir çünkü eşitleme altyapısı dayanan `Microsoft.InfromADUserAccountEnabled.xxx` bağlantı doğru olanı seçin.

    > [!Note]
    > Bu mantık sonucu olarak Azure AD Connect PDC çalışmaya öykünücü ile parola geri yazma için iletişim kurabilmesi gerekir. Bu el ile etkinleştirmeniz gerekirse, Azure AD Connect PDC öykünücüsüne üzerinde sağ tıklayarak bağlayabilirsiniz **özellikleri** sonra seçerek Active Directory Eşitlemesi bağlayıcının **directory yapılandırma bölümler**. Burada **etki alanı denetleyicisi bağlantı ayarları** bölümünü arayın ve **yalnızca tercih edilen etki alanı denetleyicilerini kullan** başlıklı kutuyu işaretleyin. Tercih edilen DC PDC öykünücüsü olsa bile, Azure AD Connect parola geri yazma için PDC bağlanmak dener.

8. Kullanıcı hesabı bulunduktan sonra uygun AD ormanında doğrudan parola sıfırlama deneyin.
9. Parola ayarlama işlemi başarılı olursa, biz kullanıcı parolalarını değiştirildi söyleyin.
    > [!NOTE]
    > Durumda parola eşitleme, kullanarak Azure AD kullanıcı parolasının eşitlendiğinde yoktur şirket içi parola ilkesi bulut parola ilkesi zayıf bir fırsat. Bu durumda, biz yine ne olursa olsun şirket içi İlkesi olduğu ve bunun yerine bu parola karmasını eşitlemek parola karma eşitlemesi izin uygulayın. Bu, çoklu oturum açma sağlamak için parola eşitleme ya da Federasyon kullanıyorsanız, şirket içi İlkesi bulutta bağımsız uygulandığını sağlar.

10. Parola işlemi başarısız ayarlarsanız, biz hata kullanıcıya döndürür ve bunları yeniden deneyin sağlayabilirsiniz.
    * İşlemi aşağıdaki nedeniyle başarısız olabilir
        * Hizmet oldu
        * Seçili parola kuruluş ilkeleri karşılamayan
        * Kullanıcının yerel AD içinde bulamadık

    Biz çoğu bu durumlar için belirli bir ileti vardır ve sorunu çözmek için yapabileceklerini kullanıcı söyleyin.

## <a name="configuring-password-writeback"></a>Parola geri yazma özelliğini yapılandırma

Otomatik güncelleştirme özelliğini kullanmanızı öneririz [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) parola geri yazma özelliğini kullanmak istiyorsanız.

DirSync ve Azure AD eşitleme olan artık parola geri yazma makaleyi etkinleştirme desteklenen anlamına gelir [yükseltme DirSync ve Azure AD eşitleme](connect/active-directory-aadconnect-dirsync-deprecated.md) geçişinizin ile yardımcı olmak için daha fazla bilgi bulunur.

Aşağıdaki adımları zaten yapılandırdığınız Azure AD Connect kullanarak ortam varsayın [Express](./connect/active-directory-aadconnect-get-started-express.md) veya [özel](./connect/active-directory-aadconnect-get-started-custom.md) ayarlar.

1. Yapılandırma ve Azure AD Connect sunucunuza parola geri yazma günlüğüne etkinleştirmek ve başlatmak için **Azure AD Connect** Yapılandırma Sihirbazı.
2. Hoş Geldiniz ekranında tıklatın **yapılandırma**.
3. Ek görevler ekranında tıklatın **eşitleme seçeneklerini özelleştirme** ve ardından **sonraki**.
4. Bağlanma Azure AD ekranında, genel yönetici kimlik bilgilerini girin ve seçin **sonraki**.
5. Bağlan dizinleri ve etki alanı ve OU, filtreleme seçebileceğiniz **sonraki**.
6. İsteğe bağlı özellikler ekranında yanındaki kutuyu işaretleyin **parola geri yazma** tıklatıp **sonraki**.
   ![Azure AD CONNECT'te parola geri yazma etkinleştir][Writeback]
7. Ekran yapılandırmak için hazır, tıklatın **yapılandırma** ve işlemin tamamlanması için bekleyin.
8. Tamamlayın, tıklatın yapılandırma gördüğünüzde **çıkış**

Parola geri yazma için genel sorun giderme görevlerini bakın için [parola geri yazma sorun giderme](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) bizim sorun giderme makalede.

## <a name="active-directory-permissions"></a>Active Directory izinleri

Azure AD Connect yardımcı programında belirtilen hesabın parola sıfırlama, parola değiştirme, yazma izinleri üzerinde lockoutTime olmalıdır ve genişletilmiş pwdLastSet yazma izinlerini hakları üzerinde kök nesne **her etki alanı** , Orman **veya** OU'lar SSPR kapsamında olmasını istediğiniz kullanıcı.

Yukarıdaki başvurduğu hangi hesabın emin değilseniz Azure Active Directory Connect yapılandırma kullanıcı arabirimini açın ve görünüm geçerli yapılandırma seçeneğini tıklatın. Hesap listelenir "Altındaki dizinler eşitlenen" izni eklemeniz gerekir

Bu izinlerin ayarlanması MA hizmet hesabının bu orman içindeki kullanıcı hesapları adına parolaları yönetmek her bir orman için sağlar. **Bu izinleri atamazsanız geri yazma doğru yapılandırılmış gibi görünüyor olsa da sonra kullanıcılar buluttan şirket içi parolalarını yönetme girişiminde bulunduğunda hatalarla.**

> [!NOTE]
> Bu bir saat veya daha fazla bilgi için bu izinlerin dizininizdeki tüm nesnelere çoğaltmak için kadar sürebilir.
>

Parola geri yazmanın gerçekleşmesini sağlamak için gerekli izinleri ayarlamak için

1. Active Directory Kullanıcıları ve bilgisayarları uygun etki alanı yönetim izinleri olan bir hesap ile açma
2. Görünüm menüsünden Gelişmiş Özellikler açık olduğundan emin olun
3. Sol bölmede, etki alanının kökünü temsil eden nesne sağ tıklayın ve Özellikler'i seçin
    * Güvenlik sekmesini tıklatın
    * Ardından Gelişmiş'i tıklatın.
4. İzinleri sekmesinden Ekle
5. İzinler (Azure AD Connect kurulumdan) uygulanmakta olan hesabı seç
6. Açılan kutusu uygulanacağı alt kullanıcı nesneleri seçin.
7. İzinler altında aşağıdaki onay kutularını işaretleyin
    * Süresi dolmasın parola
    * Parola Sıfırlama
    * Parolayı Değiştir
    * LockoutTime yazma
    * PwdLastSet yazma
8. Uygula/Tamam aracılığıyla uygulamak ve açık iletişim kutularını çıkmak için'ı tıklatın.

## <a name="licensing-requirements-for-password-writeback"></a>Parola geri yazma için lisans gereksinimleri

Lisans, bkz: ilgili bilgi [lisansları parola geri yazma için gereken](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) veya aşağıdaki siteleri

* [Azure Active Directory fiyatlandırma site](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Güvenli üretken Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Şirket içi parola geri yazma için desteklenen kimlik doğrulama modu

Parola geri yazma aşağıdaki kullanıcı parola türleri için geçerlidir:

* **Yalnızca bulut kullanıcıları**: parola geri yazma şirket içi parola olduğundan bu durumda, uygulanmaz
* **Kullanıcıları parola eşitlenmiş**: desteklenen parola geri yazma
* **Federe kullanıcılar**: desteklenen parola geri yazma
* **Doğrudan kimlik doğrulama kullanıcıların**: desteklenen parola geri yazma

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>Parola geri yazma için desteklenen kullanıcı ve yönetim işlemleri

Parolalar, aşağıdaki durumlarda geri yazılır:

* **Desteklenen son kullanıcı işlemleri**
  * Tüm son kullanıcı Self Servis gönüllü değiştirme parola işlemi
  * Tüm son kullanıcı Self Servis zorla parola değiştirme işlemi (örneğin, parola sona erme)
  * Tüm son kullanıcı Self Servis parola sıfırlama kaynaklanan [parola sıfırlama portalı](https://passwordreset.microsoftonline.com)
* **Desteklenen yöneticisi işlemleri**
  * Tüm yönetici Self Servis gönüllü değiştirme parola işlemi
  * Tüm yönetici Self Servis zorla parola değiştirme işlemi (örneğin, parola sona erme)
  * Tüm yönetici Self Servis parola sıfırlama kaynaklanan [parola sıfırlama portalı](https://passwordreset.microsoftonline.com)
  * Gelen sıfırlama herhangi bir yönetici tarafından başlatılan son kullanıcı parola [Klasik Azure portalı](https://manage.windowsazure.com)
  * Gelen sıfırlama herhangi bir yönetici tarafından başlatılan son kullanıcı parola [Azure portalı](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Parola geri yazma için desteklenmeyen kullanıcı ve yönetim işlemleri

Parolalar **değil** geri aşağıdaki durumların herhangi birinde yazabilirsiniz:

* **Desteklenmeyen son kullanıcı işlemleri**
  * PowerShell v1, v2 veya Azure AD Graph API kullanarak kendi parolasını sıfırlama herhangi bir son kullanıcı
* **Desteklenmeyen yöneticisi işlemleri**
  * Gelen sıfırlama herhangi bir yönetici tarafından başlatılan son kullanıcı parola [Office Yönetim Portalı](https://portal.office.com)
  * Bir yönetici tarafından başlatılan son kullanıcı parolayı PowerShell v1, v2 veya Azure AD Graph API Sıfırla

Bu sınırlamalara kaldırmak için çalışıyoruz, ancak biz biz henüz paylaşabilirsiniz belirli bir zaman çizelgesi yok.

## <a name="password-writeback-security-model"></a>Parola geri yazma güvenlik modeli

Parola geri yazma yüksek güvenlikli bir hizmettir.  Bilgilerinizi korumalı emin olmak için aşağıda açıklanan 4 katmanlı güvenlik modeli etkinleştirme.

* **Kiracı özgü service bus geçişi**
  * Hizmetini ayarlama, biz Microsoft hiçbir zaman erişimi rastgele oluşturulmuş güçlü bir parola tarafından korunan bir kiracıya özgü hizmet veri yolu geçişi ayarlayın.
* **Kilitli ve şifreleme açısından güçlü bir parola şifreleme anahtarı**
  * Service bus geçişi oluşturulduktan sonra kablo üzerinden geldiğinde parolayı şifrelemek için kullanırız güçlü bir simetrik anahtar oluşturuyoruz. Bu anahtar yalnızca deposundaki şirketinizin gizli yoğun kilitlenir ve dizindeki herhangi bir parola gibi denetlenen bulutta yaşar.
* **Endüstri Standart TLS**
  1. Bir parola sıfırlama veya değiştirme işlemi bulutta oluşur, düz metin parola alın ve, ortak anahtarla şifreleyebilirsiniz.
  2. Biz sonra service bus geçişi için Microsoft'un SSL sertifikaları kullanarak şifrelenmiş bir kanal üzerinden gönderilen bir HTTPS iletisi yerleştirin.
  3. Hizmet veri yolunu ileti geldikten sonra şirket içi aracınızı uyanır ve önceden oluşturulan güçlü parola kullanarak Service Bus kimliğini doğrular.
  4. Şirket içi aracı şifrelenmiş iletinin seçer, biz oluşturulan özel anahtarı kullanarak şifresini çözer.
  5. Şirket içi Aracısı sonra AD DS SetPassword API'si aracılığıyla parola ayarlamaya çalışır.
     * Bu AD şirket içi parola ilkenizi (karmaşıklık, yaş, geçmiş, filtreleri, vb.) zorunlu kurmamızı sağlayan bulutta adımdır.
* **İleti sona erme** 
  * Şirket içi hizmetiniz çalışmadığı için Service Bus ileti bulunur, zaman aşımına uğrayacaktır ve daha güvenliğini artırmak için birkaç dakika sonra kaldırılır.

### <a name="password-writeback-encryption-details"></a>Parola geri yazma şifreleme ayrıntıları

Bir kullanıcı, Maksimum hizmet güvenilirlik emin olmak için şirket içi ortamınızda ulaşır ve güvenlik aşağıda açıklanan önce gönderdikten sonra bir parola sıfırlama isteği geçtiği şifreleme adımları.

* **1. adım - parola şifreleme 2048 bit RSA anahtarı ile** - bir kullanıcı şirket içi için ilk olarak, gönderilen parola geri yazılması için bir parola, 2048 bit RSA anahtarıyla şifrelenir gönderdikten sonra.
* **2. adım - paket düzeyi şifreleme AES-GCM ile** - sonra paketin tamamını (parola + gerekli meta veriler), AES-GCM kullanılarak şifrelenir. Bu şifreleme doğrudan erişimi olan herkes için temel alınan ServiceBus kanal görüntüleme/oynama içeriğiyle engeller.
* **Adım 3 - tüm iletişimin TLS / SSL** -ServiceBus tüm iletişimi SSL/TLS kanalda olur. Bu şifreleme içeriği yetkisiz üçüncü tarafların güvenliğini sağlar.
* **Otomatik anahtar geçişi altı ayda** - 6 ayda veya parola geri yazma özelliğini devre dışı / Azure AD Connect üzerinde yeniden etkinleştirilmiş her zaman biz geçişi tüm otomatik olarak anahtarları en fazla hizmeti güvenlik ve güvenliği sağlamak için.

### <a name="password-writeback-bandwidth-usage"></a>Parola geri yazma bant genişliği kullanımı

Parola geri yazma isteklerini aşağıdaki koşullarda yalnızca şirket içi Aracısı'nı geri gönderdiği düşük bant genişlikli bir hizmettir:

1. Etkinleştirme veya Azure AD Connect aracılığı özelliği devre dışı bırakırken gönderilen iki iletileri.
2. Hizmetin çalıştığı sürece her beş dakikada bir hizmet sinyalinin olarak tek bir ileti gönderilir.
3. İki ileti gönderilen her zaman yeni bir parola gönderildi
    * İşlemi gerçekleştirmek için bir istek olarak ilk ileti
    * İşlemin sonucunu içeren ve aşağıdaki durumlarda gönderilen ikinci ileti:
        * Her bir kullanıcı Self Servis parola sıfırlama sırasında yeni bir parola gönderilir.
        * Her bir kullanıcı parolasını değiştirme işlemi sırasında yeni bir parola gönderilir.
        * Her bir kullanıcı yönetici tarafından başlatılan parola (Azure Yönetim Portalı yalnızca) sıfırlama sırasında yeni bir parola gönderilir.

#### <a name="message-size-and-bandwidth-considerations"></a>İleti boyutu ve bant genişliği konuları

Her yukarıda açıklanan iletisinin boyutu genellikle aşırı yükü altında bile altında 1 kb'tır, parola geri yazma hizmetin kendisini birkaç kilobit bant genişliğinin tüketilmesine neden olabilir. Her ileti gerçek zamanlı olarak gönderilen olduğundan, bir parola güncelleştirme işlemi tarafından yalnızca gerekli olduğunda ve ileti boyutu kadar küçük olduğundan geri yazma özelliği bant genişliği kullanımını etkili bir şekilde gerçek ölçülebilir bir etkisi olması için çok küçük kalır.

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisans ile ilgili sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Azure AD CONNECT'te parola geri yazma etkinleştir"
