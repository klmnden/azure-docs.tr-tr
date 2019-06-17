---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Workstars | Microsoft Docs'
description: Azure Active Directory ve Workstars arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 51a4a4e4-ff60-4971-b3f8-a0367b70d220
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: c13c6ab180a172c034d25ac84781f5d3f83ae186
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67086631"
---
# <a name="tutorial-azure-active-directory-integration-with-workstars"></a>Öğretici: Workstars ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Workstars tümleştirme konusunda bilgi edinin.
Azure AD ile Workstars tümleştirme ile aşağıdaki avantajları sağlar:

* Workstars erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Workstars için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Workstars yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Workstars tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Workstars destekler **IDP** tarafından başlatılan

## <a name="adding-workstars-from-the-gallery"></a>Galeriden Workstars ekleme

Azure AD'de Workstars tümleştirmesini yapılandırmak için Workstars Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Workstars eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Workstars**seçin **Workstars** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Workstars](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Workstars adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Workstars ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Workstars ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Workstars çoklu oturum açmayı yapılandırma](#configure-workstars-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Workstars test kullanıcısı oluşturma](#create-workstars-test-user)**  - kullanıcı Azure AD gösterimini bağlı Workstars Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Workstars yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Workstars** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Workstars etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna bir URL yazın: `https://workstars.com`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.workstars.com/saml/login_check`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek yanıt URL'si ile güncelleştirin. İlgili kişi [Workstars istemci Destek ekibine](https://support.workstars.com/) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Workstars kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-workstars-single-sign-on"></a>Workstars tek oturum açmayı yapılandırın

1. Başka bir tarayıcı penceresinde Workstars şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Ana araç çubuğunda tıklatın **ayarları**.

    ![Workstars ayarları](./media/workstars-tutorial/tutorial_workstars_sett.png)

3. Git **oturum** > **ayarları**.

    ![Workstars oturum açma](./media/workstars-tutorial/tutorial_workstars_signon.png)

    ![Workstars ayarları](./media/workstars-tutorial/tutorial_workstars_settings.png)

4. Üzerinde **tek oturum üzerinde (SAML) - ayarları** sayfasında, aşağıdaki adımları gerçekleştirin:
    
    ![Workstars saml](./media/workstars-tutorial/tutorial_workstars_saml.png)

    a. İçinde **kimlik sağlayıcı adı** metin kutusuna **Office 365**.

    b. İçinde **kimlik sağlayıcısı varlık kimliği** metin değerini yapıştırın **Azure AD tanımlayıcısı**, hangi Azure Portalı'ndan kopyaladığınız.

    c. İndirilen sertifika dosyasının içeriği Not Defteri'ne kopyalayın ve ardından yapıştırın **x509 sertifika** metin. 

    d. İçinde **SAML SSO URL** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.
    
    e. İçinde **uzak oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si**, hangi Azure Portalı'ndan kopyaladığınız. 

    f. seçin **ad kimliği** olarak **e-posta (varsayılan)** .

    g. **Onayla**'ya tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için Workstars erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Workstars**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Workstars**.

    ![Uygulamalar listesinde Workstars bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-workstars-test-user"></a>Workstars test kullanıcısı oluşturma

Bu bölümde, Britta Simon Workstars içinde adlı bir kullanıcı oluşturun. Çalışmak [Workstars Destek ekibine](https://support.workstars.com) Workstars platform kullanıcıları eklemek için.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Workstars kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Workstars için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

