---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile Igloo yazılım | Microsoft Docs'
description: Azure Active Directory ve Igloo yazılım arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/06/2019
ms.author: jeedes
ms.openlocfilehash: 60ec322758a3c3b83e7b24cfba37daaec02fc4c0
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59270838"
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Öğretici: Igloo yazılım ile Azure Active Directory Tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile Igloo yazılım tümleştirme konusunda bilgi edinin.
Azure AD ile Igloo yazılım tümleştirme ile aşağıdaki avantajları sağlar:

* Igloo yazılım erişimi, Azure AD'de kontrol edebilirsiniz.
* Azure AD hesaplarına otomatik olarak (çoklu oturum açma) Igloo yazılım için oturum açmış, kullanıcıların etkinleştirebilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla ayrıntı bilmek istiyorsanız, bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesi Igloo yazılımıyla yapılandırmak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir Azure AD ortamını yoksa, bir aylık deneme alabilirsiniz [burada](https://azure.microsoft.com/pricing/free-trial/)
* Abonelik igloo yazılım çoklu oturum açma etkin

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD çoklu oturum açma bir test ortamında test edin.

* Destekleyen igloo yazılımı **SP** tarafından başlatılan
* Destekleyen igloo yazılımı **zamanında** kullanıcı sağlama

## <a name="adding-igloo-software-from-the-gallery"></a>Galeriden Igloo yazılım ekleme

Azure AD'de Igloo yazılım tümleştirmesini yapılandırmak için Igloo yazılım Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Igloo yazılım eklemek için aşağıdaki adımları gerçekleştirin:**

1. İçinde **[Azure portalında](https://portal.azure.com)**, sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları** seçeneği.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Igloo yazılım**seçin **Igloo yazılım** sonucu panelinden ardından **Ekle** uygulama eklemek için Ekle düğmesine.

     ![Sonuç listesinde igloo yazılım](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, yapılandırma ve Azure AD çoklu oturum açma Igloo yazılım adlı bir test kullanıcı tabanlı test **Britta Simon**.
Tek iş için oturum açma için bir Azure AD kullanıcısının Igloo yazılımla ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Yapılandırma ve Azure AD çoklu oturum açma Igloo yazılım ile test etmek için aşağıdaki yapı taşlarını tamamlanması gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Igloo yazılım çoklu oturum açmayı yapılandırma](#configure-igloo-software-single-sign-on)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Igloo yazılım test kullanıcısı oluşturma](#create-igloo-software-test-user)**  - kullanıcı Azure AD gösterimini bağlı Igloo yazılım Britta simon'un bir karşılığı vardır.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure AD çoklu oturum açma Azure portalında etkinleştirin.

Azure AD çoklu oturum açma Igloo yazılımıyla yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. İçinde [Azure portalında](https://portal.azure.com/), **Igloo yazılım** uygulama tümleştirme sayfasında **çoklu oturum açma**.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** iletişim kutusunda, **SAML/WS-Federasyon** modu, çoklu oturum açmayı etkinleştirmek için.

    ![Çoklu oturum açma seçim modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. Üzerinde **temel SAML yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![İgloo yazılım etki alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. İçinde **oturum açma URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<company name>.igloocommmunities.com`

    b. İçinde **tanımlayıcı** kutusuna şu biçimi kullanarak bir URL yazın: `https://<company name>.igloocommmunities.com/saml.digest`

    c. İçinde **yanıt URL'si** metin kutusuna şu biçimi kullanarak bir URL yazın: `https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerler gerçek oturum açma URL'si, tanımlayıcı ve yanıt URL'si ile güncelleştirin. İlgili kişi [Igloo yazılım istemcisi Destek ekibine](https://www.igloosoftware.com/services/support) bu değerleri almak için. Gösterilen desenleri de başvurabilirsiniz **temel SAML yapılandırma** bölümünde Azure portalında.

5. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **SAML imzalama sertifikası** bölümünde **indirme** indirmek için **sertifika (Base64)** bilgisayarınızdaki belirli seçenekler ihtiyacınıza göre ve kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. Üzerinde **Igloo yazılımını kurma** bölümünde, ihtiyacınıza göre uygun URL'lerini kopyalayın.

    ![Yapılandırma URL'leri kopyalayın](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-igloo-software-single-sign-on"></a>Igloo yazılım çoklu oturum açmayı yapılandırın

1. Farklı bir web tarayıcı penceresinde Igloo yazılım şirketinizin sitesi için bir yönetici olarak oturum açın.

2. Git **Denetim Masası**.

     ![Denetim Masası](./media/igloo-software-tutorial/ic799949.png "Denetim Masası")

3. İçinde **üyelik** sekmesinde **oturum ayarları**.

    ![Oturum açma ayarları](./media/igloo-software-tutorial/ic783968.png "oturum açma ayarları")

4. SAML yapılandırma bölümünde tıklayın **SAML kimlik doğrulamasını yapılandırma**.

    ![SAML yapılandırma](./media/igloo-software-tutorial/ic783969.png "SAML yapılandırma")

5. İçinde **genel yapılandırma** bölümünde, aşağıdaki adımları gerçekleştirin:

    ![Genel yapılandırma](./media/igloo-software-tutorial/ic783970.png "genel yapılandırma")

    a. İçinde **bağlantı adı** metin yapılandırmanızı için özel bir ad yazın.

    b. İçinde **Idp'nin oturum açma URL'si** metin değerini yapıştırın **oturum açma URL'si** , Azure Portalı'ndan kopyaladığınız.

    c. İçinde **IDP oturum kapatma URL'si** metin değerini yapıştırın **oturum kapatma URL'si** , Azure Portalı'ndan kopyaladığınız.

    d. Seçin **oturum kapatma yanıt ve İstek HTTP türü** olarak **POST**.

    e. Açık, **base-64** Defteri'nde kodlanmış sertifika Azure portalından indirilen, içeriğini, panoya kopyalayın ve ardından ona yapıştırın **ortak sertifika** metin.

6. İçinde **yanıt ve kimlik doğrulama Yapılandırması**, aşağıdaki adımları gerçekleştirin:

    ![Yanıt ve kimlik doğrulama Yapılandırması](./media/igloo-software-tutorial/IC783971.png "yanıt ve kimlik doğrulaması yapılandırması")
  
    a. Olarak **kimlik sağlayıcısı**seçin **Microsoft ADFS**.

    b. Olarak **tanımlayıcı türü**seçin **e-posta adresi**. 

    c. İçinde **e-posta özniteliği** metin kutusuna **emailaddress**.

    d. İçinde **ad özniteliği** metin kutusuna **givenname**.

    e. İçinde **son Name özniteliği** metin kutusuna **Soyadı**.

7. Yapılandırmayı tamamlamak için aşağıdaki adımları gerçekleştirin:

    ![Oturum açma kullanıcı oluşturma](./media/igloo-software-tutorial/IC783972.png "oturum açma kullanıcı oluşturma") 

    a. Olarak **oturum açma kullanıcı oluşturma**seçin **oturum açtığında, sitenizde yeni bir kullanıcı oluşturmak**.

    b. Olarak **oturum açma ayarları**seçin **ekranında "Oturum Aç" düğmesi kullanım SAML**.

    c. **Kaydet**’e tıklayın.

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

Bu bölümde, Azure çoklu oturum açma Igloo yazılıma erişim vererek kullanmak Britta Simon etkinleştirin.

1. Azure portalında **kurumsal uygulamalar**seçin **tüm uygulamaları**, ardından **Igloo yazılım**.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Igloo yazılım**.

    ![Uygulamalar listesinde Igloo yazılım bağlantı](common/all-applications.png)

3. Soldaki menüde **kullanıcılar ve gruplar**.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. Tıklayın **Kullanıcı Ekle** düğmesine ve ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylaması ardından içinde herhangi bir rolü değer bekleniyor durumunda **rolü Seç** 'a tıklayın listeden bir kullanıcı için uygun rolü Seç iletişim kutusu **seçin** ekranın alt kısmındaki düğmesi.

7. İçinde **atama Ekle** iletişim tıklatın **atama** düğmesi.

### <a name="create-igloo-software-test-user"></a>Igloo yazılım test kullanıcısı oluşturma

Hiçbir eylem öğesini, Igloo yazılım için kullanıcı sağlamayı yapılandırma yoktur.  

Atanan kullanıcı Igloo yazılıma erişim panelini kullanarak oturum açmak çalıştığında, Igloo yazılım, kullanıcının mevcut olup olmadığını denetler.  Varsa kullanıcı hesabı kullanılabilir henüz Igloo yazılımı tarafından otomatik olarak oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Igloo yazılım kutucuğa tıkladığınızda, otomatik olarak SSO'yu ayarlama Igloo yazılım için oturum açmanız. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)