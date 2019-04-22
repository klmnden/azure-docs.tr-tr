---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Sprinklr | Microsoft Docs'
description: Azure Active Directory ve Sprinklr arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: 1c3b95686b8c91552615a9014102fd6a14f8c385
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59277077"
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Öğretici: Sprinklr ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Sprinklr tümleştirme konusunda bilgi edinin.
Azure AD ile Sprinklr tümleştirme ile aşağıdaki avantajları sağlar:

* Sprinklr erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) Sprinklr için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile Sprinklr yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik Sprinklr çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Sprinklr destekler **SP** tarafından başlatılan

## <a name="adding-sprinklr-from-the-gallery"></a>Galeriden Sprinklr ekleme

Azure AD'de Sprinklr tümleştirmesini yapılandırmak için Sprinklr Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Sprinklr eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Sprinklr**seçin **Sprinklr** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde Sprinklr](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Sprinklr adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Sprinklr ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Sprinklr ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Sprinklr çoklu oturum açmayı yapılandırma](#configure-sprinklr-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Sprinklr test kullanıcısı oluşturma](#create-sprinklr-test-user)**  - kullanıcı Azure AD gösterimini bağlı Sprinklr Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile Sprinklr yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Sprinklr** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Sprinklr etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.sprinklr.com`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<subdomain>.sprinklr.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. İlgili kişi [Sprinklr istemci Destek ekibine](https://www.sprinklr.com/contact-us/) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Sprinklr kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-sprinklr-single-sign-on"></a>Sprinklr tek oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Sprinklr şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **Yönetim \> ayarları**.

    ![Yönetim](./media/sprinklr-tutorial/ic782907.png "Yönetim")

1. Git **iş ortağı yönetme \> çoklu oturum açma** üzerinde sol bölmeden.

    ![İş ortağı yönetme](./media/sprinklr-tutorial/ic782908.png "iş ortağı yönetme")

1. Tıklayın **+ çoklu oturum açma ekleme**.

    ![Çoklu oturum açmaların](./media/sprinklr-tutorial/ic782909.png "çoklu oturum açmaların")

1. Üzerinde **çoklu oturum açma** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmaların](./media/sprinklr-tutorial/ic782910.png "çoklu oturum açmaların")

    a. İçinde **adı** metin yapılandırmanız için bir ad yazın (örneğin: *WAADSSOTest*).

    b. **Etkin**’i seçin.

    c. Seçin **yeni SSO sertifikası kullanacak**.

    d. Base-64 kodlanmış sertifikanızı Not Defteri'nde açın, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **kimlik sağlayıcısı sertifikası** metin.

    e. Yapıştırma **Azure AD tanımlayıcısı** Azure Portalı'ndan kopyaladığınız değeri **varlık kimliği** metin.

    f. Yapıştırma **oturum açma URL'si** Azure Portalı'ndan kopyaladığınız değeri **kimlik sağlayıcısı oturum açma URL'si** metin.

    g. Yapıştırma **oturum kapatma URL'si** Azure Portalı'ndan kopyaladığınız değeri **kimlik sağlayıcısı oturum kapatma URL'si** metin.

    h. Olarak **SAML kullanıcı kimliği türü**seçin **onaylamayı kullanıcının sprinklr.com kullanıcı adını içeren**.

    i. Olarak **SAML kullanıcı kimliği konumu**seçin **kullanıcı kimliğidir konu deyiminin ad tanımlayıcı öğesinde**.

    j. **Kaydet**’e tıklayın.

    ![SAML](./media/sprinklr-tutorial/ic782911.png "SAML")

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

Bu bölümde, Azure çoklu oturum açma kullanmak için Sprinklr erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Sprinklr**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Sprinklr**.

    ![Uygulamalar listesinde Sprinklr bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-sprinklr-test-user"></a>Sprinklr test kullanıcısı oluşturma

1. Sprinklr şirketinizin sitesi için bir yönetici olarak oturum açın.

1. Git **Yönetim \> ayarları**.

    ![Yönetim](./media/sprinklr-tutorial/ic782907.png "Yönetim")

1. Git **yönetme istemci \> kullanıcılar** sol bölmeden.

    ![Ayarları](./media/sprinklr-tutorial/ic782914.png "ayarları")

1. Tıklayın **kullanıcı ekleme**.

    ![Ayarları](./media/sprinklr-tutorial/ic782915.png "ayarları")

1. Üzerinde **düzenleme kullanıcı** iletişim kutusunda, aşağıdaki adımları gerçekleştirin:

    ![Kullanıcıyı Düzenle](./media/sprinklr-tutorial/ic782916.png "düzenleme kullanıcı")

    a. İçinde **e-posta**, **ad** ve **Soyadı** metin kutuları, sağlamak istediğiniz bir Azure AD kullanıcı hesabı bilgilerini yazın.

    b. Seçin **parola devre dışı**.

    c. Seçin **dil**.

    d. Seçin **kullanıcı türü**.

    e. Tıklayın **güncelleştirme**.

    > [!IMPORTANT]
    > **Parola devre dışı** bir kimlik sağlayıcısı ile oturum açmak bir kullanıcı seçilmesi gerekir. 

1. Git **rol**ve ardından aşağıdaki adımları gerçekleştirin:

    ![İş ortağı rolleri](./media/sprinklr-tutorial/ic782917.png "ortak rolleri")

    a. Gelen **genel** listesinden **ALL_Permissions**.  

    b. Tıklayın **güncelleştirme**.

> [!NOTE]
> Herhangi diğer Sprinklr kullanıcı hesabı oluşturma araçları kullanabilir veya API Azure AD'ye kullanıcı hesapları sağlamak için Sprinklr tarafından sağlanan.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli Sprinklr kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama Sprinklr için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
