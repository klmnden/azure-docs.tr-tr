---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile HRworks çoklu oturum açma | Microsoft Docs'
description: Azure Active Directory ve HRworks çoklu oturum açma arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c4c5d434-3f8a-411e-83a5-c3d5276ddc0a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 06a10cab81b1253658f505b3cd3f2c520ef9cea8
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266350"
---
# <a name="tutorial-azure-active-directory-integration-with-hrworks-single-sign-on"></a>Öğretici: Azure Active Directory Tümleştirmesi ile HRworks çoklu oturum açma

Bu öğreticide, HRworks çoklu oturum açma, Azure Active Directory (Azure AD) ile tümleştirme konusunda bilgi edinin.
HRworks çoklu oturum açma, Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Erişimi için HRworks çoklu oturum açma, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak HRworks çoklu oturum açma (çoklu oturum açma) ile Azure AD hesaplarına oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi HRworks çoklu oturum açma ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik HRworks çoklu oturum açma çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* HRworks çoklu oturum açmayı destekleyen **SP** tarafından başlatılan

## <a name="adding-hrworks-single-sign-on-from-the-gallery"></a>Galeriden HRworks çoklu oturum açma ekleme

Azure AD tümleştirilmesi, HRworks çoklu oturum açmayı yapılandırmak için HRworks çoklu oturum açma Galeriden, yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden HRworks çoklu oturum açma eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **HRworks çoklu oturum açma**seçin **HRworks çoklu oturum açma** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![HRworks çoklu oturum açma sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açmayı test etme ile HRworks tek temelinde oturum açma adlı bir test kullanıcı **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ile ilgili kullanıcı içinde HRworks çoklu oturum açma arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma ile HRworks çoklu oturum açmayı test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[HRworks tek oturum açma çoklu oturum açma yapılandırma](#configure-hrworks-single-sign-on-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[HRworks çoklu oturum açmayı test kullanıcısı oluşturma](#create-hrworks-single-sign-on-test-user)**  - içinde HRworks Çoklu kullanıcı Azure AD gösterimini bağlantılı oturum Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma HRworks çoklu oturum açma ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **HRworks çoklu oturum açma** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![HRworks tek oturum açma etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://login.hrworks.de/?companyId=<companyId>&directssologin=true`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [oturum açma HRworks tek istemci Destek ekibine](mailto:nadja.sommerfeld@hrworks.de) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **HRworks tek oturum açma kurulumunu** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-hrworks-single-sign-on-single-sign-on"></a>HRworks tek oturum açma çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde HRworks çoklu oturum açma için yönetici olarak oturum açın.

2. Tıklayarak **yönetici** > **Temelleri** > **güvenlik** > **çoklu oturum açma** gelen sol tarafındaki menü çubuğu ve aşağıdaki adımları gerçekleştirin:

       ![Çoklu oturum açmayı yapılandırın](./media/hrworks-single-sign-on-tutorial/configure01.png)

    a. Denetleme **kullanım çoklu oturum açma** kutusu.

    b. Seçin **XML meta veri** olarak **yöntemi giriş Meta verileri**.

    c. Seçin **bireysel Nameıd tanımlayıcı** olarak **Nameıd değeri**.

    d. Not Defteri'nde, Azure portalından indirdiğiniz meta veri XML açın, içeriğini kopyalayın ve ardından yapıştırın **meta verileri** metin.

    e. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alanına **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alanında, kullanıcı adı gibi yazın BrittaSimon@contoso.com.

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma HRworks çoklu oturum açma için erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **HRworks çoklu oturum açma**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **HRworks çoklu oturum açma**.

    ![Uygulamalar listesinde HRworks çoklu oturum açma bağlantısı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-hrworks-single-sign-on-test-user"></a>HRworks çoklu oturum açmayı test kullanıcısı oluşturma

Azure AD kullanıcılarının etkinleştirmek için oturum içinde HRworks çoklu oturum açma, bunlar HRworks çoklu oturum açma ile sağlanması gerekir. HRworks çoklu oturum açma içinde sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. HRworks tek oturum açmaya yönetici olarak oturum açın.

2. Tıklayarak **yönetici** > **kişiler** > **kişiler** > **yeni kişi** gelen Menü çubuğunda sol tarafındaki.

     ![Çoklu oturum açmayı yapılandırın](./media/hrworks-single-sign-on-tutorial/configure02.png)

3. Açılan tıklayın **sonraki**.

    ![Çoklu oturum açmayı yapılandırın](./media/hrworks-single-sign-on-tutorial/configure03.png)

4. Üzerinde **ülke yasal koşullar için oluştur yeni kişiyle** ilgili ayrıntıları gibi açılır, dolgu **ad**, **Soyadı** tıklatıp **Oluştur**.
    
    ![Çoklu oturum açmayı yapılandırın](./media/hrworks-single-sign-on-tutorial/configure04.png)

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli HRworks çoklu oturum açma kutucuğa tıkladığınızda, size otomatik olarak için HRworks tek SSO'yu ayarlama oturum oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

