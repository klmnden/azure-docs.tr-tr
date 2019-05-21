---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile OpenAthens | Microsoft Docs'
description: Azure Active Directory ve OpenAthens arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3eca6fc3ab788ee7085c0df5f6c9770858af29ba
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65891725"
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Öğretici: OpenAthens ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile OpenAthens tümleştirme konusunda bilgi edinin.
Azure AD ile OpenAthens tümleştirme ile aşağıdaki avantajları sağlar:

* OpenAthens erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) OpenAthens için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile OpenAthens yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* OpenAthens tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* OpenAthens destekler **IDP** tarafından başlatılan

* OpenAthens destekler **zamanında** kullanıcı sağlama

## <a name="adding-openathens-from-the-gallery"></a>Galeriden OpenAthens ekleme

Azure AD'de OpenAthens tümleştirmesini yapılandırmak için OpenAthens Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden OpenAthens eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **OpenAthens**seçin **OpenAthens** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde OpenAthens](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma OpenAthens adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının OpenAthens ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma OpenAthens ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[OpenAthens çoklu oturum açmayı yapılandırma](#configure-openathens-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[OpenAthens test kullanıcısı oluşturma](#create-openathens-test-user)**  - kullanıcı Azure AD gösterimini bağlı OpenAthens Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile OpenAthens yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **OpenAthens** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

5. Üzerinde **temel SAML yapılandırma** bölümünde, karşıya yükleme **hizmet sağlayıcısı meta veri dosyası**, hangi adımları Bu öğreticinin ilerleyen bölümlerinde açıklanan.

    a. Tıklayın **meta veri dosyasını karşıya yükleme**.

    ![openathens meta verilerini karşıya yükleme](common/upload-metadata.png)

    b. Tıklayarak **klasör logosu** meta veri dosyası seçin ve **karşıya**.

    ![Karşıya yükleme meta verileri Openathens Gözat](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** değeri get otomatik doldurulur **temel SAML yapılandırma** bölümünde metin:

    ![OpenAthens etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-identifier.png)

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-openathens-single-sign-on"></a>OpenAthens tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde OpenAthens şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Seçin **bağlantıları** altındaki listeden **Yönetim** sekmesi. 

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application1.png)

3. Seçin **SAML 1.1/2.0**ve ardından **yapılandırma** düğmesi.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application2.png)
    
4. Yapılandırmasını eklemek için seçin **Gözat** Azure portalından indirdiğiniz meta veri .xml dosyasını karşıya yükleme düğmesini ve ardından **Ekle**.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application3.png)

5. Altında aşağıdaki adımları **ayrıntıları** sekmesi.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application4.png)

    a. İçinde **görünen adı eşlemesi**seçin **kullanım özniteliği**.

    b. İçinde **Display name özniteliği** metin kutusunda, değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    c. İçinde **benzersiz kullanıcı eşleme**seçin **kullanım özniteliği**.

    d. İçinde **benzersiz kullanıcı özniteliği** metin kutusunda, değeri girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. İçinde **durumu**, üç onay kutusunu seçin.

    f. İçinde **yerel hesap**seçin **otomatik olarak**.

    g. Seçin **değişiklikleri kaydetmek**.

    h. Gelen **<> / bağlı olan taraf** sekmesinde, kopya **meta veri URL'si** ve bu indirmek için tarayıcıda açın **SP meta veri XML** dosya. Şirket bu SP meta veri dosyası karşıya **temel SAML yapılandırma** Azure AD'de bölümü.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application5.png)

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

Bu bölümde, Azure çoklu oturum açma kullanmak için OpenAthens erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **OpenAthens**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **OpenAthens**.

    ![Uygulamalar listesinde OpenAthens bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-openathens-test-user"></a>OpenAthens test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı OpenAthens oluşturulur. OpenAthens destekler **just-ın-time kullanıcı sağlamayı**, varsayılan olarak etkindir. Bu bölümde, hiçbir eylem öğesini yoktur. Bir kullanıcı OpenAthens içinde zaten mevcut değilse yeni bir kimlik doğrulamasından sonra oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli OpenAthens kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama OpenAthens için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

