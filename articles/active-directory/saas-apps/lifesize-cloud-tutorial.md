---
title: 'Öğretici: Azure Active Directory Tümleştirmesi Lifesize bulutla | Microsoft Docs'
description: Azure Active Directory ve Lifesize bulut arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f751eb9dfb8ef65bea80993ddbd3a682b1d47111
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098051"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Öğretici: Lifesize bulut ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Lifesize bulut tümleştirme konusunda bilgi edinin.
Azure AD ile Lifesize bulut tümleştirme ile aşağıdaki avantajları sağlar:

* Lifesize Bulutuna erişimi olan Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Lifesize buluta oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Lifesize Bulutla yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Lifesize bulut çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Lifesize bulutun desteklediği **SP** tarafından başlatılan

* Lifesize bulutun desteklediği **otomatik** kullanıcı sağlama

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Galeriden Lifesize bulut ekleme

Azure AD'de Lifesize bulut tümleştirmesini yapılandırmak için Lifesize bulut Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Lifesize bulut eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Lifesize bulut**seçin **Lifesize bulut** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Lifesize bulut](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırdığınız ve Azure AD çoklu oturum açmayı test Lifesize bulutla adlı bir test kullanıcı tabanlı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı Lifesize bulutta arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Lifesize Bulutu ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Lifesize bulut çoklu oturum açmayı yapılandırma](#configure-lifesize-cloud-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Test kullanıcısı Lifesize bulut oluşturma](#create-lifesize-cloud-test-user)**  - kullanıcı Azure AD gösterimini bağlı Lifesize bulutta Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Lifesize Bulutla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Lifesize bulut** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Lifesize bulut etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-relay.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://login.lifesizecloud.com/ls/?acs`

    b. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://login.lifesizecloud.com/<companyname>`

    c. Tıklayın **ek URL'lerini ayarlayın**.

    d. İçinde **geçiş durumu** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://webapp.lifesizecloud.com/?ent=<identifier>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ile geçiş durumunu güncelleştirin. İlgili kişi [Lifesize bulut istemci Destek ekibine](https://www.lifesize.com/en/support) oturum açma URL'si ve tanımlayıcı değerlerini ve almak için geçiş durumu değeri SSO yapılandırmasından öğreticinin ilerleyen bölümlerinde açıklanan alabilirsiniz. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Lifesize bulut kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-lifesize-cloud-single-sign-on"></a>Lifesize bulut çoklu oturum açmayı yapılandırın

1. Uygulamanız, yönetici ayrıcalıklarıyla Lifesize bulut uygulamasına oturum açma için yapılandırılmış SSO edinmek için.

2. Sağ üst köşede adınıza tıklayın ve ardından **Gelişmiş ayarlar**.

    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

3. Gelişmiş ayarlar artık tıklayarak **SSO yapılandırma** bağlantı. Bu, Örneğiniz için SSO yapılandırma sayfası açılır.

    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

4. Artık SSO Yapılandırması kullanıcı Arabirimi aşağıdaki değerleri yapılandırın.

    ![Çoklu oturum açmayı yapılandırın](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    a. İçinde **kimlik sağlayıcısını veren** metin değerini yapıştırın **Azure Ad tanımlayıcısı** , Azure Portalı'ndan kopyaladığınız.

    b.  İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. Base-64 kodlanmış sertifikanızı Azure portalından indirdiğiniz Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin.
  
    d. Ad metin kutusu için SAML öznitelik eşlemelerini değer olarak girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    e. SAML özniteliği eşlemesinde için **Soyadı** metin kutusuna bir değer olarak girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`

    f. SAML özniteliği eşlemesinde için **e-posta** metin kutusuna bir değer olarak girin `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

5. Tıklayabilirsiniz yapılandırmayı kontrol etmek için **Test** düğmesi.

    >[!NOTE]
    >Başarılı test etmek için Azure AD'de Yapılandırma Sihirbazı'nı tamamlamak ve ayrıca kullanıcılara veya test gerçekleştirebilirsiniz gruplara erişim sağlamak gerekir.

6. Üzerinde kontrol ederek SSO etkinleştirme **SSO etkinleştirme** düğmesi.

7. Artık tıklayarak **güncelleştirme** böylece tüm ayarları kaydedildi. Bu RelayState değeri oluşturur. Kopyalama metin kutusuna oluşturulan RelayState değeri içinde yapıştırın **geçiş durumu** metin kutusunun altında **Lifesize bulut etki alanı ve URL'ler** bölümü.

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

Bu bölümde, Azure çoklu oturum açma Lifesize buluta erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Lifesize bulut**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Lifesize bulut**.

    ![Uygulamalar listesinde Lifesize bulut bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-lifesize-cloud-test-user"></a>Test kullanıcısı Lifesize bulut oluşturma

Bu bölümde, Britta Simon Lifesize bulutta adlı bir kullanıcı oluşturun. Otomatik kullanıcı hazırlama Lifesize bulut desteklemiyor. Azure AD, başarılı kimlik doğrulamasından sonra kullanıcı uygulamayı otomatik olarak sağlanır.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Lifesize bulut kutucuğa tıkladığınızda, oturum açma sayfası Lifesize bulut uygulamasının almanız gerekir. Burada, kullanıcı adınızı girmeniz gerekir ve bundan sonra yeniden yönlendirilen uygulama giriş sayfasına.

Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
