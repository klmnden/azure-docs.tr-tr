---
title: Azure AD SSPR'yi parola geri yazma | Microsoft Docs
description: Azure AD kullanın ve şirket içi dizine parolaları geri yazma için Azure AD Connect
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: fce92845591f20f7f2e9cff1a856e0904629255b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054803"
---
# <a name="password-writeback-overview"></a>Parola geri yazma genel bakış

Parola geri yazma ile Azure parolalarını şirket içi Active Directory'nize geri yazmak için Active Directory'yi (Azure AD) yapılandırabilirsiniz. Parola geri yazma özelliğini ayarlama ve şirket karmaşık Self Servis parola sıfırlama (SSPR) çözümünü yönetme ihtiyacını ortadan kaldırır ve kullanıcılarınızın nerede olurlarsa olsunlar, şirket içi parolalarını sıfırlamak, uygun bir bulut tabanlı yol sağlar. Parola geri yazma'nin bir bileşeni olan [Azure Active Directory Connect](./../connect/active-directory-aadconnect.md) , etkinleştirilebilir ve geçerli Premium aboneleri tarafından kullanılan [Azure Active Directory sürümleri](../fundamentals/active-directory-whatis.md).

Parola geri yazma, aşağıdaki özellikleri sağlar:

* **Sıfır Gecikmeli geri bildirim sağlar**: parola geri yazma eşzamanlı bir işlem olduğu. Parola ilkesini karşılamadığı veya değil sıfırlama veya herhangi bir nedenle değiştirdiyseniz, kullanıcılarınızın hemen bildirilir.
* **Active Directory Federasyon Hizmetleri (AD FS) veya diğer Federasyon teknolojileri kullanan kullanıcılar için destekleyen parolasını sıfırlar**: Federasyon kullanıcı hesapları, Azure AD kiracınıza eşitlenmiş sürece ile parola geri yazma, bunlar yükümlü Şirket içi Active Directory parolalarını buluttan yönetin.
* **Destekler parolasını sıfırlar kullanan kullanıcılar için** [parola karması eşitleme](./../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md): parola sıfırlama hizmeti bir eşitlenmiş kullanıcı hesabı için parola karması eşitlemeyi etkin olduğunu algıladığında, biz hem de bu sıfırlama hesabın şirket içi ve bulut parola aynı anda. Parola geri yazma özelliğini parola karması eşitlemeyi bağlı değildir.
* **Geçişli kimlik doğrulaması kullanan kullanıcılar için destekleyen parolasını sıfırlar**: doğrudan kimlik doğrulama hesabı, Azure AD kiracınıza eşitlenmiş sürece ile parola geri yazma, kendi şirket içi Active yönetebilir oldukları Buluttan Directory parolalarını.
* **Destekler parola değişikliklerini erişim panelinde ve Office 365**: federe olduğunda veya parolayla eşitlenen kullanıcılar Biz bu parolaları yerel Active Directory ortamınızı geri yazma süresi dolmuş ya da süresi dolmuş olmayan kullanıcıların parolalarını değiştirmek için gelir.
* **Bunları Azure portalından bir yönetici sıfırlar parola geri yazmayı destekler**: her bir yönetici bir kullanıcının parolasını sıfırlar [Azure portalında](https://portal.azure.com), kullanıcının Federasyon veya parolayla eşitlenen ayarladığımız parola yöneticinin yerel Active Directory içinde de seçer. Bu işlevsellik şu anda Office Yönetim Portalı'nda desteklenmiyor.
* **Şirket içi Active Directory parola ilkelerinizi zorlar**: bir kullanıcının parolasını sıfırlar, biz size bu dizine göndermeden önce şirket içi Active Directory ilkenizi karşıladığından emin olun. Bu gözden geçirme geçmişini, karmaşıklık, yaş, parola filtreleri ve yerel Active Directory içinde tanımladığınız herhangi bir parola kısıtlama denetimi içerir.
* **Herhangi bir gelen güvenlik duvarı kuralları gerektirmeyen**: parola geri yazma, temel alınan bir iletişim kanalı bir Azure Service Bus geçişini kullanır. Bu özelliğin çalışması, güvenlik duvarında gelen bağlantı noktalarının açık gerekmez.
* **Şirket içi Active Directory'de korumalı gruplardaki mevcut kullanıcı hesapları için desteklenmiyor**: korunan grupları hakkında daha fazla bilgi için bkz. [korumalı hesaplar ve gruplar Active Directory'de](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Parola geri yazma nasıl çalışır?

Sıfırlama veya değiştirme parolalarını bulutta eşitlenmiş kullanıcı gelen Federasyon veya parola karması, aşağıdakiler gerçekleşir:

1. Kullanıcının parola türüne sahip görmek için denetleyin. Yönetilen şirket içi parolanın olduğunu görüyoruz varsa:
   * Biz, geri yazma hizmetinde çalışır duruma olup olmadığını denetleyin. Hazır ve çalışır durumda, biz devam kullanıcı sağlar.
   * Geri yazma hizmetinde yedekleme değil ise, biz kullanıcı parolalarını hemen sıfırlanamaz söyleyin.
2. Ardından, kullanıcı uygun kimlik doğrulama geçitleri geçirir ve ulaştığında **parolayı Sıfırla** sayfası.
3. Kullanıcı yeni bir parola seçer ve bunu doğrular.
4. Kullanıcı seçtiğinde **Gönder**, biz düz metin parola geri yazma Kurulum işlemi sırasında oluşturulmuş simetrik anahtarla şifrelemek.
5. Parolayı şifrelemek sonra bunu bir HTTPS kanalı üzerinden (ayarladığınız biz de için geri yazma Kurulum işlemi sırasında), bir kiracıya özgü service bus geçişi gönderilen bir yükte dahil ediyoruz. Bu geçiş, yalnızca şirket içi yüklemenizi bildiği rastgele oluşturulmuş bir parolayla korunuyor.
6. Service bus iletiyi ulaştıktan sonra parola sıfırlama uç noktası otomatik olarak uyanır ve sıfırlama isteği bekliyor olduğunu görür.
7. Hizmet bulut çapa özniteliği kullanılarak kullanıcı için ardından arar. Bu arama başarılı olması:

    * Kullanıcı nesnesi Active Directory Bağlayıcı alanında bulunmalıdır.
    * Kullanıcı nesnesi karşılık gelen meta veri deposu (MV) nesnesine bağlanmalıdır.
    * Kullanıcı nesnesi ilgili Azure Active Directory Bağlayıcısı nesneye bağlı olması gerekir.
    * Active Directory Bağlayıcısı nesnesinden MV bağlantısını eşitleme kuralı olmalıdır `Microsoft.InfromADUserAccountEnabled.xxx` bağlantısına. <br> <br>
    Çağrı buluttan geldiğinde, eşitleme altyapısı kullanan **cloudAnchor** Azure Active Directory Bağlayıcı alanı nesne aramak için özniteliği. Ardından bağlantıyı MV nesnesine geri izler ve sonra bağlantı geri Active Directory nesnesi için izler. Eşitleme altyapısı aynı kullanıcı birden çok Active Directory nesnelerini (çok ormanlı) olabileceğinden kullanır `Microsoft.InfromADUserAccountEnabled.xxx` doğru olanı seçmek için bağlantı.

    > [!Note]
    > Sonucunda bu mantık, için parola geri yazma, Azure AD Connect çalışmak için birincil etki alanı denetleyicisi (PDC) öykünücüsü ile iletişim kurabildiğini olması gerekir. Bu el ile etkinleştirmeniz gerekirse, Azure AD Connect için PDC öykünücüsü bağlanabilirsiniz. Sağ **özellikleri** Active Directory eşitleme bağlayıcısının ardından **dizin bölümlerini Yapılandır**. Buradan arayın **etki alanı denetleyicisi bağlantı ayarları** bölümünde ve kutucuğu seçin **yalnızca tercih edilen etki alanı denetleyicileri kullanmak**. Tercih edilen etki alanı denetleyicisi PDC öykünücüsü değilse bile Azure AD Connect parola geri yazma için PDC'ye bağlanma girişiminde bulunur.

8. Kullanıcı hesabı bulunduğunda, sizi doğrudan uygun Active Directory ormanındaki parola sıfırlama girişimi.
9. Parola ayarlama işlemi başarılı olursa, biz kullanıcının parolasını değiştirdiğinden söyleyin.
    > [!NOTE]
    > Kullanıcının parola, Parola Eşitleme'yi kullanarak Azure AD'ye eşitlenmişse, şirket içi parola ilkesini bulut parola ilkesini zayıf bir fırsat yoktur. Bu durumda, biz yine de ne olursa olsun şirket içi İlkesi olduğu ve bunun yerine, parola karması eşitleme için parola karması eşitleme kullanın uygular. Bu ilke, çoklu oturum açma sağlamak için parola eşitleme veya Federasyon kullanıyorsanız şirket içi ilkenizi bulutta olursa olsun uygulanmasını sağlar.

10. Parola işleminin başarısız olması ayarlarsanız, biz kullanıcıya bir hata döndürür ve yeniden denemelerini olanak tanır. İşlemi nedeniyle başarısız olabilir:
    * Hizmet oldu.
    * Seçili parola kuruluşun ilkelerini karşılamadı.
    * Biz kullanıcı yerel Active Directory'de gelmeyebilir.

    Biz, belirli bir ileti birçok durumda olması ve sorunu çözmek için yapabileceklerini kullanıcıya bildirin.

## <a name="configure-password-writeback"></a>Parola geri yazmayı yapılandırın

Otomatik güncelleştirme özelliğini kullanmanızı öneririz [Azure AD Connect](./../connect/active-directory-aadconnect-get-started-express.md) parola geri yazma özelliğini kullanmak istiyorsanız.

DirSync ve Azure AD Sync artık parola geri yazma özelliğini etkinleştirmek için bir yol olarak desteklenir. Geçişinizi ile yardımcı olmak daha fazla bilgi için bkz. [DirSync ve Azure AD eşitleme ' yükseltme](../connect/active-directory-aadconnect-dirsync-deprecated.md).

Zaten yapılandırdığınız Azure AD Connect, ortamınızda kullanarak aşağıdaki adımları varsayar [Express](./../connect/active-directory-aadconnect-get-started-express.md) veya [özel](./../connect/active-directory-aadconnect-get-started-custom.md) ayarları.

1. Parola geri yazma özelliğini etkinleştirmek ve yapılandırmak için Azure AD Connect sunucusu için oturum açın ve başlangıç **Azure AD Connect** Yapılandırma Sihirbazı.
2. Üzerinde **Hoş Geldiniz** sayfasında **yapılandırma**.
3. Üzerinde **ek görevler** sayfasında **eşitleme seçeneklerini özelleştirme**ve ardından **sonraki**.
4. Üzerinde **Azure ad Connect** sayfasında genel yönetici kimlik bilgilerini girin ve ardından **sonraki**.
5. Üzerinde **dizinleri Bağla** ve **etki alanı/OU** sayfa filtresi seçin **sonraki**.
6. Üzerinde **isteğe bağlı özellikler** sayfasında, yanındaki kutuyu işaretleyin **parola geri yazma** seçip **sonraki**.
   ![Azure AD CONNECT'te parola geri yazmayı etkinleştirme][Writeback]
7. Üzerinde **yapılandırma için hazır** sayfasında **yapılandırma** işleminin tamamlanması için bekleyin.
8. Son yapılandırma gördüğünüzde, seçmek **çıkış**.

Parola geri yazma için ilgili genel sorun giderme görevleri görmek için bölüm [parola geri yazma sorunlarını gidermek](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) sorun giderme makalemizde.

## <a name="active-directory-permissions"></a>Active Directory izinleri

Belirtilen hesaba SSPR kapsamında olmasını istiyorsanız Azure AD Connect yardımcı programında şu öğeleri ayarlamanız gerekir:

* **Parola sıfırlama** 
* **Parola değiştirme** 
* **Yazma izinleri** üzerinde `lockoutTime`  
* **Yazma izinleri** üzerinde `pwdLastSet`
* **Hakları genişletilmiş** ya da üzerinde:
   * Kök nesnenin *her etki alanı* o ormandaki
   * SSPR kapsamında olmasını istediğiniz kullanıcı kuruluş birimi (OU)

Emin değilseniz ne açıklanan hesabın hesap, Azure Active Directory Connect yapılandırma kullanıcı arabirimini açın ve seçin başvurduğu **geçerli yapılandırmayı görüntüleme** seçeneği. Altında listelendiğini izni eklemek istediğiniz hesap **eşitlenen dizinler**.

Bu izinleri ayarlarsanız, her ormana yönelik MA hizmet hesabının bu orman içindeki kullanıcı hesapları adına parolaları yönetebilirsiniz. 

> [!IMPORTANT]
> Bu izinleri atamazsanız geri yazma doğru yapılandırılmış gibi görünüyor olsa bile kullanıcılar buluttan şirket içi parolalarını yönetme girişiminde bulunduğunuzda daha sonra kullanıcılar hataları karşılaşır.
>

> [!NOTE]
> Bu bir saat veya daha fazla bilgi için dizininizdeki tüm nesneler için çoğaltılması için bu izinlere kadar sürebilir.
>

Parola geri yazmanın gerçekleşmesini sağlamak için uygun izinleri ayarlamak için aşağıdaki adımları tamamlayın:

1. Active Directory Kullanıcıları ve bilgisayarları uygun etki alanı yönetim izinleri olan bir hesap ile açın.
2. Gelen **görünümü** menüsünde emin **Gelişmiş Özellikler** açıktır.
3. Sol bölmede bulunan seçin ve etki alanı kökünde temsil eden nesneye sağ tıklayın **özellikleri** > **güvenlik** > **Gelişmiş**.
4. Gelen **izinleri** sekmesinde **Ekle**.
5. İzinler (Azure AD Connect kurulumunun) uygulanmakta olan bir hesabı seçin.
6. İçinde **uygulandığı** aşağı açılan listesinden **Descendent kullanıcı** nesneleri.
7. Altında **izinleri**, aşağıdakileri seçin:
    * **Parola sıfırlama**
    * **Parola değiştirme**
    * **LockoutTime yazma**
    * **PwdLastSet yazma**
8. Seçin **Uygula/Tamam** değişiklikleri uygulamak ve açık bir iletişim kutusu çıkın.

## <a name="licensing-requirements-for-password-writeback"></a>Parola geri yazma için lisans gereksinimleri

Lisanslama hakkında daha fazla bilgi için bkz. [parola geri yazma için lisans gerekli](concept-sspr-licensing.md) veya aşağıdaki siteleri:

* [Site fiyatlandırma Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-that-are-supported-for-password-writeback"></a>Parola geri yazma için desteklenen şirket içi kimlik doğrulama modları

Aşağıdaki kullanıcı parola türleri için parola geri yazma desteği:

* **Yalnızca bulut kullanıcılarına**: parola geri yazma şirket içi parola olduğundan bu durumda, uygulanmaz.
* **Kullanıcıları parola eşitlenmiş**: parola geri yazma desteklenir.
* **Federe kullanıcılar**: parola geri yazma desteklenir.
* **Geçişli kimlik doğrulaması kullanıcılar**: parola geri yazma desteklenir.

### <a name="user-and-admin-operations-that-are-supported-for-password-writeback"></a>Parola geri yazma için desteklenen kullanıcı ve yönetici işlemleri

Parolalar, aşağıdaki durumlarda geri yazılır:

* **Desteklenen son kullanıcı işlemleri**
  * Tüm son kullanıcı Self Servis gönüllü parola işlemi Değiştir
  * Tüm son kullanıcı Self Servis zorla parola işlemi, örneğin, parola sona erme süresini değiştirme
  * Tüm son kullanıcı Self Servis parola sıfırlama kaynaklanan [parola sıfırlama portalı](https://passwordreset.microsoftonline.com)
* **Desteklenen yönetici işlemleri**
  * Bir yönetici Self Servis gönüllü parola işlemi Değiştir
  * Tüm yönetici zorla Self Servis parola işlemi, örneğin, parola sona erme değiştirme
  * Bir yönetici Self Servis parola sıfırlama kaynaklanan [parola sıfırlama portalı](https://passwordreset.microsoftonline.com)
  * Tüm son kullanıcı yönetici tarafından başlatılan parola gelen sıfırlama [Azure portalı](https://portal.azure.com)

### <a name="user-and-admin-operations-that-are-not-supported-for-password-writeback"></a>Parola geri yazma için desteklenen kullanıcı ve yönetici işlemleri

Parolalar *değil* aşağıdaki durumlarda hiçbirinde yazılır:

* **Desteklenmeyen son kullanıcı işlemleri**
  * PowerShell sürüm 1, sürüm 2 veya Azure AD Graph API'yi kullanarak kendi parolasını sıfırlama herhangi bir son kullanıcı
* **Desteklenmeyen yönetici işlemleri**
  * Tüm son kullanıcı yönetici tarafından başlatılan parola gelen sıfırlama [Office Yönetim Portalı](https://portal.office.com)
  * PowerShell sürüm 1, sürüm 2 veya Azure AD Graph API'si sıfırlama herhangi bir son kullanıcı yönetici tarafından başlatılan parola

Bu kısıtlamaları kaldırmak için çalışıyoruz, ancak biz size henüz paylaşabilirsiniz belirli bir zaman çizelgesi yok.

## <a name="password-writeback-security-model"></a>Parola geri yazma güvenlik modeli

Parola geri yazma özelliğini bir yüksek oranda güvenli bir hizmettir. Bilgileriniz korunur emin olmak için aşağıda açıklandığı bir dört katmanlı güvenlik modeli etkinleştiririz:

* **Kiracıya özgü service bus geçişi**
  * Hizmet ayarlarsanız, size Microsoft hiçbir zaman erişimi olan rastgele oluşturulmuş güçlü bir parola ile korunan bir kiracıya özgü service bus geçişi ayarlayın.
* **Kilitli, şifreleme açısından güçlü bir parola şifreleme anahtarı**
  * Service bus geçişi oluşturulduktan sonra kablo üzerinden gelen parolayı şifrelemek için kullandığımız güçlü bir simetrik anahtar oluştururuz. Bu anahtar yalnızca yoğun kilitli ve denetlenen dizindeki başka bir parola olduğu gibi bulutta şirketinizin gizli dizi deposu içinde bulunur.
* **Sektörde standart Aktarım Katmanı Güvenliği (TLS)**
  1. Parola sıfırlama veya değiştirme işlemi bulutta oluşur, düz metin parolayı almak ve ortak anahtarınızı şifrelemek.
  2. Biz, şifrelenmiş bir kanal üzerinden Microsoft SSL sertifikaları için service bus Geçişi'ni kullanarak gönderilen bir HTTPS iletisi yerleştirin.
  3. Service bus İleti ulaştıktan sonra şirket içi aracınızı uyanır ve service bus-daha önce oluşturulmuş güçlü bir parola kullanarak kimliğini doğrular.
  4. Şirket içi aracı şifreli ileti seçer ve biz oluşturulan özel anahtarı kullanarak şifresini çözer.
  5. Şirket içi aracı AD DS SetPassword API'sinden parola ayarlama girişiminde bulunur. Bu adım ne kurmamızı (örneğin, karmaşıklık, yaş, geçmiş ve filtreleri) Active Directory şirket içi parola ilkenizi zorunlu tutmak bulutun içinde bulunuyor.
* **İleti süre sonu ilkeleri** 
  * Şirket içi hizmetiniz çalışmadığı için service bus ileti bulunur, zaman aşımına uğrar ve birkaç dakika sonra kaldırılır. Zaman aşımı ve iletinin kaldırılması ve güvenliği daha da artırır.

### <a name="password-writeback-encryption-details"></a>Parola geri yazma şifreleme ayrıntıları

Kullanıcı parola sıfırlama gönderdikten sonra şirket içi ortamınızda gelmeden önce sıfırlama isteği şifreleme adımları boyunca gider. Şifreleme adımları en yüksek hizmet güvenilirliği ve güvenliği emin olun. Bunlar aşağıda açıklanmıştır:

* **1. adım: Parola şifrelemesini 2048 bit RSA anahtarı ile**: bir kullanıcı şirket içine geri yazılması için bir parola gönderdikten sonra gönderilen parola 2048 bit RSA anahtarıyla şifrelenir.
* **2. adım: Paket düzeyinde şifreleme, AES gcm'ye**: tüm paketi, parola ve gerekli meta veriler, AES-GCM kullanılarak şifrelenir. Bu şifreleme, doğrudan erişimi olan herkes görüntülemesini veya içeriğiyle oynama, temel alınan ServiceBus kanala engeller.
* **3. adım: Tüm iletişimi TLS/SSL üzerinden gerçekleşir**: ServiceBus ile tüm iletişimin bir SSL/TLS kanalda olur. Bu şifreleme, içeriği yetkisiz Üçüncü taraflardan güvenliğini sağlar.
* **Otomatik anahtar geçişi altı ayda**: her altı aylık veya her zaman parola geri yazmayı devre dışı ve ardından Azure AD Connect'i yeniden etkinleştirildi. Biz otomatik olarak en yüksek hizmet güvenlik ve güvenilirlik sağlamak için tüm anahtarları.

### <a name="password-writeback-bandwidth-usage"></a>Parola geri yazma bant genişliği kullanımı

Parola geri yazmayı yalnızca aşağıdaki durumlarda şirket içi aracı dön istekleri gönderen düşük bant genişliğine sahip bir hizmettir:

* Bu özellik etkin veya Azure AD Connect devre dışı olduğunda iki ileti gönderilir.
* Hizmet çalışıyor olduğu sürece her beş dakikada bir hizmet sinyali olarak bir ileti gönderilir.
* İki ileti gönderilen her zaman yeni bir parola gönderildi:
    * İlk ileti işlemi gerçekleştirmek için isteğidir.
    * İkinci bir ileti, işlemin sonucunu içerir ve aşağıdaki durumlarda gönderilir:
        * Her bir kullanıcı Self Servis parola sıfırlama sırasında yeni bir parola gönderilir.
        * Her bir kullanıcı parolasını değiştirme işlemi sırasında yeni bir parola gönderilir.
        * Her seferinde yeni bir parola sıfırlama (yalnızca Azure yönetim portallarında) bir yönetici tarafından başlatılan kullanıcı parolası sırasında gönderilir.

#### <a name="message-size-and-bandwidth-considerations"></a>İleti boyutu ve bant genişliği konuları

Genellikle her biri daha önce açıklanan ileti boyutu altında 1 KB ' dir. Aşırı yük altında bile, parola geri yazma hizmetinin kendisi birkaç kilobit / sn bant genişliği tüketiyor. Her ileti gerçek zamanlı olarak gönderdiğinden parola güncelleştirme işlemi tarafından yalnızca gerekli olduğunda ve ileti boyutu kadar küçük olduğu için geri yazma özelliği bant genişliği kullanımını ölçülebilir bir etkisi için çok küçük.

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md).
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Azure AD CONNECT'te parola geri yazmayı etkinleştirme"
