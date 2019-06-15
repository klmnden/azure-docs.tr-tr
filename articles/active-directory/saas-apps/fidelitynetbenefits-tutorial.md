---
title: 'Öğretici: Uygunluk NetBenefits ile Azure Active Directory Tümleştirme | Microsoft Docs'
description: Azure Active Directory ve uygunluk NetBenefits arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 77dc8a98-c0e7-4129-ab88-28e7643e432a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ce37aea9e700907ebfda9aa181b7f0eb638af35
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102724"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>Öğretici: Uygunluk NetBenefits ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile uygunluk NetBenefits tümleştirme konusunda bilgi edinin.
Azure AD ile bir doğruluk NetBenefits tümleştirme ile aşağıdaki avantajları sağlar:

* Uygunluk NetBenefits erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) için uygunluk NetBenefits oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile uygunluk NetBenefits yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Uygunluk NetBenefits tek oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Uygunluk NetBenefits destekler **IDP** tarafından başlatılan

* Uygunluk NetBenefits destekler **zamanında** kullanıcı sağlama

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>Galeriden uygunluk NetBenefits ekleme

Azure AD'de uygunluk NetBenefits tümleştirmesini yapılandırmak için uygunluk NetBenefits Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden uygunluk NetBenefits eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **uygunluk NetBenefits**seçin **uygunluk NetBenefits** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde uygunluk NetBenefits](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma uygunluk adlı bir test kullanıcı tabanlı NetBenefits test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısı ve uygunluk NetBenefits ilgili kullanıcı arasında bir bağlantı ilişki kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma uygunluk NetBenefits ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Uygunluk NetBenefits çoklu oturum açmayı yapılandırma](#configure-fidelity-netbenefits-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Uygunluk NetBenefits test kullanıcısı oluşturma](#create-fidelity-netbenefits-test-user)**  - kullanıcı Azure AD gösterimini bağlıdır kalitesini NetBenefits Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile uygunluk NetBenefits yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **uygunluk NetBenefits** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Uygunluk NetBenefits etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın:

    Test ortamı için:  `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    Üretim ortamı için:  `urn:sp:fidelity:geninbndnbparts20`

    b. İçinde **yanıt URL'si** metin kutusuna bir URL yazın, uygulama zamanında uygunluk tarafından sağlanması ya da atanan uygunluk istemci hizmet yöneticinize başvurun.

5. Uygunluk NetBenefits uygulama, özel öznitelik eşlemelerini SAML belirteci öznitelikleri yapılandırmanıza ekleyin gerektiren belirli bir biçimde SAML onaylamalarını bekler. Varsayılan öznitelikler listesinde aşağıdaki ekran görüntüsünde gösterilmektedir oysa **NameIdentifier** ile eşlenmiş **user.userprincipalname**. Uygunluk NetBenefits uygulama bekliyor **NameIdentifier** ile eşlenecek **EmployeeID** veya, kuruluşunuz için uygun olan diğer talep **NameIdentifier**tıklayarak özellik eşlemesi düzenlemeniz gerekir böylece **Düzenle** simgesi ve değişiklik öznitelik eşlemesi.

    ![image](common/edit-attribute.png)

    >[!Note]
    >Statik ve dinamik Federasyon uygunluk NetBenefits destekler. Statik süre kullanıcı yalnızca sağlama destekler yalnızca zaman kullanıcı sağlama ve dinamik anlamına gelir. temel SAML kullanmaz anlamına gelir. JIT kullanmak için temel sağlama müşteriler Azure AD'de kullanıcının doğum tarihi vb. gibi bazı çok talep eklemeniz gerekir. Tarafından sağlanan bu ayrıntıları, atanan **uygunluk istemci Service Manager** ve bunlar bu dinamik Federasyon Örneğiniz için etkinleştirmeniz gerekir.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **uygunluk NetBenefits kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure Ad tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-fidelity-netbenefits-single-sign-on"></a>Uygunluk NetBenefits tek oturum açmayı yapılandırın

Çoklu oturum açmayı yapılandırma **uygunluk NetBenefits** tarafı, indirilen göndermek için ihtiyacınız **Federasyon meta verileri XML** ve uygun Azure portalına kopyalanan URL'lerden [uygunluk NetBenefits Destek ekibine](mailto:SSOMaintenance@fmr.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

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

Bu bölümde, Azure çoklu oturum açma kullanmak için uygunluk NetBenefits erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **uygunluk NetBenefits**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **uygunluk NetBenefits**.

    ![Uygulamalar listesinde uygunluk NetBenefits bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-fidelity-netbenefits-test-user"></a>Uygunluk NetBenefits test kullanıcısı oluşturma

Bu bölümde, Britta Simon uygunluk NetBenefits adlı bir kullanıcı oluşturun. Statik Federasyon oluşturuyorsanız, Lütfen, atanan iş **uygunluk istemci Service Manager** kullanıcıları uygunluk NetBenefits platformu oluşturmak için. Bu kullanıcılar oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi.

Dinamik Federasyon için kullanıcılar, kullanıcı tam zamanında sağlama kullanılarak oluşturulur. JIT kullanmak için temel sağlama müşteriler Azure AD'de kullanıcının doğum tarihi vb. gibi bazı çok talep eklemeniz gerekir. Tarafından sağlanan bu ayrıntıları, atanan **uygunluk istemci Service Manager** ve bunlar bu dinamik Federasyon Örneğiniz için etkinleştirmeniz gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli uygunluk NetBenefits kutucuğa tıkladığınızda, size otomatik olarak uygunluk SSO'yu ayarlama NetBenefits için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

