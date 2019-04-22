---
title: 'Öğretici: Küçük geliştirme ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve küçük geliştirmeler arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 59c8a112-41e1-4337-9ef3-3d7029780d61
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 19d9624c5bb60f47ef4bfa1b0629327780c2a9c7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59266313"
---
# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a>Öğretici: Küçük geliştirme ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile küçük iyileştirmeler tümleştirme konusunda bilgi edinin.
Küçük geliştirme Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Küçük iyileştirmeler erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak küçük iyileştirmeleri (çoklu oturum açma) oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile küçük iyileştirmeler yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Küçük iyileştirmeler tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Küçük iyileştirmeler destekler **SP** tarafından başlatılan

## <a name="adding-small-improvements-from-the-gallery"></a>Galeriden küçük geliştirmeler ekleme

Azure AD'ye küçük iyileştirmeler tümleştirmesini yapılandırmak için küçük geliştirme Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden küçük iyileştirmeler eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **küçük iyileştirmeler**seçin **küçük iyileştirmeler** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde küçük iyileştirmeler](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma küçük adlı bir test kullanıcı tabanlı geliştirme ile test etme **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı küçük geliştirmeleri arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma küçük geliştirme ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Küçük iyileştirmeler çoklu oturum açmayı yapılandırma](#configure-small-improvements-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Küçük iyileştirmeler test kullanıcısı oluşturma](#create-small-improvements-test-user)**  - kullanıcı Azure AD gösterimini bağlı küçük geliştirmeleri Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile küçük iyileştirmeler yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **küçük iyileştirmeler** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bilgileri küçük geliştirmeler etki alanı ve URL'leri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.small-improvements.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.small-improvements.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [küçük iyileştirmeler istemci Destek ekibine](mailto:support@small-improvements.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **küçük iyileştirmeler kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-small-improvements-single-sign-on"></a>Küçük iyileştirmeler çoklu oturum açmayı yapılandırın

1. Başka bir tarayıcı penceresinde küçük iyileştirmeler şirketinizin sitesi için yönetici olarak oturum açın.

1. Pano ana sayfasında **Yönetim** soldaki düğme.

    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

1. Tıklayın **SAML SSO** düğmesini **tümleştirmeler** bölümü.

    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_07.png) 

1. SSO Kurulumu sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırın](./media/smallimprovements-tutorial/tutorial_smallimprovements_08.png)  

    a. İçinde **HTTP uç noktası** metin değerini yapıştırın **oturum açma URL'si**, hangi Azure Portalı'ndan kopyaladığınız.

    b. İndirilen sertifikanızı Not Defteri'nde açın, içeriği kopyalayın ve ardından yapıştırın **x509 sertifika** metin. 

    c. SSO ve oturum açma form kimlik doğrulaması seçeneği kullanıcılar için kullanılabilir olmasını istiyorsanız, daha sonra kontrol **oturum açma/parola ile çok erişmesini** seçeneği.  

    d. SSO oturum açma düğmesi olarak adlandırmak için uygun değeri girin **SAML istemi** metin.  

    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü **brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, küçük iyileştirmeler erişim vererek, Azure çoklu oturum açma kullanılacak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **küçük iyileştirmeler**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **küçük iyileştirmeler**.

    ![Uygulamalar listesini küçük geliştirme bağlantıdaki](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-small-improvements-test-user"></a>Küçük iyileştirmeler test kullanıcısı oluşturma

Küçük iyileştirmeler oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunlar küçük artışlarını sağlanması gerekir. Küçük iyileştirmeler söz konusu olduğunda sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Küçük iyileştirmeler şirketinizin sitesi için bir yönetici olarak oturum.

1. Giriş sayfasından sol tıklayın menüsüne gidin **Yönetim**.

1. Tıklayın **kullanıcı dizini** kullanıcı yönetim bölümünden düğmesi.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

1. Tıklayın **kullanıcı ekleme**.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

1. Üzerinde **Add Users** iletişim kutusunda, aşağıdaki adımları gerçekleştirin: 

    ![Bir Azure AD test kullanıcısı oluşturma](./media/smallimprovements-tutorial/tutorial_smallimprovements_12.png)

    a. Girin **ad** gibi kullanıcının **Britta**.

    b. Girin **Soyadı** gibi kullanıcının **Simon**.

    c. Girin **e-posta** gibi kullanıcının **brittasimon@contoso.com**.

    d. Ayrıca, kişisel bir ileti girin seçebilirsiniz **bildirim e-posta Gönder** kutusu. Sonra bildirim göndermesini istemiyorsanız, bu onay kutusunun işaretini kaldırın.

    e. Tıklayın **kullanıcılar oluşturma**.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli küçük iyileştirmeler kutucuğa tıkladığınızda, size otomatik olarak küçük SSO'yu ayarlama geliştirmeleri için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)