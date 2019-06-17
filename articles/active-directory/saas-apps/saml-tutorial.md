---
title: 'Öğretici: SAML 1.1 belirteci ile Azure Active Directory Tümleştirmesi etkinleştirilmiş LOB uygulaması | Microsoft Docs'
description: Azure Active Directory arasında çoklu oturum açmayı yapılandırma hakkında bilgi edinin ve SAML 1.1 belirteci etkin bir LOB uygulaması.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ced1d88d-0e48-40d5-9aea-ef991cd9d270
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2cc3f8389eb0b98da5c172adf65ff4dae38ca29d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67092002"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Öğretici: SAML 1.1 belirteci ile Azure Active Directory Tümleştirmesi etkin bir LOB uygulaması

Bu öğreticide, şunların nasıl tümleştireceğinizi SAML 1.1 belirteci LOB uygulaması Azure Active Directory (Azure AD) ile etkin.
SAML 1.1 belirteci tümleştirme etkin LOB uygulamanızı Azure AD ile aşağıdaki faydaları sağlar:

* Azure'da denetleyebilirsiniz SAML 1.1 belirteç erişimi olan AD etkin LOB uygulaması.
* Kullanıcılarınızın otomatik olarak oturum açma için etkinleştirebilirsiniz. SAML 1.1 belirteci özellikli LOB (çoklu oturum açma) ile kendi Azure AD hesapları.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure'u yapılandırmak için SAML 1.1 belirteci AD tümleştirmesiyle etkin LOB uygulaması, aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* SAML 1.1 belirteci LOB uygulaması tek oturum açma etkin abonelik etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* SAML 1.1 belirteci etkin LOB uygulamanın desteklediği **SP** tarafından başlatılan

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>Etkin LOB uygulaması Galerisi SAML 1.1 belirteci ekleme

SAML 1.1 belirteci tümleştirmesini yapılandırmak için Azure AD ile LOB uygulaması etkin, SAML 1.1 belirteci etkin LOB uygulaması Galerisi yönetilen SaaS listenizden eklemeniz gerekir.

**SAML 1.1 belirteci etkin galerisinden LOB uygulaması eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAML 1.1 belirteci etkin LOB uygulaması**seçin **SAML 1.1 belirteci etkin LOB uygulaması** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuçlar listesinde etkin LOB uygulaması SAML 1.1 belirteci](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırmanız ve SAML 1.1 belirteci ile Azure AD çoklu oturum açmayı test etkin adlı bir test kullanıcı tabanlı uygulama LOB **Britta Simon**.
Tek iş için oturum açma için LOB uygulaması kurulması gerekiyorsa bir Azure AD kullanıcısı ile ilgili kullanıcı SAML 1.1 belirteç arasında bir bağlantı ilişki etkin.

Yapılandırmanıza ve sınamanıza Azure AD çoklu oturum açma SAML 1.1 belirteci ile etkin LOB uygulaması, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Yapılandırma LOB uygulamasını Çoklu oturum açma SAML 1.1 belirteci etkin](#configure-saml-11-token-enabled-lob-app-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[SAML 1.1 etkin belirteci oluşturma LOB uygulamanızı test kullanıcısı](#create-saml-11-token-enabled-lob-app-test-user)**  - olmasını Britta simon'un SAML 1.1 belirteç içinde bir karşılığı etkin kullanıcı Azure AD gösterimini bağlı LOB uygulaması.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure'u yapılandırmak için AD çoklu oturum açma SAML 1.1 belirteci ile LOB uygulaması etkin, aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **SAML 1.1 belirteci etkin LOB uygulaması** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![SAML 1.1 belirteci LOB uygulama etki alanı ve URL'ler tek oturum açma bilgisi etkin](common/sp-identifier.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://your-app-url`

    b. İçinde **tanımlayıcı (varlık kimliği)** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://your-app-url`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL ve tanımlayıcıdır ile güncelleştirin. Bu değerleri almak LOB uygulaması istemci Destek ekibiyle iletişim SAML 1.1 belirteci etkin. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **etkin LOB uygulamanızı kurma SAML 1.1 belirteci** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-saml-11-token-enabled-lob-app-single-sign-on"></a>Yapılandırma SAML 1.1 belirteci etkin LOB uygulamasını Çoklu oturum açma

Çoklu oturum açmayı yapılandırma **SAML 1.1 belirteci etkin LOB uygulaması** tarafı, indirilen göndermek için ihtiyacınız **sertifika (Base64)** ve Azure Portalı'ndan uygun kopyalanan URL'leri SAML 1.1 belirteç etkin LOB uygulaması Takım destekler. Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

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

Bu bölümde, Azure'ı kullanmak Britta Simon etkinleştirme çoklu erişim SAML 1.1 belirteç verme ile oturum etkin LOB uygulaması.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **SAML 1.1 belirteci etkin LOB uygulaması**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde yazın ve **SAML 1.1 belirteci etkin LOB uygulaması**.

    ![SAML 1.1 belirteci uygulamalar listesinde LOB uygulaması bağlantı etkin](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-saml-11-token-enabled-lob-app-test-user"></a>SAML 1.1 etkin belirteci oluşturma LOB uygulamanızı test kullanıcısı

Bu bölümde, Britta Simon adlı bir kullanıcı oluşturma SAML 1.1 belirteci etkin LOB uygulaması. İş SAML 1.1 belirteci ile LOB uygulaması Destek ekibine SAML 1.1 belirteci kullanıcılar LOB uygulaması platformu etkin eklemek etkin. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

' A tıkladığınızda SAML 1.1 belirteci etkin erişim panelinde LOB uygulama kutucuğunda, otomatik olarak oturum açmış olmanıza SAML 1.1 belirteci LOB uygulamanızı SSO'yu ayarladığınız etkin. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

