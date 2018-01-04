---
title: Azure AD SSPR'yi parola geri yazma ile | Microsoft Docs
description: "Kullanım Azure AD ve bir şirket içi dizine parolaları geri yazma için Azure AD Connect"
services: active-directory
keywords: "Active directory parola yönetimi, Azure AD parola yönetimi self servis parola sıfırlama"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 12/06/2017
ms.author: joflore
ms.custom: it-pro
<<<<<<< HEAD
ms.openlocfilehash: 8ca760c3f144cda15920dd401c6a8726d3d53da0
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: HT
=======
ms.openlocfilehash: 6dfc3246b210b382665eeef2d638945c91d5b62f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="password-writeback-overview"></a>Parola geri yazma genel bakış

Parola geri yazma ile Azure parolalar, şirket içi Active Directory'ye geri yazma için Active Directory'ye (Azure AD) yapılandırabilirsiniz. Parola geri yazma içi karmaşık Self Servis parola sıfırlama (SSPR) çözümü ayarlamanıza gerek kaldırır ve bulut tabanlı kolay bir yol, kullanıcılarınızın nerede olurlarsa olsunlar şirket içi parolalarını sıfırlamaya sağlar. Parola geri yazma özelliğini bir bileşenidir [Azure Active Directory Connect](./connect/active-directory-aadconnect.md) , etkinleştirilebilir ve Premium geçerli aboneler tarafından kullanılan [Azure Active Directory sürümleri](active-directory-whatis.md).

Parola geri yazma aşağıdaki özellikleri sağlar:

* **Sıfır Gecikmeli geri bildirim sağlar**: parola geri yazma eşzamanlı bir işlem değil. Parolalarını İlkesi karşılamayan veya bırakılamadı sıfırlama veya herhangi bir nedenle değişti, kullanıcılarınızın hemen bildirilir.
* **Destekler parola sıfırlamaları Active Directory Federasyon Hizmetleri (AD FS) veya diğer Federasyon teknolojileri kullanan kullanıcılar için**: Azure AD kiracınıza eşitlenen Federasyon kullanıcısı hesapları sürece sahip parola geri yazma, bunlar olan Şirket içi Active Directory parolalarını buluttan yönetin.
* **Destekler parola sıfırlamaları kullanan kullanıcılar için** [parola karması eşitlemesi](./connect/active-directory-aadconnectsync-implement-password-synchronization.md): parola sıfırlama hizmeti bir eşitlenmiş kullanıcı hesabı için parola karma eşitlemesi etkin olduğunu algıladığında, biz içi hem de bu hesabın sıfırlama ve parola aynı anda bulut.
* **Erişim paneli ve Office 365 destekler parola değişikliklerini**: federe olduğunda veya Biz bu parolaları yerel Active Directory ortamınızı geri yazma süresi dolmuş ya da süresi dolmuş olmayan kullanıcıların parolalarını değiştirmek için gelen parola eşitlenen kullanıcılar.
* **Bir yönetici bunları Azure portalından sıfırlandıktan sonra parola geri yazma destekleyen**: her bir yönetici bir kullanıcının parolasını sıfırlar [Azure portal](https://portal.azure.com), kullanıcının Federasyon ya da eşitlenmiş parola, parola ayarlar yöneticinin yerel Active Directory'de de seçer. Bu işlev Office Yönetim Portalı'nda şu anda desteklenmiyor.
* **Şirket içi Active Directory parola ilkeleri zorunlu tutar**: bir kullanıcının parolasını sıfırlar, biz Biz bu dizine yürütme önce şirket içi Active Directory ilkeniz karşıladığından emin olun. Bu gözden geçirme geçmişi, karmaşıklık, yaş, parola filtreleri ve yerel Active Directory içinde tanımlanan herhangi bir parola kısıtlamaları denetimi içerir.
* **Tüm gelen güvenlik duvarı kurallarını gerektirmeyen**: parola geri yazma, temel alınan bir iletişim kanalı olarak Azure Service Bus geçişini kullanır. Bu özelliğin çalışması, güvenlik duvarında gelen bağlantı noktalarının açmak zorunda değilsiniz.
* **Şirket içi Active Directory'de korunan grupları içinde mevcut kullanıcı hesapları için desteklenmeyen**: korunan grupları hakkında daha fazla bilgi için bkz [korumalı hesapları ve grupları Active Directory'de](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Parola geri yazma nasıl çalışır?

Eşitlenen kullanıcı sıfırlama veya buluttaki parolalarını değiştirmek için gelen bir federe ya da parola karması, aşağıdakiler gerçekleşir:

1. Kullanıcının parola türüne sahip görmek için denetleyin. Parolanın yönetilen şirket içi olduğunu görürseniz:
   * Geri yazma hizmeti çalışır olup olmadığını denetleyin. Hazır ve çalışır durumda, biz devam kullanıcının izin verin.
   * Geri yazma hizmetin çalışır durumda değil, biz kullanıcı parolalarını şimdi sıfırlanamaz bildirin.
2. Ardından, kullanıcı uygun kimlik doğrulama geçitleri geçirir ve ulaştığında **parola sıfırlama** sayfası.
3. Kullanıcı yeni bir parola seçer ve bunu doğrular.
4. Kullanıcı seçtiğinde **gönderme**, biz düz metin parola geri yazma Kurulum işlemi sırasında oluşturulmuş simetrik anahtar ile şifreleme.
5. Parola şifreleme sonrasında biz (ayarladığınız biz de için geri yazma Kurulum işlemi sırasında), Kiracı özgü service bus geçişi için bir HTTPS kanal üzerinden gönderilir bir yükü dahil edin. Bu geçiş, yalnızca şirket içi yüklemenizi bilir rastgele oluşturulmuş bir parola tarafından korunuyor.
6. Hizmet veri yolu İleti ulaştıktan sonra parola sıfırlama uç noktası otomatik olarak uyanır ve bekleyen sıfırlama isteği olduğunu görür.
7. Hizmeti daha sonra kullanıcı için bulut bağlayıcı özniteliğini kullanarak arar. Bu arama başarılı olması:

    * Kullanıcı nesnesi Active Directory Bağlayıcısı alanında bulunması gerekir.
    * Kullanıcı nesnesi karşılık gelen meta veri deposu (MV) nesneye bağlı olması gerekir.
    * Kullanıcı nesnesi ilgili Azure Active Directory Bağlayıcısı nesneye bağlı olması gerekir.
    * Active Directory Bağlayıcısı nesnesinden MV bağlantısını eşitleme kuralına sahip olmalıdır `Microsoft.InfromADUserAccountEnabled.xxx` bağlantıyı. <br> <br>
    Çağrı buluttan geldiğinde, eşitleme altyapısı kullanan **cloudAnchor** Azure Active Directory Bağlayıcısı alanı nesnesini aramak için öznitelik. Sonra bağlantı geri MV nesne izler ve sonra bağlantı geri Active Directory nesnesi izler. Eşitleme altyapısı aynı kullanıcı birden çok Active Directory nesnelerini (Çoklu orman) olabileceğinden kullanır `Microsoft.InfromADUserAccountEnabled.xxx` bağlantı doğru olanı seçin.

    > [!Note]
    > Bu mantığı sonucu olarak için parola geri yazma özelliğini Azure AD Connect çalışmak için birincil etki alanı denetleyicisi (PDC) öykünücüsü ile iletişim kuramıyor olması gerekir. Bu el ile etkinleştirmeniz gerekirse PDC öykünücüsü Azure AD Connect bağlanabilir. Sağ **özellikleri** Active Directory Eşitlemesi bağlayıcının seçip **dizin bölümlerini Yapılandır**. Buradan, Ara **etki alanı denetleyicisi bağlantı ayarları** bölümünde ve kutucuğu işaretleyin **yalnızca tercih edilen etki alanı denetleyicilerinin kullandığı**. Tercih edilen etki alanı denetleyicisi PDC öykünücüsü olsa bile, Azure AD Connect parola geri yazma için PDC bağlanmaya çalışır.

8. Kullanıcı hesabı bulundu, sizi doğrudan uygun Active Directory ormanındaki parola sıfırlama girişimi.
9. Parola ayarlama işlemi başarılı olursa, biz kullanıcı parolalarını değiştirildi söyleyin.
    > [!NOTE]
    > Parola Eşitleme kullanarak Azure AD ile eşitlenen kullanıcının parolası varsa, şirket içi parola ilkesi bulut parola ilkesi zayıf bir fırsat bulunur. Bu durumda, biz yine ne olursa olsun şirket içi İlkesi olduğu ve bunun yerine bu parola karmasını eşitlemek için parola karma eşitlemesi kullanmak uygulayın. Bu ilke, çoklu oturum açma sağlamak için parola eşitleme ya da Federasyon kullanıyorsanız, şirket içi İlkesi bulutta bağımsız uygulandığını sağlar.

10. Parola işlemi başarısız ayarlarsanız, biz hata kullanıcıya döndürür ve bunları yeniden deneyin sağlayabilirsiniz. İşlemi nedeniyle başarısız olabilir:
    * Hizmet oldu.
    * Seçili parola kuruluşun ilkelerini karşılamadı.
    * Biz kullanıcı yerel Active Directory'de bulamayabilir.

    Biz çoğu bu durumlar için belirli bir ileti vardır ve bu sorunu gidermek için yapabileceklerini kullanıcı söyleyin.

## <a name="configure-password-writeback"></a>Parola geri yazma özelliğini yapılandırın

Otomatik güncelleştirme özelliğini kullanmanızı öneririz [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) parola geri yazma özelliğini kullanmak istiyorsanız.

DirSync ve Azure AD eşitleme artık parola geri yazma özelliğini etkinleştirmek için bir yol desteklenir. Geçiş ile yardımcı olmak daha fazla bilgi için bkz: [yükseltme DirSync ve Azure AD eşitleme](connect/active-directory-aadconnect-dirsync-deprecated.md).

Aşağıdaki adımlarda zaten yapılandırdığınız Azure AD Connect, ortamınızda kullanarak varsayılmaktadır [Express](./connect/active-directory-aadconnect-get-started-express.md) veya [özel](./connect/active-directory-aadconnect-get-started-custom.md) ayarlar.

1. Yapılandırmak ve parola geri yazma özelliğini etkinleştirmek için Azure AD Connect sunucunuza oturum açın ve Başlat **Azure AD Connect** Yapılandırma Sihirbazı.
2. Üzerinde **Hoş Geldiniz** sayfasında, **yapılandırma**.
3. Üzerinde **ek görevleri** sayfasında, **eşitleme seçeneklerini özelleştirme**ve ardından **sonraki**.
4. Üzerinde **Azure ad Connect** sayfasında genel yönetici kimlik bilgilerini girin ve ardından **sonraki**.
5. Üzerinde **dizinleri bağlanmak** ve **etki alanı/OU** sayfaları filtreleme, seçin **sonraki**.
6. Üzerinde **isteğe bağlı özellikler** sayfasında, yanındaki kutuyu işaretleyin **parola geri yazma** seçip **sonraki**.
   ![Azure AD CONNECT'te parola geri yazma etkinleştir][Writeback]
7. Üzerinde **yapılandırma için hazır** sayfasında, **yapılandırma** ve işleminin tamamlanması için bekleyin.
8. Son yapılandırma gördüğünüzde seçin **çıkış**.

Parola geri yazma için genel sorun giderme görevlerini görmek için bölüm [parola geri yazma sorun giderme](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) bizim sorun giderme makalede.

## <a name="active-directory-permissions"></a>Active Directory izinleri

Belirtilen hesaba SSPR kapsamında olmasını istiyorsanız Azure AD Connect yardımcı programında şu öğeler ayarlamanız gerekir:

* **Parola sıfırlama** 
* **Parola değiştirme** 
* **Yazma izinleri** üzerinde`lockoutTime`  
* **Yazma izinleri** üzerinde`pwdLastSet`
* **Hakları genişletilmiş** her iki:
   * Kök nesne *her etki alanı* o ormandaki
   * SSPR kapsamında olmasını istediğiniz kullanıcı kuruluş birimlerini (OU)

Emin değilseniz ne açıklanan hesabın hesap, Azure Active Directory Connect yapılandırma kullanıcı arabirimini açın ve seçin başvurduğu **görünümü geçerli yapılandırması** seçeneği. Altında listelendiğini izni eklemeniz hesabı **eşitlenen dizinleri**.

Bu izinleri ayarlarsanız, her orman için MA hizmet hesabının bu orman içindeki kullanıcı hesapları adına parolaları yönetebilirsiniz. 

> [!IMPORTANT]
> Bu izinleri atamazsanız geri yazma doğru yapılandırılmış gibi görünüyor olsa bile kullanıcılar buluttan şirket içi parolalarını yönetme girişiminde bulunduğunuzda daha sonra kullanıcılar hataları karşılaşır.
>

> [!NOTE]
> İşlemin bir saat veya bu izinlerin dizininizdeki tüm nesneler için çoğaltılması için daha fazla sürebilir.
>

Parola geri yazmanın gerçekleşmesini sağlamak için gerekli izinleri ayarlamak için aşağıdaki adımları tamamlayın:

1. Active Directory Kullanıcıları ve bilgisayarları uygun etki alanı yönetim izinleri olan bir hesap ile açın.
2. Gelen **Görünüm** menüsünde emin olun **Gelişmiş Özellikler** açıktır.
3. Seçin ve etki alanı kökünü temsil eden nesne sol panelinde sağ **özellikleri** > **güvenlik** > **Gelişmiş**.
4. Gelen **izinleri** sekmesine **Ekle**.
5. İzinler (Azure AD Connect kurulum) uygulanmakta olan hesabı seçin.
6. İçinde **uygulandığı** aşağı açılan listesinden, **Descendent kullanıcı** nesneleri.
7. Altında **izinleri**, aşağıdaki kutularını seçin:
    * **Parola sıfırlama**
    * **Parola değiştirme**
    * **LockoutTime yazma**
    * **PwdLastSet yazma**
8. Seçin **Uygula/Tamam** değişiklikleri uygulayın ve tüm açık iletişim kutularını kapatın.

## <a name="licensing-requirements-for-password-writeback"></a>Parola geri yazma için lisans gereksinimleri

Lisanslama hakkında daha fazla bilgi için bkz: [lisansları parola geri yazma için gereken](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) veya aşağıdaki siteleri:

* [Site fiyatlandırma Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-that-are-supported-for-password-writeback"></a>Şirket içi parola geri yazma için desteklenen kimlik doğrulama modları

Parola geri yazma desteği aşağıdaki kullanıcı parola türleri için:

* **Yalnızca bulut kullanıcıları**: parola geri yazma şirket içi parola olduğundan bu durumda, uygulanmaz.
* **Kullanıcıları parola eşitlenmiş**: parola geri yazma desteklenir.
* **Federe kullanıcılar**: parola geri yazma desteklenir.
* **Doğrudan kimlik doğrulama kullanıcıların**: parola geri yazma desteklenir.

### <a name="user-and-admin-operations-that-are-supported-for-password-writeback"></a>Parola geri yazma için desteklenen kullanıcı ve yönetim işlemleri

Parolalar, aşağıdaki durumlarda geri yazılır:

* **Desteklenen son kullanıcı işlemleri**
  * Tüm son kullanıcı Self Servis gönüllü değiştirme parola işlemi
  * Tüm son kullanıcı Self Servis zorla parola işlemi, örneğin, parola sona erme değiştirme
  * Tüm son kullanıcı Self Servis parola sıfırlama kaynaklanan [parola sıfırlama portalı](https://passwordreset.microsoftonline.com)
* **Desteklenen yöneticisi işlemleri**
  * Tüm yönetici Self Servis gönüllü değiştirme parola işlemi
  * Tüm yönetici Self Servis zorla parola işlemi, örneğin, parola sona erme değiştirme
  * Tüm yönetici Self Servis parola sıfırlama kaynaklanan [parola sıfırlama portalı](https://passwordreset.microsoftonline.com)
  * Gelen sıfırlama herhangi bir yönetici tarafından başlatılan son kullanıcı parola [Azure portalı](https://portal.azure.com)

### <a name="user-and-admin-operations-that-are-not-supported-for-password-writeback"></a>Parola geri yazma için desteklenmeyen kullanıcı ve yönetim işlemleri

Parolalar *değil* geri aşağıdaki durumların herhangi birinde yazabilirsiniz:

* **Desteklenmeyen son kullanıcı işlemleri**
  * PowerShell sürüm 1, sürüm 2 veya Azure AD Graph API kullanarak kendi parolasını sıfırlama herhangi bir son kullanıcı
* **Desteklenmeyen yöneticisi işlemleri**
  * Gelen sıfırlama herhangi bir yönetici tarafından başlatılan son kullanıcı parola [Office Yönetim Portalı](https://portal.office.com)
  * Bir yönetici tarafından başlatılan son kullanıcı parolayı PowerShell sürüm 1, sürüm 2 veya Azure AD Graph API Sıfırla

Bu sınırlamalara kaldırmak için çalışıyoruz ancak biz henüz paylaşabilirsiniz belirli bir zaman çizelgesi bulunmuyor.

## <a name="password-writeback-security-model"></a>Parola geri yazma güvenlik modeli

Parola geri yazma yüksek güvenlikli bir hizmettir. Bilgilerinizi korumalı emin olmak için aşağıda açıklanan dört katmanlı güvenlik modeli etkinleştirme:

* **Kiracı özgü service bus geçişi**
  * Hizmetini ayarlama, biz Microsoft hiçbir zaman erişimi rastgele oluşturulmuş güçlü bir parola tarafından korunan bir kiracıya özgü hizmet veri yolu geçişi ayarlayın.
* **Kilitli ve şifreleme açısından güçlü bir parola şifreleme anahtarı**
  * Service bus geçişi oluşturulduktan sonra kablo üzerinden geldiğinde parolayı şifrelemek için kullanırız güçlü bir simetrik anahtar oluşturuyoruz. Bu anahtar yalnızca yoğun kilitlenir ve dizindeki başka bir parola gibi denetlenen bulutta şirketinizin gizli deposundaki yaşar.
* **Endüstri Standart Aktarım Katmanı Güvenliği (TLS)**
  1. Bir parola sıfırlama veya değiştirme işlemi bulutta oluşur, düz metin parola alın ve, ortak anahtarla şifreleyebilirsiniz.
  2. Biz, service bus geçişi için Microsoft SSL sertifikaları kullanılarak şifrelenmiş bir kanal üzerinden gönderilen bir HTTPS iletisi yerleştirin.
  3. Hizmet veri yolundaki İleti ulaştıktan sonra şirket içi aracınızı uyanır ve hizmet veri yolu için önceden oluşturulan güçlü parola kullanarak kimliğini doğrular.
  4. Şirket içi Aracısı şifrelenmiş iletinin seçer ve biz oluşturulan özel anahtarı kullanarak şifresini çözer.
  5. Şirket içi aracı AD DS SetPassword API'si aracılığıyla parola ayarlamaya çalışır. Bu adım bize (örneğin, karmaşıklık, yaş, geçmiş ve filtreleri) Active Directory şirket içi parola ilkenizi zorunlu tutmanıza olanak tanır, bulutta bağlıdır.
* **İleti sona erme** 
  * Şirket içi hizmetiniz aşağı olduğundan iletiyi hizmet veri yolundaki bulunur, zaman aşımına uğradı ve birkaç dakika sonra kaldırılır. Zaman aşımı ve iletinin kaldırılması ve daha güvenliği artırır.

### <a name="password-writeback-encryption-details"></a>Parola geri yazma şifreleme ayrıntıları

Kullanıcı parola sıfırlama gönderdikten sonra şirket içi ortamınızda gelmeden önce sıfırlama isteği şifreleme adımları boyunca geçer. Şifreleme adımları maksimum hizmet güvenilirlik ve güvenlik emin olun. Bunlar aşağıda açıklanmıştır:

* **1. adım: Parola şifreleme 2048 bit RSA anahtarı ile**: bir kullanıcı şirket içi geri yazılması için bir parola gönderdikten sonra gönderilen parola 2048 bit RSA anahtarıyla şifrelenir.
* **2. adım: Paket düzeyi şifreleme AES-GCM ile**: tüm paketi, parolayı ve gerekli meta veriler, AES-GCM kullanılarak şifrelenir. Bu şifreleme doğrudan erişimi olan herkes için temel alınan ServiceBus kanal görüntüleme veya içerikle oynama engeller.
* **3. adım: TLS/SSL üzerinden tüm iletişimin**: bir SSL/TLS kanalı ServiceBus tüm iletişimi olur. Bu şifreleme içeriği yetkisiz üçüncü tarafların güvenliğini sağlar.
* **Otomatik anahtar geçişi altı ayda**: altı ayda ya da her zaman parola geri yazma özelliğini devre dışı ve Azure AD Connect'i yeniden etkinleştirilebilir. Biz otomatik olarak en fazla hizmeti güvenlik ve güvenliği sağlamak için tüm anahtarlar üzerinden alma.

### <a name="password-writeback-bandwidth-usage"></a>Parola geri yazma bant genişliği kullanımı

Parola geri yazma, yalnızca aşağıdaki durumlarda şirket içi Aracısı dön istekleri gönderir düşük bant genişlikli hizmetidir:

* Özellik etkin veya Azure AD Connect aracılığıyla devre dışı olduğunda iki iletileri gönderilir.
* Hizmetin çalıştığı sürece her beş dakikada bir hizmet sinyalinin olarak tek bir ileti gönderilir.
* İki ileti gönderilen her zaman yeni bir parola gönderildi:
    * İlk iletinin işlemi gerçekleştirmek için isteğidir.
    * İkinci bir ileti işlemin sonucunu içerir ve aşağıdaki durumlarda gönderilir:
        * Her bir kullanıcı Self Servis parola sıfırlama sırasında yeni bir parola gönderilir.
        * Her bir kullanıcı parolasını değiştirme işlemi sırasında yeni bir parola gönderilir.
        * Her bir kullanıcı yönetici tarafından başlatılan parola (yalnızca Azure Yönetim Portalı) sıfırlama sırasında yeni bir parola gönderilir.

#### <a name="message-size-and-bandwidth-considerations"></a>İleti boyutu ve bant genişliği konuları

Her biri daha önce açıklanan ileti boyutu genellikle altında 1 KB'tır. Aşırı yük altında bile, parola geri yazma hizmetin kendisini birkaç kilobit bant genişliğinin tüketilmesine neden olabilir. Her ileti gerçek zamanlı olarak gönderdiğinden parola güncelleştirme işlemi tarafından yalnızca gerekli olduğunda ve ileti boyutu kadar küçük olduğu için geri yazma özelliği bant genişliği kullanımını ölçülebilir bir etkisi için çok küçük.

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisansla ilgili bir sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Azure AD CONNECT'te parola geri yazma etkinleştir"
