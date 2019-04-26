---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile iProva | Microsoft Docs'
description: Azure Active Directory ve iProva arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 1eaeef9b-4479-4a9f-b1b2-bc13b857c75c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 395d3887e35d6e9c043d4d947b605e71eb58bd6b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60270884"
---
# <a name="tutorial-azure-active-directory-integration-with-iprova"></a>Öğretici: İProva ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile iProva tümleştirme konusunda bilgi edinin.
Azure AD ile iProva tümleştirme ile aşağıdaki avantajları sağlar:

* İProva erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak iProva (çoklu oturum açma) ile kendi Azure AD hesapları için oturum açmanız, kullanıcılarınızın etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda, Azure portalında yönetebilir.

Azure AD ile bir hizmet (SaaS) uygulamasını tümleştirme olarak yazılım hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile iProva yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/) Web sitesi.
* SSO etkin abonelik iProva.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin:

* iProva SP tarafından başlatılan SSO'yu destekler.

## <a name="add-iprova-from-the-gallery"></a>Galeriden iProva Ekle

Azure AD'de iProva tümleştirmesini yapılandırmak için iProva Galeriden yönetilen SaaS uygulamaları listenize ekleyin.

Galeriden iProva eklemek için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com), sol bölmede seçin **Azure Active Directory**.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Git **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için seçin **yeni uygulama** iletişim kutusunun üst.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **iProva**. Seçin **iProva** sonuç paneli ve ardından **Ekle** uygulama eklemek için.

     ![sonuç listesinde iProva](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırın ve iProva Britta Simon adlı bir test kullanıcı tabanlı Azure AD çoklu oturum açmayı sınayın.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki içinde iProva oluşturmanız gerekir.

Yapılandırma ve Azure AD çoklu oturum açma iProva ile test etmek için aşağıdaki yapı taşlarını tamamlayın:

- [İProva yapılandırma bilgilerini almak](#retrieve-configuration-information-from-iprova) sonraki adımlar için bir hazırlık olarak.
- [Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on) kullanıcılarınız bu özelliği kullanmak etkinleştirmek için.
- [İProva çoklu oturum açmayı yapılandırma](#configure-iprova-single-sign-on) üzerinde uygulama tarafından çoklu oturum açma ayarları yapılandırmak için.
- [Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user) Azure AD çoklu oturum açma Britta Simon ile test etmek için.
- [Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user) Britta Simon, Azure AD çoklu oturum açma kullanmak üzere etkinleştirmek için.
- [Bir iProva test kullanıcısı oluşturma](#create-an-iprova-test-user) Britta Simon kullanıcı Azure AD gösterimini bağlı iProva içinde bir karşılığı sağlamak için.
- [Çoklu oturum açmayı test](#test-single-sign-on) yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="retrieve-configuration-information-from-iprova"></a>İProva yapılandırma bilgilerini alma

Bu bölümde, Azure AD çoklu oturum açmayı yapılandırmak için iProva bilgi alın.

1. Bir web tarayıcısı açın ve gidin **SAML2 bilgisi** aşağıdaki URL deseni kullanılarak iProva sayfasında:

    | | |
    |-|-|
    | `https://SUBDOMAIN.iprova.nl/saml2info`|
    | `https://SUBDOMAIN.iprova.be/saml2info`|
    | | |

     ![İProva SAML2 bilgileri sayfasını görüntüle](media/iprova-tutorial/iprova-saml2-info.png)

2. Başka bir tarayıcı sekmesinde sonraki adımlar ile devam ederken tarayıcı sekmesini açık bırakın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile iProva yapılandırmak için aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **iProva** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. İçinde **tek bir oturum açma yönteminizi seçmeniz** Seç iletişim kutusunda **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim kutusu.

    ![Temel bir SAML yapılandırma simgesini Düzenle](common/edit-urls.png)

4. İçinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları izleyin.

    a. Dolgu **tanımlayıcı** etiketi görüntülenir; değeri **Entityıd** üzerinde **iProva SAML2 bilgisi** sayfası. Bu sayfa, diğer tarayıcı sekmesinde hala açıktır.

    b. Dolgu **yanıt URL'si** etiketi görüntülenir; değeri **yanıt URL'si** üzerinde **iProva SAML2 bilgisi** sayfası. Bu sayfa, diğer tarayıcı sekmesinde hala açıktır.

    c. Dolgu **oturum açma URL'si** etiketi görüntülenir; değeri **oturum açma URL'si** üzerinde **iProva SAML2 bilgisi** sayfası. Bu sayfa, diğer tarayıcı sekmesinde hala açıktır.

    ![oturum açma bilgileri çoklu iProva etki alanı ve URL'ler](common/sp-identifier-reply.png)

5. İProva uygulamanın belirli bir biçimde SAML onaylamalarını bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **kullanıcı öznitelikleri** iletişim kutusu.

    ![Kullanıcı öznitelikleri iletişim kutusu](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** konusundaki **kullanıcı öznitelikleri** iletişim kutusunda, önceki görüntüde gösterildiği gibi SAML belirteci özniteliği yapılandırabilirsiniz. Şu adımları izleyin.

    | Ad | Kaynak özniteliği| Ad alanı |
    | ---------------| -------- | -----|
    | `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    | | |

    a. Seçin **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim kutusu.

    ![Kullanıcı talepleri](common/new-save-attribute.png)

    ![Kullanıcı talepleri iletişim kutusu yönetme](common/new-attribute-details.png)

    b. İçinde **adı** kutusunda, ilgili satır için gösterilen öznitelik adını girin.

    c. Gelen **Namespace** listesinde, ilgili satır için gösterilen ad alanı değeri girin.

    d. Seçin **kaynak** olarak seçeneğini **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri girin.

    f. **Tamam**’ı seçin.

    g. **Kaydet**’i seçin.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünden **kopyalama** kopyalamak için simge **uygulama Federasyon meta veri URL'si**  ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-iprova-single-sign-on"></a>İProva çoklu oturum açmayı yapılandırın

1. Oturum açmak için iProva kullanarak **yönetici** hesabı.

2. Açık **Git** menüsü.

3. Seçin **Uygulama Yönetimi**.

4. Seçin **genel** içinde **sistem ayarlarını** paneli.

5. **Düzenle**’yi seçin.

6. Ekranı aşağı kaydırarak **erişim denetimi**.

    ![iProva erişim denetimi ayarları](media/iprova-tutorial/iprova-accesscontrol.png)

7. Ayar Bul **otomatik olarak oturum açan kullanıcılar, ağ hesaplarıyla**ve şekilde değiştirin **Evet, kimlik doğrulama ile SAML**. Artık ek seçenekler görünür.

8. Seçin **ayarlanan**.

9. **İleri**’yi seçin.

10. iProva Federasyon verileri URL'den indirin veya bir dosyadan yüklemek isteyip istemediğinizi sorar. Seçin **URL'den** seçeneği.

    ![Azure AD meta verileri indirme](media/iprova-tutorial/iprova-download-metadata.png)

11. "Yapılandırma Azure AD çoklu oturum açma" bölümü, son adımda kaydettiğiniz meta veri URL'sini yapıştırın.

12. Azure AD'den meta verileri indirmek için ok şeklinde düğmesini seçin.

13. Yükleme tamamlandığında, onay iletisi **geçerli federasyon veri dosyasını indirdiğiniz** görünür.

14. **İleri**’yi seçin.

15. Skip **Test oturum açma** şimdilik seçeneğini belirtin ve **sonraki**.

16. İçinde **kullanılacak talep** açılan kutusunda **windowsaccountname**.

17. **Son**’u seçin.

18. Şimdi Geri **genel ayarlarını Düzenle** ekran. Sayfasının en altına kadar kaydırın ve seçin **Tamam** yapılandırmanızı kaydetmek için.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adlı bir test kullanıcısı oluşturun.

1. Azure portalında, sol bölmede seçin **Azure Active Directory** > **kullanıcılar** > **tüm kullanıcılar**.

    ![Kullanıcıları, grupları ve tüm kullanıcıların bağlantılarını](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. İçinde **kullanıcı** iletişim kutusunda, aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** gibi bir ad girin, kutusunda **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** kutusuna *adınız\@yourcompanydomain.extension*. 
    BrittaSimon@contoso.com bunun bir örneğidir.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.

    d. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma iProva erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar** > **tüm uygulamaları** > **iProva**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **iProva**.

    ![Uygulamalar listesinde iProva bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar bağlantı](common/users-groups-blade.png)

4. Seçin **Kullanıcı Ekle**ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim kutusu.

    ![Atama Ekle iletişim kutusu](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** içinde **kullanıcılar** listeleyin ve ardından **seçin** ekranın alt kısmındaki.

6. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin. Seçin **seçin** ekranın alt kısmındaki.

7. İçinde **atama Ekle** iletişim kutusunda **atama**.

### <a name="create-an-iprova-test-user"></a>Bir iProva test kullanıcısı oluşturma

1. Oturum açmak için iProva kullanarak **yönetici** hesabı.

2. Açık **Git** menüsü.

3. Seçin **Uygulama Yönetimi**.

4. Seçin **kullanıcılar** içinde **kullanıcılar ve kullanıcı grupları** paneli.

5. **Add (Ekle)** seçeneğini belirleyin.

6. İçinde **kullanıcıadı** kutusuna *brittasimon\@yourcompanydomain.extension*. 
    BrittaSimon@contoso.com bunun bir örneğidir.

7. İçinde **tam adı** gibi tam bir ad girin, kutusunda **BrittaSimon**.

8. Seçin **parolasız (kullanım çoklu oturum açma)** seçeneği.

9. İçinde **e-posta adresi** kutusuna *adınız\@yourcompanydomain.extension*. 
   BrittaSimon@contoso.com bunun bir örneğidir.

10. Sayfanın sonuna kaydırın ve seçin **son**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim paneli kullanarak Azure AD çoklu oturum açma yapılandırmanızı test.

Erişim Paneli'nde iProva kutucuğu seçtiğinizde, otomatik olarak SSO'yu ayarlama iProva için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [iProva - olduğu saml2 tabanlı çoklu oturum açmayı yapılandırma](https://webshare.iprova.nl/0wqwm45yn09f5poh/Document.aspx)
