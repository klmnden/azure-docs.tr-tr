---
title: Bir Microsoft uygulamasında oturum açmada sorun | Microsoft Docs
description: Birinci taraf Microsoft Applications (Office 365 gibi) Azure AD kullanarak oturum açarken genişlettiklerinde karşılaştığı yaygın sorunları giderme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 256ca5c2f26a6bac6bdfd09e4dd6294ec5a569ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60292206"
---
# <a name="problems-signing-in-to-a-microsoft-application"></a>Bir Microsoft uygulamasında oturum açma sorunları

Microsoft Applications (örneğin, Office 365 Exchange, SharePoint, Yammer, vb.) atanmış ve yönetilen biraz farklı 3 taraf SaaS uygulamaları ya da diğer uygulamalar üzerinde çoklu oturum açma için Azure AD ile tümleştirin.

Bir kullanıcı Microsoft tarafından yayımlanan bir uygulamaya erişim elde edebilirsiniz üç ana yolu vardır.

-   Office 365 veya diğer Ücretli paketleri için uygulamalar, kullanıcılar ile erişim izni verilen **lisans ataması** bizim grup tabanlı lisans atama özelliği kullanarak bir grup veya kullanıcı hesabı için doğrudan.

-   Microsoft veya üçüncü taraf yayımlar uygulamalar için ücretsiz olarak herkes için kullanıcılar erişim aracılığıyla verilebilir **kullanıcı onayı**. Bu, uygulamanın Azure AD iş veya Okul hesabıyla oturum açın ve kendi hesapta bazı sınırlı bir veri kümesi erişimi izin anlamına gelir.

-   Microsoft veya bir 3. taraf yayımlar uygulamalar için ücretsiz olarak herkes için kullanıcılar ayrıca erişim aracılığıyla verilebilir **yönetici onayı**. Başka bir deyişle, bir yönetici tarafından kuruluşunuzdaki herkes uygulamaya bir genel yönetici hesabıyla oturum açın ve kuruluşunuzdaki herkes için erişim vermek için uygulama kullanılabilir belirledi.

İle sorununuzu gidermek için Başlat [uygulama erişimi, dikkate alınması gereken genel sorun alanlarından](#general-problem-areas-with-application-access-to-consider) ve Kılavuzu okuyun: Microsoft Application erişim ayrıntıları almak için sorun giderme adımları.

## <a name="general-problem-areas-with-application-access-to-consider"></a>Genel sorun alanlarından dikkate alınması gereken uygulama erişimi

Nereden başlayacağınızı hakkında bir fikir varsa, ayrıntılarına ulaşabilirsiniz genel sorunlu alanları listesi aşağıda verilmiştir, ancak hızlı bir şekilde kullanmaya başlamak için adım adım kılavuzun okuma öneririz: Çözüm: Microsoft Application erişim sorunlarını giderme adımları.

-   [Kullanıcı hesabı ile ilgili sorunlar](#problems-with-the-users-account)

-   [Grupları ile ilgili sorunlar](#problems-with-groups)

-   [Koşullu erişim ilkeleri ile ilgili sorunlar](#problems-with-conditional-access-policies)

-   [Uygulama onay ile ilgili sorunlar](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a>Microsoft Application erişim sorunlarını giderme adımları

Kullanıcılarının bir Microsoft uygulamasında oturum açamıyorum istemeyenler içine çalışmadığında bazı yaygın sorunlar aşağıda verilmiştir.

- İlk denetlenecek genel sorunlar

  * Emin kullanıcının oturum açmak için **URL'yi düzeltin** ve yerel uygulama URL'si değil.

  * Kullanıcı hesabı olduğundan emin olun **kilitli değil.**

  * Emin **kullanıcı hesabının var** Azure Active Directory'de. [Bir kullanıcı hesabı Azure Active Directory'de mevcut olup olmadığını denetleyin](#problems-with-the-users-account)

  * Kullanıcı hesabı olduğundan emin olun **etkin** için oturum açmalar. [Kullanıcı hesabınızın durumunu denetleyin](#problems-with-the-users-account)

  * Kullanıcının emin **parola süresi değil veya unutulursa.** [Bir kullanıcının parolasını sıfırlama](#reset-a-users-password) veya [Self Servis parola sıfırlamayı etkinleştirin](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

  * Emin **multi-Factor Authentication** kullanıcı erişimi engellemediğini. [Bir kullanıcının çok faktörlü kimlik doğrulama durumu kontrol](#check-a-users-multi-factor-authentication-status) veya [bir kullanıcının kimlik doğrulaması iletişim bilgileri kontrol edin](#check-a-users-authentication-contact-info)

  * Emin bir **koşullu erişim ilkesi** veya **kimlik koruması** İlkesi, kullanıcı erişimini engellemediğinden. [Belirli bir koşullu erişim ilkesi denetleyin](#problems-with-conditional-access-policies) veya [belirli bir uygulamanın koşullu erişim ilkesini denetleme](#check-a-specific-applications-conditional-access-policy) veya [özel koşullu erişim ilkesi devre dışı bırak](#disable-a-specific-conditional-access-policy)

  * Emin olun kullanıcının **kimlik doğrulaması iletişim bilgileri** yürütülebilmesi çok faktörlü kimlik doğrulaması veya koşullu erişim ilkelerinin izin güncel olduğundan. [Bir kullanıcının çok faktörlü kimlik doğrulama durumu kontrol](#check-a-users-multi-factor-authentication-status) veya [bir kullanıcının kimlik doğrulaması iletişim bilgileri kontrol edin](#check-a-users-authentication-contact-info)

- İçin **Microsoft** **Lisansı gerektiren uygulamalar** (Office 365 gibi), yukarıdaki genel sorunlardan çizgili sonra denetlemek için belirli bazı sorunlar şunlardır:

  * Bir kullanıcı sağlayın ya da sahip bir **lisansı atandı.** [Bir kullanıcının lisans atanmış denetleyin](#check-a-users-assigned-licenses) veya [lisans atanmış Grup denetleyin](#check-a-groups-assigned-licenses)

  * Lisans ise **atanan bir** **statik grup**, emin **kullanıcının üye olduğu** o grubun. [Bir kullanıcının grup üyeliklerini denetle](#check-a-users-group-memberships)

  * Lisans ise **atanan bir** **dinamik grup**, emin **dinamik Grup Kuralı düzgün şekilde ayarlandığını**. [Dinamik grup üyeliği ölçütlerini denetleyin](#check-a-dynamic-groups-membership-criteria)

  * Lisans ise **atanan bir** **dinamik grup**, dinamik Grup olduğundan emin olun **işleme tamamlandı** üyeliğini ve bu **kullanıcının üye olduğu**  (Bu işlem biraz zaman alabilir). [Bir kullanıcının grup üyeliklerini denetle](#check-a-users-group-memberships)

  *  Lisans atanmış olduğundan emin olun, sonra lisans olduğundan emin olun **süresinin dolmadığını**.

  *  Lisans olduğundan emin olun **uygulamanın** eriştikleri.

- İçin **Microsoft** **lisans gerektirmeyen uygulamalar**, kontrol etmek için diğer işlemlerden bazıları aşağıda verilmiştir:

  * Uygulamanın istediği, **kullanıcı düzeyi izinleri** (örneğin "Bu kullanıcıların posta kutularına erişim"), kullanıcı uygulamada oturum açtığını emin olun ve gerçekleştirdiği bir **kullanıcıdüzeyionayıişlemi** kendi veri erişim izin vermek için.

  * Uygulamanın istediği, **yönetici düzeyi izinlerini** (örneğin "tüm kullanıcıların posta kutularına erişim"), genel yönetici yürüttü emin olun bir **yönetici düzeyi onayı işlemi tüm kullanıcılar adına** kuruluştaki.

## <a name="problems-with-the-users-account"></a>Kullanıcı hesabı ile ilgili sorunlar

Uygulama erişimi, uygulamaya atanmış bir kullanıcı ile ilgili bir sorun nedeniyle engellenebilir. Sorun giderme ve kullanıcıları ve hesap ayarlarına çözmekte bazı yollar şunlardır:

-   [Bir kullanıcı hesabı Azure Active Directory'de mevcut olup olmadığını denetleyin](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Kullanıcı hesabınızın durumunu denetleyin](#check-a-users-account-status)

-   [Bir kullanıcının parolasını sıfırlama](#reset-a-users-password)

-   [Self servis parola sıfırlamayı etkinleştirme](#enable-self-service-password-reset)

-   [Bir kullanıcının çok faktörlü kimlik doğrulaması durumunu denetleyin](#check-a-users-multi-factor-authentication-status)

-   [Bir kullanıcının kimlik doğrulaması iletişim bilgileri kontrol edin](#check-a-users-authentication-contact-info)

-   [Bir kullanıcının grup üyeliklerini denetle](#check-a-users-group-memberships)

-   [Bir kullanıcının atanan lisansları denetleme](#check-a-users-assigned-licenses)

-   [Bir kullanıcı bir lisans atayın](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Bir kullanıcı hesabı Azure Active Directory'de mevcut olup olmadığını denetleyin

Bir kullanıcı hesabının mevcut olup olmadığını denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  Veri içermeyen eksik ve beklediğiniz gibi göründüğünü emin olmak için kullanıcı nesnesinin özelliklerini denetleyin.

### <a name="check-a-users-account-status"></a>Kullanıcı hesabınızın durumunu denetleyin

Bir kullanıcı hesabı durumunu denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **profili**.

8.  Altında **ayarları** emin **oturum açmayı engelle** ayarlanır **Hayır**.

### <a name="reset-a-users-password"></a>Bir kullanıcının parolasını sıfırlama

Bir kullanıcının parolasını sıfırlamak için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **parolayı Sıfırla** kullanıcı bölmenin üstünde düğme.

8.  tıklayın **parolayı Sıfırla** düğmesini **parolayı Sıfırla** bölmesi görünür.

9.  Kopyalama **geçici parola** veya **yeni bir parola girin** kullanıcı.

10. Bu yeni parolayı kullanıcıya iletişim, Azure Active Directory'ye sonraki oturum açma sırasında bu parolayı değiştirmeniz gerekiyor.

### <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

Self Servis parola sıfırlamayı etkinleştirmek için aşağıdaki dağıtım adımları izleyin:

-   [Azure Active Directory parolalarını sıfırlamalarına olanak tanıma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

-   [Kullanıcılara sıfırlama veya Active Directory şirket içi parolalarını değiştirme olanağı](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

### <a name="check-a-users-multi-factor-authentication-status"></a>Bir kullanıcının çok faktörlü kimlik doğrulaması durumunu denetleyin

Bir kullanıcının çok faktörlü kimlik doğrulaması durumunu denetlemek için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm kullanıcılar**.

6. tıklayın **multi-Factor Authentication** bölmenin üstünde düğme.

7. Bir kez **multi-Factor Authentication Yönetim Portalı** yüklendiğinde olun, bulunduğunuz **kullanıcılar** sekmesi.

8. Kullanıcı, arama, filtreleme veya sıralama kullanıcılar listesinde bulun.

9. Kullanıcı listesinden kullanıcıyı seçin ve **etkinleştirme**, **devre dışı**, veya **zorla** istediğiniz gibi çok faktörlü kimlik doğrulaması.

   * **Not**: Bir kullanıcı olarak bulunuyorsa bir **zorlanan** durumunda ayarlayın bunları **devre dışı bırakılmış** geçici olarak geri hesaba izin vermek için. Daha sonra geri içinde bulundukları sonra durumlarına değiştirebilirsiniz **etkin** kişi bilgileri, sonraki oturum açma sırasında yeniden kaydolmak için bunları yeniden istemek için. Alternatif olarak, adımları izleyebilirsiniz [bir kullanıcının kimlik doğrulaması iletişim bilgileri kontrol](#check-a-users-authentication-contact-info) doğrulamak veya bunlar için bu veri kümesi için.

### <a name="check-a-users-authentication-contact-info"></a>Bir kullanıcının kimlik doğrulaması iletişim bilgileri kontrol edin

Çok faktörlü kimlik doğrulaması, koşullu erişim, kimlik koruması ve parola sıfırlama için kullanılan kullanıcı kimlik doğrulaması iletişim bilgileri denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **profili**.

8.  Ekranı aşağı kaydırarak **kimlik doğrulaması iletişim bilgileri**.

9.  **Gözden geçirme** veri, güncelleştirme ve kullanıcı için gerektiği şekilde kayıtlı.

### <a name="check-a-users-group-memberships"></a>Bir kullanıcının grup üyeliklerini denetle

Bir kullanıcının grup üyeliklerini denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **grupları** olduğu kullanıcı grupları görmek için bir üyesidir.

### <a name="check-a-users-assigned-licenses"></a>Bir kullanıcının atanan lisansları denetleme

Bir kullanıcının lisans atanmış denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

### <a name="assign-a-user-a-license"></a>Bir kullanıcı bir lisans atayın 

Bir kullanıcıya bir lisans atamak için bu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7.  tıklayın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

8.  Tıklayın **atama** düğmesi.

9.  Seçin **bir veya daha çok ürünlerin** kullanılabilir ürünler listesinden.

10. **İsteğe bağlı** tıklayın **atama seçenekleri** hedefle ürünleri atamak için öğesi. Tıklayın **Tamam** bu ne zaman tamamlanır.

11. Tıklayın **atama** bu lisans bu kullanıcıya atamak için düğme.

## <a name="problems-with-groups"></a>Grupları ile ilgili sorunlar

Uygulama erişimi, uygulamaya atanmış bir gruba bir sorun nedeniyle engellenebilir. Sorun giderme ve gruplar ve grup üyelikleri ile çözmekte bazı yollar şunlardır:

-   [Bir grubun üyeliğini denetleyin](#check-a-groups-membership)

-   [Dinamik grup üyeliği ölçütlerini denetleyin](#check-a-dynamic-groups-membership-criteria)

-   [Bir grubun atanmış lisansları denetleme](#check-a-groups-assigned-licenses)

-   [Bir grup lisansları yeniden işle](#reprocess-a-groups-licenses)

-   [Bir gruba lisans atama](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Bir grubun üyeliğini denetleyin

Bir grubun üyeliğini denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubunun ve **satıra tıklayın** seçin.

7.  tıklayın **üyeleri** bu gruba atanan kullanıcılar listesini gözden geçirebilirsiniz.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Dinamik grup üyeliği ölçütlerini denetleyin 

Dinamik grubun Üyelik ölçütleri denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubunun ve **satıra tıklayın** seçin.

7.  tıklayın **dinamik Üyelik kuralları.**

8.  Gözden geçirme **basit** veya **Gelişmiş** bu grup için tanımlanan kuralı ve bu grubun bir üyesi olmasını istediğiniz kullanıcıyı bu ölçütleri karşıladığından emin olun.

### <a name="check-a-groups-assigned-licenses"></a>Bir grubun atanmış lisansları denetleme

Lisans atanmış Grup denetlemek için şu adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklayın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubunun ve **satıra tıklayın** seçin.

7.  tıklayın **lisansları** , şu anda Grup lisansları görmek üzere atanır.

### <a name="reprocess-a-groups-licenses"></a>Bir grup lisansları yeniden işle

Bir grubun atanmış lisansları yeniden işle için bu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm grupları**.

6. **Arama** ilgilendiğiniz grubunun ve **satıra tıklayın** seçin.

7. tıklayın **lisansları** , şu anda Grup lisansları görmek üzere atanır.

8. tıklayın **suretiyle** düğmesini bu grubun üyelerine atanan lisansları'nın güncel olduğundan emin olun. Bu, boyutu ve karmaşıklığı grubunun bağlı olarak uzun zaman alabilir.

   >[!NOTE]
   >Daha hızlı bir şekilde bunun için geçici olarak bir lisans kullanıcıya doğrudan atamayı göz önünde bulundurun. [Bir kullanıcıya bir lisans atamanız](#problems-with-application-consent).
   >
   >

### <a name="assign-a-group-a-license"></a>Bir gruba lisans atama

Bir gruba lisans atamak için bu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm grupları**.

6. **Arama** ilgilendiğiniz grubunun ve **satıra tıklayın** seçin.

7. tıklayın **lisansları** , şu anda Grup lisansları görmek üzere atanır.

8. Tıklayın **atama** düğmesi.

9. Seçin **bir veya daha çok ürünlerin** kullanılabilir ürünler listesinden.

10. **İsteğe bağlı** tıklayın **atama seçenekleri** hedefle ürünleri atamak için öğesi. Tıklayın **Tamam** bu ne zaman tamamlanır.

11. Tıklayın **atama** bu gruba bu lisansları atamak için düğme. Bu, boyutu ve karmaşıklığı grubunun bağlı olarak uzun zaman alabilir.

    >[!NOTE]
    >Daha hızlı bir şekilde bunun için geçici olarak bir lisans kullanıcıya doğrudan atamayı göz önünde bulundurun. [Bir kullanıcıya bir lisans atamanız](#problems-with-application-consent).
    > 
    >

## <a name="problems-with-conditional-access-policies"></a>Koşullu erişim ilkeleri ile ilgili sorunlar

### <a name="check-a-specific-conditional-access-policy"></a>Belirli bir koşullu erişim ilkesi denetleyin

Denetleyin ya da tek bir koşullu erişim ilkesi doğrulamak için:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Gezinti menüsünde.

5. tıklayın **koşullu erişim** Gezinti öğesi.

6. İnceleme ilgilendiğiniz ilkeye tıklayın.

7. Hiç belirli bir koşul, atamaları veya kullanıcı erişimini engelliyor olabilecek diğer ayarları gözden geçirin.

   >[!NOTE]
   >Geçici olarak, olmayan etkileyen emin olmak için bu ilkeyi devre dışı bırakmak isteyebilirsiniz oturum açmalar. Bunu yapmak için ayarlanmış **ilkesini etkinleştir** geç **yok** tıklatıp **Kaydet** düğmesi.
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Belirli bir uygulamanın koşullu erişim ilkesini denetleme

Denetleyin ya da tek bir uygulamanın şu anda doğrulamak için koşullu erişim ilkesi yapılandırmış:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Gezinti menüsünde.

5.  tıklayın **tüm uygulamaları**.

6.  Uygulama görünen adı veya uygulama kimliği ile oturum açmak ilgilendiğiniz uygulama veya kullanıcı için arama çalışıyor.

     >[!NOTE]
     >Aradığınız uygulamayı görmüyorsanız tıklayın **filtre** listeye kapsamını genişletin ve düğme **tüm uygulamaları**. Daha fazla sütun görmek istiyorsanız, tıklayın **sütunları** düğmesini uygulamalarınız için ek ayrıntıları ekleyin.
     >
     >

7.  tıklayın **koşullu erişim** Gezinti öğesi.

8.  İnceleme ilgilendiğiniz ilkeye tıklayın.

9.  Hiç belirli bir koşul, atamaları veya kullanıcı erişimini engelliyor olabilecek diğer ayarları gözden geçirin.

     >[!NOTE]
     >Geçici olarak, olmayan etkileyen emin olmak için bu ilkeyi devre dışı bırakmak isteyebilirsiniz oturum açmalar. Bunu yapmak için ayarlanmış **ilkesini etkinleştir** geç **yok** tıklatıp **Kaydet** düğmesi.
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Belirli bir koşullu erişim ilkesi devre dışı bırak

Denetleyin ya da tek bir koşullu erişim ilkesi doğrulamak için:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Gezinti menüsünde.

5.  tıklayın **koşullu erişim** Gezinti öğesi.

6.  İnceleme ilgilendiğiniz ilkeye tıklayın.

7.  Ayarlayarak bu ilkeyi devre dışı **ilkesini etkinleştir** geç **Hayır** tıklatıp **Kaydet** düğmesi.

## <a name="problems-with-application-consent"></a>Uygulama onay ile ilgili sorunlar

Uygun izne onay işlemi değil yeniden oluştuğundan uygulama erişim engellenebilir. Sorun giderme ve uygulama onay sorunlarını çözmek bazı yollar şunlardır:

-   [Bir kullanıcı düzeyi onayı işlemi gerçekleştirme](#perform-a-user-level-consent-operation)

-   [Herhangi bir uygulama için yönetici düzeyi onayı işlemi gerçekleştirme](#perform-administrator-level-consent-operation-for-any-application)

-   [Tek kiracılı uygulama için yönetici düzeyi onayı gerçekleştirin](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Çok kiracılı bir uygulama için yönetici düzeyi onayı gerçekleştirin](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Bir kullanıcı düzeyi onayı işlemi gerçekleştirme

-   İzinleri isteyen tüm Open ID Connect özellikli bir uygulama için kullanıcı düzeyi onayı için oturum açmış kullanıcı için uygulamanın oturum açma ekranı giderek gerçekleştirir.

-   Bu program aracılığıyla yapmak istiyorsanız, bkz. [tek tek kullanıcı onayı isteyen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>Herhangi bir uygulama için yönetici düzeyi onayı işlemi gerçekleştirme

-   İçin **yalnızca V1 uygulama modelini kullanarak geliştirilen uygulamalar**, yönetici düzeyi onayı ekleyerek gerçekleşmesi için bu zorlayabilirsiniz "**? yönetici komut istemi =\_onay**" sonuna bir uygulamanın oturum URL.

-   İçin **herhangi bir uygulama geliştirilirken V2 uygulama modelini kullanarak**, bu yönetici düzeyi onayı başlığı altında verilen yönergeleri izleyerek gerçekleşmesi için zorunlu kılabilir **bir dizin Yöneticisiizinlereilişkinistek** bölümünü [yönetici onay uç noktası kullanarak](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Tek kiracılı uygulama için yönetici düzeyi onayı gerçekleştirin

-   İçin **tek kiracılı uygulamalar** izinleri (gelenler geliştirdiğiniz veya kuruluşunuzda kendi), istek gerçekleştirebileceğiniz bir **Yönetim düzeyi onayı** tüm adına işlemi Genel yönetici olarak oturum açma ve tıklayarak tarafından kullanıcılar **izinleri verin** üst kısmındaki düğmeye **uygulama kayıt defteri -&gt; tüm uygulamalar -&gt; bir uygulama - seçin&gt; Gerekli izinler** bölmesi.

-   İçin **herhangi bir uygulama geliştirdiğinizde V1 veya V2 uygulama modelini kullanarak**, bu yönetici düzeyi onayı başlığı altında verilen yönergeleri izleyerek gerçekleşmesi için zorunlu kılabilir **bir dizin yönetici izinleri iste**  bölümünü [yönetici onay uç noktası kullanarak](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Çok kiracılı bir uygulama için yönetici düzeyi onayı gerçekleştirin

-   İçin **çok kiracılı uygulamaları** (bir uygulama veya bir üçüncü taraf, Microsoft, geliştiren gibi), izin istemek, gerçekleştirebileceğiniz bir **Yönetim düzeyi onayı** işlemi. Genel yönetici olarak oturum açın ve tıklayarak **izinleri verin** düğmesini **kurumsal uygulamalar -&gt; tüm uygulamalar -&gt; bir uygulama - seçin&gt; izinleri**  bölmesinde (kullanılabilir olan en kısa sürede).

-   Bu yönetici düzeyi onayı başlığı altında verilen yönergeleri izleyerek oluşmasına da zorunlu kılabilir **bir dizin yönetici izinleri istemek** bölümünü [yönetici onay uç noktası kullanarak](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Sonraki adımlar
[Yönetici onay uç noktası kullanma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

