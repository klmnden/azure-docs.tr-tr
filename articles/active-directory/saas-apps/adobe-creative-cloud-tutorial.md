---
title: 'Öğretici: Adobe Creative Cloud ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Adobe Creative Cloud arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: c199073f-02ce-45c2-b515-8285d4bbbca2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35bd52904ab081e41cb43a346288234c18a7f43b
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65899094"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Öğretici: Adobe Creative Cloud ile Azure Active Directory Tümleştirme

Bu öğreticide, Adobe Creative Cloud, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
Adobe Creative Cloud, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Adobe Creative Cloud erişimi olan Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Adobe Creative Cloud için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Adobe Creative Cloud ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Adobe Creative Cloud'a çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Adobe Creative Cloud'a destekler **SP** tarafından başlatılan

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Adobe Creative Cloud galeri ekleme

Azure AD'de Adobe Creative Cloud tümleştirmesini yapılandırmak için Adobe Creative Cloud Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Adobe Creative Cloud galerideki eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Adobe Creative Cloud**seçin **Adobe Creative Cloud** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde Adobe Creative Cloud](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma adlı bir test kullanıcı tabanlı Adobe Creative Cloud ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve Adobe Creative Cloud ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Adobe Creative Cloud ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Adobe Creative Cloud çoklu oturum açmayı yapılandırma](#configure-adobe-creative-cloud-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Adobe Creative Cloud test kullanıcısı oluşturma](#create-adobe-creative-cloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı Adobe Creative cloud'da Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Adobe Creative Cloud ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Adobe Creative Cloud** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Adobe Creative Cloud etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusunda, değer olarak girin: `https://adobe.com`.

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://www.okta.com/saml2/service-provider/<token>`

    > [!NOTE]
    > Tanımlayıcı değerini gerçek değil. Bu değer, gerçek tanımlayıcısıyla güncelleştirin. İlgili kişi [Adobe Creative Cloud istemci Destek ekibine](https://www.adobe.com/au/creativecloud/business/teams/plans.html) bu değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Adobe Creative Cloud'a uygulama belirli bir biçimde SAML onaylamalarını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelikleri değerlerini yönetebilirsiniz **kullanıcı öznitelikleri** uygulama tümleştirme sayfasında bölümü. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için düğmeyi **kullanıcı öznitelikleri** iletişim.

    ![image](common/edit-attribute.png)

6. İçinde **kullanıcı taleplerini** bölümünde **kullanıcı öznitelikleri** iletişim kutusunda, SAML belirteci özniteliği yukarıdaki görüntüde gösterilen şekilde yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Ad | Kaynak özniteliği|
    |----- | --------- |
    | FirstName | User.givenName |
    | LastName | User.surname |
    | E-posta | User.Mail

    a. Tıklayın **Ekle yeni talep** açmak için **yönetmek, kullanıcı talepleri** iletişim.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. İçinde **adı** metin kutusuna, bu satır için gösterilen öznitelik adı yazın.

    c. Bırakın **Namespace** boş.

    d. Kaynağı olarak **özniteliği**.

    e. Gelen **kaynak özniteliği** listesinde, ilgili satır için gösterilen öznitelik değeri yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Kullanıcıların talep SAML yanıtta doldurulacak değeri geçerli bir Office 365 ExO lisans için e-posta olması gerekir.

7. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. Üzerinde **Adobe Creative Cloud ayarlamak** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-adobe-creative-cloud-single-sign-on"></a>Adobe Creative Cloud'a çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde için oturum [Adobe Yönetici Konsolu](https://adminconsole.adobe.com) yönetici olarak.

2. Git **ayarları** üst gezinti çubuğunda ve ardından **kimlik**. Etki alanlarının listesi açılır. Tıklayın **yapılandırma** etki alanınıza yönelik bağlantı. Ardından üzerinde aşağıdaki adımları gerçekleştirin **tek oturum yapılandırması gerekli** bölümü. Daha fazla bilgi için [bir etki alanı Kurulumu](https://helpx.adobe.com/enterprise/using/set-up-domain.html)

    ![Ayarları](https://helpx.adobe.com/content/dam/help/en/enterprise/using/configure-microsoft-azure-with-adobe-sso/_jcr_content/main-pars/procedure_719391630/proc_par/step_3/step_par/image/edit-sso-configuration.png "ayarları")

    a. Tıklayın **Gözat** için Azure AD'den yüklenen sertifikayı karşıya yüklemek için **IDP sertifika**.

    b. İçinde **IDP veren** metin değerini put **SAML varlık kimliği** bölümünden kopyaladığınız **yapılandırma oturum açma** bölümü Azure Portalı'nda.

    c. İçinde **IDP'nin oturum açma URL'si** metin değerini put **SAML SSO hizmet URL'si** bölümünden kopyaladığınız **yapılandırma oturum açma** bölümü Azure Portalı'nda.

    d. Seçin **HTTP - Redirect** olarak **IDP bağlama**.

    e. Seçin **e-posta adresi** olarak **kullanıcı oturum açma ayarı**.

    f. Tıklayın **Kaydet** düğmesi.

3. Pano artık XML sunacaktır **"Meta verileri indirme"** dosya. Bu, Adobe EntityDescriptor URL ve AssertionConsumerService URL'sini içerir. Lütfen dosyayı açın ve bunları Azure AD'ye yapılandırın.

    ![Çoklu oturum açma uygulama tarafında yapılandırma](./media/adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Adobe için sağlanan EntityDescriptor değeri kullanın **tanımlayıcı** üzerinde **uygulama ayarlarını yapılandırma** iletişim.

    b. Adobe için sağlanan AssertionConsumerService değeri kullanın **yanıt URL'si** üzerinde **uygulama ayarlarını yapılandırma** iletişim.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon\@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Adobe Creative Cloud erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Adobe Creative Cloud**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Adobe Creative Cloud**.

    ![Uygulamalar listesini Adobe Creative Cloud bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-adobe-creative-cloud-test-user"></a>Adobe Creative Cloud test kullanıcısı oluşturma

Azure AD kullanıcılarının Adobe Creative Cloud oturum etkinleştirmek için Adobe Creative Cloud sağlanması gerekir. Adobe Creative Cloud söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Bir kullanıcı hesapları sağlamak için aşağıdaki adımları gerçekleştirin:

1. Oturum [Adobe Yönetici Konsolu](https://adminconsole.adobe.com) yönetici olarak site.

2. Adobe Konsolu içinden kullanıcı Federasyon kimliği ekleyin ve bunları bir ürün profiline atayın. Kullanıcı ekleme ile ilgili ayrıntılı bilgi için bkz [Adobe Yönetici konsolunda kullanıcı ekleme](https://helpx.adobe.com/enterprise/using/users.html#Addusers) 

3. Bu noktada, e-posta adresi/upn Adobe signın forma SEKME tuşuna basın, yazın ve Azure AD ile federasyona eklenmesi:
   * Web erişimi: www\.adobe.com > oturum açma
   * Masaüstü uygulaması yardımcı program içinde > oturum açma
   * Uygulama içinde > Yardım > oturum açma

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Adobe Creative Cloud kutucuğa tıkladığınızda, otomatik olarak Adobe Creative Cloud'ı SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
  
- [(Adobe.com) etki alanını ayarlama](https://helpx.adobe.com/enterprise/using/set-up-domain.html)
  
- [Azure, Adobe SSO (adobe.com) ile kullanım için yapılandırın](https://helpx.adobe.com/enterprise/kb/configure-microsoft-azure-with-adobe-sso.html)
