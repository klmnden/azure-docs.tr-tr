---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı hazırlama için G Suite'i yapılandırma | Microsoft Docs"
description: Otomatik olarak sağlama ve sağlamasını G suite'te Azure AD'den kullanıcı hesapları hakkında bilgi edinin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea1f4d4a6b60961515826a1ba7409bf149b318e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60277174"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı hazırlama için G Suite'i yapılandırma

Bu öğreticinin amacı, size otomatik olarak sağlama ve sağlamasını G Suite Azure Active Directory (Azure AD) kullanıcı hesaplarını nasıl göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD kullanıcı sağlama hizmeti üzerinde oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmet yapar, nasıl çalıştığını ve sık sorulan sorular önemli ayrıntılar için bkz. [otomatik kullanıcı hazırlama ve sağlamayı kaldırma Azure Active Directory ile SaaS uygulamalarına](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

G Suite ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

- Azure AD aboneliği
- Abonelik bir G Suite çoklu oturum açma etkin
- Google Apps aboneliği veya Google Cloud Platform abonelik.

> [!NOTE]
> Bu öğreticideki adımları test etmek için üretim ortamı kullanarak önermiyoruz.

Bu öğreticideki adımları test etmek için bu önerileri izlemelidir:

- Gerekli olmadıkça, üretim ortamında kullanmayın.
- Azure AD deneme ortamı yoksa, şunları yapabilirsiniz [bir aylık deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-g-suite"></a>G Suite kullanıcıları atama

Azure Active Directory "atamaları" adlı bir kavram, hangi kullanıcıların seçilen uygulamalara erişimi alması belirlemek için kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca kullanıcıların ve grupların, "Azure AD'de bir uygulama için atandı" eşitlenir.

Yapılandırıp sağlama hizmetini etkinleştirmeden önce hangi kullanıcıların veya grupların Azure AD'de uygulamanıza erişmeniz karar vermeniz gerekir. Bu karar yaptıktan sonra bu kullanıcılar uygulamanıza yönergelerini takip ederek atayabilirsiniz [kurumsal bir uygulamayı kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

> [!IMPORTANT]
> Tek bir öneririz, Azure AD kullanıcı sağlama yapılandırmayı test etmek için G Suite atanabilir. Daha sonra ek kullanıcılar ve grupları atayabilirsiniz.
> 
> G Suite için bir kullanıcıya atadığınızda, seçin **kullanıcı** veya **grubu** rol ataması iletişim kutusunda. **Varsayılan erişim** rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirin

Bu bölümde Azure AD'nize G Suite API'sini sağlama kullanıcı hesabı ile bağlantı kurma işleminde size rehberlik eder. Ayrıca oluşturma, güncelleştirme ve devre dışı kullanıcı ve Grup ataması Azure AD'de göre G Suite atanan kullanıcı hesapları için sağlama hizmetini yapılandırmanıza yardımcı olur.

>[!TIP]
>SAML tabanlı çoklu oturum açma G paketleri için etkinleştirme yönergeleri izleyerek isteyebilirsiniz [Azure portalında](https://portal.azure.com). Bu iki özellik birbirine tamamlayıcı rağmen otomatik sağlama bağımsız olarak, çoklu oturum açma yapılandırılabilir.

### <a name="configure-automatic-user-account-provisioning"></a>Hesap otomatik kullanıcı sağlamayı yapılandırma

> [!NOTE]
> G Suite için kullanıcı sağlamayı otomatikleştirmek için başka bir kaydının uygulanabilir bir seçenek kullanmaktır [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en). GADS şirket içi Active Directory kimliklerinizi G Suite sağlar. Buna karşılık, e-posta özellikli gruplar G Suite ve Azure Active Directory (bulut) kullanıcıları Bu öğreticide bir çözüm sağlar. 

1. Oturum [Google Apps Yönetici Konsolu](https://admin.google.com/) yönetici hesabı ve ardından **güvenlik**. Bağlantıyı görmüyorsanız, bunun altında gizlenebilir **diğer denetimler** ekranın alt kısmındaki menü.

    ![Güvenlik'i seçin.][10]

1. Üzerinde **güvenlik** sayfasında **API Başvurusu**.

    ![API başvurusunu seçin.][15]

1. Seçin **etkinleştirme API erişimi**.

    ![API başvurusunu seçin.][16]

   > [!IMPORTANT]
   > G Suite için sağlamak istediğiniz her bir kullanıcı için kendi kullanıcı adı Azure Active Directory'de *gerekir* özel bir etki alanına bağlı. Örneğin, görünüm gibi kullanıcı adları bob@contoso.onmicrosoft.com G Suite tarafından kabul edilmez. Öte yandan, bob@contoso.com kabul edilir. Mevcut bir kullanıcının etki alanı, Azure AD'de özelliklerini düzenleyerek değiştirebilirsiniz. Aşağıdaki adımlarda Azure Active Directory ve G Suite için özel bir etki alanı ayarlama hakkında yönergeler ekledik.

1. Azure Active Directory'ye özel etki alanı henüz eklemediniz, ardından aşağıdaki adımları uygulayın:
  
    a. İçinde [Azure portalında](https://portal.azure.com), sol gezinti bölmesinde seçin **Active Directory**. Dizin listesinde dizininizi seçin.

    b. Seçin **etki alanı adı** sol gezinti bölmesinde, seçip **Ekle**.

    ![Domain](./media/google-apps-provisioning-tutorial/domain_1.png)

    ![Etki alanı ekleme](./media/google-apps-provisioning-tutorial/domain_2.png)

    c. Etki alanı adınızı yazın **etki alanı adı** alan. Bu etki alanı adı, G Suite için kullanmayı planladığınız aynı etki alanı adı olmalıdır. Ardından **etki alanı Ekle** düğmesi.

    ![Etki alanı adı](./media/google-apps-provisioning-tutorial/domain_3.png)

    d. Seçin **sonraki** doğrulama sayfasına gidin. Bu etki alanının sahibi olduğunuzu doğrulamak için etki alanının DNS kayıtlarını bu sayfada sağlanan değerlere göre düzenleyin. Kullanarak doğrulamak, tercih edebileceğiniz **MX kaydı** veya **TXT kayıtlarının**için yaptığınız seçime bağlı olarak **kayıt türü** seçeneği.

    Azure AD etki alanı adlarıyla doğrulama hakkında daha kapsamlı yönergeler için bkz: [kendi etki alanı adınızı Azure AD'ye ekleme](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).

    ![Domain](./media/google-apps-provisioning-tutorial/domain_4.png)

    e. Dizininize eklemek istediğiniz tüm etki alanları için önceki adımları yineleyin.

    > [!NOTE]
    > Özel etki alanı için kullanıcı hazırlama, kaynak Azure AD etki alanı adı eşleşmelidir. Bunlar eşleşmiyorsa, öznitelik eşlemesi özelleştirme uygulayarak sorunu çözmenize yardımcı olabilir.

1. Tüm etki alanlarınızı Azure AD ile doğruladıktan sonra bunları Google Apps ile yeniden doğrulamalısınız. Google ile zaten kayıtlı değilse her etki alanı için aşağıdaki adımları uygulayın:

    a. İçinde [Google Apps Yönetici Konsolu](https://admin.google.com/)seçin **etki alanları**.

    ![Etki alanı seçin][20]

    b. Seçin **bir etki alanına veya etki alanı diğer ad ekleyin**.

    ![Yeni bir etki alanı ekleme][21]

    c. Seçin **başka bir etki alanı ekleme**ve eklemek istediğiniz etki alanının adını yazın.

    ![Etki alanı adınızı yazın][22]

    d. Seçin **devam ve etki alanı sahipliğini doğrulama**. Ardından, etki alanı adının ait olduğunu doğrulamak için adımları izleyin. Google ile etki alanınızı doğrulayın konusunda kapsamlı yönergeler için bkz. [, Google Apps ile site sahipliği doğrulamak](https://support.google.com/webmasters/answer/35179).

    e. Google Apps eklemek için istediğinize herhangi ek bir etki alanı için önceki adımları yineleyin.

    > [!WARNING]
    > G Suite kiracınız için birincil etki alanı değiştirirseniz ve zaten çoklu oturum açma Azure AD ile yapılandırdıysanız, #3. Adım 2. adım altında yineleyin vardır: Çoklu oturum açmayı etkinleştirin.

1. İçinde [Google Apps Yönetici Konsolu](https://admin.google.com/)seçin **yönetici rolleri**.

    ![Google Apps'ı seçin][26]

1. Kullanıcı sağlamayı yönetmek için kullanmak istediğiniz yönetici hesabı belirleyin. İçin **Yönetici rolü** o hesabı, Düzen **ayrıcalıkları** bu rol için. Tümünü etkinleştirdiğinizden emin olun **yönetici API ayrıcalıkları** böylece sağlamak için bu hesabı kullanılabilir.

    ![Google Apps'ı seçin][27]

    > [!NOTE]
    > Bir üretim ortamında yapılandırıyorsanız, G Suite'te özellikle bu adım için bir yönetici hesabı oluşturulacak en iyi yöntem olacaktır. Bu hesaplar gerekli API ayrıcalıklara sahip bir yönetici rolünü ilişkili olması gerekir.

1. İçinde [Azure portalında](https://portal.azure.com), Gözat **Azure Active Directory** > **Kurumsal uygulamaları** > **tümuygulamalar** bölümü.

1. Çoklu oturum açma için G Suite zaten yapılandırdıysanız, G Suite Örneğiniz için arama alanını kullanarak arayın. Aksi takdirde seçin **Ekle**ve ardından arama **G Suite** veya **Google Apps** uygulama galerisinde. Arama sonuçlarından uygulamanızı seçin ve ardından uygulamalar listesine ekleyin.

1. G Suite örneğinizi seçin ve ardından **sağlama** sekmesi.

1. Ayarlama **hazırlama modu** için **otomatik**. 

    ![Sağlama](./media/google-apps-provisioning-tutorial/provisioning.png)

1. Altında **yönetici kimlik bilgileri** bölümünden **Authorize**. Bu, yeni bir tarayıcı penceresinde bir Google yetkilendirme iletişim kutusu açılır.

1. G Suite kiracınıza değişiklik yapmak için Azure Active Directory izin vermek istediğinizi onaylayın. **Kabul Et**’i seçin.

    ![İzinleri doğrulayın.][28]

1. Azure portalında **Test Bağlantısı** için uygulamanızı Azure AD'ye bağlanabildiğinden emin olun. Bağlantı başarısız olursa, G Suite hesabınız takım Yöneticisi izinlerine sahip olduğundan emin olun. Daha sonra deneyin **Authorize** adım yeniden uygulayın.

1. Bir kişi veya grup sağlama hatası bildirimlerini alması gereken e-posta adresini girin **bildirim e-posta** alan. Ardından onay kutusunu seçin.

1. Seçin **kaydedin.**

1. Altında **eşlemeleri** bölümünden **eşitleme Azure Active Directory Kullanıcıları için Google Apps**.

1. İçinde **öznitelik eşlemelerini** bölümünde, G Suite için Azure AD'den eşitlenen kullanıcı özniteliklerini gözden geçirin. Öznitelikleri **eşleşen** özellikleri, G Suite güncelleştirme işlemleri için kullanıcı hesaplarını eşleştirmek için kullanılır. Seçin **Kaydet** değişiklikleri uygulamak için.

1. Azure AD sağlama hizmeti için G Suite etkinleştirmek için değiştirin **sağlama durumu** için **üzerinde** içinde **ayarları**.

1. **Kaydet**’i seçin.

Bu işlem, herhangi bir kullanıcı ya da G Suite kullanıcılar ve Gruplar bölümünde atanan grupları ilk eşitleme başlar. İlk eşitleme yaklaşık 40 dakikada hizmet çalışırken oluşan sonraki eşitlemeler uzun sürer. Kullanabileceğiniz **eşitleme ayrıntıları** bölüm ilerlemeyi izlemek ve etkinlik günlüklerini sağlama için bağlantıları izleyin. Bu günlükler, uygulamanızdan sağlama hizmeti tarafından gerçekleştirilen tüm eylemler açıklanmaktadır.

Azure AD günlüklerini sağlama okuma hakkında daha fazla bilgi için bkz. [hesabı otomatik kullanıcı hazırlama raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kullanıcı hesabı, kurumsal uygulamalar için sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](google-apps-tutorial.md)

<!--Image references-->

[10]: ./media/google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/google-apps-provisioning-tutorial/gapps-auth.png
