---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile beğenme | Microsoft Docs'
description: Azure Active Directory ve beğenme arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 50f6762c8046850da1e4541f2ccb7688542f7d54
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098485"
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a>Öğretici: Azure Active Directory Tümleştirmesi ile beğenme

Bu öğreticide, Azure Active Directory (Azure AD) ile beğenme tümleştirme konusunda bilgi edinin.
Beğenme Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Beğenme erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) beğenme için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile beğenme yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik beğenme çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Beğenme destekler **SP** tarafından başlatılan

## <a name="adding-kudos-from-the-gallery"></a>Galeriden beğenme ekleme

Azure AD'de beğenme tümleştirmesini yapılandırmak için beğenme Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden beğenme eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **beğenme**seçin **beğenme** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde beğenme](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma beğenme adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve beğenme ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma beğenme ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Beğenme çoklu oturum açmayı yapılandırma](#configure-kudos-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Beğenme test kullanıcısı oluşturma](#create-kudos-test-user)**  - kullanıcı Azure AD gösterimini bağlı beğenme Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile beğenme yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **beğenme** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Beğenme etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın:  `https://<company>.kudosnow.com`

    > [!NOTE]
    > Değer, gerçek değil. Değerini gerçek oturum açma URL'si ile güncelleştirin. İlgili kişi [beğenme istemci Destek ekibine](http://success.kudosnow.com/home) değeri alınamıyor. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **ayarlayın beğenme** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-kudos-single-sign-on"></a>Beğenme çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde beğenme şirket sitenize yönetici olarak oturum açın.

1. Üstteki menüden **ayarlar simgesine**.

    ![Ayarları](./media/kudos-tutorial/ic787806.png "ayarları")

1. Tıklayın **tümleştirmeler > SSO** ve aşağıdaki adımları gerçekleştirin:

    ![SSO](./media/kudos-tutorial/ic787807.png "SSO")

    a. İçinde **oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    b. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **X.509 sertifikası** metin kutusu

    c. İçinde **için oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. İçinde **beğenme URL'niz** metin şirket adınızı yazın.

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
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için beğenme erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **beğenme**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **beğenme**.

    ![Uygulamalar listesinde beğenme bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-kudos-test-user"></a>Beğenme test kullanıcısı oluşturma

Beğenme için oturum açmak Azure AD kullanıcılarının etkinleştirmek için bunların beğenme sağlanması gerekir. Beğenme söz konusu olduğunda, sağlama bir el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Oturum açın, **beğenme** şirketinizin sitesi yöneticisi olarak.

1. Üstteki menüden **ayarlar simgesine**.

   ![Ayarları](./media/kudos-tutorial/ic787806.png "ayarları")

1. Tıklayın **kullanıcı yönetici**.

1. Tıklayın **kullanıcılar** sekmesine ve ardından **kullanıcı ekleme**.

   ![Kullanıcı Yöneticisi](./media/kudos-tutorial/ic787809.png "Kullanıcı Yöneticisi")

1. İçinde **kullanıcı ekleme** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı ekleme](./media/kudos-tutorial/ic787810.png "kullanıcı ekleme")

    a. Tür **ad**, **Soyadı**, **e-posta** ve diğer ayrıntıları istediğiniz ilgili metin kutularına zbilgisayarlar geçerli bir Azure Active Directory hesabı.

    b. Tıklayın **oluşturacağı**.

> [!NOTE]
> Herhangi diğer beğenme kullanıcı hesabı oluşturma araçları kullanabilir veya API'leri için AAD kullanıcı hesapları sağlamak beğenme tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli beğenme kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlayın beğenme için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
