---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile EverBridge | Microsoft Docs'
description: Azure Active Directory ve EverBridge arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 886cfc59ed41e25c8c3953888690f58e4cc4c252
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64695327"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Öğretici: EverBridge ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile EverBridge tümleştirme konusunda bilgi edinin.
Azure AD ile EverBridge tümleştirme ile aşağıdaki avantajları sağlar:

* EverBridge erişimi, Azure AD'de kontrol edebilirsiniz.
* Otomatik olarak (çoklu oturum açma) EverBridge için kendi Azure AD hesapları ile oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD Tümleştirmesi ile EverBridge yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa alabileceğiniz bir [ücretsiz hesap](https://azure.microsoft.com/free/)
* Abonelik EverBridge çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* EverBridge destekler **IDP** tarafından başlatılan

## <a name="adding-everbridge-from-the-gallery"></a>Galeriden EverBridge ekleme

Azure AD'de EverBridge tümleştirmesini yapılandırmak için EverBridge Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden EverBridge eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **EverBridge**seçin **EverBridge** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde EverBridge](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma EverBridge adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının EverBridge ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma EverBridge ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[EverBridge Manager portalı çoklu oturum açma olarak EverBridge yapılandırma](#configure-everbridge-as-everbridge-manager-portal-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[EverBridge Manager portalı çoklu oturum açma olarak EverBridge yapılandırma](#configure-everbridge-as-everbridge-member-portal-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
4. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
5. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
6. **[EverBridge test kullanıcısı oluşturma](#create-everbridge-test-user)**  - kullanıcı Azure AD gösterimini bağlı EverBridge Britta simon'un bir karşılığı vardır.
7. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma ile EverBridge yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **EverBridge** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

    >[!NOTE]
    >Manager portalı ya da başka bir deyişle Azure portalı ve Everbridge portalı üzerinde her iki ucunda üye portalı olarak uygulamanın yapılandırmalar yapmanız gerekir.

4. Yapılandırmak için **EverBridge** olarak uygulama **EverBridge Manager portalı**, **temel SAML yapılandırma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![EverBridge etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. İçinde **tanımlayıcı** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://sso.everbridge.net/<API_Name>`

    b. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı ve yanıt URL'si ile güncelleştirin. İlgili kişi [EverBridge Destek ekibine](mailto:support@everbridge.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Yapılandırmak için **EverBridge** olarak uygulama **EverBridge üye portalı**, **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

   * Uygulamada yapılandırmak istiyorsanız **IDP** başlatılan modu:

    ![EverBridge etki alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    * İçinde **tanımlayıcı** metin kutusuna bir URL şu biçimi kullanarak: `https://sso.everbridge.net/<API_Name>/<Organization_ID>`

    * İçinde **yanıt URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`

* Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    ![EverBridge etki alanı ve URL'ler tek oturum açma bilgileri](common/both-signonurl.png)

    * İçinde **oturum açma URL'si** metin kutusuna bir URL şu biçimi kullanarak: `https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek tanımlayıcısı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. İlgili kişi [EverBridge Destek ekibine](mailto:support@everbridge.com) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

6. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **Federasyon meta veri XML**  bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. Üzerinde **EverBridge kümesi** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-everbridge-as-everbridge-manager-portal-single-sign-on"></a>EverBridge EverBridge Manager portalı tek oturum açma yapılandırma

1. SSO için yapılandırılmış almak için **EverBridge** olarak **EverBridge Manager portalı** uygulama, aşağıdaki adımları gerçekleştirin: 
 
2. Farklı bir web tarayıcı penceresinde EverBridge için yönetici olarak oturum açın.

3. Üstteki menüden **ayarları** sekmenize **çoklu oturum açma** altında **güvenlik**.
   
     ![Çoklu oturum açmayı yapılandırın](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
     a. İçinde **adı** metin kimlik sağlayıcısının adını yazın (örneğin: şirketinizin adı).
   
     b. İçinde **API adı** metin API adını yazın.
   
     c. Tıklayın **Dosya Seç** düğmesine Azure portalından indirilen meta veri dosyasını karşıya yükleyin.
   
     d. SAML kimlik konumda seçin **kimliğidir konu deyiminin NameIdentifier öğesinde**.
   
     e. İçinde **kimlik sağlayıcısı oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.
   
     f. Hizmet sağlayıcısı tarafından başlatılan istek bağlamasındaki seçin **HTTP yeniden yönlendirme**.

     g. **Kaydet**’e tıklayın.

### <a name="configure-everbridge-as-everbridge-member-portal-single-sign-on"></a>EverBridge EverBridge üye Portal çoklu oturum açma yapılandırma

Çoklu oturum açmayı yapılandırma **EverBridge** olarak **EverBridge üye portalı**, indirilen göndermem gerekiyor **Federasyon meta verileri XML** için [Everbridge Destek](mailto:support@everbridge.com). Bunlar, her iki kenarı da düzgün ayarlandığından SAML SSO bağlantı sağlamak için bu ayarı ayarlayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. İçinde **adı** alana **BrittaSimon**.
  
    b. İçinde **kullanıcı adı** alan türü `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. Seçin **Show parola** onay kutusunu işaretleyin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için EverBridge erişim vererek Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **EverBridge**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **EverBridge**.

    ![Uygulamalar listesinde EverBridge bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-everbridge-test-user"></a>EverBridge test kullanıcısı oluşturma

Bu bölümde, Britta Simon Everbridge içinde adlı bir kullanıcı oluşturun. Çalışmak [EverBridge Destek ekibine](mailto:support@everbridge.com) Everbridge platform kullanıcıları eklemek için. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce EverBridge etkinleştirildi. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim paneli EverBridge kutucuğa tıkladığınızda, size otomatik olarak SSO'yu ayarlama EverBridge için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

