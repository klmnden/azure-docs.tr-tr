---
title: "Bir Microsoft uygulaması için oturum açma sorunları | Microsoft Docs"
description: "Birinci taraf Microsoft Azure AD (örneğin, Office 365) kullanarak Applications oturum açarken karşılaştığı yaygın sorunlarını giderme"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 5638434270ee82d2b9737ea8eed8b5a8c62f7121
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## <a name="problems-signing-in-to-a-microsoft-application"></a>Bir Microsoft uygulaması için oturum açma sorunları

Microsoft Applications (örneğin, Office 365 Exchange, SharePoint, Yammer, vb.) atanan ve biraz farklı 3 taraf SaaS uygulamaları veya diğer uygulamalar üzerinde çoklu oturum açma için Azure AD ile tümleştirmek yönetilebilir.

Bir kullanıcı bir Microsoft yayımlanan uygulamaya erişim sağlayabilmek için üç ana yolu vardır.

-   Office 365 veya diğer Ücretli paketlerini uygulamalar için erişim aracılığıyla kullanıcılara verilen **lisans atama** bizim grup tabanlı lisans atama özelliği kullanarak bir grup veya kullanıcı hesapları için doğrudan.

-   Microsoft veya üçüncü taraf yayımlayan uygulamaları için ücretsiz olarak herkes için kullanıcılara üzerinden erişim izni **kullanıcı izni**. This0 anlamına gelir uygulama kendi Azure AD iş veya Okul hesabı ile oturum açın ve kendi hesaplarına bazı sınırlı bir veri kümesi erişiminiz izin verin.

-   Microsoft ya da bir 3. taraf yayımlar uygulamalar için herkes için ücretsiz olarak, kullanıcılar ayrıca aracılığıyla erişimi verilebilir **yönetici izni**. Bu, bir yöneticinin kuruluşunuzdaki herkes tarafından uygulama bir genel yönetici hesabı ile oturum açın ve kuruluştaki herkesin erişim vermek için uygulama kullanılabilir belirledi anlamına gelir.

İle sorunu gidermek için başlangıç [uygulama erişimi dikkate alınması gereken genel sorun alanlarından](#general-problem-areas-with-application-access-to-consider) ve okunur [izlenecek yol: Microsoft Application erişim sorunlarını giderme adımları](#walkthrough-steps-to-troubleshoot-microsoft-application-access) bilgi almak için.

## <a name="general-problem-areas-with-application-access-to-consider"></a>Genel sorun alanlarından dikkate alınması gereken uygulama erişimi

Başlatılacağı konum hakkında bir fikir varsa, ayrıntılarına geçebilir genel sorunlu alanları listesi aşağıdadır ancak hızlı bir şekilde kullanmaya başlamak için izlenecek okumanız önerilir: [izlenecek yol: Microsoft Application erişim sorunlarını giderme adımları](#walkthrough-steps-to-troubleshoot-microsoft-application-access).

-   [Kullanıcı hesabının sorunları](#problems-with-the-users-account)

-   [Grupları sorunları](#problems-with-groups)

-   [Koşullu erişim ilkeleri sorunları](#problems-with-conditional-access-policies)

-   [Uygulama onayı sorunları](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a>Microsoft Application erişim sorunlarını giderme adımları

Bir Microsoft uygulaması, kullanıcılar oturum açamaz aşağıda bazı yaygın sorunlar terimleri içine çalıştırılır.

-   İlk denetlemek için genel sorunlar

  * Emin olun kullanıcı olduğundan oturum açma için **URL'yi düzeltin** ve bir yerel uygulama URL değil.

  * Kullanıcının hesabı olduğundan emin olun **kilitli değil.**

  * Emin olun **kullanıcının hesabı zaten** Azure Active Directory'de. [Bir kullanıcı hesabı Azure Active Directory içinde olup olmadığını kontrol edin](#problems-with-the-users-account)

  * Kullanıcının hesabı olduğundan emin olun **etkin** için oturum açmalar. [Bir kullanıcının hesap durumunu denetle](#problems-with-the-users-account)

  * Kullanıcının emin olun **parola süresi değil veya unutulursa.** [Bir kullanıcının parolasını sıfırlamak](#reset-a-users-password) veya [Self Servis parola sıfırlama etkinleştirme](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started)

   * Emin olun **çok faktörlü kimlik doğrulaması** kullanıcı erişimini engellemediğinden. [Bir kullanıcının çok faktörlü kimlik doğrulama durumunu denetleme](#check-a-users-multi-factor-authentication-status) veya [bir kullanıcının kimlik doğrulaması ile ilgili kişi bilgileri](#check-a-users-authentication-contact-info)

   * Emin olun bir **koşullu erişim ilkesi** veya **kimlik koruması** İlkesi kullanıcı erişimini engellemediğinden. [Belirli bir koşullu erişim ilkesini denetleme](#problems-with-conditional-access-policies) veya [belirli bir uygulamanın koşullu erişim ilkesini denetleme](#check-a-specific-applications-conditional-access-policy) veya [belirli koşullu erişim ilkesini devre dışı bırak](#disable-a-specific-conditional-access-policy)

   * Olduğundan emin olun kullanıcının **kimlik doğrulaması kişi bilgilerini** yürütülebilmesi çok faktörlü kimlik doğrulaması veya koşullu erişim ilkeleri izin güncel değil. [Bir kullanıcının çok faktörlü kimlik doğrulama durumunu denetleme](#check-a-users-multi-factor-authentication-status) veya [bir kullanıcının kimlik doğrulaması ile ilgili kişi bilgileri](#check-a-users-authentication-contact-info)

-   İçin **Microsoft** **lisans gerektiren uygulamalar** (Office365 gibi), yukarıdaki genel sorunlar çizgili sonra denetlemek için belirli bazı sorunlar şunlardır:

   * Kullanıcı sağlayın ya da sahip bir **lisansı atanmış.** [Bir kullanıcının atanan lisansları denetleme](#check-a-users-assigned-licenses) veya [bir grubun atanan lisansları denetleme](#check-a-groups-assigned-licenses)

   * Lisans ise **atanan bir** **statik grup**, emin **kullanıcının üyesi olduğu** o grubun. [Bir kullanıcı grup üyeliklerini denetleyin](#check-a-users-group-memberships)

   * Lisans ise **atanan bir** **dinamik grup**, emin **dinamik Grup kural doğru olarak ayarlandığında**. [Dinamik bir grubun üyeliğini ölçütlerini denetleyin](#check-a-dynamic-groups-membership-criteria)

   * Lisans ise **atanan bir** **dinamik grup**, dinamik grup sahip olduğundan emin olun **işleme tamamlandı** üyeliğine ve, **kullanıcının üyesi olduğu** (Bu işlem biraz zaman alabilir). [Bir kullanıcı grup üyeliklerini denetleyin](#check-a-users-group-memberships)

   *  Lisans atandığı emin yaptıktan sonra lisans olduğundan emin olun **süresi**.

   *  Lisans olduğundan emin olun **uygulama için** erişmekte olan.

-   İçin **Microsoft** **bir lisans gerektirmeyen uygulamalar**, denetlenecek diğer bazı öğeler şunlardır:

   * Uygulama istiyorsa **kullanıcı düzeyinde izinler** (örneğin "Bu kullanıcının posta kutusu erişim"), kullanıcı uygulamaya imzaladığı emin olun ve gerçekleştirdiği bir **kullanıcı düzeyinde onay işlemi** kendi veri erişim izin vermek için.

   * Uygulama istiyorsa **yönetici düzeyi izinler** (örneğin "tüm kullanıcının posta kutularına erişim"), genel yönetici yürüttü emin olun bir **tüm kullanıcılar adına yönetici düzeyi onay işlemi** kuruluştaki.

## <a name="problems-with-the-users-account"></a>Kullanıcı hesabının sorunları

Uygulama erişimi, uygulamaya atanan bir kullanıcı ile ilgili bir sorun nedeniyle engellenebilir. Aşağıdaki sorun giderme ve kullanıcıların ve hesap ayarları ile sorunları bazı yöntemler şunlardır:

-   [Bir kullanıcı hesabı Azure Active Directory içinde olup olmadığını kontrol edin](#check-if-a-user-account-exists-in-azure-active-directory)

-   [Bir kullanıcının hesap durumunu denetle](#check-a-users-account-status)

-   [Bir kullanıcının parolasını sıfırlama](#reset-a-users-password)

-   [Self servis parola sıfırlamayı etkinleştirme](#enable-self-service-password-reset)

-   [Bir kullanıcının çok faktörlü kimlik doğrulama durumunu denetle](#check-a-users-multi-factor-authentication-status)

-   [Bir kullanıcının kimlik doğrulaması ile ilgili kişi bilgileri](#check-a-users-authentication-contact-info)

-   [Bir kullanıcı grup üyeliklerini denetleyin](#check-a-users-group-memberships)

-   [Bir kullanıcının atanan lisansları denetleme](#check-a-users-assigned-licenses)

-   [Bir kullanıcı bir lisans atama](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Bir kullanıcı hesabı Azure Active Directory içinde olup olmadığını kontrol edin

Bir kullanıcı hesabının mevcut olup olmadığını denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  Beklediğiniz ve hiçbir veri eksik gibi göründüğünü emin olmak için kullanıcı nesnesinin özelliklerini denetleyin.

### <a name="check-a-users-account-status"></a>Bir kullanıcının hesap durumunu denetle

Bir kullanıcı hesabı durumunu denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **profil**.

8.  Altında **ayarları** emin **blok oturum** ayarlanır **Hayır**.

### <a name="reset-a-users-password"></a>Bir kullanıcının parolasını sıfırlama

Bir kullanıcının parolasını sıfırlamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **parola sıfırlama** kullanıcı dikey pencerenin üstündeki düğmesi.

8.  tıklatın **parola sıfırlama** düğmesini **parola sıfırlama** görünür dikey.

9.  Kopya **geçici parola** veya **yeni bir parola girin** kullanıcı için.

10. Bu yeni parolayı kullanıcıya iletişim, Azure Active Directory'ye sonraki oturum açma sırasında bu parolayı değiştirmeniz gerekiyor.

### <a name="enable-self-service-password-reset"></a>Self Servis parola sıfırlama etkinleştirme

Self Servis parola sıfırlamayı etkinleştirmek için dağıtım adımları izleyin:

-   [Azure Active Directory parolalarını sıfırlamalarına olanak tanıma](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [Sıfırlama veya Active Directory şirket içi parolalarını değiştirmek kullanıcıları etkinleştir](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a>Bir kullanıcının çok faktörlü kimlik doğrulama durumunu denetle

Bir kullanıcının çok faktörlü kimlik doğrulama durumunu denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  tıklatın **çok faktörlü kimlik doğrulaması** dikey pencerenin üstündeki düğmesi.

7.  Bir kez **multi-Factor Authentication Yönetim Portalı** yüklendiğinde emin olduğunuz üzerinde **kullanıcılar** sekmesi.

8.  Kullanıcı, arama, filtreleme veya sıralama kullanıcı listesinde bulun.

9.  Kullanıcı kullanıcılar listesinden seçin ve **etkinleştirmek**, **devre dışı**, veya **zorla** istediğiniz gibi çok faktörlü kimlik doğrulaması.

  * **Not**: kullanıcı ise bir **zorlanmış** durumunda, ayarlayın bunları **devre dışı** geçici olarak kendi hesaba geri izin vermek için. Daha sonra geri oldukları sonra durumlarına değiştirebilirsiniz **etkin** yeniden sonraki oturum açma sırasında kişi bilgileri yeniden kaydetmek için bunları gerektirecek şekilde. Alternatif olarak, içindeki adımları takip edebilirsiniz [bir kullanıcının kimlik doğrulaması ile ilgili kişi bilgileri](#check-a-users-authentication-contact-info) doğrulamak veya onlar için bu veri kümesi için.

### <a name="check-a-users-authentication-contact-info"></a>Bir kullanıcının kimlik doğrulaması ile ilgili kişi bilgileri

Çok faktörlü kimlik doğrulaması, koşullu erişim, kimlik koruması ve parola sıfırlama için kullanılan kullanıcı kimlik doğrulaması kişi bilgilerini denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **profil**.

8.  Ekranı aşağı kaydırarak **kimlik doğrulaması kişi bilgilerini**.

9.  **Gözden geçirme** veri, güncelleştirme ve kullanıcı için gerektiği şekilde kaydedildi.

### <a name="check-a-users-group-memberships"></a>Bir kullanıcı grup üyeliklerini denetleyin

Bir kullanıcı grup üyeliklerini denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **grupları** olduğu kullanıcı grupları görmek için bir üyesidir.

### <a name="check-a-users-assigned-licenses"></a>Bir kullanıcının atanan lisansları denetleme

Bir kullanıcının atanan lisansları denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

### <a name="assign-a-user-a-license"></a>Bir kullanıcı bir lisans atama 

Bir kullanıcıya bir lisans atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

8.  tıklatın **atamak** düğmesi.

9.  Seçin **bir veya daha fazla ürün** mevcut ürünler listesinden.

10. **İsteğe bağlı** tıklatın **atama seçenekleri** ürünleri granularly atanacak öğe. Tıklatın **Tamam** ne zaman bu tamamlandı.

11. Tıklatın **atamak** bu lisans bu kullanıcıya atamak için düğmesi.

## <a name="problems-with-groups"></a>Grupları sorunları

Uygulama erişimi, uygulamaya atanan bir grubu ile ilgili bir sorun nedeniyle engellenebilir. Aşağıdaki sorun giderme ve gruplar ve grup üyelikleri ile sorunları bazı yöntemler şunlardır:

-   [Bir grubun üyeliğini kontrol edin](#check-a-groups-membership)

-   [Dinamik bir grubun üyeliğini ölçütlerini denetleyin](#check-a-dynamic-groups-membership-criteria)

-   [Bir grubun atanan lisansları denetleme](#check-a-groups-assigned-licenses)

-   [Bir grubun lisansları yeniden işleyin](#reprocess-a-groups-licenses)

-   [Bir gruba bir lisans atama](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Bir grubun üyeliğini kontrol edin

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubu için ve **satıra tıklayın** seçin.

7.  tıklatın **üyeleri** bu gruba atanmış kullanıcıların listesini gözden geçirebilirsiniz.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Dinamik bir grubun üyeliğini ölçütlerini denetleyin 

Dinamik grubun Üyelik ölçütleri denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubu için ve **satıra tıklayın** seçin.

7.  tıklatın **dinamik Üyelik kuralları.**

8.  Gözden geçirme **basit** veya **Gelişmiş** bu grup için tanımlanan kuralı ve bu grubun üyesi olmasını istediğiniz kullanıcıyı bu ölçütleri karşıladığından emin olun.

### <a name="check-a-groups-assigned-licenses"></a>Bir grubun atanan lisansları denetleme

Bir grubun atanan lisansları denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubu için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** hangi Grup şu anda lisansları görmek için atanır.

### <a name="reprocess-a-groups-licenses"></a>Bir grubun lisansları yeniden işleyin

Bir grubun atanan lisansları yeniden işlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubu için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** hangi Grup şu anda lisansları görmek için atanır.

8.  tıklatın **yeniden işleyin** düğmesi bu grubun üyeleri için atanan lisansları güncel olduğundan emin olun. Bu, boyut ve Grup karmaşıklığına bağlı olarak uzun bir süre devam edebilir.

   >[!NOTE]
   >Bu daha hızlı yapmak için kullanıcıya bir lisans'geçici olarak doğrudan atama göz önünde bulundurun. [Bir kullanıcı bir lisans atamak](#problems-with-application-consent).
   >
   >

### <a name="assign-a-group-a-license"></a>Bir gruba bir lisans atama

Bir gruba bir lisans atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm grupları**.

6.  **Arama** ilgilendiğiniz grubu için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** hangi Grup şu anda lisansları görmek için atanır.

8.  tıklatın **atamak** düğmesi.

9.  Seçin **bir veya daha fazla ürün** mevcut ürünler listesinden.

10. **İsteğe bağlı** tıklatın **atama seçenekleri** ürünleri granularly atanacak öğe. Tıklatın **Tamam** ne zaman bu tamamlandı.

11. Tıklatın **atamak** bu gruba bu lisansları atamak için düğmesi. Bu, boyut ve Grup karmaşıklığına bağlı olarak uzun bir süre devam edebilir.

   >[!NOTE]
   >Bu daha hızlı yapmak için kullanıcıya bir lisans'geçici olarak doğrudan atama göz önünde bulundurun. [Bir kullanıcı bir lisans atamak](#problems-with-application-consent).
   > 
   >

## <a name="problems-with-conditional-access-policies"></a>Koşullu erişim ilkeleri sorunları

### <a name="check-a-specific-conditional-access-policy"></a>Belirli bir koşullu erişim ilkesine denetleyin

Denetleyin ya da tek koşullu erişim ilkesi doğrulamak için:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Gezinti menüsünde.

5.  tıklatın **koşullu erişim** Gezinti öğesi.

6.  inceleyerek ilgilendiğiniz İlkesi'ni tıklatın.

7.  Belirli bir koşul, atamaları ya da kullanıcı erişimi engelliyor olabilecek diğer ayarları gözden geçirin.

   >[!NOTE]
   >Geçici olarak bu değil etkileyen emin olmak için bu ilkeyi devre dışı bırakmak isteyebilir oturum açmalar. Bunu yapmak için ayarlayın **ilkesini etkinleştir** geç **Hayır** tıklatıp **kaydetmek** düğmesi.
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Belirli bir uygulamanın koşullu erişim ilkesini denetleme

Denetleyin veya şu anda tek bir uygulamanın doğrulamak için koşullu erişim ilkesi yapılandırılır:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Gezinti menüsünde.

5.  tıklatın **tüm uygulamaları**.

6.  Uygulama görünen adı veya uygulama kimliği ile oturum açmak ilgilendiğiniz uygulama veya kullanıcı arama çalışıyor.

     >[!NOTE]
     >Aradığınız uygulama görmüyorsanız tıklatın **filtre** düğmesini tıklatın ve listeye kapsamını genişletmek **tüm uygulamaları**. Daha fazla sütun görmek istiyorsanız, tıklayın **sütunları** ek ayrıntılar uygulamalarınız için Ekle düğmesini.
     >
     >

7.  tıklatın **koşullu erişim** Gezinti öğesi.

8.  inceleyerek ilgilendiğiniz İlkesi'ni tıklatın.

9.  Belirli bir koşul, atamaları ya da kullanıcı erişimi engelliyor olabilecek diğer ayarları gözden geçirin.

     >[!NOTE]
     >Geçici olarak bu değil etkileyen emin olmak için bu ilkeyi devre dışı bırakmak isteyebilir oturum açmalar. Bunu yapmak için ayarlayın **ilkesini etkinleştir** geç **Hayır** tıklatıp **kaydetmek** düğmesi.
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Belirli bir koşullu erişim ilkesine devre dışı bırak

Denetleyin ya da tek koşullu erişim ilkesi doğrulamak için:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Gezinti menüsünde.

5.  tıklatın **koşullu erişim** Gezinti öğesi.

6.  inceleyerek ilgilendiğiniz İlkesi'ni tıklatın.

7.  İlke ayarı devre dışı **ilkesini etkinleştir** geç **Hayır** tıklatıp **kaydetmek** düğmesi.

## <a name="problems-with-application-consent"></a>Uygulama onayı sorunları

Uygun izinleri onay işlemi değil oluştuğu için uygulama erişimi engellenebilir. Aşağıdaki sorun giderme ve uygulama izin sorunları çözmeye bazı yöntemler şunlardır:

-   [Bir kullanıcı düzeyinde onay işlemi gerçekleştirme](#perform-a-user-level-consent-operation)

-   [Herhangi bir uygulama için yönetici düzeyi onay işlemi gerçekleştirme](#perform-administrator-level-consent-operation-for-any-application)

-   [Bir tek kiracılı uygulama için yönetici düzeyi onay gerçekleştirme](#perform-administrator-level-consent-for-a-single-tenant-application)

-   [Çok kiracılı uygulama için yönetici düzeyi onay gerçekleştirme](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Bir kullanıcı düzeyinde onay işlemi gerçekleştirme

-   İzinleri isteyen herhangi Open ID Connect özellikli bir uygulama için uygulama oturum açma ekranına giderek oturum açmış kullanıcı için bir kullanıcı düzeyinde onay gerçekleştirir.

-   Bu program aracılığıyla yapmak istiyorsanız, bkz: [tek tek kullanıcı izni isteyen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>Herhangi bir uygulama için yönetici düzeyi onay işlemi gerçekleştirme

-   İçin **yalnızca V1 uygulama modeli kullanılarak geliştirilen uygulamaları**, ekleyerek gerçekleşmesi için bu administrator düzeyinde onayı zorlayabilirsiniz "**? yönetici komut isteminde =\_onayı**" bir uygulamanın oturum açma URL'si sonuna.

-   İçin **herhangi bir uygulama geliştirilen V2 uygulama modelini kullanarak**, başlığı altında verilen yönergeleri izleyerek gerçekleşmesi için bu administrator düzeyinde onayı zorunlu kılabilir **directory yönetici olarak izinleri isteği** bölümünü [yönetici onayı uç noktası kullanarak](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Bir tek kiracılı uygulama için yönetici düzeyi onay gerçekleştirme

-   İçin **tek Kiracı uygulamaları** izinleri (gelenler geliştirdiğiniz veya, kuruluşunuzda sahip), istek gerçekleştirebileceğiniz bir **yönetici düzeyi izin** işlemi genel yönetici olarak oturum açmayı ve üzerinde tıklatarak tüm kullanıcılar adına **izinleri verin** en üstündeki düğmesi **uygulama kayıt defteri -&gt; tüm uygulamalar -&gt; bir uygulama - seçin&gt; gerekli izinler** dikey.

-   İçin **herhangi bir uygulama geliştirilen V1 veya V2 uygulama modelini kullanarak**, başlığı altında verilen yönergeleri izleyerek gerçekleşmesi için bu administrator düzeyinde onayı zorunlu kılabilir **directory yönetici olarak izinleri isteği** bölümünü [yönetici onayı uç noktası kullanarak](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Çok kiracılı uygulama için yönetici düzeyi onay gerçekleştirme

-   İçin **çok kiracılı uygulamalara** (bir uygulama bir üçüncü taraf veya Microsoft, geliştirir gibi) bu istek izinlerine gerçekleştirebileceğiniz bir **yönetici düzeyi onay** işlemi. Genel yönetici olarak oturum açın ve tıklayarak **izinleri verin** altında düğmesini **kurumsal uygulamalar -&gt; tüm uygulamalar -&gt; bir uygulama - seçin&gt; izinleri** dikey (kullanılabilir yakında).

-   Başlığı altında verilen yönergeleri izleyerek gerçekleşmesi için bu administrator düzeyinde onayı zorunlu kılabilir **directory yönetici olarak izinleri isteği** bölümünü [yönetici onayı uç noktası kullanarak](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Sonraki adımlar
[Yönetici onayı uç noktası kullanma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes#using-the-admin-consent-endpoint)

