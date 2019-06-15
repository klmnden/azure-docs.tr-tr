---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Yazı tahtası öğrenin - Shibboleth | Microsoft Docs'
description: Azure Active Directory ve Yazı tahtası öğrenin - Shibboleth arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e435cbb4-c0f0-400e-943c-5c923fa8ddf2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2e409733f3f84df04d1f863340c28b4ea508179
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106187"
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a>Öğretici: Yazı tahtası öğrenin - Shibboleth ile Azure Active Directory Tümleştirme

Bu öğreticide, Yazı tahtası öğrenin - Shibboleth Azure Active Directory (Azure AD) ile tümleştirmeyi öğrenin.
Yazı tahtası öğrenin - Shibboleth Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:

* Yazı tahtası öğrenin - Shibboleth erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak Yazı tahtası öğrenin - Shibboleth (çoklu oturum açma) ile kendi Azure AD hesapları için oturum açmış, kullanıcılarınızın etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Yazı tahtası öğrenin - Shibboleth ile yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Yazı tahtası öğrenin - aboneliği etkin Shibboleth çoklu oturum açma

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Yazı tahtası öğrenin - Shibboleth destekler **SP** tarafından başlatılan

## <a name="adding-blackboard-learn---shibboleth-from-the-gallery"></a>Shibboleth galerisinden Yazı tahtası öğrenin - ekleme

Yazı tahtası öğrenin - Azure AD'ye Shibboleth tümleştirmesini yapılandırmak için Yazı tahtası öğrenin - galerisinden Shibboleth yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Shibboleth galerisinden Yazı tahtası öğrenin - eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Yazı tahtası öğrenin - Shibboleth**seçin **Yazı tahtası öğrenin - Shibboleth** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

    ![Yazı tahtası öğrenin - Shibboleth sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Yazı tahtası bilgi ile test etme - Shibboleth adlı bir test kullanıcı tabanlı **Britta Simon**.
Shibboleth tek oturum açma için iş, bir Azure AD kullanıcısı ile ilgili Yazı tahtası öğrenin - kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Yazı tahtası öğrenin ile-test etme Shibboleth, aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Shibboleth çoklu oturum açma Yazı tahtası öğrenin - yapılandırma](#configure-blackboard-learn---shibboleth-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Yazı tahtası öğrenin - Shibboleth test kullanıcısı oluşturma](#create-blackboard-learn---shibboleth-test-user)**  - Yazı tahtası öğrenin - kullanıcı Azure AD gösterimini bağlı Shibboleth Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Yazı tahtası öğrenin - Shibboleth ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Yazı tahtası öğrenin - Shibboleth** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Yazı tahtası öğrenin - Shibboleth etki alanı ve URL'ler çoklu oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/Login`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `https://<yourblackoardlearnserver>.blackboardlearn.com/shibboleth-sp`

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/SAML2/POST`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Yazı tahtası öğrenin - Shibboleth istemci Destek ekibine](https://www.blackboard.com/forms/contact-us_form.aspx) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. Üzerinde **Yazı tahtası öğrenin - Shibboleth kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-blackboard-learn---shibboleth-single-sign-on"></a>Yazı tahtası yapılandırma öğrenin - Shibboleth çoklu oturum açma

Çoklu oturum açmayı yapılandırma **Yazı tahtası öğrenin - Shibboleth** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** ve uygun Azure portalına kopyalanan URL'lerden [Yazı tahtası Bilgi - Shibboleth Destek ekibine](https://www.blackboard.com/forms/contact-us_form.aspx). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

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

Bu bölümde, erişim izni verme Yazı tahtası öğrenin - Shibboleth Azure çoklu oturum açmayı kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Yazı tahtası öğrenin - Shibboleth**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Yazı tahtası öğrenin - Shibboleth**.

    ![-Uygulamalar listesini Shibboleth bağlantıyı Yazı tahtası öğrenin](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-blackboard-learn---shibboleth-test-user"></a>Yazı tahtası öğrenin - Shibboleth test kullanıcısı oluşturma

Bu bölümde, Britta Simon Yazı tahtası öğrenin - Shibboleth içinde adlı bir kullanıcı oluşturun. Çalışmak [Yazı tahtası öğrenin - Shibboleth Destek ekibine](https://www.blackboard.com/forms/contact-us_form.aspx) Yazı tahtası öğrenin - Shibboleth platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Yazı tahtası öğrenin - erişim Paneli'nde Shibboleth kutucuğa tıkladığınızda, otomatik olarak Yazı tahtası öğrenin - Shibboleth SSO'yu ayarlamak için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
