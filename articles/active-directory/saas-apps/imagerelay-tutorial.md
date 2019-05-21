---
title: 'Öğretici: Görüntü geçişi ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve görüntü geçiş arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d2d41af8fa04b03ab8d18277d377f3700575cd1
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65898149"
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Öğretici: Görüntü geçişi ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile görüntü geçiş tümleştirme konusunda bilgi edinin.
Görüntü geçişi, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Görüntü geçişi erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) görüntü geçiş için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile görüntü geçişi yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Görüntü geçiş çoklu oturum açmayı abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Geçişi destekler görüntü **SP** tarafından başlatılan

## <a name="adding-image-relay-from-the-gallery"></a>Galeriden görüntü geçiş ekleniyor

Azure AD'de görüntü geçiş tümleştirmesini yapılandırmak için yönetilen SaaS uygulamalar listesine Galeriden görüntü geçiş eklemeniz gerekir.

**Galeriden görüntü geçiş eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **görüntü geçiş**seçin **görüntü geçiş** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Sonuç listesinde görüntü geçiş](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma görüntü adlı bir test kullanıcı tabanlı geçişi test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve görüntü geçiş ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma görüntü geçişi ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Görüntü geçiş çoklu oturum açmayı yapılandırma](#configure-image-relay-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Görüntü geçiş test kullanıcısı oluşturma](#create-image-relay-test-user)**  - kullanıcı Azure AD gösterimini bağlantılı görüntü geçiş Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile görüntü geçişi yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **görüntü geçiş** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Görüntü geçiş etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.imagerelay.com/`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [görüntü geçiş istemci Destek ekibine](http://support.imagerelay.com/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Set görüntü Relay** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-image-relay-single-sign-on"></a>Görüntü geçiş çoklu oturum açmayı yapılandırın

1. Başka bir tarayıcı penceresinde görüntü geçiş şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Üst araç çubuğunda tıklatın **kullanıcıları ve izinleri** iş yükü.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_06.png) 

3. Tıklayın **yeni bir izin oluşturmak**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_08.png)

4. İçinde **tek oturum açma ayarları** iş yükü, select **bu grubu için yalnızca oturum açma aracılığıyla çoklu oturum açma** onay kutusunu işaretleyin ve ardından **Kaydet**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_09.png) 

5. Git **hesap ayarları**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_10.png) 

6. Git **tek oturum açma ayarları** iş yükü.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_11.png)

7. Üzerinde **SAML ayarlarını** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_12.png)

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    b. İçinde **oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. Olarak **ad kimliği biçimi**seçin **urn: OASIS: adları: tc: SAML:1.1:nameid-biçimi: emailAddress**.

    d. Olarak **hizmet sağlayıcısı (görüntü geçiş) gelen istekleri için bağlama seçeneklerini**seçin **POST bağlama**.

    e. Altında **x.509 sertifikası**, tıklayın **güncelleştirme sertifika**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. İndirilen sertifikanın Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **x.509 sertifikası** metin.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. İçinde **kullanıcı tam zamanında sağlama** bölümünden **tam zamanında kullanıcı sağlamayı etkinleştirin**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. İzin grubu seçin (örneğin, **SSO temel**) yalnızca çoklu oturum açma oturum açmak için izin.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. **Kaydet**’e tıklayın.

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

Bu bölümde, görüntü geçiş için erişim izni verdiğinizde, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **görüntü geçiş**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **görüntü geçiş**.

    ![Uygulamalar listesinde görüntü geçiş bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-image-relay-test-user"></a>Görüntü geçiş test kullanıcısı oluşturma

Bu bölümün amacı, görüntü geçiş Britta Simon adlı bir kullanıcı oluşturmaktır.

**Britta Simon görüntü geçiş adlı bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Görüntü geçiş şirketinizin sitesi için bir yönetici olarak oturum.

2. Git **kullanıcıları ve izinleri** seçip **SSO kullanıcı oluştur**.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. Girin **e-posta**, **ad**, **Soyadı**, ve **şirket** sağlama ve (için izin grubu seçmek istediğiniz kullanıcının Örneğin, temel SSO) yalnızca çoklu oturum açma oturum grup olduğu.

    ![Çoklu oturum açmayı yapılandırın](./media/imagerelay-tutorial/tutorial_imagerelay_22.png)

4. **Oluştur**’a tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde görüntü geçiş kutucuğa tıkladığınızda, otomatik olarak görüntü SSO'yu ayarlama geçiş için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)